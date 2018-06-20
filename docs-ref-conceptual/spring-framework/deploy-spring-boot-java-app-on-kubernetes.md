---
title: Distribuire un'app Spring Boot su Kubernetes nel servizio contenitore di Azure
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot in un cluster Kubernetes in Microsoft Azure.
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 9eb37f302835ea40e92b5212d5bbc305d1311bc4
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954642"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a><span data-ttu-id="95a39-103">Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="95a39-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>

<span data-ttu-id="95a39-104">**[Kubernetes]** e **[Docker]** sono soluzioni open source che consentono agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni in esecuzione nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="95a39-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="95a39-105">Questa esercitazione illustra in modo dettagliato la combinazione di queste due tecnologie open source note per sviluppare e distribuire un'applicazione Spring Boot in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-105">This tutorial walks you though combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="95a39-106">In particolare, è possibile usare *[Spring Boot]* per lo sviluppo dell'applicazione, *[Kubernetes]* per la distribuzione dei contenitori e il [servizio contenitore di Azure] per l'hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="95a39-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Container Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="95a39-107">prerequisiti</span><span class="sxs-lookup"><span data-stu-id="95a39-107">Prerequisites</span></span>

* <span data-ttu-id="95a39-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="95a39-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="95a39-109">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="95a39-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="95a39-110">Un [Java Developer Kit (JDK)] aggiornato.</span><span class="sxs-lookup"><span data-stu-id="95a39-110">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="95a39-111">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="95a39-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="95a39-112">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="95a39-112">A [Git] client.</span></span>
* <span data-ttu-id="95a39-113">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="95a39-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="95a39-114">A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95a39-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="95a39-115">Creare l'app Web introduttiva di Spring Boot in Docker</span><span class="sxs-lookup"><span data-stu-id="95a39-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="95a39-116">La procedura seguente illustra come creare un'applicazione Web di Spring Boot e come testarla in locale.</span><span class="sxs-lookup"><span data-stu-id="95a39-116">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="95a39-117">Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95a39-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="95a39-118">-- o --</span><span class="sxs-lookup"><span data-stu-id="95a39-118">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="95a39-119">Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory.</span><span class="sxs-lookup"><span data-stu-id="95a39-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="95a39-120">Passare alla directory del progetto completato.</span><span class="sxs-lookup"><span data-stu-id="95a39-120">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="95a39-121">Usare Maven per compilare ed eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="95a39-121">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="95a39-122">Testare l'app Web passando a http://localhost:8080 oppure eseguendo il comando `curl` seguente:</span><span class="sxs-lookup"><span data-stu-id="95a39-122">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="95a39-123">Dovrebbe essere visualizzato il messaggio seguente: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="95a39-123">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="95a39-125">Creare un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="95a39-125">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="95a39-126">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="95a39-126">Open a command prompt.</span></span>

1. <span data-ttu-id="95a39-127">Accedere all'account di Azure:</span><span class="sxs-lookup"><span data-stu-id="95a39-127">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="95a39-128">Creare un gruppo di risorse per le risorse di Azure usate in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="95a39-128">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="95a39-129">Creare un registro contenitori privato di Azure nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="95a39-129">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="95a39-130">L'esercitazione effettua il push dell'app di esempio come immagine Docker in questo registro nei passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="95a39-130">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="95a39-131">Sostituire `wingtiptoysregistry` con un nome univoco per il registro.</span><span class="sxs-lookup"><span data-stu-id="95a39-131">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="95a39-132">Effettuare il push dell'app nel registro contenitori</span><span class="sxs-lookup"><span data-stu-id="95a39-132">Push your app to the container registry</span></span>

