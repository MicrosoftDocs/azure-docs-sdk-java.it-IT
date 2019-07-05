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
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a><span data-ttu-id="e4a87-103">Distribuire un'app MicroProfile nel cloud con Docker e Azure</span><span class="sxs-lookup"><span data-stu-id="e4a87-103">Deploy a MicroProfile app to the cloud by using Docker and Azure</span></span>

<span data-ttu-id="e4a87-104">Questo articolo mostra come creare un pacchetto dell'applicazione [MicroProfile.io] in un contenitore Docker ed eseguire l'applicazione in Istanze di Azure Container.</span><span class="sxs-lookup"><span data-stu-id="e4a87-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
> <span data-ttu-id="e4a87-105">Questa procedura funziona con qualsiasi implementazione di MicroProfile.io purché l'immagine del contenitore Docker sia autoeseguibile, ovvero includa il runtime.</span><span class="sxs-lookup"><span data-stu-id="e4a87-105">This procedure works with any implementation of MicroProfile.io, as long as the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4a87-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e4a87-106">Prerequisites</span></span>

<span data-ttu-id="e4a87-107">Per completare questa esercitazione è necessario soddisfare i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="e4a87-107">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="e4a87-108">Una sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="e4a87-108">An Azure subscription.</span></span> <span data-ttu-id="e4a87-109">Se non si ha ancora una sottoscrizione di Azure, è possibile iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="e4a87-109">If you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="e4a87-110">[Interfaccia della riga di comando di Azure] installata.</span><span class="sxs-lookup"><span data-stu-id="e4a87-110">The [Azure CLI], installed.</span></span>
* <span data-ttu-id="e4a87-111">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="e4a87-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="e4a87-112">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere [Supporto a lungo termine di Java per Azure e Azure Stack](https://aka.ms/azure-jdks).</span><span class="sxs-lookup"><span data-stu-id="e4a87-112">For more information about the JDKs that are available to use when you develop on Azure, see [Java long-term support for Azure and Azure Stack](https://aka.ms/azure-jdks).</span></span>
* <span data-ttu-id="e4a87-113">Strumento di compilazione di [Apache Maven] (versione 3 o successiva).</span><span class="sxs-lookup"><span data-stu-id="e4a87-113">The [Apache Maven] build tool (version 3 or later).</span></span>
* <span data-ttu-id="e4a87-114">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="e4a87-114">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="e4a87-115">Esempio MicroProfile Hello Azure</span><span class="sxs-lookup"><span data-stu-id="e4a87-115">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="e4a87-116">In questo articolo si usa l'esempio [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure).</span><span class="sxs-lookup"><span data-stu-id="e4a87-116">In this article, you use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample.</span></span> <span data-ttu-id="e4a87-117">Clonarlo, compilarlo ed eseguirlo in locale usando i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e4a87-117">Clone, build, and run it locally by using the following commands:</span></span>

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

<span data-ttu-id="e4a87-118">È possibile testare l'applicazione chiamando `curl` o usando un [browser](http://localhost:8080/api/hello):</span><span class="sxs-lookup"><span data-stu-id="e4a87-118">You can test the application by calling `curl` or by using a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="e4a87-119">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="e4a87-119">Deploy the app to Azure</span></span>

<span data-ttu-id="e4a87-120">A questo punto trasferire l'applicazione in Azure usando i servizi [Istanze di Azure Container] e [Registro Azure Container].</span><span class="sxs-lookup"><span data-stu-id="e4a87-120">Now bring this application to Azure by using the [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="e4a87-121">Creare un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="e4a87-121">Build a Docker image</span></span>

<span data-ttu-id="e4a87-122">Il progetto di esempio include un documento Dockerfile che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="e4a87-122">The sample project provides a Dockerfile that you can use.</span></span> <span data-ttu-id="e4a87-123">Non è però necessario che Docker sia installato perché si userà la funzionalità di compilazione di Registro Azure Container per compilare l'immagine nel cloud.</span><span class="sxs-lookup"><span data-stu-id="e4a87-123">You don't need to have Docker installed, though, because you'll use the Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="e4a87-124">Per compilare l'immagine e prepararla per l'esecuzione in Azure, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="e4a87-124">To build the image and prepare to run it on Azure, do the following:</span></span>

1. <span data-ttu-id="e4a87-125">Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4a87-125">Install and sign in to the Azure CLI.</span></span>
1. <span data-ttu-id="e4a87-126">Creare un gruppo di risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="e4a87-126">Create an Azure resource group.</span></span>
1. <span data-ttu-id="e4a87-127">Creare un'istanza di Registro Azure Container.</span><span class="sxs-lookup"><span data-stu-id="e4a87-127">Create an Azure container registry instance.</span></span>
1. <span data-ttu-id="e4a87-128">Compilare un'immagine Docker.</span><span class="sxs-lookup"><span data-stu-id="e4a87-128">Build a Docker image.</span></span>
1. <span data-ttu-id="e4a87-129">Pubblicare l'immagine Docker nell'istanza di Registro Azure Container creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e4a87-129">Publish the Docker image to the previously created container registry instance.</span></span>
1. <span data-ttu-id="e4a87-130">(Facoltativo) Compilare e pubblicare l'immagine nell'istanza di Registro Container con un unico comando.</span><span class="sxs-lookup"><span data-stu-id="e4a87-130">(Optional) Build and publish the image to the container registry instance in one command.</span></span>


