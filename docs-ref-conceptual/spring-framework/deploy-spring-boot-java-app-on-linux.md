---
title: Distribuire un'app Web Spring Boot su Linux nel servizio contenitore di Azure
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot come app Web Linux in Microsoft Azure.
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: container-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 49d94d11ad6a4e103ded849e477d99f01955c693
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090864"
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a><span data-ttu-id="16623-103">Distribuire un'applicazione Spring Boot in Linux nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="16623-103">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>

<span data-ttu-id="16623-104">Questa esercitazione illustra l'uso di [Docker] per sviluppare e distribuire un'applicazione [Spring Boot] in un host Linux nel [servizio contenitore di Azure].</span><span class="sxs-lookup"><span data-stu-id="16623-104">This tutorial walks you through using [Docker] to develop and deploy a [Spring Boot] application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16623-105">prerequisiti</span><span class="sxs-lookup"><span data-stu-id="16623-105">Prerequisites</span></span>

<span data-ttu-id="16623-106">Per completare la procedura di questa esercitazione, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="16623-106">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="16623-107">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="16623-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="16623-108">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="16623-108">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="16623-109">Un [Java Developer Kit (JDK)] aggiornato.</span><span class="sxs-lookup"><span data-stu-id="16623-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="16623-110">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="16623-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="16623-111">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="16623-111">A [Git] client.</span></span>
* <span data-ttu-id="16623-112">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="16623-112">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="16623-113">A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.</span><span class="sxs-lookup"><span data-stu-id="16623-113">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="16623-114">Creare l'app Web introduttiva di Spring Boot in Docker</span><span class="sxs-lookup"><span data-stu-id="16623-114">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="16623-115">I passaggi seguenti illustrano i passaggi necessari per creare una semplice applicazione Web Spring Boot e testarla localmente.</span><span class="sxs-lookup"><span data-stu-id="16623-115">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="16623-116">Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-116">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="16623-117">- o-</span><span class="sxs-lookup"><span data-stu-id="16623-117">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="16623-118">Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory appena creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-118">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="16623-119">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-119">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="16623-120">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-120">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="16623-121">Dopo aver creato l'app Web, passare alla directory `target` in cui si trova il file JAR e avviare l'app Web. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-121">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="16623-122">Testare l'app Web esplorandola localmente tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="16623-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="16623-123">Ad esempio, se è disponibile un cURL e il server Tomcat è configurato per l'esecuzione sulla porta 80:</span><span class="sxs-lookup"><span data-stu-id="16623-123">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="16623-124">Dovrebbe essere visualizzato il messaggio seguente: **Hello Docker World!**</span><span class="sxs-lookup"><span data-stu-id="16623-124">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="16623-126">Creare un Registro contenitori di Azure da usare come registro Docker privato</span><span class="sxs-lookup"><span data-stu-id="16623-126">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="16623-127">La procedura seguente illustra come usare il portale di Azure per creare un Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="16623-127">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="16623-128">Se si vuole usare l'interfaccia della riga di comando di Azure anziché il portale di Azure, seguire i passaggi in [Creare un registro per contenitori Docker privati usando l'interfaccia della riga di comando di Azure 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="16623-128">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
>

1. <span data-ttu-id="16623-129">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="16623-129">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="16623-130">Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="16623-130">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="16623-131">Fare clic sull'icona di menu **+ Nuovo**, su **Contenitori** e quindi su **Registro contenitori di Azure**.</span><span class="sxs-lookup"><span data-stu-id="16623-131">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Creare un nuovo Registro contenitori di Azure][AR01]

1. <span data-ttu-id="16623-133">Quando viene visualizzata la pagina delle informazioni per il modello di Registro contenitori di Azure, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="16623-133">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Creare un nuovo Registro contenitori di Azure][AR02]

1. <span data-ttu-id="16623-135">Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="16623-135">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Configurare le impostazioni del Registro contenitori di Azure][AR03]

