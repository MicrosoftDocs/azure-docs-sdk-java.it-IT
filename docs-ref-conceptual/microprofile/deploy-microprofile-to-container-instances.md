---
title: Distribuire un'app MicroProfile nel cloud con Docker e Azure
description: Informazioni su come distribuire un'app MicroProfile nel cloud usando Docker e Istanze di Azure Container.
services: container-instances;container-retistry
documentationcenter: java
author: brunoborges
manager: routlaw
editor: brunoborges
ms.assetid: ''
ms.author: brborges
ms.date: 11/21/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 22870b7ba32f115e7270c63d1bf42cbfc6531d7e
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338785"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a>Distribuire un'applicazione MicroProfile nel cloud con Docker e Azure

Questo articolo mostra come creare un pacchetto dell'applicazione [MicroProfile.io] in un contenitore Docker ed eseguire l'applicazione in Istanze di Azure Container.

> [!NOTE]
>
> Questa procedura funziona con qualsiasi implementazione di MicroProfile.io finché l'immagine del contenitore Docker è autoeseguibile, ovvero include il runtime.

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura di questa esercitazione, saranno necessari i prerequisiti seguenti:

* Sottoscrizione di Azure. Se non se ne ha già una, è possibile iscriversi per ottenere un [account Azure gratuito].
* [Interfaccia della riga di comando di Azure].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* Strumento di compilazione [Maven] di Apache (versione 3 o successiva).
* Un client [Git].

## <a name="microprofile-hello-azure-sample"></a>Esempio MicroProfile Hello Azure

In questo articolo si userà l'esempio [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure):

### <a name="clone-build-and-run-locally"></a>Clonare, compilare ed eseguire localmente

```bash
$ git clone https://github.com/Azure-Samples/microprofile-hello-azure.git
$ mvn clean package
...
[INFO] BUILD SUCCESS
...
$ mvn payara-micro:start
...
[2018-07-30T13:34:51.553-0700] [] [INFO] [] [PayaraMicro] [tid: _ThreadID=1 _ThreadName=main] [timeMillis: 1532982891553] [levelValue: 800] Payara Micro  5.182 #badassmicrofish (build 303) ready in 10,304 (ms)
...
```

È possibile testare l'applicazione chiamando `curl` o accedendovi tramite un [browser](http://localhost:8080/api/hello):

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a>Distribuisci in Azure

A questo punto è possibile trasferire l'applicazione nel cloud usando i servizi [Istanze di contenitore di Azure] e [Registro contenitori di Azure].

### <a name="build-a-docker-image"></a>Creare un'immagine Docker

Il progetto di esempio fornisce già un documento Dockerfile che è possibile usare. Non è tuttavia necessario installare Docker perché si userà la funzionalità di compilazione del Registro contenitori di Azure per creare l'immagine nel cloud.

Per compilare l'immagine e predisporre l'esecuzione in Azure, è necessario seguire questa procedura:

1. Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso con l'interfaccia stessa
1. Creare un gruppo di risorse di Azure
1. Creare un'istanza di Registro Azure Container
1. Compilare l'immagine Docker
1. Pubblicare l'immagine Docker nell'istanza di Registro Azure Container creata in precedenza
1. (Facoltativo) Eseguire la compilazione e la pubblicazione in Registro Azure Container con un solo comando


#### <a name="set-up-azure-cli"></a>Configurare l'interfaccia della riga di comando di Azure

Assicurarsi di avere una sottoscrizione di Azure, di aver installato l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e di aver eseguito l'autenticazione all'account:

```bash
az login
```

#### <a name="create-a-resource-group"></a>Creare un gruppo di risorse

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a>Creare un'istanza di Registro contenitori di Azure

Questo comando creerà un registro contenitori auspicabilmente univoco a livello globale con un nome di base e un numero casuale.

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>Compilare l'immagine Docker

Anche se si può facilmente creare l'immagine Docker in locale usando Docker stesso, può essere opportuno creare l'immagine nel cloud per alcuni motivi:

1. Non è necessario installare Docker in locale
1. L'operazione è molto più veloce, fatta eccezione per il tempo di caricamento del contesto, perché la creazione avverrà altrove
1. L'elaborazione nel cloud ha accesso a una velocità Internet maggiore, quindi offre download più veloci
1. L'immagine viene inserita direttamente nel Registro contenitori

Per queste ragioni, l'immagine verrà creata con la funzionalità di [compilazione del Registro contenitori di Azure]:

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a>Distribuire l'immagine Docker da Registro Azure Container a Istanze di Azure Container

Ora che l'immagine è disponibile in Registro Azure Container, eseguire il push di un'istanza di contenitore in Istanze di Azure Container. Prima di tutto occorre tuttavia verificare che sia possibile eseguire l'autenticazione in Registro Azure Container:

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>Testare l'applicazione MicroProfile distribuita

L'applicazione dovrebbe essere ora operativa. Per testarla dalla riga di comando, provare il comando seguente:

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

Congratulazioni! È stata compilata e distribuita un'applicazione MicroProfile come contenitore Docker in Microsoft Azure.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:

* [Accedere ad Azure dall'interfaccia della riga di comando di Azure](/azure/xplat-cli-connect)

<!-- URL List -->

[Compilazione del Registro contenitori di Azure]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Istanze di contenitore di Azure]: https://docs.microsoft.com/azure/container-instances/
[Registro contenitori di Azure]:  https://docs.microsoft.com/azure/container-registry