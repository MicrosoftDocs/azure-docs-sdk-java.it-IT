---
title: Pubblicare un contenitore Docker usando Azure Toolkit for IntelliJ
description: Informazioni su come pubblicare un'app Web in Microsoft Azure come contenitore Docker usando il Toolkit di Azure per IntelliJ.
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: f92040b34b9897d9feea8d2ec5e8748e75fff7f7
ms.sourcegitcommit: 381a865f2849be8e3044d24cd4b3b54e9bbb7abc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2018
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="078d6-103">Pubblicare un'app Web come contenitore Docker usando Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="078d6-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="078d6-104">I contenitori Docker sono un metodo molto diffuso per la distribuzione di applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="078d6-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="078d6-105">L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server.</span><span class="sxs-lookup"><span data-stu-id="078d6-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="078d6-106">Azure Toolkit for IntelliJ semplifica questo processo per gli sviluppatori Java aggiungendo la funzionalità *Publish as Docker Container* (Pubblica come contenitore Docker) per la distribuzione in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="078d6-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="078d6-107">Questo articolo illustra la procedura da seguire per pubblicare le applicazioni in Azure come contenitori Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="078d6-108">Altre informazioni su Docker sono disponibili nel [sito Web Docker].</span><span class="sxs-lookup"><span data-stu-id="078d6-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="078d6-109">Pubblicare l'app Web in Azure usando un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="078d6-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="078d6-110">Per pubblicare l'app Web, è necessario creare un elemento pronto per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="078d6-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="078d6-111">Per altre informazioni, vedere la sezione [Altre informazioni sulla creazione di elementi](#artifacts).</span><span class="sxs-lookup"><span data-stu-id="078d6-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="078d6-112">Dopo aver completato la distribuzione guidata almeno una volta, la maggior parte delle impostazioni viene usata come impostazioni predefinite all'esecuzione successiva.</span><span class="sxs-lookup"><span data-stu-id="078d6-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="078d6-113">Aprire il progetto dell'app Web in IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="078d6-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="078d6-114">Per avviare la procedura guidata **Publish as Docker Container** (Pubblica come contenitore Docker), eseguire una di queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="078d6-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="078d6-115">Fare clic sul progetto con il pulsante destro del mouse nella finestra degli strumenti **Project** (Progetto), fare clic su **Azure** e quindi fare clic su **Publish as Docker Container** (Pubblica come contenitore Docker):</span><span class="sxs-lookup"><span data-stu-id="078d6-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![Comando Publish as Docker Container (Pubblica come contenitore Docker)][PUB01]

   * <span data-ttu-id="078d6-117">Sulla barra degli strumenti di IntelliJ fare clic sul pulsante **Publish Group** (Pubblica gruppo) e quindi fare clic su **Publish as Docker Container** (Pubblica come contenitore Docker):</span><span class="sxs-lookup"><span data-stu-id="078d6-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="078d6-118">![Comando Publish as Docker Container][PUB02] (Pubblica come contenitore Docker)</span><span class="sxs-lookup"><span data-stu-id="078d6-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="078d6-119">Verrà visualizzata la procedura guidata **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure).</span><span class="sxs-lookup"><span data-stu-id="078d6-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB03]

3. <span data-ttu-id="078d6-121">Nella finestra **Type an image name, select the artifact's path and check a Docker host to be used** (Digitare un nome immagine, scegliere il percorso dell'elemento e selezionare un host Docker da usare) seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="078d6-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="078d6-122">a.</span><span class="sxs-lookup"><span data-stu-id="078d6-122">a.</span></span> <span data-ttu-id="078d6-123">Nella casella **Docker image name** (Nome immagine Docker) immettere un nome univoco per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="078d6-124">La procedura guidata crea automaticamente un nome, ma è possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="078d6-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="078d6-125">b.</span><span class="sxs-lookup"><span data-stu-id="078d6-125">b.</span></span> <span data-ttu-id="078d6-126">Nell'area **Hosts** (Host) vengono visualizzati tutti gli host Docker già creati.</span><span class="sxs-lookup"><span data-stu-id="078d6-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="078d6-127">Eseguire una di queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="078d6-127">Do either of the following:</span></span> 
      * <span data-ttu-id="078d6-128">Se è disponibile un host Docker esistente, è possibile distribuirvi l'app Web.</span><span class="sxs-lookup"><span data-stu-id="078d6-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="078d6-129">Per creare un host Docker, fare clic sul segno più verde (**+**).</span><span class="sxs-lookup"><span data-stu-id="078d6-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="078d6-130">Verrà visualizzata la finestra di dialogo **Create Docker Host** (Crea host Docker).</span><span class="sxs-lookup"><span data-stu-id="078d6-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB04a]

