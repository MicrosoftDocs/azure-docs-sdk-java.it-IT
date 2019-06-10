---
title: Distribuire un'app Spring Boot in Kubernetes nel servizio Azure Kubernetes
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot in un cluster Kubernetes in Microsoft Azure.
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 9ab781d27e8968ab867efc65f3ac422ac6253a6a
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270852"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a><span data-ttu-id="89672-103">Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="89672-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Kubernetes Service</span></span>

<span data-ttu-id="89672-104">**[Kubernetes]** e **[Docker]** sono soluzioni open source che consentono agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni eseguite in contenitori.</span><span class="sxs-lookup"><span data-stu-id="89672-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications running in containers.</span></span>

<span data-ttu-id="89672-105">Questa esercitazione illustra come combinare queste due diffuse tecnologie open source per sviluppare e distribuire un'applicazione Spring Boot in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="89672-105">This tutorial walks you through combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="89672-106">In particolare, si userà *[Spring Boot]* per lo sviluppo dell'applicazione, *[Kubernetes]* per la distribuzione del contenitore e il [servizio Azure Kubernetes] per l'hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="89672-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Kubernetes Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="89672-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="89672-107">Prerequisites</span></span>

* <span data-ttu-id="89672-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="89672-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="89672-109">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="89672-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="89672-110">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="89672-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="89672-111">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="89672-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="89672-112">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="89672-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="89672-113">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="89672-113">A [Git] client.</span></span>
* <span data-ttu-id="89672-114">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="89672-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="89672-115">A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.</span><span class="sxs-lookup"><span data-stu-id="89672-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="89672-116">Creare l'app Web introduttiva di Spring Boot in Docker</span><span class="sxs-lookup"><span data-stu-id="89672-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="89672-117">La procedura seguente illustra come creare un'applicazione Web di Spring Boot e come testarla in locale.</span><span class="sxs-lookup"><span data-stu-id="89672-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="89672-118">Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="89672-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="89672-119">-- o --</span><span class="sxs-lookup"><span data-stu-id="89672-119">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="89672-120">Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory.</span><span class="sxs-lookup"><span data-stu-id="89672-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="89672-121">Passare alla directory del progetto completato.</span><span class="sxs-lookup"><span data-stu-id="89672-121">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="89672-122">Usare Maven per compilare ed eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="89672-122">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="89672-123">Testare l'app Web passando a `http://localhost:8080` oppure con il comando `curl` seguente:</span><span class="sxs-lookup"><span data-stu-id="89672-123">Test the web app by browsing to `http://localhost:8080`, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="89672-124">Dovrebbe essere visualizzato il messaggio **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="89672-124">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="89672-126">Creare un'istanza di Registro Azure Container usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="89672-126">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="89672-127">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="89672-127">Open a command prompt.</span></span>

1. <span data-ttu-id="89672-128">Accedere all'account di Azure:</span><span class="sxs-lookup"><span data-stu-id="89672-128">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="89672-129">Scegliere la sottoscrizione di Azure:</span><span class="sxs-lookup"><span data-stu-id="89672-129">Choose your Azure Subscription:</span></span>
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. <span data-ttu-id="89672-130">Creare un gruppo di risorse per le risorse di Azure usate in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="89672-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="89672-131">Creare un registro contenitori privato di Azure nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="89672-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="89672-132">L'esercitazione effettua il push dell'app di esempio come immagine Docker in questo registro nei passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="89672-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="89672-133">Sostituire `wingtiptoysregistry` con un nome univoco per il registro.</span><span class="sxs-lookup"><span data-stu-id="89672-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a><span data-ttu-id="89672-134">Eseguire il push dell'app nel registro contenitori tramite Jib</span><span class="sxs-lookup"><span data-stu-id="89672-134">Push your app to the container registry via Jib</span></span>

