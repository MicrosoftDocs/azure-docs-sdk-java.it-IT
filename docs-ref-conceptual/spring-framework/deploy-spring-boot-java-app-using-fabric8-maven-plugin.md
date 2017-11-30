---
title: Distribuire un'app Spring Boot con il plug-in Maven Fabric8
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot su Microsoft Azure tramite il plug-in Fabric8 per Apache Maven.
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Maven
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: yuwzho;robmcm
ms.custom: 
ms.openlocfilehash: 6c0e0e233b44b2b54e869b443edf3a255e7fbfb4
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a><span data-ttu-id="40927-104">Distribuire un'app Spring Boot con il plug-in Maven Fabric8</span><span class="sxs-lookup"><span data-stu-id="40927-104">Deploy a Spring Boot app using the Fabric8 Maven Plugin</span></span>

<span data-ttu-id="40927-105">**[Fabric8]**  è una soluzione open source basata su  **[Kubernetes]** che consente agli sviluppatori di creare applicazioni in contenitori Linux.</span><span class="sxs-lookup"><span data-stu-id="40927-105">**[Fabric8]** is an open-source solution that is built on **[Kubernetes]**, which helps developers create applications in Linux containers.</span></span>

<span data-ttu-id="40927-106">Questa esercitazione illustra l'uso del plug-in Fabric8 per Maven per sviluppare e distribuire un'applicazione in un host Linux nel [servizio contenitore di Azure].</span><span class="sxs-lookup"><span data-stu-id="40927-106">This tutorial walks you through using the Fabric8 plugin for Maven to develop to deploy an application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40927-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="40927-107">Prerequisites</span></span>

<span data-ttu-id="40927-108">Per completare la procedura di questa esercitazione, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="40927-108">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="40927-109">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="40927-109">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="40927-110">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="40927-110">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="40927-111">Un [Java Developer Kit (JDK)] aggiornato.</span><span class="sxs-lookup"><span data-stu-id="40927-111">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="40927-112">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="40927-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="40927-113">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="40927-113">A [Git] client.</span></span>
* <span data-ttu-id="40927-114">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="40927-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="40927-115">A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.</span><span class="sxs-lookup"><span data-stu-id="40927-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="40927-116">Creare l'app Web introduttiva di Spring Boot in Docker</span><span class="sxs-lookup"><span data-stu-id="40927-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="40927-117">La procedura seguente illustra come creare un'applicazione Web di Spring Boot e come testarla in locale.</span><span class="sxs-lookup"><span data-stu-id="40927-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="40927-118">Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   <span data-ttu-id="40927-119">- o-</span><span class="sxs-lookup"><span data-stu-id="40927-119">-- or --</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. <span data-ttu-id="40927-120">Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory.</span><span class="sxs-lookup"><span data-stu-id="40927-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="40927-121">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-121">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   <span data-ttu-id="40927-122">- o-</span><span class="sxs-lookup"><span data-stu-id="40927-122">-- or --</span></span>
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. <span data-ttu-id="40927-123">Usare Maven per compilare ed eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="40927-123">Use Maven to build and run the sample app.</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