4. <span data-ttu-id="078d6-132">Nella finestra **Configure the new virtual machine** (Configura la nuova macchina virtuale) specificare le informazioni seguenti sull'host Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="078d6-133">La procedura guidata genera automaticamente la maggior parte delle informazioni, ma è possibile modificarle.</span><span class="sxs-lookup"><span data-stu-id="078d6-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="078d6-134">a.</span><span class="sxs-lookup"><span data-stu-id="078d6-134">a.</span></span> <span data-ttu-id="078d6-135">Nella casella **Name** (Nome) immettere un nome univoco per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="078d6-136">Questo non è lo stesso nome immagine Docker specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="078d6-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="078d6-137">b.</span><span class="sxs-lookup"><span data-stu-id="078d6-137">b.</span></span> <span data-ttu-id="078d6-138">Nella casella **Subscription** (Sottoscrizione) immettere la sottoscrizione di Azure usata per l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="078d6-139">c.</span><span class="sxs-lookup"><span data-stu-id="078d6-139">c.</span></span> <span data-ttu-id="078d6-140">Nella casella **Region** (Area) immettere l'area geografica in cui si trova l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="078d6-141">d.</span><span class="sxs-lookup"><span data-stu-id="078d6-141">d.</span></span> <span data-ttu-id="078d6-142">Nella scheda **OS and Size** (Sistema operativo e dimensioni) seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="078d6-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="078d6-143">**Host OS** (Sistema operativo host): specificare il sistema operativo della macchina virtuale in cui è presente l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="078d6-144">**Size** (Dimensioni): immettere le dimensioni della macchina virtuale per l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="078d6-145">e.</span><span class="sxs-lookup"><span data-stu-id="078d6-145">e.</span></span> <span data-ttu-id="078d6-146">Nella scheda **Resource Group** (Gruppo di risorse) selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="078d6-147">**New resource group** (Nuovo gruppo di risorse): consente di creare un nuovo gruppo di risorse per l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="078d6-148">**Existing resource group** (Gruppo di risorse esistente): consente di specificare un gruppo di risorse dal proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="078d6-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="078d6-149">f.</span><span class="sxs-lookup"><span data-stu-id="078d6-149">f.</span></span> <span data-ttu-id="078d6-150">Nella scheda **Network** (Rete) selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="078d6-151">**New virtual network** (Nuova rete virtuale): consente di creare una nuova rete virtuale per l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="078d6-152">**Existing virtual network** (Rete virtuale esistente): consente di specificare una rete virtuale esistente dal proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="078d6-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="078d6-153">g.</span><span class="sxs-lookup"><span data-stu-id="078d6-153">g.</span></span> <span data-ttu-id="078d6-154">Nella scheda **Storage** (Archiviazione) selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="078d6-155">**New storage account** (Nuovo account di archiviazione): consente di creare un nuovo account di archiviazione per l'host.</span><span class="sxs-lookup"><span data-stu-id="078d6-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="078d6-156">**Existing storage account** (Account di archiviazione esistente): consente di specificare un account di archiviazione esistente dal proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="078d6-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="078d6-157">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="078d6-157">Click **Next**.</span></span>  
     <span data-ttu-id="078d6-158">Verrà visualizzata la finestra **Configure log in credentials and port settings** (Configurare le credenziali di accesso e le impostazioni delle porte).</span><span class="sxs-lookup"><span data-stu-id="078d6-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![Finestra Configure log in credentials and port settings (Configurare le credenziali di accesso e le impostazioni delle porte)][PUB05]