1. <span data-ttu-id="16623-137">Una volta creato il registro contenitori, passare al registro contenitori stesso nel portale di Azure e quindi fare clic su **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="16623-137">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="16623-138">Prendere nota del nome utente e della password per i passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="16623-138">Take note of the username and password for the next steps.</span></span>

   ![Chiavi di accesso al Registro contenitori di Azure][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="16623-140">Configurare Maven per l'uso delle chiavi di accesso del Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="16623-140">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="16623-141">Passare alla directory di configurazione dell'installazione di Maven e aprire il file *settings.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="16623-141">Navigate to the configuration directory for your Maven installation and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="16623-142">Aggiungere le impostazioni di accesso al Registro contenitori di Azure dalla sezione precedente di questa esercitazione alla raccolta `<servers>` nel file *settings.xml* file. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-142">Add your Azure Container Registry access settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="16623-143">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="16623-143">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="16623-144">Aggiornare la raccolta `<properties>` nel file *pom.xml* con il valore del server di accesso per il Registro contenitori di Azure dalla sezione precedente di questa esercitazione. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-144">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="16623-145">Aggiornare la raccolta `<plugins>` nel file *pom.xml* in modo che `<plugin>` contenga l'indirizzo del server di accesso e il nome del registro per il Registro contenitori di Azure dalla sezione precedente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="16623-145">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="16623-146">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="16623-146">For example:</span></span>

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

1. <span data-ttu-id="16623-147">Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire il comando seguente per ricompilare l'applicazione ed effettuare il push del contenitore nel Registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="16623-147">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="16623-148">Quando si effettua il push del contenitore Docker in Azure, è possibile che venga visualizzato un messaggio di errore simile a uno dei seguenti, anche se il contenitore Docker è stato creato correttamente:</span><span class="sxs-lookup"><span data-stu-id="16623-148">When you are pushing your Docker container to Azure, you may receive an error message that is similar to one of the following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="16623-149">In questo caso, è possibile che sia necessario accedere all'account Azure dalla riga di comando di Docker. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-149">If this happens, you may need to sign in to your Azure account from the Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="16623-150">È quindi possibile effettuare il push del contenitore dalla riga di comando. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="16623-150">You can then push your container from the command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="16623-151">Creare un'app Web in Linux nel servizio app di Azure usando l'immagine del contenitore</span><span class="sxs-lookup"><span data-stu-id="16623-151">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="16623-152">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="16623-152">Browse to the [Azure portal] and sign in.</span></span>

2. <span data-ttu-id="16623-153">Fare clic sull'icona di menu **+ Nuovo**, su **Web e dispositivi mobili**, quindi su **App Web in Linux**.</span><span class="sxs-lookup"><span data-stu-id="16623-153">Click the menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Creare una nuova app Web nel portale di Azure][LX01]

3. <span data-ttu-id="16623-155">Quando viene visualizzata la pagina **App Web in Linux**, immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="16623-155">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="16623-156">a.</span><span class="sxs-lookup"><span data-stu-id="16623-156">a.</span></span> <span data-ttu-id="16623-157">Immettere un nome univoco per il **Nome app**, ad esempio: "*wingtiptoyslinux*".</span><span class="sxs-lookup"><span data-stu-id="16623-157">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="16623-158">b.</span><span class="sxs-lookup"><span data-stu-id="16623-158">b.</span></span> <span data-ttu-id="16623-159">Scegliere una **Sottoscrizione** dall'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="16623-159">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="16623-160">c.</span><span class="sxs-lookup"><span data-stu-id="16623-160">c.</span></span> <span data-ttu-id="16623-161">Selezionare un **Gruppo di risorse** esistente o specificare un nome per creare uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="16623-161">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="16623-162">d.</span><span class="sxs-lookup"><span data-stu-id="16623-162">d.</span></span> <span data-ttu-id="16623-163">Fare clic su **Configura contenitore** e immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="16623-163">Click **Configure container** and enter the following information:</span></span>

   * <span data-ttu-id="16623-164">Scegliere **Registro di sistema privato**.</span><span class="sxs-lookup"><span data-stu-id="16623-164">Choose **Private registry**.</span></span>

   * <span data-ttu-id="16623-165">**Immagine e tag facoltativo**: specificare il nome del contenitore dai passaggi precedenti, ad esempio: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span><span class="sxs-lookup"><span data-stu-id="16623-165">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

   * <span data-ttu-id="16623-166">**URL server**: specificare l'URL del registro dai passaggi precedenti, ad esempio "*<https://wingtiptoysregistry.azurecr.io>*"</span><span class="sxs-lookup"><span data-stu-id="16623-166">**Server URL**: Specify your registry URL from earlier; for example: "*<https://wingtiptoysregistry.azurecr.io>*"</span></span>

   * <span data-ttu-id="16623-167">**Nome utente di accesso** e **Password**: specificare le credenziali di accesso dalle **Chiavi di accesso** usate nei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="16623-167">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="16623-168">e.</span><span class="sxs-lookup"><span data-stu-id="16623-168">e.</span></span> <span data-ttu-id="16623-169">Dopo aver immesso tutte le informazioni precedenti, fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="16623-169">Once you have entered all of the above information, click **OK**.</span></span>

   ![Configurare le impostazioni dell'app Web][LX02]

4. <span data-ttu-id="16623-171">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="16623-171">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="16623-172">Azure eseguirà automaticamente il mapping delle richieste Internet al server Tomcat incorporato in esecuzione sulla porta standard 80 o 8080.</span><span class="sxs-lookup"><span data-stu-id="16623-172">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="16623-173">Se tuttavia il server Tomcat incorporato è stato configurato per l'esecuzione su una porta personalizzata, è necessario aggiungere all'app Web una variabile di ambiente che definisce la porta del server Tomcat incorporato.</span><span class="sxs-lookup"><span data-stu-id="16623-173">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="16623-174">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="16623-174">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="16623-175">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="16623-175">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="16623-176">Fare clic sull'icona per **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="16623-176">Click the icon for **App Services**.</span></span> <span data-ttu-id="16623-177">(Vedere l'elemento 1 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="16623-177">(See item #1 in the image below.)</span></span>
>
> 3. <span data-ttu-id="16623-178">Selezionare l'app Web dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="16623-178">Select your web app from the list.</span></span> <span data-ttu-id="16623-179">(Elemento 2 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="16623-179">(Item #2 in the image below.)</span></span>
>
> 4. <span data-ttu-id="16623-180">Fare clic su **Impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="16623-180">Click **Application Settings**.</span></span> <span data-ttu-id="16623-181">(Elemento 3 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="16623-181">(Item #3 in the image below.)</span></span>
>
> 5. <span data-ttu-id="16623-182">Nella sezione **Impostazioni app** aggiungere una nuova variabile di ambiente denominata **PORT** e immettere il numero di porta personalizzato come valore.</span><span class="sxs-lookup"><span data-stu-id="16623-182">In the **App settings** section, add a new environment variable named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="16623-183">(Elemento 4 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="16623-183">(Item #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="16623-184">Fare clic su **Save**.</span><span class="sxs-lookup"><span data-stu-id="16623-184">Click **Save**.</span></span> <span data-ttu-id="16623-185">(Elemento 5 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="16623-185">(Item #5 in the image below.)</span></span>
>
> ![Salvataggio di un numero di porta personalizzato nel portale di Azure][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="16623-187">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="16623-187">Next steps</span></span>

<span data-ttu-id="16623-188">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="16623-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="16623-189">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="16623-189">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="16623-190">Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="16623-190">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="16623-191">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="16623-191">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="16623-192">Per maggiori dettagli sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).</span><span class="sxs-lookup"><span data-stu-id="16623-192">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="16623-193">Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="16623-193">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="16623-194">Per altre informazioni su come iniziare a creare una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="16623-194">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="16623-195">Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].</span><span class="sxs-lookup"><span data-stu-id="16623-195">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Servizio contenitore di Azure]: https://azure.microsoft.com/services/container-service/
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Portale di Azure]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
