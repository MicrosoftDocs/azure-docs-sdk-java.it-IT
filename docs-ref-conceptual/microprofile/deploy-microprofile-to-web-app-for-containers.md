---
title: Distribuire un servizio MicroProfile basato su Java in app Web per contenitori di Azure
description: Informazioni su come distribuire un servizio MicroProfile usando Docker e app Web per contenitori di Azure
services: container-registry;app-service
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: container-registry;app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 1eb0e7d7a718a1c106adebbf89011f6e3fa1504e
ms.sourcegitcommit: c2019ba6da6c7c28b17b5a85f89e49bb5e570ba4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040249"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>Distribuire un servizio MicroProfile basato su Java in app Web per contenitori di Azure

MicroProfile è un ottimo modo per compilare applicazioni Java estremamente piccole che possono essere rapidamente e facilmente distribuite in servizi come [app Web per contenitori di Azure](https://azure.microsoft.com/services/app-service/containers/). In questa esercitazione verrà creato un semplice microservizio basato su MicroProfile che verrà eseguito in un contenitore Docker, distribuito in un [Registro contenitori di Azure](https://azure.microsoft.com/services/container-registry/) e quindi ospitato con app Web per contenitori di Azure.

> [!NOTE]
>
> Questa procedura funziona con qualsiasi implementazione di MicroProfile.io finché l'immagine del contenitore Docker è autoeseguibile, ovvero include il runtime.

Più concretamente, questo esempio usa [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile 1.3](https://microprofile.io/) per creare un piccolo file WAR Java (5.085 byte nel computer degli autori) e impacchettarlo in un'immagine Docker di circa 174 MB. Questa immagine Docker contiene tutto il necessario per una distribuzione in contenitori di questa app Web.

Data la modalità di funzionamento di Docker, spesso non è necessario distribuire di nuovo l'intera immagine Docker da 174 MB ogni volta che si modifica il codice sorgente dell'applicazione. Docker caricherà infatti solo le differenze, che hanno dimensioni nettamente inferiori. L'esecuzione di una nuova versione di un'applicazione MicroProfile tramite una pipeline di integrazione continua/distribuzione continua è quindi estremamente efficiente e veloce, con una ridotta possibilità di errore e una rapida iterazione dello sviluppo.

Questa esercitazione inizierà con la creazione e l'esecuzione del codice in locale, quindi il codice verrà distribuito come app Web in Azure. In entrambi i casi, Docker semplificherà e standardizzerà queste attività. Prima di iniziare, verrà creato un Registro contenitori di Azure nel quale archiviare i contenitori Docker.

## <a name="creating-an-azure-container-registry"></a>Creazione di un Registro contenitori di Azure

Il Registro contenitori di Azure verrà creato usando il [portale di Azure](http://portal.azure.com), ma sono disponibili alternative come l'interfaccia della riga di comando di Azure. Seguire questa procedura per creare un nuovo Registro contenitori di Azure:

1. Accedere al [portale di Azure](http://portal.azure.com) e creare una nuova risorsa Registro contenitori di Azure. Specificare un nome per il registro. Questo nome dovrà essere impostato come proprietà `docker.registry` in `pom.xml`. Modificare le impostazioni predefinite secondo le esigenze, quindi fare clic su "Crea".

1. Quando il registro contenitori diventa disponibile (circa 30 secondi dopo aver fatto clic su "Crea"), fare clic sul registro contenitori, quindi sul collegamento "Chiavi di accesso" nell'area dei menu a sinistra. Qui è necessario abilitare l'impostazione "Utente amministratore" per rendere il registro contenitori accessibile dai computer (per il push dei contenitori Docker) e per consentire l'accesso dall'istanza di app Web per contenitori di Azure che verrà configurata più avanti nell'esercitazione.

1. Nell'area "Chiavi di accesso" notare i valori `username` e `password` che verranno copiati e incollati nel file Maven globale `settings.xml`. Per altre informazioni sulle impostazioni Maven, vedere il sito Web del [progetto Apache Maven](https://maven.apache.org/settings.html). A titolo di riferimento, di seguito è riportata una versione offuscata del file `${user.home}/.m2/settings.xml` presente nel sistema degli autori:

    ```xml
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
          <server>
            <id>jogilescr.azurecr.io</id>
            <username>jogilescr</username>
            <password>ojoirshois.this-isn't-real.hrihslirhlishrglih</password>
          </server>
        </servers>
    </settings>
    ```

Terminata l'operazione si può passare alla compilazione e all'esecuzione dell'applicazione MicroProfile nell'ambiente locale.

## <a name="creating-our-microprofile-application"></a>Creazione dell'applicazione MicroProfile

Questo esempio si basa su un'applicazione di esempio disponibile in GitHub, che verrà clonata per poi esaminarne il codice. Seguire questa procedura per clonare il codice nel computer:

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

In questa directory è presente un file `pom.xml` che viene usato per specificare il progetto nel formato usato dallo strumento di compilazione Maven. Questo file può essere modificato in base alle proprie esigenze. In particolare, le proprietà `docker.registry` e `docker.name` devono essere sostituite con i valori `docker.registry` e `docker.name` creati durante la configurazione del Registro contenitori di Azure.

Un altro file importante presente in questa directory è il documento Dockerfile, riprodotto di seguito:

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

Questo documento Dockerfile crea semplicemente un nuovo contenitore Docker basato sul contenitore Docker Payara Micro ed esegue la copia nel file WAR creato nell'ambito del processo di compilazione. Espone anche la porta 8080 per rendere il servizio accessibile quando è in esecuzione in un contenitore Docker.

Nella directory `src` è anche presente la classe `Application` riprodotta di seguito:

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

L'annotazione `@ApplicationPath("/api")` specifica l'endpoint di base per questo microservizio. In altre parole, in tutti gli endpoint `/api` precederà il resto dell'URL necessario per accedere a qualsiasi endpoint REST specifico.

All'interno del pacchetto `api` è presente una classe denominata `API` che contiene il codice seguente:

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld.api;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

import static javax.ws.rs.core.MediaType.TEXT_HTML;

@ApplicationScoped
@Path("/")
public class API {

    @GET
    @Path("/helloworld")
    @Produces(TEXT_HTML)
    public String info() {
        return "Hello, world!";
    }
}
```

Con l'uso dell'annotazione `@Path("/helloworld")` è possibile vedere che questo endpoint REST, se in combinazione con il valore `/api` specificato nella classe `Application`, sarà `/api/helloworld`. Quando questo endpoint viene chiamato usando una richiesta HTTP GET, il metodo produrrà testo/HTML e si tratta semplicemente di una stringa "Hello, world!" hardcoded.

È stato così esaminato tutto il codice necessario per creare un microservizio con MicroProfile. Ora è possibile usare Maven per la compilazione, l'inserimento in un contenitore Docker e l'esecuzione nell'ambiente locale. A tale scopo seguire questa procedura:

1. Eseguire `mvn clean package` e attendere il termine dell'operazione.

1. Eseguire `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`, ad esempio `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`, se `docker.registry` è `jogilescr.azurecr.io` e `docker.name` è `samples/docker-helloworld`.

1. Provare ad accedere a [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) e [http://localhost:8080/health](http://localhost:8080/health) nel Web browser. Se viene visualizzata la risposta "Hello, world!" prevista (e informazioni relative all'integrità per l'endpoint [/health](http://localhost:8080/health)), l'applicazione MicroProfile è stata distribuita correttamente nel computer locale.

## <a name="pushing-to-the-azure-container-registry"></a>Esecuzione del push nel Registro contenitori di Azure

Ora che è stata compilata ed eseguita l'applicazione MicroProfile nel computer locale, il passo successivo è eseguire il push di questo contenitore nel registro contenitori. In questa esercitazione si usa il Registro contenitori di Azure, ma è possibile usare qualsiasi registro contenitori purché il file `pom.xml` venga modificato in modo da puntare al percorso corrispondente.

1. Eseguire `mvn clean package` per pulire, compilare e creare un'immagine Docker locale.
2. Eseguire `mvn dockerfile:push` per effettuare il push nel Registro contenitori di Azure.

A questo punto, l'immagine del contenitore Docker è stata caricata nel Registro contenitori di Azure, ma non è ancora in esecuzione perché è necessario distribuirla in un'istanza di app Web per contenitori di Azure. Questa operazione verrà eseguita ora.

## <a name="creating-an-azure-web-app-for-containers-instance"></a>Creazione di un'istanza di app Web per contenitori di Azure

1. Tornare al [portale di Azure](http://portal.azure.com) e creare una nuova istanza di app Web per contenitori, sotto la voce "Web e dispositivi mobili" nel menu. Alcune informazioni utili:

   1. Il nome specificato qui sarà l'URL pubblico dell'app Web (anche se è possibile aggiungere un dominio personalizzato in seguito), quindi è opportuno scegliere un nome facile da ricordare.

   1. Quando si passa alla sezione "Configura contenitore" è possibile selezionare "Registro contenitori di Azure" per "Origine immagine", quindi selezionare l'immagine corretta dagli elenchi a discesa.

   1. Non è necessario specificare alcun valore nel campo "File di avvio".

1. Dopo aver creato l'istanza (operazione molto rapida), fare clic su di essa e quindi sulla voce di menu "Impostazioni applicazione". Qui è necessario aggiungere una nuova impostazione applicazione, in cui la chiave è `WEBSITES_PORT` e il valore è `8080`. Si indica così ad Azure quale porta si vuole esporre nel contenitore. Di questa porta verrà eseguito il mapping alla porta 80 esternamente.

1. Facoltativamente, fare clic sul collegamento "Contenitore Docker" e abilitare "Distribuzione continua" in modo che ogni volta che si aggiorna l'immagine del Registro contenitori di Azure, questa venga aggiornata automaticamente nell'istanza di app Web per contenitori di Azure.

1. Dovrebbe essere possibile accedere alle istanze ospitate in Azure agli indirizzi `http://<appname>.azurewebsites.net/microprofile/api/helloworld` e `http://<appname>.azurewebsites.net/health`.

## <a name="summary"></a>Summary

Con questa esercitazione è stato esaminato il processo di creazione di un semplice microservizio basato su MicroProfile, successivamente inserito in un contenitore Docker e quindi eseguito nell'ambiente locale e pubblicato in Azure. L'estensione del microservizio per mettere a disposizione altre funzioni utili non rientra nell'ambito di questa esercitazione. Sono tuttavia disponibili molti consigli ed esercitazioni su MicroProfile in Internet. È consigliabile vedere [MicroProfile.io](https://microprofile.io/) per altri contenuti.