6. <span data-ttu-id="078d6-160">Selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-160">Select one of the following options:</span></span>

      * <span data-ttu-id="078d6-161">**Import credentials from Azure Key Vault** (Importa credenziali da Azure Key Vault): consente di specificare un set di credenziali salvato in precedenza e archiviato nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="078d6-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="078d6-162">Un insieme di credenziali delle chiavi di Azure creato con un'entità servizio o un account specifico non è automaticamente accessibile da un altro account o entità servizio che condivide la sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="078d6-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="078d6-163">Per consentire a un altro account oppure a un'altra entità servizio di usare l'insieme di credenziali delle chiavi, è necessario usare il portale di Azure per aggiungere l'account o l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="078d6-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="078d6-164">**New log in credentials** (Nuove credenziali di accesso): consente di creare un nuovo set di credenziali di accesso.</span><span class="sxs-lookup"><span data-stu-id="078d6-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="078d6-165">Se si seleziona questa opzione, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="078d6-165">If you select this option, do the following:</span></span>

        <span data-ttu-id="078d6-166">a.</span><span class="sxs-lookup"><span data-stu-id="078d6-166">a.</span></span> <span data-ttu-id="078d6-167">Nella scheda **VM Credentials** (Credenziali VM) specificare le informazioni seguenti per le credenziali di accesso alla macchina virtuale dell'host Docker: \* **Username** (Nome utente): nome utente delle credenziali di accesso alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="078d6-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host: \* **Username**: Enter the username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="078d6-168">\* **Password** e **Confirm** (Conferma): consente di specificare la password per accedere alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="078d6-168">\* **Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="078d6-169">\* **SSH**: immettere le impostazioni Secure Shell (SSH) per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-169">\* **SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="078d6-170">È possibile selezionare una delle opzioni seguenti: **None** (Nessuna): specifica che la macchina virtuale non consente le connessioni SSH.</span><span class="sxs-lookup"><span data-stu-id="078d6-170">You can select one of the following options: \* **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="078d6-171">\* **Auto-generate** (Genera automaticamente): crea automaticamente le impostazioni necessarie per la connessione tramite SSH.</span><span class="sxs-lookup"><span data-stu-id="078d6-171">\* **Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="078d6-172">\* **Import from directory** (Importa da directory): consente di specificare una directory che contiene un set di impostazioni SSH salvate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="078d6-172">\* **Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="078d6-173">La directory deve contenere i due file seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-173">The directory must contain the following two files:</span></span>
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        <span data-ttu-id="078d6-174">b.</span><span class="sxs-lookup"><span data-stu-id="078d6-174">b.</span></span> <span data-ttu-id="078d6-175">Nella scheda **Docker Daemon Access** (Accesso daemon Docker) specificare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-175">On the **Docker Daemon Access** tab, provide the following information:</span></span>

          ![Creare un host Docker][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="078d6-177">Dopo aver immesso le informazioni richieste, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="078d6-177">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="078d6-178">Verrà visualizzata nuovamente la procedura guidata **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure).</span><span class="sxs-lookup"><span data-stu-id="078d6-178">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB07]

8. <span data-ttu-id="078d6-180">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="078d6-180">Click **Next**.</span></span>  
    <span data-ttu-id="078d6-181">Verrà visualizzata la finestra **Configure the Docker container to be created** (Configurare il contenitore Docker da creare).</span><span class="sxs-lookup"><span data-stu-id="078d6-181">The **Configure the Docker container to be created** window opens.</span></span>

   ![Finestra Configure the Docker container to be created (Configurare il contenitore Docker da creare)][PUB08]