1. <span data-ttu-id="89672-135">Accedere a Registro Azure Container dall'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="89672-135">Log in to your Azure Container Registry from the Azure CLI.</span></span>
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. <span data-ttu-id="89672-136">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o " */users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="89672-136">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="89672-137">Aggiornare la raccolta `<properties>` nel file *pom.xml* con il nome registro dell'istanza di Registro Azure Container e la versione più recente di [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).</span><span class="sxs-lookup"><span data-stu-id="89672-137">Update the `<properties>` collection in the *pom.xml* file with the registry name for your Azure Container Registry and the latest version of [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="89672-138">Aggiornare la raccolta `<plugins>` nel file *pom.xml* in modo che `<plugin>` contenga `jib-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="89672-138">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the `jib-maven-plugin`.</span></span>

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="89672-139">Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire questo comando per creare l'immagine ed eseguire il push dell'immagine nel registro:</span><span class="sxs-lookup"><span data-stu-id="89672-139">Navigate to the completed project directory for your Spring Boot application and run the following command to build the image and push the image to the registry:</span></span>

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> <span data-ttu-id="89672-140">Per motivi di sicurezza dell'interfaccia della riga di comando di Azure e Registro Azure Container, la credenziale creata da `az acr login` è valida per un'ora. Se viene visualizzato l'errore *401 Non autorizzato*, è possibile eseguire di nuovo il comando `az acr login -n <your registry name>` per ripetere l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="89672-140">Due to the security concern of Azure Cli and Azure Container Registry, the credential created by `az acr login` is valid for 1 hour, if you meet *401 Unauthorized* error, you can run the `az acr login -n <your registry name>` command again to reauthenticate.</span></span>
>

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="89672-141">Creare un cluster Kubernetes nel servizio Azure Container usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="89672-141">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="89672-142">Creare un cluster Kubernetes nel servizio Azure Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="89672-142">Create a Kubernetes cluster in Azure Kubernetes Service.</span></span> <span data-ttu-id="89672-143">Il comando seguente crea un cluster *kubernetes* nel gruppo di risorse *wingtiptoys-kubernetes*, con *wingtiptoys-akscluster* come nome del cluster e *wingtiptoys-kubernetes* come prefisso DNS:</span><span class="sxs-lookup"><span data-stu-id="89672-143">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-akscluster* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   <span data-ttu-id="89672-144">Il completamento di questo comando può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="89672-144">This command may take a while to complete.</span></span>

1. <span data-ttu-id="89672-145">Quando si usa Registro Azure Container con il servizio Azure Kubernetes è necessario concedere al servizio Azure Kubernetes l'accesso in pull a Registro Azure Container.</span><span class="sxs-lookup"><span data-stu-id="89672-145">When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), you need to grant Azure Kubernetes Service pull access to Azure Container Registry.</span></span> <span data-ttu-id="89672-146">Azure crea un'entità servizio predefinita quando si crea il servizio Azure Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="89672-146">Azure creates a default service principal when you are creating Azure Kubernetes Service.</span></span> <span data-ttu-id="89672-147">Eseguire questi script in bash o PowerShell per concedere al servizio Azure Kubernetes l'accesso a Registro Azure Container. Per altre informazioni, vedere [Eseguire l'autenticazione con Registro Azure Container dal servizio Azure Kubernetes].</span><span class="sxs-lookup"><span data-stu-id="89672-147">Please run the following scripts in bash or Powershell to grant AKS access to ACR, see more details at [Authenticate with Azure Container Registry from Azure Kubernetes Service].</span></span>

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  <span data-ttu-id="89672-148">-- o --</span><span class="sxs-lookup"><span data-stu-id="89672-148">-- or --</span></span>

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. <span data-ttu-id="89672-149">Installare `kubectl` usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="89672-149">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="89672-150">È possibile che gli utenti Linux debbano aggiungere al comando il prefisso `sudo`, perché distribuisce l'interfaccia della riga di comando di Kubernetes in `/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="89672-150">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az aks install-cli
   ```

1. <span data-ttu-id="89672-151">Scaricare le informazioni sulla configurazione del cluster, in modo da consentire la gestione del cluster dall'interfaccia Web di Kubernetes e `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="89672-151">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="89672-152">Distribuire l'immagine nel cluster Kubernetes</span><span class="sxs-lookup"><span data-stu-id="89672-152">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="89672-153">Questa esercitazione distribuisce l'app usando `kubectl`, quindi consente di esplorare la distribuzione tramite l'interfaccia Web di Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="89672-153">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-kubectl"></a><span data-ttu-id="89672-154">Eseguire la distribuzione con kubectl</span><span class="sxs-lookup"><span data-stu-id="89672-154">Deploy with kubectl</span></span>

1. <span data-ttu-id="89672-155">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="89672-155">Open a command prompt.</span></span>