1. <span data-ttu-id="95a39-133">Passare alla directory di configurazione dell'installazione di Maven (impostazione predefinita: ~/.m2/ or C:\Users\nomeutente\.m2) e aprire il file *settings.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="95a39-133">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="95a39-134">Recuperare la password per il registro contenitori dall'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-134">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="95a39-135">Aggiungere l'ID e la password del registro contenitori di Azure a una nuova raccolta `<server>` nel file *settings.xml*.</span><span class="sxs-lookup"><span data-stu-id="95a39-135">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="95a39-136">I valori `id` e `username` corrispondono al nome del registro.</span><span class="sxs-lookup"><span data-stu-id="95a39-136">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="95a39-137">Usare il valore `password` del comando precedente, senza virgolette.</span><span class="sxs-lookup"><span data-stu-id="95a39-137">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="95a39-138">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="95a39-138">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="95a39-139">Aggiornare la raccolta `<properties>` nel file *pom.xml* con il valore del server di accesso per il registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-139">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="95a39-140">Aggiornare la raccolta `<plugins>` nel file *pom.xml* in modo che `<plugin>` contenga l'indirizzo del server di accesso e il nome del registro per il registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-140">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. <span data-ttu-id="95a39-141">Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire il comando seguente per creare il contenitore Docker ed effettuare il push dell'immagine nel registro:</span><span class="sxs-lookup"><span data-stu-id="95a39-141">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="95a39-142">Quando Maven effettua il push dell'immagine in Azure, è possibile che venga visualizzato un messaggio di errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="95a39-142">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="95a39-143">Se viene visualizzato questo errore, accedere ad Azure dalla riga di comando di Docker.</span><span class="sxs-lookup"><span data-stu-id="95a39-143">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="95a39-144">Effettuare quindi il push del contenitore:</span><span class="sxs-lookup"><span data-stu-id="95a39-144">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="95a39-145">Creare un cluster Kubernetes nel servizio contenitore di Azure usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="95a39-145">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="95a39-146">Creare un cluster Kubernetes nel servizio contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-146">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="95a39-147">Il comando seguente crea un cluster *kubernetes* nel gruppo di risorse *wingtiptoys-kubernetes* con *wingtiptoys-containerservice* come nome del cluster e *wingtiptoys-kubernetes* come prefisso DNS:</span><span class="sxs-lookup"><span data-stu-id="95a39-147">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="95a39-148">Il completamento di questo comando può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="95a39-148">This command may take a while to complete.</span></span>

1. <span data-ttu-id="95a39-149">Installare `kubectl` usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-149">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="95a39-150">È possibile che gli utenti Linux debbano aggiungere al comando il prefisso `sudo`, perché distribuisce l'interfaccia della riga di comando di Kubernetes in `/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="95a39-150">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="95a39-151">Scaricare le informazioni sulla configurazione del cluster, in modo da consentire la gestione del cluster dall'interfaccia Web di Kubernetes e `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="95a39-151">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="95a39-152">Distribuire l'immagine nel cluster Kubernetes</span><span class="sxs-lookup"><span data-stu-id="95a39-152">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="95a39-153">Questa esercitazione distribuisce l'app usando `kubectl`, quindi consente di esplorare la distribuzione tramite l'interfaccia Web di Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="95a39-153">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="95a39-154">Eseguire la distribuzione con l'interfaccia Web di Kubernetes</span><span class="sxs-lookup"><span data-stu-id="95a39-154">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="95a39-155">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="95a39-155">Open a command prompt.</span></span>

1. <span data-ttu-id="95a39-156">Aprire il sito Web di configurazione per il cluster Kubernetes nel browser predefinito:</span><span class="sxs-lookup"><span data-stu-id="95a39-156">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="95a39-157">All'apertura del sito Web di configurazione di Kubernetes nel browser, fare clic sul collegamento **deploy a containerized app** (Distribuire un'app inclusa in contenitori):</span><span class="sxs-lookup"><span data-stu-id="95a39-157">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Sito Web di configurazione di Kubernetes][KB01]