1. <span data-ttu-id="40927-124">Testare l'app Web passando a http://localhost:8080 oppure eseguendo il comando `curl` seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-124">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="40927-125">Dovrebbe essere visualizzato il messaggio **Hello Docker World**.</span><span class="sxs-lookup"><span data-stu-id="40927-125">You should see a **Hello Docker World** message displayed.</span></span>

   ![Esplorare l'applicazione di esempio localmente][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a><span data-ttu-id="40927-127">Installare l'interfaccia della riga di comando di Kubernetes e creare un gruppo di risorse di Azure usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="40927-127">Install the Kubernetes command-line interface and create an Azure resource group using the Azure CLI</span></span>

1. <span data-ttu-id="40927-128">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="40927-128">Open a command prompt.</span></span>

1. <span data-ttu-id="40927-129">Accedere al proprio account di Azure digitando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-129">Type the following command to log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="40927-130">Seguire le istruzioni per completare il processo di accesso</span><span class="sxs-lookup"><span data-stu-id="40927-130">Follow the instructions to complete the login process</span></span>
   
   <span data-ttu-id="40927-131">Nell'interfaccia della riga di comando di Azure verrà visualizzato un elenco degli account, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-131">The Azure CLI will display a list of your accounts; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="40927-132">Se non si dispone ancora dell'interfaccia della riga di comando di Kubernetes (`kubectl`) installata, è possibile installarla tramite l'interfaccia della riga di comando di Azure, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-132">If you do not already have the Kubernetes command-line interface (`kubectl`) installed, you can install using the Azure CLI; for example:</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="40927-133">È possibile che gli utenti Linux debbano aggiungere al comando il prefisso `sudo`, perché distribuisce l'interfaccia della riga di comando di Kubernetes in `/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="40927-133">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   >
   > <span data-ttu-id="40927-134">Se si dispone già di `kubectl` installato e la versione di `kubectl` è obsoleta, potrebbe essere visualizzato un messaggio di errore simile all'esempio seguente quando si tenta di completare la procedura descritta più avanti in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="40927-134">If you already have `kubectl`) installed and your version of `kubectl` is too old, you may see an error message similar to the following example when you attempt to complete the steps listed later in this article:</span></span>
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > <span data-ttu-id="40927-135">In questo caso, sarà necessario reinstallare `kubectl` per aggiornare la versione in uso.</span><span class="sxs-lookup"><span data-stu-id="40927-135">If this happens, you will need to reinstall `kubectl` to update your version.</span></span>
   >

1. <span data-ttu-id="40927-136">Creare un gruppo di risorse per le risorse di Azure che verranno usate in questa esercitazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-136">Create a resource group for the Azure resources that you will use in this tutorial; for example:</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   <span data-ttu-id="40927-137">Dove:</span><span class="sxs-lookup"><span data-stu-id="40927-137">Where:</span></span>  
      * <span data-ttu-id="40927-138">*wingtiptoys-kubernetes* è un nome univoco per il gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="40927-138">*wingtiptoys-kubernetes* is a unique name for your resource group</span></span>  
      * <span data-ttu-id="40927-139">*westeurope* è una posizione geografica appropriata per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="40927-139">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="40927-140">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del gruppo di risorse, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-140">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a><span data-ttu-id="40927-141">Creare un cluster Kubernetes usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="40927-141">Create a Kubernetes cluster using the Azure CLI</span></span>

1. <span data-ttu-id="40927-142">Creare un cluster Kubernetes nel nuovo gruppo di risorse, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-142">Create a Kubernetes cluster in your new resource group; for example:</span></span>  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   <span data-ttu-id="40927-143">Dove:</span><span class="sxs-lookup"><span data-stu-id="40927-143">Where:</span></span>  
      * <span data-ttu-id="40927-144">*wingtiptoys kubernetes* è il nome del gruppo di risorse creato in precedenza in questo articolo</span><span class="sxs-lookup"><span data-stu-id="40927-144">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="40927-145">*wingtiptoys-cluster* è un nome univoco per il cluster Kubernetes</span><span class="sxs-lookup"><span data-stu-id="40927-145">*wingtiptoys-cluster* is a unique name for your Kubernetes cluster</span></span>
      * <span data-ttu-id="40927-146">*wingtiptoys* è un nome DNS univoco per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="40927-146">*wingtiptoys* is a unique name DNS name for your application</span></span>

   <span data-ttu-id="40927-147">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del cluster, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-147">The Azure CLI will display the results of your cluster creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. <span data-ttu-id="40927-148">Scaricare le credenziali per il nuovo cluster Kubernetes, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-148">Download your credentials for your new Kubernetes cluster; for example:</span></span>  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. <span data-ttu-id="40927-149">Verificare la connessione con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-149">Verify your connection with the following command:</span></span>
   ```shell 
   kubectl get nodes
   ```

   <span data-ttu-id="40927-150">Verrà visualizzato un elenco di nodi e stati, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-150">You should see a list of nodes and statuses like the following example:</span></span>

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="40927-151">Creare un registro contenitori di Azure privato usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="40927-151">Create a private Azure container registry using the Azure CLI</span></span>

1. <span data-ttu-id="40927-152">Creare un registro contenitori di Azure privato nel gruppo di risorse per ospitare l'immagine di Docker, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-152">Create a private Azure container registry in your resource group to host your Docker image; for example:</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="40927-153">Dove:</span><span class="sxs-lookup"><span data-stu-id="40927-153">Where:</span></span>  
      * <span data-ttu-id="40927-154">*wingtiptoys kubernetes* è il nome del gruppo di risorse creato in precedenza in questo articolo</span><span class="sxs-lookup"><span data-stu-id="40927-154">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="40927-155">*wingtiptoysregistry* è un nome univoco per il registro privato</span><span class="sxs-lookup"><span data-stu-id="40927-155">*wingtiptoysregistry* is a unique name for your private registry</span></span>
      * <span data-ttu-id="40927-156">*westeurope* è una posizione geografica appropriata per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="40927-156">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="40927-157">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del registro, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-157">The Azure CLI will display the results of your registry creation; for example:</span></span>  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. <span data-ttu-id="40927-158">Recuperare la password per il registro contenitori dall'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="40927-158">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   <span data-ttu-id="40927-159">Nell'interfaccia della riga di comando di Azure verrà visualizzata la password del registro, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-159">The Azure CLI will display the password for your registry; for example:</span></span>  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="40927-160">Passare alla directory di configurazione dell'installazione di Maven (impostazione predefinita: ~/.m2/ or C:\Users\nomeutente\.m2) e aprire il file *settings.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="40927-160">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="40927-161">Aggiungere l'URL, il nome utente e la password di Registro contenitori di Azure a una nuova raccolta `<server>` nel file *settings.xml*.</span><span class="sxs-lookup"><span data-stu-id="40927-161">Add your Azure Container Registry URL, username and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="40927-162">I valori `id` e `username` corrispondono al nome del registro.</span><span class="sxs-lookup"><span data-stu-id="40927-162">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="40927-163">Usare il valore `password` del comando precedente, senza virgolette.</span><span class="sxs-lookup"><span data-stu-id="40927-163">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="40927-164">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="40927-164">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="40927-165">Aggiornare la raccolta `<properties>` nel file *pom.xml* con il valore del server di accesso per il registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="40927-165">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="40927-166">Aggiornare la raccolta `<plugins>` nel file *pom.xml* in modo che `<plugin>` contenga l'indirizzo del server di accesso e il nome del registro per il registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="40927-166">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="40927-167">Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire il comando Maven seguente per creare il contenitore Docker ed effettuare il push dell'immagine nel registro:</span><span class="sxs-lookup"><span data-stu-id="40927-167">Navigate to the completed project directory for your Spring Boot application, and run the following Maven command to build the Docker container and push the image to your registry:</span></span>

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   <span data-ttu-id="40927-168">Maven visualizzerà i risultati della compilazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-168">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a><span data-ttu-id="40927-169">Configurare l'app Spring Boot per usare il plug-in Maven Fabric8</span><span class="sxs-lookup"><span data-stu-id="40927-169">Configure your Spring Boot app to use the Fabric8 Maven plugin</span></span>

1. <span data-ttu-id="40927-170">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="40927-170">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="40927-171">Aggiornare la raccolta `<plugins>` nel file *pom.xml* per aggiungere il plug-in Maven Fabric8:</span><span class="sxs-lookup"><span data-stu-id="40927-171">Update the `<plugins>` collection in the *pom.xml* file to add the Fabric8 Maven plugin:</span></span>

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="40927-172">Passare alla directory principale di origine per l'applicazione Spring Boot (ad esempio: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" o "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*") e creare una nuova cartella denominata "*fabric8*".</span><span class="sxs-lookup"><span data-stu-id="40927-172">Navigate to the main source directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*"), and create a new folder named "*fabric8*".</span></span>

1. <span data-ttu-id="40927-173">Creare tre file di frammenti YAML nella nuova cartella *fabric8*:</span><span class="sxs-lookup"><span data-stu-id="40927-173">Create three YAML fragment files in the new *fabric8* folder:</span></span>

   <span data-ttu-id="40927-174">a.</span><span class="sxs-lookup"><span data-stu-id="40927-174">a.</span></span> <span data-ttu-id="40927-175">Creare un file denominato **deployment.yml** con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-175">Create a file named **deployment.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   <span data-ttu-id="40927-176">b.</span><span class="sxs-lookup"><span data-stu-id="40927-176">b.</span></span> <span data-ttu-id="40927-177">Creare un file denominato **secrets.yml** con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-177">Create a file named **secrets.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   <span data-ttu-id="40927-178">c.</span><span class="sxs-lookup"><span data-stu-id="40927-178">c.</span></span> <span data-ttu-id="40927-179">Creare un file denominato **service.yml** con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="40927-179">Create a file named **service.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. <span data-ttu-id="40927-180">Eseguire il comando seguente di Maven per compilare il file dell'elenco di risorse di Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="40927-180">Run the following Maven command to build the Kubernetes resource list file:</span></span>

   ```shell
   mvn fabric8:resource
   ```

   <span data-ttu-id="40927-181">Questo comando unisce in tutti i file YAML delle risorse Kubernetes nella cartella *src/main/fabric8* in un unico file YAML che contiene un elenco di risorse Kubernetes, può essere applicato al cluster Kubernetes direttamente o esportato in un grafico di Helm.</span><span class="sxs-lookup"><span data-stu-id="40927-181">This command merges all Kubernetes resource yaml files under the *src/main/fabric8* folder to a YAML file that contains a Kubernetes resource list, which can be applied to Kubernetes cluster directly or export to a helm chart.</span></span>

   <span data-ttu-id="40927-182">Maven visualizzerà i risultati della compilazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-182">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="40927-183">Eseguire il comando seguente di Maven per applicare il file dell'elenco di risorse al cluster Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="40927-183">Run the following Maven command to apply the resource list file to your Kubernetes cluster:</span></span>

   ```shell
   mvn fabric8:apply
   ```

   <span data-ttu-id="40927-184">Maven visualizzerà i risultati della compilazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-184">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="40927-185">Dopo la distribuzione dell'app nel cluster, eseguire una query sull'indirizzo IP esterno tramite l'applicazione `kubectl`, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-185">Once the app is deployed to the cluster, query the external IP address using the `kubectl` application; for example:</span></span>
   ```shell
   kubectl get svc -w
   ```

   <span data-ttu-id="40927-186">In `kubectl` verranno visualizzati gli indirizzi IP interni ed esterni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-186">`kubectl` will display your internal and external IP addresses; for example:</span></span>
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   <span data-ttu-id="40927-187">Per aprire l'applicazione in un Web browser, è possibile usare l'indirizzo IP esterno.</span><span class="sxs-lookup"><span data-stu-id="40927-187">You can use the external IP address to open your application in a web browser.</span></span>

   ![Esplorare l'applicazione di esempio esternamente][SB02]

## <a name="delete-your-kubernetes-cluster"></a><span data-ttu-id="40927-189">Eliminare il cluster Kubernetes</span><span class="sxs-lookup"><span data-stu-id="40927-189">Delete your Kubernetes cluster</span></span>

<span data-ttu-id="40927-190">Quando il cluster Kubernetes non è più necessario, è possibile usare il comando `az group delete` per rimuovere il gruppo di risorse e, di conseguenza, tutte le relative risorse, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40927-190">When your Kubernetes cluster is no longer needed, you can use the `az group delete` command to remove the resource group, which will remove all of its related resources; for example:</span></span>

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a><span data-ttu-id="40927-191">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="40927-191">Next steps</span></span>

<span data-ttu-id="40927-192">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="40927-192">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="40927-193">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="40927-193">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="40927-194">Distribuire un'applicazione Spring Boot in Linux nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="40927-194">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)
* [<span data-ttu-id="40927-195">Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="40927-195">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="40927-196">Per altre informazioni su come usare Azure con Java, vedere il [Centro per sviluppatori Java di Azure] e gli [strumenti Java per Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="40927-196">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="40927-197">Per maggiori dettagli sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).</span><span class="sxs-lookup"><span data-stu-id="40927-197">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="40927-198">Per informazioni su come iniziare a creare applicazioni Spring Boot proprie, vedere **Spring Initializr** all'indirizzo <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="40927-198">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at <https://start.spring.io/>.</span></span>

<span data-ttu-id="40927-199">Per altre informazioni su come iniziare a creare una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="40927-199">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at <https://start.spring.io/>.</span></span>

<span data-ttu-id="40927-200">Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].</span><span class="sxs-lookup"><span data-stu-id="40927-200">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[servizio contenitore di Azure]: https://azure.microsoft.com/services/container-service/
[Centro per sviluppatori Java di Azure]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB02.png