1. <span data-ttu-id="89672-156">Eseguire il contenitore nel cluster Kubernetes usando il comando `kubectl run`.</span><span class="sxs-lookup"><span data-stu-id="89672-156">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="89672-157">Specificare un nome di servizio per l'app in Kubernetes e il nome completo dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="89672-157">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="89672-158">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="89672-158">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="89672-159">In questo comando:</span><span class="sxs-lookup"><span data-stu-id="89672-159">In this command:</span></span>

   * <span data-ttu-id="89672-160">Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `run`.</span><span class="sxs-lookup"><span data-stu-id="89672-160">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="89672-161">Il parametro `--image` specifica il nome combinato del server di accesso e dell'immagine come `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`.</span><span class="sxs-lookup"><span data-stu-id="89672-161">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="89672-162">Esporre esternamente il cluster Kubernetes usando il comando `kubectl expose`.</span><span class="sxs-lookup"><span data-stu-id="89672-162">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="89672-163">Specificare il nome del servizio, la porta TCP pubblica usata per accedere all'app e la porta di destinazione interna su cui è in ascolto l'app.</span><span class="sxs-lookup"><span data-stu-id="89672-163">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="89672-164">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="89672-164">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="89672-165">In questo comando:</span><span class="sxs-lookup"><span data-stu-id="89672-165">In this command:</span></span>

   * <span data-ttu-id="89672-166">Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `expose deployment`.</span><span class="sxs-lookup"><span data-stu-id="89672-166">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="89672-167">Il parametro `--type` specifica che il cluster usa il bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="89672-167">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="89672-168">Il parametro `--port` specifica la porta TCP pubblica, ovvero 80.</span><span class="sxs-lookup"><span data-stu-id="89672-168">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="89672-169">Si accede all'app tramite questa porta.</span><span class="sxs-lookup"><span data-stu-id="89672-169">You access the app on this port.</span></span>

   * <span data-ttu-id="89672-170">Il parametro `--target-port` specifica la porta TCP interna, ovvero 8080.</span><span class="sxs-lookup"><span data-stu-id="89672-170">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="89672-171">Il servizio di bilanciamento del carico inoltra le richieste all'app su questa porta.</span><span class="sxs-lookup"><span data-stu-id="89672-171">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="89672-172">Dopo la distribuzione dell'app nel cluster, eseguire query sull'indirizzo IP esterno e aprirlo nel Web browser:</span><span class="sxs-lookup"><span data-stu-id="89672-172">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![Esplorare l'app di esempio in Azure][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="89672-174">Eseguire la distribuzione con l'interfaccia Web di Kubernetes</span><span class="sxs-lookup"><span data-stu-id="89672-174">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="89672-175">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="89672-175">Open a command prompt.</span></span>

1. <span data-ttu-id="89672-176">Aprire il sito Web di configurazione per il cluster Kubernetes nel browser predefinito:</span><span class="sxs-lookup"><span data-stu-id="89672-176">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. <span data-ttu-id="89672-177">All'apertura del sito Web di configurazione di Kubernetes nel browser, fare clic sul collegamento **deploy a containerized app** (Distribuire un'app inclusa in contenitori):</span><span class="sxs-lookup"><span data-stu-id="89672-177">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Sito Web di configurazione di Kubernetes][KB01]

1. <span data-ttu-id="89672-179">Quando viene visualizzata la pagina **Resource Creation** (Creazione risorse), specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="89672-179">When the **Resource Creation** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="89672-180">a.</span><span class="sxs-lookup"><span data-stu-id="89672-180">a.</span></span> <span data-ttu-id="89672-181">Selezionare **CREATE AN APP** (CREA APP).</span><span class="sxs-lookup"><span data-stu-id="89672-181">Select **CREATE AN APP**.</span></span>

   <span data-ttu-id="89672-182">b.</span><span class="sxs-lookup"><span data-stu-id="89672-182">b.</span></span> <span data-ttu-id="89672-183">Immettere il nome dell'applicazione Spring Boot per **App name** (Nome app), ad esempio: "*gs-spring-boot-docker*".</span><span class="sxs-lookup"><span data-stu-id="89672-183">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="89672-184">c.</span><span class="sxs-lookup"><span data-stu-id="89672-184">c.</span></span> <span data-ttu-id="89672-185">Immettere il server di accesso e l'immagine del contenitore dai passaggi precedenti in **Container image** (Immagine del contenitore), ad esempio: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span><span class="sxs-lookup"><span data-stu-id="89672-185">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="89672-186">d.</span><span class="sxs-lookup"><span data-stu-id="89672-186">d.</span></span> <span data-ttu-id="89672-187">Scegliere **External** (Esterno) per **Service** (Servizio).</span><span class="sxs-lookup"><span data-stu-id="89672-187">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="89672-188">e.</span><span class="sxs-lookup"><span data-stu-id="89672-188">e.</span></span> <span data-ttu-id="89672-189">Specificare le porte esterne ed interne nelle caselle di testo **Port** (Porta) e **Target port** (Porta di destinazione).</span><span class="sxs-lookup"><span data-stu-id="89672-189">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Sito Web di configurazione di Kubernetes][KB02]


1. <span data-ttu-id="89672-191">Fare clic su **Deploy** (Distribuisci) per distribuire il contenitore.</span><span class="sxs-lookup"><span data-stu-id="89672-191">Click **Deploy** to deploy the container.</span></span>

   ![Distribuzione di Kubernetes][KB05]