1. <span data-ttu-id="95a39-159">Quando viene visualizzata la pagina **Deploy a containerized app** (Distribuire un'app inclusa in contenitori), specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="95a39-159">When the **Deploy a containerized app** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="95a39-160">a.</span><span class="sxs-lookup"><span data-stu-id="95a39-160">a.</span></span> <span data-ttu-id="95a39-161">Selezionare **Specify app details below** (Specificare più avanti i dettagli dell'app).</span><span class="sxs-lookup"><span data-stu-id="95a39-161">Select **Specify app details below**.</span></span>

   <span data-ttu-id="95a39-162">b.</span><span class="sxs-lookup"><span data-stu-id="95a39-162">b.</span></span> <span data-ttu-id="95a39-163">Immettere il nome dell'applicazione Spring Boot per **App name** (Nome app), ad esempio: "*gs-spring-boot-docker*".</span><span class="sxs-lookup"><span data-stu-id="95a39-163">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="95a39-164">c.</span><span class="sxs-lookup"><span data-stu-id="95a39-164">c.</span></span> <span data-ttu-id="95a39-165">Immettere il server di accesso e l'immagine del contenitore dai passaggi precedenti in **Container image** (Immagine del contenitore), ad esempio: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span><span class="sxs-lookup"><span data-stu-id="95a39-165">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="95a39-166">d.</span><span class="sxs-lookup"><span data-stu-id="95a39-166">d.</span></span> <span data-ttu-id="95a39-167">Scegliere **External** (Esterno) per **Service** (Servizio).</span><span class="sxs-lookup"><span data-stu-id="95a39-167">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="95a39-168">e.</span><span class="sxs-lookup"><span data-stu-id="95a39-168">e.</span></span> <span data-ttu-id="95a39-169">Specificare le porte esterne ed interne nelle caselle di testo **Port** (Porta) e **Target port** (Porta di destinazione).</span><span class="sxs-lookup"><span data-stu-id="95a39-169">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Sito Web di configurazione di Kubernetes][KB02]


1. <span data-ttu-id="95a39-171">Fare clic su **Deploy** (Distribuisci) per distribuire il contenitore.</span><span class="sxs-lookup"><span data-stu-id="95a39-171">Click **Deploy** to deploy the container.</span></span>

   ![Distribuire un contenitore][KB05]

1. <span data-ttu-id="95a39-173">Al termine della distribuzione dell'applicazione, l'applicazione Spring Boot verrà elencata in **Services** (Servizi).</span><span class="sxs-lookup"><span data-stu-id="95a39-173">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Servizi Kubernetes][KB06]

1. <span data-ttu-id="95a39-175">Se si fa clic sul collegamento per **External endpoints** (Endpoint esterni), è possibile visualizzare l'applicazione Spring Boot in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="95a39-175">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Servizi Kubernetes][KB07]

   ![Esplorare l'app di esempio in Azure][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="95a39-178">Eseguire la distribuzione con kubectl</span><span class="sxs-lookup"><span data-stu-id="95a39-178">Deploy with kubectl</span></span>

1. <span data-ttu-id="95a39-179">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="95a39-179">Open a command prompt.</span></span>

1. <span data-ttu-id="95a39-180">Eseguire il contenitore nel cluster Kubernetes usando il comando `kubectl run`.</span><span class="sxs-lookup"><span data-stu-id="95a39-180">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="95a39-181">Specificare un nome di servizio per l'app in Kubernetes e il nome completo dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="95a39-181">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="95a39-182">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="95a39-182">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="95a39-183">In questo comando:</span><span class="sxs-lookup"><span data-stu-id="95a39-183">In this command:</span></span>

   * <span data-ttu-id="95a39-184">Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `run`.</span><span class="sxs-lookup"><span data-stu-id="95a39-184">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="95a39-185">Il parametro `--image` specifica il nome combinato del server di accesso e dell'immagine come `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`.</span><span class="sxs-lookup"><span data-stu-id="95a39-185">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="95a39-186">Esporre esternamente il cluster Kubernetes usando il comando `kubectl expose`.</span><span class="sxs-lookup"><span data-stu-id="95a39-186">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="95a39-187">Specificare il nome del servizio, la porta TCP pubblica usata per accedere all'app e la porta di destinazione interna su cui è in ascolto l'app.</span><span class="sxs-lookup"><span data-stu-id="95a39-187">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="95a39-188">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="95a39-188">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="95a39-189">In questo comando:</span><span class="sxs-lookup"><span data-stu-id="95a39-189">In this command:</span></span>

   * <span data-ttu-id="95a39-190">Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `expose deployment`.</span><span class="sxs-lookup"><span data-stu-id="95a39-190">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="95a39-191">Il parametro `--type` specifica che il cluster usa il bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="95a39-191">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="95a39-192">Il parametro `--port` specifica la porta TCP pubblica, ovvero 80.</span><span class="sxs-lookup"><span data-stu-id="95a39-192">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="95a39-193">Si accede all'app tramite questa porta.</span><span class="sxs-lookup"><span data-stu-id="95a39-193">You access the app on this port.</span></span>

   * <span data-ttu-id="95a39-194">Il parametro `--target-port` specifica la porta TCP interna, ovvero 8080.</span><span class="sxs-lookup"><span data-stu-id="95a39-194">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="95a39-195">Il servizio di bilanciamento del carico inoltra le richieste all'app su questa porta.</span><span class="sxs-lookup"><span data-stu-id="95a39-195">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="95a39-196">Dopo la distribuzione dell'app nel cluster, eseguire query sull'indirizzo IP esterno e aprirlo nel Web browser:</span><span class="sxs-lookup"><span data-stu-id="95a39-196">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Esplorare l'app di esempio in Azure][SB02]


