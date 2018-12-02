---
title: Distribuire un'app MicroProfile nel cloud con Docker e Azure
description: Informazioni su come distribuire un'app MicroProfile nel cloud usando Docker e Istanze di contenitore di Azure.
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
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="7d73e-103">Distribuire un'applicazione MicroProfile nel cloud con Docker e Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="7d73e-104">Questo articolo mostra come creare un pacchetto dell'applicazione [MicroProfile.io] in un contenitore Docker ed eseguire l'applicazione in Istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="7d73e-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="7d73e-105">Questa procedura funziona con qualsiasi implementazione di MicroProfile.io finché l'immagine del contenitore Docker è autoeseguibile, ovvero include il runtime.</span><span class="sxs-lookup"><span data-stu-id="7d73e-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d73e-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7d73e-106">Prerequisites</span></span>

<span data-ttu-id="7d73e-107">Per completare la procedura di questa esercitazione, saranno necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="7d73e-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="7d73e-108">Sottoscrizione di Azure. Se non se ne ha già una, è possibile iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="7d73e-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="7d73e-109">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="7d73e-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="7d73e-110">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="7d73e-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="7d73e-111">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="7d73e-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="7d73e-112">Strumento di compilazione [Maven] di Apache (versione 3 o successiva).</span><span class="sxs-lookup"><span data-stu-id="7d73e-112">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="7d73e-113">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="7d73e-113">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="7d73e-114">Esempio MicroProfile Hello Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-114">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="7d73e-115">In questo articolo si userà l'esempio [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure):</span><span class="sxs-lookup"><span data-stu-id="7d73e-115">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="7d73e-116">Clonare, compilare ed eseguire localmente</span><span class="sxs-lookup"><span data-stu-id="7d73e-116">Clone, build, and run locally</span></span>

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

<span data-ttu-id="7d73e-117">È possibile testare l'applicazione chiamando `curl` o accedendovi tramite un [browser](http://localhost:8080/api/hello):</span><span class="sxs-lookup"><span data-stu-id="7d73e-117">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="7d73e-118">Distribuisci in Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-118">Deploy to Azure</span></span>

<span data-ttu-id="7d73e-119">A questo punto è possibile trasferire l'applicazione nel cloud usando i servizi [Istanze di contenitore di Azure] e [Registro contenitori di Azure].</span><span class="sxs-lookup"><span data-stu-id="7d73e-119">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="7d73e-120">Creare un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="7d73e-120">Build a Docker image</span></span>

<span data-ttu-id="7d73e-121">Il progetto di esempio fornisce già un documento Dockerfile che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="7d73e-121">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="7d73e-122">Non è tuttavia necessario installare Docker perché si userà la funzionalità di compilazione del Registro contenitori di Azure per creare l'immagine nel cloud.</span><span class="sxs-lookup"><span data-stu-id="7d73e-122">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="7d73e-123">Per compilare l'immagine e predisporre l'esecuzione in Azure, è necessario seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="7d73e-123">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="7d73e-124">Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso con l'interfaccia stessa</span><span class="sxs-lookup"><span data-stu-id="7d73e-124">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="7d73e-125">Creare un gruppo di risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-125">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="7d73e-126">Creare un'istanza di Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-126">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="7d73e-127">Compilare l'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="7d73e-127">Build the Docker image</span></span>
1. <span data-ttu-id="7d73e-128">Pubblicare l'immagine Docker nell'istanza di Registro contenitori di Azure creata in precedenza</span><span class="sxs-lookup"><span data-stu-id="7d73e-128">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="7d73e-129">(Facoltativo) Eseguire la compilazione e la pubblicazione nel Registro contenitori di Azure con un solo comando</span><span class="sxs-lookup"><span data-stu-id="7d73e-129">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="7d73e-130">Configurare l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-130">Set up Azure CLI</span></span>

<span data-ttu-id="7d73e-131">Assicurarsi di avere una sottoscrizione di Azure, di aver installato l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e di aver eseguito l'autenticazione all'account:</span><span class="sxs-lookup"><span data-stu-id="7d73e-131">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="7d73e-132">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="7d73e-132">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="7d73e-133">Creare un'istanza di Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-133">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="7d73e-134">Questo comando creerà un registro contenitori auspicabilmente univoco a livello globale con un nome di base e un numero casuale.</span><span class="sxs-lookup"><span data-stu-id="7d73e-134">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="7d73e-135">Compilare l'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="7d73e-135">Build the Docker image</span></span>

<span data-ttu-id="7d73e-136">Anche se si può facilmente creare l'immagine Docker in locale usando Docker stesso, può essere opportuno creare l'immagine nel cloud per alcuni motivi:</span><span class="sxs-lookup"><span data-stu-id="7d73e-136">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="7d73e-137">Non è necessario installare Docker in locale</span><span class="sxs-lookup"><span data-stu-id="7d73e-137">No need to install Docker locally</span></span>
1. <span data-ttu-id="7d73e-138">L'operazione è molto più veloce, fatta eccezione per il tempo di caricamento del contesto, perché la creazione avverrà altrove</span><span class="sxs-lookup"><span data-stu-id="7d73e-138">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="7d73e-139">L'elaborazione nel cloud ha accesso a una velocità Internet maggiore, quindi offre download più veloci</span><span class="sxs-lookup"><span data-stu-id="7d73e-139">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="7d73e-140">L'immagine viene inserita direttamente nel Registro contenitori</span><span class="sxs-lookup"><span data-stu-id="7d73e-140">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="7d73e-141">Per queste ragioni, l'immagine verrà creata con la funzionalità di [compilazione del Registro contenitori di Azure]:</span><span class="sxs-lookup"><span data-stu-id="7d73e-141">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="7d73e-142">Distribuire l'immagine Docker dal Registro contenitori di Azure a Istanze di contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-142">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="7d73e-143">Ora che l'immagine è disponibile nel Registro contenitori di Azure, eseguire il push di un'istanza di contenitore in Istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="7d73e-143">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="7d73e-144">Prima di tutto occorre tuttavia verificare che sia possibile eseguire l'autenticazione nel Registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="7d73e-144">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="7d73e-145">Testare l'applicazione MicroProfile distribuita</span><span class="sxs-lookup"><span data-stu-id="7d73e-145">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="7d73e-146">L'applicazione dovrebbe essere ora operativa.</span><span class="sxs-lookup"><span data-stu-id="7d73e-146">Your application should now be up and running.</span></span> <span data-ttu-id="7d73e-147">Per testarla dalla riga di comando, provare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7d73e-147">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="7d73e-148">Congratulazioni!</span><span class="sxs-lookup"><span data-stu-id="7d73e-148">Congratulations!</span></span> <span data-ttu-id="7d73e-149">È stata compilata e distribuita un'applicazione MicroProfile come contenitore Docker in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7d73e-149">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d73e-150">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="7d73e-150">Next steps</span></span>

<span data-ttu-id="7d73e-151">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="7d73e-151">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="7d73e-152">Accedere ad Azure dall'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="7d73e-152">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Compilazione del Registro contenitori di Azure]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Istanze di contenitore di Azure]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Registro contenitori di Azure]:  https://docs.microsoft.com/azure/container-registry
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry