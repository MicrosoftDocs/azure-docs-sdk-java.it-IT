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
ms.openlocfilehash: 4ecfbf92bc30bc491c991e30111cce8375da7f1a
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533596"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>Distribuire un servizio MicroProfile basato su Java in app Web per contenitori di Azure

MicroProfile è una soluzione ottimale per compilare applicazioni Java estremamente piccole che è possibile distribuire in modo semplice e rapido in servizi come l'[app Web per contenitori di Azure](https://azure.microsoft.com/services/app-service/containers/). In questa esercitazione viene creato un semplice microservizio basato su MicroProfile che viene eseguito in un contenitore Docker, distribuito in un'istanza di [Registro Azure Container](https://azure.microsoft.com/services/container-registry/) e quindi ospitato con l'app Web per contenitori di Azure.

> [!NOTE]
> Questa procedura funziona con qualsiasi implementazione di MicroProfile.io purché l'immagine del contenitore Docker sia autoeseguibile, ovvero includa il runtime.

In questo esempio si usano [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile 1.3](https://microprofile.io/) per creare un file WAR (Web Application Requirement) Java di piccole dimensioni (all'incirca 5.000 byte) e quindi aggiungerlo a un pacchetto in un'immagine Docker di circa 174 MB. L'immagine Docker contiene tutto il necessario per una distribuzione in contenitori dell'app Web.

Non è necessario distribuire di nuovo l'intera immagine Docker da 174 MB ogni volta che si modifica il codice sorgente dell'applicazione perché Docker carica solo le differenze, che hanno dimensioni nettamente inferiori. Di conseguenza, l'esecuzione di una nuova versione di un'applicazione MicroProfile tramite una pipeline CI/CD è quindi estremamente efficiente e veloce, con una ridotta possibilità di errore e una rapida iterazione dello sviluppo.

In questa esercitazione si inizierà con la creazione e l'esecuzione del codice in locale prima di distribuire il codice come app Web in Azure. In entrambe le fasi è possibile usare Docker per semplificare e standardizzare queste attività. Prima di iniziare, verrà creata un'istanza di Registro Azure Container nella quale archiviare i contenitori Docker.

## <a name="create-an-azure-container-registry-instance"></a>Creare un'istanza di Registro Azure Container

Oltre al [portale di Azure](http://portal.azure.com) è possibile usare altri metodi per creare l'istanza di Registro Azure Container, ad esempio l'interfaccia della riga di comando di Azure. Per creare una nuova istanza di Registro Azure Container, seguire questa procedura:

1. Accedere al [portale di Azure](http://portal.azure.com) e creare una nuova risorsa di Registro Azure Container. Specificare un nome per il registro. Questo nome deve essere impostato come proprietà `docker.registry` nel file *pom.xml*. Modificare le impostazioni predefinite secondo le esigenze, quindi selezionare **Crea**.

1. Dopo circa 30 secondi, quando l'istanza di Registro Container diventa disponibile, selezionare l'istanza e quindi, nel riquadro sinistro, selezionare il collegamento **Chiavi di accesso**. 

    Assicurarsi di abilitare l'impostazione *Utente amministratore* in modo da poter accedere a questa istanza di Registro Container dai computer ed eseguirvi il push dei contenitori Docker, nonché per consentire l'accesso dall'istanza dell'app Web per contenitori di Azure che verrà configurata più avanti.

1. Nel riquadro **Chiavi di accesso** copiare i valori di **nome utente** e **password** e incollarli nel file *settings.xml* globale di Maven. Per altre informazioni sulle impostazioni Maven, visitare il sito Web del [progetto Apache Maven](https://maven.apache.org/settings.html). 

   Come riferimento, ecco un esempio di file *${user.home}/.m2/settings.xml*:

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

A questo punto, dopo aver creato l'istanza di Registro Container, si può passare alla compilazione e all'esecuzione dell'applicazione MicroProfile in locale.

## <a name="create-your-microprofile-application"></a>Creare l'applicazione MicroProfile

Questo esempio di codice si basa su un'applicazione di esempio disponibile in GitHub. Per clonare il file nel computer, immettere i comandi seguenti:

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

In questa directory è presente un file *pom.xml* che viene usato per specificare il progetto nel formato usato dallo strumento di compilazione Maven. Questo file può essere modificato in base alle proprie esigenze. In particolare, i valori delle proprietà `docker.registry` e `docker.name` devono essere sostituiti con i valori delle proprietà `docker.registry` e `docker.name` create durante la configurazione dell'istanza di Registro Azure Container.

Un altro file importante presente in questa directory è il documento Dockerfile, riprodotto di seguito:

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

Il documento Dockerfile crea un nuovo contenitore Docker basato sul contenitore Docker Payara Micro e ne esegue la copia nel file WAR creato durante il processo di compilazione. Espone inoltre la porta 8080 in modo che il servizio sia accessibile quando è in esecuzione in un contenitore Docker.

Quando si apre la directory *src*, viene individuata la classe `Application` riprodotta di seguito:

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

L'annotazione *@ApplicationPath("/api")* specifica l'endpoint di base per questo microservizio. In altre parole, in tutti gli endpoint l'annotazione */api* precede il resto dell'URL necessario per accedere a qualsiasi endpoint REST specifico.

All'interno del pacchetto *api* è presente una classe denominata `API` che contiene il codice seguente:

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

Usando l'annotazione *@Path("/helloworld")* , è possibile vedere che questo endpoint REST, se combinato con l'annotazione */api* specificata nella classe `Application`, sarà */api/helloworld*. Quando si chiama questo endpoint usando una richiesta HTTP GET, il metodo produce testo/HTML e si tratta semplicemente di una stringa "Hello, world!" hardcoded.

Fino a questo punto, in questo articolo è stato esaminato tutto il codice necessario per creare un microservizio con MicroProfile. Ora è possibile usare Maven per la compilazione, l'inserimento in un contenitore Docker e l'esecuzione nell'ambiente locale, eseguendo le operazioni seguenti:

1. Eseguire `mvn clean package` e attendere il completamento dell'operazione.

1. Eseguire `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`. Ad esempio, *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*, dove *\<docker.registry>* è *jogilescr.azurecr.io* e *\<docker.name>* è *samples/docker-helloworld*.

1. Provare ad accedere a [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) e [http://localhost:8080/health](http://localhost:8080/health) nel Web browser. Se viene visualizzata la risposta "Hello, world!" prevista (e informazioni relative all'integrità per l'endpoint [/health](http://localhost:8080/health)), l'applicazione MicroProfile è stata distribuita correttamente nel computer locale.

## <a name="push-the-container-to-the-azure-container-registry-instance"></a>Eseguire il push del contenitore nell'istanza di Registro Azure Container

Ora che è stata compilata ed eseguita l'applicazione MicroProfile nel computer locale, eseguire il push di questo contenitore nell'istanza di Registro Container. 

> [!NOTE]
> Anche se in questo articolo si usa un'istanza di Registro Azure Container, è possibile usare qualsiasi altra istanza purché il file *pom.xml* venga modificato in modo da puntare al percorso corrispondente.

1. Per pulire, compilare e creare un'immagine Docker locale, eseguire `mvn clean package`.
2. Per eseguire il push del contenitore nell'istanza di Registro Azure Container, eseguire `mvn dockerfile:push`.

L'immagine del contenitore Docker verrà ora caricata nell'istanza di Registro Azure Container, ma non è ancora in esecuzione. È necessario distribuirla in un'istanza dell'app Web per contenitori di Azure. 

## <a name="create-an-azure-web-app-for-containers-instance"></a>Creare un'istanza dell'app Web per contenitori di Azure

1. Nel [portale di Azure](http://portal.azure.com), selezionare **Web e dispositivi mobili** nel riquadro sinistro e quindi eseguire le operazioni seguenti:

   a. Specificare un nome. Il nome diventerà l'URL pubblico dell'app Web, quindi è opportuno scegliere un nome facile da ricordare. Se si preferisce, è possibile aggiungere in seguito un nome di dominio personalizzato.

   b. Nella sezione **Configura contenitore** selezionare **Registro Azure Container** in **Origine immagine** e quindi, nell'elenco a discesa, selezionare l'immagine corretta.

   c. Non è necessario specificare alcun valore nel campo **File di avvio**.

1. Dopo aver creato l'istanza selezionarla e quindi scegliere **Impostazioni applicazione**. Come valore di **Chiave** immettere **WEBSITES_PORT** e come valore di **Valore** immettere **8080**. Queste impostazioni indicano ad Azure la porta da esporre nel contenitore, nonché il numero di porta (80) per il mapping esterno.

1. (Facoltativo) Selezionare il collegamento **Contenitore Docker** e quindi abilitare **Distribuzione continua**. In questo modo ogni volta che si aggiorna l'immagine dell'istanza di Registro Azure Container, questa viene aggiornata automaticamente nell'istanza dell'app Web per contenitori di Azure.

1. A questo punto dovrebbe essere possibile accedere alle istanze ospitate in Azure agli indirizzi `http://<appname>.azurewebsites.net/microprofile/api/helloworld` e `http://<appname>.azurewebsites.net/health`.

## <a name="summary"></a>Summary

In questa esercitazione è stato esaminato il processo di creazione di un semplice microservizio basato su MicroProfile, successivamente inserito in un contenitore Docker e quindi eseguito nell'ambiente locale e pubblicato in Azure. È possibile estendere il microservizio per offrire altre funzionalità utili. Per altre informazioni, vedere [MicroProfile.io](https://microprofile.io/).