#### <a name="set-up-the-azure-cli"></a><span data-ttu-id="e4a87-131">Configurare l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="e4a87-131">Set up the Azure CLI</span></span>

<span data-ttu-id="e4a87-132">Assicurarsi di avere una sottoscrizione di Azure, di aver installato l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e di aver eseguito l'autenticazione all'account:</span><span class="sxs-lookup"><span data-stu-id="e4a87-132">Make sure that you have an Azure subscription, that you've installed [the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you're authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="e4a87-133">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="e4a87-133">Create a resource group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-a-container-registry-instance"></a><span data-ttu-id="e4a87-134">Creare un'istanza di Registro Container</span><span class="sxs-lookup"><span data-stu-id="e4a87-134">Create a container registry instance</span></span>

<span data-ttu-id="e4a87-135">Questo comando deve creare un'istanza univoca a livello globale di Registro Container con un nome di base e un numero casuale.</span><span class="sxs-lookup"><span data-stu-id="e4a87-135">This command should create a globally unique container registry instance with a basic name and a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="e4a87-136">Compilare l'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="e4a87-136">Build the Docker image</span></span>

<span data-ttu-id="e4a87-137">Anche se è possibile compilare facilmente l'immagine Docker in locale usando Docker stesso, può essere opportuno compilarla nel cloud per alcuni motivi:</span><span class="sxs-lookup"><span data-stu-id="e4a87-137">Although you can easily build the Docker image locally by using Docker itself, you might consider building it in the cloud for few reasons:</span></span>

* <span data-ttu-id="e4a87-138">Non è necessario installare Docker in locale.</span><span class="sxs-lookup"><span data-stu-id="e4a87-138">You don't have to install Docker locally.</span></span>
* <span data-ttu-id="e4a87-139">L'operazione è molto più veloce, fatta eccezione per il tempo di caricamento del contesto, perché la compilazione avverrà altrove.</span><span class="sxs-lookup"><span data-stu-id="e4a87-139">It's much faster, because the build happens elsewhere (except for context upload time).</span></span>
* <span data-ttu-id="e4a87-140">L'elaborazione nel cloud può sfruttare una velocità Internet maggiore, quindi offre download più veloci.</span><span class="sxs-lookup"><span data-stu-id="e4a87-140">The process in the cloud has faster access to the internet and, therefore, faster downloads.</span></span>
* <span data-ttu-id="e4a87-141">L'immagine viene inserita direttamente nell'istanza di Registro Container.</span><span class="sxs-lookup"><span data-stu-id="e4a87-141">The image goes directly into the container registry instance.</span></span>

<span data-ttu-id="e4a87-142">Per questi motivi, l'immagine viene compilata con la funzionalità di [compilazione di Registro Azure Container]:</span><span class="sxs-lookup"><span data-stu-id="e4a87-142">For these reasons, you build the image by using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a><span data-ttu-id="e4a87-143">Distribuire l'immagine Docker dall'istanza di Registro Azure Container a Istanze di Azure Container</span><span class="sxs-lookup"><span data-stu-id="e4a87-143">Deploy the Docker image from the Azure container registry instance to Container Instances</span></span>

<span data-ttu-id="e4a87-144">Ora che l'immagine è disponibile nell'istanza di Registro Azure Container, eseguirne il push e crearne un'istanza in Istanze di Azure Container.</span><span class="sxs-lookup"><span data-stu-id="e4a87-144">Now that the image is available on your container registry instance, push and instantiate a container instance on Container Instances.</span></span> <span data-ttu-id="e4a87-145">Assicurarsi prima che sia possibile eseguire l'autenticazione in Registro Azure Container:</span><span class="sxs-lookup"><span data-stu-id="e4a87-145">But first, make sure that you can authenticate into the container registry instance:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="e4a87-146">Testare l'applicazione MicroProfile distribuita</span><span class="sxs-lookup"><span data-stu-id="e4a87-146">Test your deployed MicroProfile application</span></span>

<span data-ttu-id="e4a87-147">L'applicazione dovrebbe essere ora operativa.</span><span class="sxs-lookup"><span data-stu-id="e4a87-147">Your application should now be up and running.</span></span> <span data-ttu-id="e4a87-148">Per testarla dall'interfaccia della riga di comando, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e4a87-148">To test it from the command-line interface, use the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="e4a87-149">Congratulazioni!</span><span class="sxs-lookup"><span data-stu-id="e4a87-149">Congratulations!</span></span> <span data-ttu-id="e4a87-150">Un'applicazione MicroProfile è stata compilata come contenitore Docker e distribuita in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e4a87-150">You've successfully built a MicroProfile application as a Docker container and deployed it to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4a87-151">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e4a87-151">Next steps</span></span>

<span data-ttu-id="e4a87-152">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere:</span><span class="sxs-lookup"><span data-stu-id="e4a87-152">For more information about the various technologies discussed in this article, see:</span></span>

* [<span data-ttu-id="e4a87-153">Accedere ad Azure dall'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="e4a87-153">Sign in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Compilazione di Registro Azure Container]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure CLI]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Istanze di Azure Container]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Registro Azure Container]:  https://docs.microsoft.com/azure/container-registry
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry
