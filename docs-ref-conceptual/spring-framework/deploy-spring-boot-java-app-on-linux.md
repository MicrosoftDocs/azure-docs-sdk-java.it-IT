---
title: Distribuire un'app Web Spring Boot nel servizio app di Azure per Container
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot come app Web Linux in Microsoft Azure.
services: azure app service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: azure app service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 407b852e24ef88d2fb075bd064f1acf2b107ddc1
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270861"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a><span data-ttu-id="9f871-103">Distribuire un'applicazione Web Spring Boot nel servizio app di Azure per Container</span><span class="sxs-lookup"><span data-stu-id="9f871-103">Deploy a Spring Boot application on Azure App Service for Container</span></span>

<span data-ttu-id="9f871-104">Questa esercitazione illustra l'uso di [Docker] per distribuire l'applicazione [Spring Boot] in contenitori e distribuire l'immagine Docker in un host Linux nel [servizio app di Azure](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).</span><span class="sxs-lookup"><span data-stu-id="9f871-104">This tutorial walks you through using [Docker] to containerize your [Spring Boot] application and deploy your own docker image to a Linux host in the [Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f871-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9f871-105">Prerequisites</span></span>

<span data-ttu-id="9f871-106">Per completare la procedura di questa esercitazione, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f871-106">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="9f871-107">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="9f871-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="9f871-108">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="9f871-108">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="9f871-109">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="9f871-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="9f871-110">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="9f871-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="9f871-111">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="9f871-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="9f871-112">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="9f871-112">A [Git] client.</span></span>
* <span data-ttu-id="9f871-113">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="9f871-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9f871-114">A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9f871-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="9f871-115">Creare l'app Web introduttiva di Spring Boot in Docker</span><span class="sxs-lookup"><span data-stu-id="9f871-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="9f871-116">I passaggi seguenti illustrano i passaggi necessari per creare una semplice applicazione Web Spring Boot e testarla localmente.</span><span class="sxs-lookup"><span data-stu-id="9f871-116">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="9f871-117">Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="9f871-118">- o-</span><span class="sxs-lookup"><span data-stu-id="9f871-118">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="9f871-119">Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory appena creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="9f871-120">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-120">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="9f871-121">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-121">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="9f871-122">Dopo aver creato l'app Web, passare alla directory `target` in cui si trova il file JAR e avviare l'app Web. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-122">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="9f871-123">Testare l'app Web esplorandola localmente tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="9f871-123">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="9f871-124">Ad esempio, se è disponibile un cURL e il server Tomcat è configurato per l'esecuzione sulla porta 80:</span><span class="sxs-lookup"><span data-stu-id="9f871-124">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="9f871-125">Dovrebbe essere visualizzato il messaggio **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="9f871-125">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="9f871-127">Creare un'istanza di Registro Azure Container da usare come registro Docker privato</span><span class="sxs-lookup"><span data-stu-id="9f871-127">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="9f871-128">La procedura seguente illustra come usare il portale di Azure per creare un'istanza di Registro Azure Container.</span><span class="sxs-lookup"><span data-stu-id="9f871-128">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9f871-129">Se si vuole usare l'interfaccia della riga di comando di Azure anziché il portale di Azure, seguire i passaggi in [Creare un registro per contenitori Docker privati usando l'interfaccia della riga di comando di Azure 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9f871-129">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
>

1. <span data-ttu-id="9f871-130">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="9f871-130">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="9f871-131">Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="9f871-131">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="9f871-132">Fare clic sull'icona di menu **+ Nuovo**, su **Contenitori** e quindi su **Registro Azure Container**.</span><span class="sxs-lookup"><span data-stu-id="9f871-132">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Creare una nuova istanza di Registro Azure Container][AR01]

1. <span data-ttu-id="9f871-134">Quando viene visualizzata la pagina delle informazioni per il modello di Registro Azure Container, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9f871-134">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Creare una nuova istanza di Registro Azure Container][AR02]

1. <span data-ttu-id="9f871-136">Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9f871-136">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Configurare le impostazioni di Registro Azure Container][AR03]

1. <span data-ttu-id="9f871-138">Una volta creato il registro contenitori, passare al registro contenitori stesso nel portale di Azure e quindi fare clic su **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="9f871-138">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="9f871-139">Prendere nota del nome utente e della password per i passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="9f871-139">Take note of the username and password for the next steps.</span></span>

   ![Chiavi di accesso a Registro Azure Container][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="9f871-141">Configurare Maven per l'uso delle chiavi di accesso di Registro Azure Container</span><span class="sxs-lookup"><span data-stu-id="9f871-141">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="9f871-142">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio "*C:\SpringBoot\gs-spring-boot-docker\complete*" o " */users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="9f871-142">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="9f871-143">Aggiornare la raccolta `<properties>` nel file *pom.xml* con l'ultima versione di [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin), oltre che con il valore del server di accesso e le impostazioni di accesso per Registro Azure Container della sezione precedente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9f871-143">Update the `<properties>` collection in the *pom.xml* file with the latest version of [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) and login server value and access settings for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="9f871-144">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-144">For example:</span></span>

   ```xml
   <properties>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. <span data-ttu-id="9f871-145">Aggiungere [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) alla raccolta `<plugins>` nel file *pom.xml*, specificare l'immagine di base in `<from>/<image>` e il nome dell'immagine finale `<to>/<image>`, quindi specificare il nome utente e la password della sezione precedente in `<to>/<auth>`.</span><span class="sxs-lookup"><span data-stu-id="9f871-145">Add [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) to the `<plugins>` collection in the *pom.xml* file, specify the base image at `<from>/<image>` and  final image name `<to>/<image>`, specify the username and password from previous section at `<to>/<auth>`.</span></span> <span data-ttu-id="9f871-146">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9f871-146">For example:</span></span>

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
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="9f871-147">Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire il comando seguente per ricompilare l'applicazione ed effettuare il push del contenitore in Registro Azure Container:</span><span class="sxs-lookup"><span data-stu-id="9f871-147">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> <span data-ttu-id="9f871-148">Quando si usa Jib per eseguire il push dell'immagine nel Registro Azure Container, l'immagine non rispetterà *Dockerfile*. Per i dettagli, vedere [questo](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html) documento.</span><span class="sxs-lookup"><span data-stu-id="9f871-148">When you are using Jib to push your image to the Azure Container Registry, the image will not honor *Dockerfile*, see [this](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html) document for details.</span></span>
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="9f871-149">Creare un'app Web in Linux nel servizio app di Azure usando l'immagine del contenitore</span><span class="sxs-lookup"><span data-stu-id="9f871-149">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="9f871-150">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="9f871-150">Browse to the [Azure portal] and sign in.</span></span>

2. <span data-ttu-id="9f871-151">Fare clic sull'icona di menu **+ Crea una risorsa**, quindi su **Web** e infine su **App Web per contenitori**.</span><span class="sxs-lookup"><span data-stu-id="9f871-151">Click the menu icon for **+ Create a resource**, then click **Web**, and then click **Web App for Containers**.</span></span>
   
   ![Creare una nuova app Web nel portale di Azure][LX01]

3. <span data-ttu-id="9f871-153">Quando viene visualizzata la pagina **App Web in Linux**, immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f871-153">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="9f871-154">a.</span><span class="sxs-lookup"><span data-stu-id="9f871-154">a.</span></span> <span data-ttu-id="9f871-155">Immettere un nome univoco per **Nome app**, ad esempio "*wingtiptoyslinux*"</span><span class="sxs-lookup"><span data-stu-id="9f871-155">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*"</span></span>

   <span data-ttu-id="9f871-156">b.</span><span class="sxs-lookup"><span data-stu-id="9f871-156">b.</span></span> <span data-ttu-id="9f871-157">Scegliere una **Sottoscrizione** dall'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="9f871-157">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="9f871-158">c.</span><span class="sxs-lookup"><span data-stu-id="9f871-158">c.</span></span> <span data-ttu-id="9f871-159">Selezionare un **Gruppo di risorse** esistente o specificare un nome per creare uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="9f871-159">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="9f871-160">d.</span><span class="sxs-lookup"><span data-stu-id="9f871-160">d.</span></span> <span data-ttu-id="9f871-161">Scegliere *Linux* come **Sistema operativo**.</span><span class="sxs-lookup"><span data-stu-id="9f871-161">Choose *Linux* as the **OS**.</span></span>

   <span data-ttu-id="9f871-162">e.</span><span class="sxs-lookup"><span data-stu-id="9f871-162">e.</span></span> <span data-ttu-id="9f871-163">Fare clic su **Piano di servizio app/Località**  e scegliere un piano esistente oppure fare clic su **Crea nuovo** per crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="9f871-163">Click **App Service plan/Location** and choose an existing app service plan, or click **Create new** to create a new app service plan.</span></span>

   <span data-ttu-id="9f871-164">f.</span><span class="sxs-lookup"><span data-stu-id="9f871-164">f.</span></span> <span data-ttu-id="9f871-165">Fare clic su **Configura contenitore** e immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f871-165">Click **Configure container** and enter the following information:</span></span>

   * <span data-ttu-id="9f871-166">Scegliere **Contenitore singolo** e **Registro Azure Container**.</span><span class="sxs-lookup"><span data-stu-id="9f871-166">Choose **Single Container** and  **Azure Container Registry**.</span></span>

   * <span data-ttu-id="9f871-167">**Registro di sistema**: scegliere il nome del contenitore creato in precedenza, ad esempio "*wingtiptoysregistry*"</span><span class="sxs-lookup"><span data-stu-id="9f871-167">**Registry**: Choose your container name created earlier; for example: "*wingtiptoysregistry*"</span></span>

   * <span data-ttu-id="9f871-168">**Immagine**: scegliere il nome dell'immagine, ad esempio "*gs-spring-boot-docker*"</span><span class="sxs-lookup"><span data-stu-id="9f871-168">**Image**: Choose the image name; for example: "*gs-spring-boot-docker*"</span></span>
   
   * <span data-ttu-id="9f871-169">**Tag**: scegliere il tag per l'immagine, ad esempio "*latest*"</span><span class="sxs-lookup"><span data-stu-id="9f871-169">**Tag**: Choose the tag for the image; for example: "*latest*"</span></span>
   
   * <span data-ttu-id="9f871-170">**File di avvio**: lasciare vuoto questo campo perché l'immagine include già un comando di avvio</span><span class="sxs-lookup"><span data-stu-id="9f871-170">**Startup File**: Keep it blank since the image already has the start up command</span></span>
   
   <span data-ttu-id="9f871-171">e.</span><span class="sxs-lookup"><span data-stu-id="9f871-171">e.</span></span> <span data-ttu-id="9f871-172">Dopo aver immesso tutte queste informazioni, fare clic su **Applica**.</span><span class="sxs-lookup"><span data-stu-id="9f871-172">Once you have entered all of the above information, click **Apply**.</span></span>

   ![Configurare le impostazioni dell'app Web][LX02]

4. <span data-ttu-id="9f871-174">Fare clic su **Create**(Crea).</span><span class="sxs-lookup"><span data-stu-id="9f871-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9f871-175">Azure eseguirà automaticamente il mapping delle richieste Internet al server Tomcat incorporato in esecuzione sulla porta standard 80 o 8080.</span><span class="sxs-lookup"><span data-stu-id="9f871-175">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="9f871-176">Se tuttavia il server Tomcat incorporato è stato configurato per l'esecuzione su una porta personalizzata, è necessario aggiungere all'app Web una variabile di ambiente che definisce la porta del server Tomcat incorporato.</span><span class="sxs-lookup"><span data-stu-id="9f871-176">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="9f871-177">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="9f871-177">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="9f871-178">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="9f871-178">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="9f871-179">Fare clic sull'icona **Servizi app** e selezionare l'app Web nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="9f871-179">Click the icon for **App Services**, and select your web app from the list.</span></span>
>
> 4. <span data-ttu-id="9f871-180">Fare clic su **Configurazione**.</span><span class="sxs-lookup"><span data-stu-id="9f871-180">Click **Configuration**.</span></span> <span data-ttu-id="9f871-181">(Elemento 1 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="9f871-181">(Item #1 in the image below.)</span></span>
>
> 5. <span data-ttu-id="9f871-182">Nella sezione **Impostazioni applicazione** aggiungere una nuova impostazione denominata **PORT** e immettere il numero di porta personalizzato come valore.</span><span class="sxs-lookup"><span data-stu-id="9f871-182">In the **Application settings** section, add a new setting named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="9f871-183">(Elementi 2, 3 e 4 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="9f871-183">(Item #2, #3, #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="9f871-184">Fare clic su **Save**.</span><span class="sxs-lookup"><span data-stu-id="9f871-184">Click **Save**.</span></span> <span data-ttu-id="9f871-185">(Elemento 5 nell'immagine seguente.)</span><span class="sxs-lookup"><span data-stu-id="9f871-185">(Item #5 in the image below.)</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="9f871-187">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9f871-187">Next steps</span></span>

<span data-ttu-id="9f871-188">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="9f871-188">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9f871-189">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="9f871-189">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="9f871-190">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9f871-190">Additional Resources</span></span>

<span data-ttu-id="9f871-191">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f871-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="9f871-192">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9f871-192">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="9f871-193">Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container</span><span class="sxs-lookup"><span data-stu-id="9f871-193">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="9f871-194">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="9f871-194">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="9f871-195">Per maggiori dettagli sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).</span><span class="sxs-lookup"><span data-stu-id="9f871-195">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="9f871-196">Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="9f871-196">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="9f871-197">Per altre informazioni su come iniziare a creare una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="9f871-197">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="9f871-198">Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].</span><span class="sxs-lookup"><span data-stu-id="9f871-198">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure per sviluppatori Java]: /java/azure/
[Azure for Java Developers]: /java/azure/
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
[Uso di Azure DevOps e Java]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

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