## <a name="next-steps"></a><span data-ttu-id="95a39-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="95a39-198">Next steps</span></span>

<span data-ttu-id="95a39-199">Per altre informazioni sull'uso di Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="95a39-199">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="95a39-200">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="95a39-200">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="95a39-201">Distribuire un'applicazione Spring Boot in Linux nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="95a39-201">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

<span data-ttu-id="95a39-202">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].</span><span class="sxs-lookup"><span data-stu-id="95a39-202">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="95a39-203">Per altre informazioni sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).</span><span class="sxs-lookup"><span data-stu-id="95a39-203">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="95a39-204">I collegamenti seguenti forniscono informazioni aggiuntive sulla creazione di applicazioni Spring Boot:</span><span class="sxs-lookup"><span data-stu-id="95a39-204">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="95a39-205">Per altre informazioni sulla creazione di una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="95a39-205">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="95a39-206">I collegamenti seguenti forniscono informazioni aggiuntive sull'uso di Kubernetes con Azure:</span><span class="sxs-lookup"><span data-stu-id="95a39-206">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="95a39-207">Introduzione a un cluster Kubernetes nel servizio contenitore</span><span class="sxs-lookup"><span data-stu-id="95a39-207">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="95a39-208">Uso dell'interfaccia utente Web Kubernetes con il servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="95a39-208">Using the Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="95a39-209">Per altre informazioni sull'uso dell'interfaccia della riga di comando di Kubernetes, vedere la Guida dell'utente di **kubectl** all'indirizzo <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="95a39-209">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="95a39-210">Il sito Web Kubernetes include alcuni articoli relativi all'uso delle immagini nei registri privati:</span><span class="sxs-lookup"><span data-stu-id="95a39-210">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="95a39-211">[Configuring Service Accounts for Pods] (Configurazione degli account del servizio per i pod)</span><span class="sxs-lookup"><span data-stu-id="95a39-211">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="95a39-212">[Namespaces] (Spazi dei nomi)</span><span class="sxs-lookup"><span data-stu-id="95a39-212">[Namespaces]</span></span>
* <span data-ttu-id="95a39-213">[Pulling an Image from a Private Registry] (Effettuare il pull di un'immagine da un registro privato)</span><span class="sxs-lookup"><span data-stu-id="95a39-213">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="95a39-214">Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].</span><span class="sxs-lookup"><span data-stu-id="95a39-214">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[servizio contenitore di Azure]: https://azure.microsoft.com/services/container-service/
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
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