1. <span data-ttu-id="89672-193">Al termine della distribuzione dell'applicazione, l'applicazione Spring Boot verrà elencata in **Services** (Servizi).</span><span class="sxs-lookup"><span data-stu-id="89672-193">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Servizi Kubernetes][KB06]

1. <span data-ttu-id="89672-195">Se si fa clic sul collegamento per **External endpoints** (Endpoint esterni), è possibile visualizzare l'applicazione Spring Boot in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="89672-195">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Servizi Kubernetes][KB07]

   ![Esplorare l'app di esempio in Azure][SB02]

## <a name="next-steps"></a><span data-ttu-id="89672-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="89672-198">Next steps</span></span>

<span data-ttu-id="89672-199">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="89672-199">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="89672-200">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="89672-200">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="89672-201">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="89672-201">Additional Resources</span></span>

<span data-ttu-id="89672-202">Per altre informazioni sull'uso di Spring Boot in Azure, vedere l'articolo seguente:</span><span class="sxs-lookup"><span data-stu-id="89672-202">For more information about using Spring Boot on Azure, see the following article:</span></span>

* [<span data-ttu-id="89672-203">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="89672-203">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

<span data-ttu-id="89672-204">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="89672-204">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="89672-205">Per altre informazioni sulla distribuzione di un'applicazione Java in Kubernetes con Visual Studio Code, vedere le [esercitazioni su Java in Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="89672-205">For more information about deploying a Java application to Kubernetes with Visual Studio Code, see [Visual Studio Code Java Tutorials].</span></span>

<span data-ttu-id="89672-206">Per altre informazioni sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).</span><span class="sxs-lookup"><span data-stu-id="89672-206">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="89672-207">I collegamenti seguenti forniscono informazioni aggiuntive sulla creazione di applicazioni Spring Boot:</span><span class="sxs-lookup"><span data-stu-id="89672-207">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="89672-208">Per altre informazioni sulla creazione di una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="89672-208">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="89672-209">I collegamenti seguenti forniscono informazioni aggiuntive sull'uso di Kubernetes con Azure:</span><span class="sxs-lookup"><span data-stu-id="89672-209">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="89672-210">Introduzione ai cluster Kubernetes nel servizio Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="89672-210">Get started with a Kubernetes cluster in Azure Kubernetes Service</span></span>](/azure/aks/intro-kubernetes)

<span data-ttu-id="89672-211">Per altre informazioni sull'uso dell'interfaccia della riga di comando di Kubernetes, vedere la guida dell'utente di **kubectl** all'indirizzo <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="89672-211">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="89672-212">Il sito Web Kubernetes include alcuni articoli relativi all'uso delle immagini nei registri privati:</span><span class="sxs-lookup"><span data-stu-id="89672-212">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="89672-213">[Configuring Service Accounts for Pods] (Configurazione degli account del servizio per i pod)</span><span class="sxs-lookup"><span data-stu-id="89672-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="89672-214">[Namespaces] (Spazi dei nomi)</span><span class="sxs-lookup"><span data-stu-id="89672-214">[Namespaces]</span></span>
* <span data-ttu-id="89672-215">[Pulling an Image from a Private Registry] (Effettuare il pull di un'immagine da un registro privato)</span><span class="sxs-lookup"><span data-stu-id="89672-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="89672-216">Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].</span><span class="sxs-lookup"><span data-stu-id="89672-216">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<span data-ttu-id="89672-217">Per altre informazioni sull'esecuzione iterativa e il debug di contenitori direttamente nel servizio Azure Kubernetes con Azure Dev Spaces, vedere [Guida introduttiva ad Azure Dev Spaces con Java]</span><span class="sxs-lookup"><span data-stu-id="89672-217">For more information about iteratively running and debugging containers directly in Azure Kubernetes Service (AKS) with Azure Dev Spaces, see [Get started on Azure Dev Spaces with Java]</span></span>

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Servizio Azure Kubernetes]: https://azure.microsoft.com/services/kubernetes-service/
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[Azure per sviluppatori Java]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Uso di Azure DevOps e Java]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/
[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/ (Configurazione degli account del servizio per i pod)
[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/ (Spazi dei nomi)
[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ (Effettuare il pull di un'immagine da un registro privato)

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[Eseguire l'autenticazione con Registro Azure Container dal servizio Azure Kubernetes]: /azure/container-registry/container-registry-auth-aks/
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: /azure/container-registry/container-registry-auth-aks/
[Esercitazioni su Java in Visual Studio Code]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Visual Studio Code Java Tutorials]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Guida introduttiva ad Azure Dev Spaces con Java]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
[Get started on Azure Dev Spaces with Java]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