9. <span data-ttu-id="078d6-183">Nella finestra **Configure the Docker container to be created** (Configurare il contenitore Docker da creare) specificare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-183">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="078d6-184">a.</span><span class="sxs-lookup"><span data-stu-id="078d6-184">a.</span></span> <span data-ttu-id="078d6-185">Nella casella **Docker container name** (Nome contenitore Docker) immettere un nome univoco per il contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-185">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="078d6-186">b.</span><span class="sxs-lookup"><span data-stu-id="078d6-186">b.</span></span> <span data-ttu-id="078d6-187">Scegliere una delle immagini Docker seguenti:</span><span class="sxs-lookup"><span data-stu-id="078d6-187">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="078d6-188">**Predefined Docker image** (Immagine Docker predefinita): consente di specificare un'immagine preesistente in Azure.</span><span class="sxs-lookup"><span data-stu-id="078d6-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="078d6-189">L'elenco di immagini Docker in questa casella è costituito da diverse immagini per cui Azure Toolkit è stato configurato per l'applicazione automatica delle patch, in modo che l'elemento venga distribuito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="078d6-189">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="078d6-190">**Custom Dockerfile** (Dockerfile personalizzato): consente di specificare un Dockerfile salvato in precedenza nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="078d6-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="078d6-191">Questa è una funzionalità più avanzata per gli sviluppatori che intendono distribuire i propri Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="078d6-191">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="078d6-192">È tuttavia compito degli sviluppatori che usano questa opzione garantire che i propri Dockerfile vengano compilati correttamente.</span><span class="sxs-lookup"><span data-stu-id="078d6-192">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="078d6-193">Dato che Azure Toolkit non esegue la convalida del contenuto in un Dockerfile personalizzato, in caso di problemi con il Dockerfile la distribuzione può avere esito negativo.</span><span class="sxs-lookup"><span data-stu-id="078d6-193">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="078d6-194">Per Azure Toolkit il Dockerfile personalizzato deve contenere un elemento app Web e prova quindi ad aprire una connessione HTTP.</span><span class="sxs-lookup"><span data-stu-id="078d6-194">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="078d6-195">Pubblicando un diverso tipo di elemento, gli sviluppatori potrebbero ricevere errori non gravi dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="078d6-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="078d6-196">c.</span><span class="sxs-lookup"><span data-stu-id="078d6-196">c.</span></span> <span data-ttu-id="078d6-197">Nella casella **Port settings** (Impostazioni porta) specificare l'associazione univoca della porta TCP per il contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-197">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="078d6-198">Al termine della procedura, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="078d6-198">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="078d6-199">Azure Toolkit inizia a distribuire l'app Web in Azure in un contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="078d6-199">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="078d6-200">A meno che non sia stato configurato IntelliJ per la distribuzione in background, viene visualizzato l'indicatore di stato **Deploying to Azure** (Distribuzione in Azure).</span><span class="sxs-lookup"><span data-stu-id="078d6-200">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![Indicatore di stato della distribuzione][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="078d6-202">Altre informazioni sulla creazione di elementi</span><span class="sxs-lookup"><span data-stu-id="078d6-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="078d6-203">Per creare un elemento pronto per la distribuzione, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="078d6-203">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="078d6-204">Aprire il progetto dell'app Web in IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="078d6-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="078d6-205">Fare clic su **File** e quindi fare clic su **Project Structure** (Struttura del progetto).</span><span class="sxs-lookup"><span data-stu-id="078d6-205">Click **File**, and then click **Project Structure**.</span></span>

   ![Comando Project Structure (Struttura del progetto)][ART01]

3. <span data-ttu-id="078d6-207">Per aggiungere un elemento, fare clic sul simbolo più di colore verde (**+**) e quindi scegliere **Web Application: Archive** (Applicazione Web: archivio).</span><span class="sxs-lookup"><span data-stu-id="078d6-207">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![Comando "Web Application: Archive" (Applicazione Web: archivio)][ART02]

4. <span data-ttu-id="078d6-209">Nella casella **Name** (Nome) immettere un nome per l'elemento, senza l'estensione *war*, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="078d6-209">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![Casella Name (Nome) dell'elemento][ART03]

<span data-ttu-id="078d6-211">Per altre informazioni sulla creazione di elementi in IntelliJ, vedere [Configuring Artifacts] (Configurazione di elementi) sul sito Web JetBrains.</span><span class="sxs-lookup"><span data-stu-id="078d6-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="078d6-212">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="078d6-212">Next steps</span></span>

<span data-ttu-id="078d6-213">Per altre risorse per Docker, vedere il [sito Web Docker] ufficiale.</span><span class="sxs-lookup"><span data-stu-id="078d6-213">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[sito Web Docker]: https://www.docker.com/
[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html (Configurazione di elementi)

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
