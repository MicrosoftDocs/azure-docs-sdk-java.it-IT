---
title: Distribuire un'app MicroProfile nel cloud con Docker e Azure
description: Informazioni su come distribuire un'app MicroProfile nel cloud usando Docker e Istanze di Azure Container.
services: container-instances;container-registry
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
ms.openlocfilehash: 6ba12bb183969103676fa988199603df6cf36bba
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533610"
---
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a>Distribuire un'app MicroProfile nel cloud con Docker e Azure

Questo articolo mostra come creare un pacchetto dell'applicazione [MicroProfile.io] in un contenitore Docker ed eseguire l'applicazione in Istanze di Azure Container.

> [!NOTE]
> Questa procedura funziona con qualsiasi implementazione di MicroProfile.io purché l'immagine del contenitore Docker sia autoeseguibile, ovvero includa il runtime.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione è necessario soddisfare i prerequisiti seguenti:

* Una sottoscrizione di Azure. Se non si ha ancora una sottoscrizione di Azure, è possibile iscriversi per ottenere un [account Azure gratuito].
* [Interfaccia della riga di comando di Azure] installata.
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere [Supporto a lungo termine di Java per Azure e Azure Stack](https://aka.ms/azure-jdks).
* Strumento di compilazione di [Apache Maven] (versione 3 o successiva).
* Un client [Git].

## <a name="microprofile-hello-azure-sample"></a>Esempio MicroProfile Hello Azure

In questo articolo si usa l'esempio [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure). Clonarlo, compilarlo ed eseguirlo in locale usando i comandi seguenti:

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

È possibile testare l'applicazione chiamando `curl` o usando un [browser](http://localhost:8080/api/hello):

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

A questo punto trasferire l'applicazione in Azure usando i servizi [Istanze di Azure Container] e [Registro Azure Container].

### <a name="build-a-docker-image"></a>Creare un'immagine Docker

Il progetto di esempio include un documento Dockerfile che è possibile usare. Non è però necessario che Docker sia installato perché si userà la funzionalità di compilazione di Registro Azure Container per compilare l'immagine nel cloud.

Per compilare l'immagine e prepararla per l'esecuzione in Azure, seguire questa procedura:

1. Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso.
1. Creare un gruppo di risorse di Azure.
1. Creare un'istanza di Registro Azure Container.
1. Compilare un'immagine Docker.
1. Pubblicare l'immagine Docker nell'istanza di Registro Azure Container creata in precedenza.
1. (Facoltativo) Compilare e pubblicare l'immagine nell'istanza di Registro Container con un unico comando.


#### <a name="set-up-the-azure-cli"></a>Configurare l'interfaccia della riga di comando di Azure

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

#### <a name="create-a-container-registry-instance"></a>Creare un'istanza di Registro Container

Questo comando deve creare un'istanza univoca a livello globale di Registro Container con un nome di base e un numero casuale.

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>Compilare l'immagine Docker

Anche se è possibile compilare facilmente l'immagine Docker in locale usando Docker stesso, può essere opportuno compilarla nel cloud per alcuni motivi:

* Non è necessario installare Docker in locale.
* L'operazione è molto più veloce, fatta eccezione per il tempo di caricamento del contesto, perché la compilazione avverrà altrove.
* L'elaborazione nel cloud può sfruttare una velocità Internet maggiore, quindi offre download più veloci.
* L'immagine viene inserita direttamente nell'istanza di Registro Container.

Per questi motivi, l'immagine viene compilata con la funzionalità di [compilazione di Registro Azure Container]:

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a>Distribuire l'immagine Docker dall'istanza di Registro Azure Container a Istanze di Azure Container

Ora che l'immagine è disponibile nell'istanza di Registro Azure Container, eseguirne il push e crearne un'istanza in Istanze di Azure Container. Assicurarsi prima che sia possibile eseguire l'autenticazione in Registro Azure Container:

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>Testare l'applicazione MicroProfile distribuita

L'applicazione dovrebbe essere ora operativa. Per testarla dall'interfaccia della riga di comando, usare il comando seguente:

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

Congratulazioni! Un'applicazione MicroProfile è stata compilata come contenitore Docker e distribuita in Microsoft Azure.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere:

* [Accedere ad Azure dall'interfaccia della riga di comando di Azure](/azure/xplat-cli-connect)

<!-- URL List -->

[Compilazione di Registro Azure Container]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Istanze di Azure Container]: https://docs.microsoft.com/azure/container-instances/
[Registro Azure Container]:  https://docs.microsoft.com/azure/container-registry
