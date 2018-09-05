---
title: Distribuire un'app MicroProfile nel cloud con Docker e Azure
description: Informazioni su come distribuire un'app MicroProfile nel cloud usando Docker e Istanze di contenitore di Azure.
services: container-instances;container-retistry
documentationcenter: java
author: brborges
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: brborges
ms.date: 07/30/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c6254d11ee1596a23076931c9a2a2370b5f52409
ms.sourcegitcommit: 3d0896f821907278547c283c54b53fbd7f4f30f0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43153859"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="c484a-103">Distribuire un'applicazione MicroProfile nel cloud con Docker e Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="c484a-104">Questo articolo mostra come creare un pacchetto dell'applicazione [MicroProfile.io] in un contenitore Docker ed eseguire l'applicazione in Istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="c484a-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="c484a-105">Questa procedura funziona con qualsiasi implementazione di MicroProfile.io finché l'immagine del contenitore Docker è autoeseguibile, ovvero include il runtime.</span><span class="sxs-lookup"><span data-stu-id="c484a-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c484a-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c484a-106">Prerequisites</span></span>

<span data-ttu-id="c484a-107">Per completare la procedura di questa esercitazione, saranno necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c484a-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="c484a-108">Sottoscrizione di Azure. Se non se ne ha già una, è possibile iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="c484a-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="c484a-109">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="c484a-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="c484a-110">Java Development Kit (JDK) aggiornato, versione 1.8 o successiva.</span><span class="sxs-lookup"><span data-stu-id="c484a-110">An up-to-date [Java Development Kit (JDK)], version 1.8 or later.</span></span>
* <span data-ttu-id="c484a-111">Strumento di compilazione [Maven] di Apache (versione 3 o successiva).</span><span class="sxs-lookup"><span data-stu-id="c484a-111">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="c484a-112">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="c484a-112">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="c484a-113">Esempio MicroProfile Hello Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-113">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="c484a-114">In questo articolo si userà l'esempio [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure):</span><span class="sxs-lookup"><span data-stu-id="c484a-114">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="c484a-115">Clonare, compilare ed eseguire localmente</span><span class="sxs-lookup"><span data-stu-id="c484a-115">Clone, build, and run locally</span></span>

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

<span data-ttu-id="c484a-116">È possibile testare l'applicazione chiamando `curl` o accedendovi tramite un [browser](http://localhost:8080/api/hello):</span><span class="sxs-lookup"><span data-stu-id="c484a-116">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="c484a-117">Distribuisci in Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-117">Deploy to Azure</span></span>

<span data-ttu-id="c484a-118">A questo punto è possibile trasferire l'applicazione nel cloud usando i servizi [Istanze di contenitore di Azure] e [Registro contenitori di Azure].</span><span class="sxs-lookup"><span data-stu-id="c484a-118">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="c484a-119">Creare un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="c484a-119">Build a Docker image</span></span>

<span data-ttu-id="c484a-120">Il progetto di esempio fornisce già un documento Dockerfile che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="c484a-120">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="c484a-121">Non è tuttavia necessario installare Docker perché si userà la funzionalità di compilazione del Registro contenitori di Azure per creare l'immagine nel cloud.</span><span class="sxs-lookup"><span data-stu-id="c484a-121">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="c484a-122">Per compilare l'immagine e predisporre l'esecuzione in Azure, è necessario seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="c484a-122">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="c484a-123">Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso con l'interfaccia stessa</span><span class="sxs-lookup"><span data-stu-id="c484a-123">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="c484a-124">Creare un gruppo di risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-124">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="c484a-125">Creare un'istanza di Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-125">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="c484a-126">Compilare l'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="c484a-126">Build the Docker image</span></span>
1. <span data-ttu-id="c484a-127">Pubblicare l'immagine Docker nell'istanza di Registro contenitori di Azure creata in precedenza</span><span class="sxs-lookup"><span data-stu-id="c484a-127">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="c484a-128">(Facoltativo) Eseguire la compilazione e la pubblicazione nel Registro contenitori di Azure con un solo comando</span><span class="sxs-lookup"><span data-stu-id="c484a-128">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="c484a-129">Configurare l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-129">Set up Azure CLI</span></span>

<span data-ttu-id="c484a-130">Assicurarsi di avere una sottoscrizione di Azure, di aver installato l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e di aver eseguito l'autenticazione all'account:</span><span class="sxs-lookup"><span data-stu-id="c484a-130">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="c484a-131">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="c484a-131">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="c484a-132">Creare un'istanza di Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-132">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="c484a-133">Questo comando creerà un registro contenitori auspicabilmente univoco a livello globale con un nome di base e un numero casuale.</span><span class="sxs-lookup"><span data-stu-id="c484a-133">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="c484a-134">Compilare l'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="c484a-134">Build the Docker image</span></span>

<span data-ttu-id="c484a-135">Anche se si può facilmente creare l'immagine Docker in locale usando Docker stesso, può essere opportuno creare l'immagine nel cloud per alcuni motivi:</span><span class="sxs-lookup"><span data-stu-id="c484a-135">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="c484a-136">Non è necessario installare Docker in locale</span><span class="sxs-lookup"><span data-stu-id="c484a-136">No need to install Docker locally</span></span>
1. <span data-ttu-id="c484a-137">L'operazione è molto più veloce, fatta eccezione per il tempo di caricamento del contesto, perché la creazione avverrà altrove</span><span class="sxs-lookup"><span data-stu-id="c484a-137">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="c484a-138">L'elaborazione nel cloud ha accesso a una velocità Internet maggiore, quindi offre download più veloci</span><span class="sxs-lookup"><span data-stu-id="c484a-138">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="c484a-139">L'immagine viene inserita direttamente nel Registro contenitori</span><span class="sxs-lookup"><span data-stu-id="c484a-139">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="c484a-140">Per queste ragioni, l'immagine verrà creata con la funzionalità di [compilazione del Registro contenitori di Azure]:</span><span class="sxs-lookup"><span data-stu-id="c484a-140">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="c484a-141">Distribuire l'immagine Docker dal Registro contenitori di Azure a Istanze di contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-141">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="c484a-142">Ora che l'immagine è disponibile nel Registro contenitori di Azure, eseguire il push di un'istanza di contenitore in Istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="c484a-142">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="c484a-143">Prima di tutto occorre tuttavia verificare che sia possibile eseguire l'autenticazione nel Registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="c484a-143">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="c484a-144">Testare l'applicazione MicroProfile distribuita</span><span class="sxs-lookup"><span data-stu-id="c484a-144">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="c484a-145">L'applicazione dovrebbe essere ora operativa.</span><span class="sxs-lookup"><span data-stu-id="c484a-145">Your application should now be up and running.</span></span> <span data-ttu-id="c484a-146">Per testarla dalla riga di comando, provare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c484a-146">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="c484a-147">Congratulazioni!</span><span class="sxs-lookup"><span data-stu-id="c484a-147">Congratulations!</span></span> <span data-ttu-id="c484a-148">È stata compilata e distribuita un'applicazione MicroProfile come contenitore Docker in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c484a-148">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c484a-149">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c484a-149">Next steps</span></span>

<span data-ttu-id="c484a-150">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="c484a-150">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="c484a-151">Accedere ad Azure dall'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="c484a-151">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Compilazione del Registro contenitori di Azure]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[Azure Container Registry Build]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
