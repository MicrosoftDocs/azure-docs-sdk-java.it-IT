---
title: Pubblicare un contenitore Docker usando Azure Toolkit for Eclipse
description: Informazioni su come pubblicare un'app Web in Microsoft Azure come contenitore Docker usando il Toolkit di Azure per Eclipse.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: aec3682cc14204ca2b395d4b851db7bb68cb8728
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954772"
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="58228-103">Pubblicare un'app Web come contenitore Docker usando il Toolkit di Azure per Eclipse</span><span class="sxs-lookup"><span data-stu-id="58228-103">Publish a web app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="58228-104">I contenitori Docker sono un metodo molto diffuso per la distribuzione di applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="58228-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="58228-105">L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server.</span><span class="sxs-lookup"><span data-stu-id="58228-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="58228-106">Il Toolkit di Azure per Eclipse semplifica questo processo per gli sviluppatori Java aggiungendo la funzionalità *Publish as Docker Container* (Pubblica come contenitore Docker) per la distribuzione in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="58228-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="58228-107">Questo articolo illustra la procedura da seguire per pubblicare le applicazioni in Azure come contenitori Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="58228-108">Altre informazioni su Docker sono disponibili nel [sito Web Docker].</span><span class="sxs-lookup"><span data-stu-id="58228-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="58228-109">Pubblicare l'app Web in Azure usando un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="58228-109">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="58228-110">Aprire il progetto dell'applicazione Web in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="58228-110">Open your web app project in Eclipse.</span></span>

1. <span data-ttu-id="58228-111">Per avviare la procedura guidata **Publish as Docker Container** (Pubblica come contenitore Docker), eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-111">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="58228-112">Nella vista **Navigator** (Strumento di navigazione) fare clic con il pulsante destro del mouse sul progetto, fare clic su **Azure** e quindi fare clic su **Publish as Docker Container** (Pubblica come contenitore Docker).</span><span class="sxs-lookup"><span data-stu-id="58228-112">In the **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![Comando Publish as Docker Container (Pubblica come contenitore Docker) nella vista Navigator (Strumento di navigazione)][PUB01]

   * <span data-ttu-id="58228-114">Sulla barra degli strumenti di Eclipse fare clic sul pulsante **Publish** (Pubblica) e quindi fare clic su **Publish as Docker Container** (Pubblica come contenitore Docker).</span><span class="sxs-lookup"><span data-stu-id="58228-114">On the Eclipse toolbar, click the **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Comando Publish as Docker Container (Pubblica come contenitore Docker) sulla barra degli strumenti di Eclipse][PUB02]
      
   <span data-ttu-id="58228-116">Verrà visualizzata la procedura guidata **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure).</span><span class="sxs-lookup"><span data-stu-id="58228-116">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB03]

1. <span data-ttu-id="58228-118">Nella finestra **Type an image name, select the artifact's path and check a Docker host to be used** (Digitare un nome immagine, scegliere il percorso dell'elemento e selezionare un host Docker da usare) seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="58228-118">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span>

   <span data-ttu-id="58228-119">a.</span><span class="sxs-lookup"><span data-stu-id="58228-119">a.</span></span> <span data-ttu-id="58228-120">Nella casella **Docker image name** (Nome immagine Docker) immettere un nome univoco per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-120">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="58228-121">La procedura guidata crea automaticamente un nome, ma è possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="58228-121">(The wizard automatically creates a name, but you can modify it.)</span></span>

   <span data-ttu-id="58228-122">b.</span><span class="sxs-lookup"><span data-stu-id="58228-122">b.</span></span> <span data-ttu-id="58228-123">Nell'area **Hosts** (Host) vengono visualizzati tutti gli host Docker già creati.</span><span class="sxs-lookup"><span data-stu-id="58228-123">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="58228-124">Eseguire una di queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="58228-124">Do either of the following:</span></span>

   * <span data-ttu-id="58228-125">Se è disponibile un host Docker esistente, è possibile distribuirvi l'app Web.</span><span class="sxs-lookup"><span data-stu-id="58228-125">If you have an existing Docker host, you can deploy your web app to it.</span></span>
   * <span data-ttu-id="58228-126">Per creare un nuovo host Docker, fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="58228-126">To create a new Docker host, click **Add**.</span></span>  
      
      <span data-ttu-id="58228-127">Verrà visualizzata la finestra di dialogo **Create Docker Host** (Crea host Docker).</span><span class="sxs-lookup"><span data-stu-id="58228-127">The **Create Docker Host** dialog box opens.</span></span>

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB04a]

1. <span data-ttu-id="58228-129">Nella finestra **Configure the new virtual machine** (Configura la nuova macchina virtuale) specificare le opzioni seguenti per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-129">In the **Configure the new virtual machine** window, specify the following options for your Docker host.</span></span> <span data-ttu-id="58228-130">La procedura guidata genera automaticamente la maggior parte delle opzioni, ma è possibile modificarle.</span><span class="sxs-lookup"><span data-stu-id="58228-130">(The wizard automatically generates most of the options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="58228-131">a.</span><span class="sxs-lookup"><span data-stu-id="58228-131">a.</span></span> <span data-ttu-id="58228-132">**Name** (Nome): immettere un nome univoco per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-132">**Name**: Enter a unique name for the Docker host.</span></span> <span data-ttu-id="58228-133">Questo non è lo stesso nome immagine Docker specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="58228-133">(It is not the same as the Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="58228-134">b.</span><span class="sxs-lookup"><span data-stu-id="58228-134">b.</span></span> <span data-ttu-id="58228-135">**Subscription** (Sottoscrizione): immettere la sottoscrizione di Azure usata per l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-135">**Subscription**: Enter the Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="58228-136">c.</span><span class="sxs-lookup"><span data-stu-id="58228-136">c.</span></span> <span data-ttu-id="58228-137">**Region** (Area): immettere l'area geografica in cui si trova l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-137">**Region**: Enter the geographical region where your host is located.</span></span>

   <span data-ttu-id="58228-138">d.</span><span class="sxs-lookup"><span data-stu-id="58228-138">d.</span></span> <span data-ttu-id="58228-139">Nella scheda **Host OS and Size** (Sistema operativo e dimensioni host) specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-139">On the **Host OS and Size** tab:</span></span> 
   * <span data-ttu-id="58228-140">**Host OS** (Sistema operativo host): immettere il sistema operativo della macchina virtuale in cui è presente l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-140">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span>
   * <span data-ttu-id="58228-141">**Size** (Dimensioni): immettere le dimensioni della macchina virtuale per l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-141">**Size**: Enter the virtual-machine size for your host.</span></span>

   <span data-ttu-id="58228-142">e.</span><span class="sxs-lookup"><span data-stu-id="58228-142">e.</span></span> <span data-ttu-id="58228-143">Nella scheda **Resource Group** (Gruppo di risorse) specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-143">On the **Resource Group** tab:</span></span> 
   * <span data-ttu-id="58228-144">**New resource group** (Nuovo gruppo di risorse): creare un nuovo gruppo di risorse per l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-144">**New resource group**: Create a new resource group for your host.</span></span>
   * <span data-ttu-id="58228-145">**Existing resource group** (Gruppo di risorse esistente): immettere un gruppo di risorse dal proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="58228-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="58228-146">f.</span><span class="sxs-lookup"><span data-stu-id="58228-146">f.</span></span> <span data-ttu-id="58228-147">Nella scheda **Network** (Rete) specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-147">On the **Network** tab:</span></span> 
   * <span data-ttu-id="58228-148">**New virtual network** (Nuova rete virtuale): creare una nuova rete virtuale per l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-148">**New virtual network**: Create a new virtual network for your host.</span></span>
   * <span data-ttu-id="58228-149">**Existing virtual network** (Rete virtuale esistente): immettere una rete virtuale esistente dal proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="58228-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="58228-150">g.</span><span class="sxs-lookup"><span data-stu-id="58228-150">g.</span></span> <span data-ttu-id="58228-151">Nella scheda **Storage** (Archiviazione) specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-151">On the **Storage** tab:</span></span> 
   * <span data-ttu-id="58228-152">**New storage account** (Nuovo account di archiviazione): creare un nuovo account di archiviazione per l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-152">**New storage account**: Create a new storage account for your host.</span></span>
   * <span data-ttu-id="58228-153">**Existing storage account** (Account di archiviazione esistente): immettere un account di archiviazione esistente dal proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="58228-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

1. <span data-ttu-id="58228-154">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="58228-154">Click **Next**.</span></span>

1. <span data-ttu-id="58228-155">Nella finestra **Configure log in credentials and port settings** (Configurare le credenziali di accesso e le impostazioni delle porte) selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-155">In the **Configure log in credentials and port settings** window, select one of the following options:</span></span>

   * <span data-ttu-id="58228-156">**Import credentials from Azure Key Vault** (Importa credenziali da Azure Key Vault): specifica un set di credenziali salvato in precedenza e archiviato nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="58228-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span> 

   >[!NOTE]
   ><span data-ttu-id="58228-157">Un Azure Key Vault creato con un'entità servizio o un account specifico non è automaticamente accessibile da un altro account o entità servizio che condivide la sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="58228-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="58228-158">Per consentire a un altro account oppure a un'altra entità servizio di usare il Key Vault, è necessario usare il portale di Azure per aggiungere l'account o l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="58228-158">To allow another account or service principal to use the Key Vault, you must use the Azure portal to add the account or service principal.</span></span>
   >

   * <span data-ttu-id="58228-159">**New log in credentials** (Nuove credenziali di accesso): crea un nuovo set di credenziali di accesso.</span><span class="sxs-lookup"><span data-stu-id="58228-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="58228-160">Se si seleziona questa opzione, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="58228-160">If you select this option, do the following:</span></span> 
    
      * <span data-ttu-id="58228-161">Nella scheda **VM Credentials** (Credenziali VM) scegliere una delle opzioni seguenti per l'accesso a macchine virtuali dell'host di Docker:</span><span class="sxs-lookup"><span data-stu-id="58228-161">On the **VM Credentials** tab, choose one of the following options for the virtual-machine login credentials of your Docker host:</span></span> 

         * <span data-ttu-id="58228-162">**Username** (Nome utente): immettere il nome utente per le credenziali di accesso alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="58228-162">**Username**: Enter the username for your virtual machine login credentials.</span></span> 
         * <span data-ttu-id="58228-163">**Password** e **Confirm** (Conferma): immettere la password per le credenziali di accesso alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="58228-163">**Password** and **Confirm**: Enter the password for your virtual machine login credentials.</span></span> 
         * <span data-ttu-id="58228-164">**SSH**: immettere le impostazioni Secure Shell (SSH) per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-164">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="58228-165">È possibile scegliere tra le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-165">You can choose from the following options:</span></span> 
            * <span data-ttu-id="58228-166">**None** (Nessuna): specifica che la macchina virtuale non consentirà connessioni SSH.</span><span class="sxs-lookup"><span data-stu-id="58228-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span> 
            * <span data-ttu-id="58228-167">**Auto-generate** (Genera automaticamente): crea automaticamente le impostazioni necessarie per la connessione tramite SSH.</span><span class="sxs-lookup"><span data-stu-id="58228-167">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span> 
            * <span data-ttu-id="58228-168">**Import from directory** (Importa da directory): specifica una directory che contiene un set di impostazioni SSH salvate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="58228-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="58228-169">La directory deve contenere i due file seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-169">The directory must contain the following two files:</span></span> 
               * <span data-ttu-id="58228-170">*id_rsa*: contiene l'identificazione RSA per un utente.</span><span class="sxs-lookup"><span data-stu-id="58228-170">*id_rsa*: Contains the RSA identification for a user.</span></span> 
               * <span data-ttu-id="58228-171">*id_rsa.pub*: contiene la chiave pubblica RSA usata per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="58228-171">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span> 
        
         ![Creare un host Docker][PUB05]

      * <span data-ttu-id="58228-173">Nella scheda **Docker Daemon Credentials** (Credenziali daemon Docker) specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-173">On the **Docker Daemon Credentials** tab, specify the following options:</span></span> 

         * <span data-ttu-id="58228-174">**Docker Daemon port** (Porta Daemon Docker): immettere la porta TCP univoca per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-174">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span> 
         * <span data-ttu-id="58228-175">**Sicurezza TLS**: immettere le impostazioni Transport Layer Security per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-175">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="58228-176">È possibile scegliere tra le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-176">You can choose from the following options:</span></span> 
            * <span data-ttu-id="58228-177">**None** (Nessuna): specifica che la macchina virtuale non consentirà connessioni TLS.</span><span class="sxs-lookup"><span data-stu-id="58228-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span> 
            * <span data-ttu-id="58228-178">**Auto-generate** (Genera automaticamente): crea automaticamente le impostazioni necessarie per la connessione tramite TLS.</span><span class="sxs-lookup"><span data-stu-id="58228-178">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span> 
            * <span data-ttu-id="58228-179">**Import from directory** (Importa da directory): specifica una directory che contiene un set di impostazioni TLS salvate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="58228-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="58228-180">In particolare, la directory deve contenere i sei file seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-180">More specifically, the directory must contain the following six files:</span></span> 
               * <span data-ttu-id="58228-181">*ca.pem* e *ca-key.pem*: contengono il certificato e la chiave pubblica per l'Autorità di certificazione TLS.</span><span class="sxs-lookup"><span data-stu-id="58228-181">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span> 
               * <span data-ttu-id="58228-182">*cert.pem* e *key.pem*: contengono il certificato client e la chiave pubblica usati per l'autenticazione TLS.</span><span class="sxs-lookup"><span data-stu-id="58228-182">*cert.pem* and *key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span> 
               * <span data-ttu-id="58228-183">*server.pem* e *server-key.pem*: contengono il certificato server e la chiave pubblica per l'host.</span><span class="sxs-lookup"><span data-stu-id="58228-183">*server.pem* and *server-key.pem*: Contain the server certificate and public key for the host.</span></span> 

         ![Creare un host Docker][PUB06]

1. <span data-ttu-id="58228-185">Dopo aver immesso tutte le informazioni indicate in precedenza, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="58228-185">After you have entered all of the preceding information, click **Finish**.</span></span>

1. <span data-ttu-id="58228-186">Nella procedura guidata **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure) fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="58228-186">In the **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB07]

1. <span data-ttu-id="58228-188">Nella finestra **Configure the Docker container to be created** (Configurare il contenitore Docker da creare) eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-188">In the **Configure the Docker container to be created** window, do the following:</span></span>

   <span data-ttu-id="58228-189">a.</span><span class="sxs-lookup"><span data-stu-id="58228-189">a.</span></span> <span data-ttu-id="58228-190">Nella casella **Docker container name** (Nome contenitore Docker) immettere un nome univoco per il contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-190">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="58228-191">b.</span><span class="sxs-lookup"><span data-stu-id="58228-191">b.</span></span> <span data-ttu-id="58228-192">Scegliere una delle immagini Docker seguenti:</span><span class="sxs-lookup"><span data-stu-id="58228-192">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="58228-193">**Predefined Docker image** (Immagine Docker predefinita): specifica un'immagine preesistente in Azure.</span><span class="sxs-lookup"><span data-stu-id="58228-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span> 

      >[!NOTE]
      ><span data-ttu-id="58228-194">L'elenco di immagini Docker in questa casella è costituito da diverse immagini per cui il Toolkit di Azure è stato configurato per l'applicazione automatica delle patch, in modo che l'elemento venga distribuito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="58228-194">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span>
      >

      * <span data-ttu-id="58228-195">**Custom Dockerfile** (Dockerfile personalizzato): specifica un Dockerfile salvato in precedenza nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="58228-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

      >[!NOTE]
      ><span data-ttu-id="58228-196">Questa è una funzionalità più avanzata per gli sviluppatori che intendono distribuire i propri Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="58228-196">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="58228-197">È tuttavia compito degli sviluppatori che usano questa opzione garantire che i propri Dockerfile vengano compilati correttamente.</span><span class="sxs-lookup"><span data-stu-id="58228-197">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="58228-198">Il Toolkit di Azure non esegue la convalida del contenuto in un Dockerfile personalizzato e di conseguenza la distribuzione potrebbe non riuscire se nel Dockerfile si verificano problemi.</span><span class="sxs-lookup"><span data-stu-id="58228-198">The Azure Toolkit does not validate the content in a custom Dockerfile, so the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="58228-199">Inoltre, il Toolkit di Azure prevede che il Dockerfile personalizzato contenga un elemento app Web e prova quindi ad aprire una connessione HTTP.</span><span class="sxs-lookup"><span data-stu-id="58228-199">In addition, the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, and it will attempt to open an HTTP connection.</span></span> <span data-ttu-id="58228-200">Pubblicando un diverso tipo di elemento, gli sviluppatori potrebbero ricevere errori non gravi dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="58228-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>
      >

   <span data-ttu-id="58228-201">c.</span><span class="sxs-lookup"><span data-stu-id="58228-201">c.</span></span> <span data-ttu-id="58228-202">**Port settings** (Impostazioni porta): immettere il binding univoco della porta TCP per il contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-202">**Port settings**: Enter the unique TCP port binding for your Docker container.</span></span>

      ![Finestra Configure the Docker container to be created (Configurare il contenitore Docker da creare)][PUB08]

1. <span data-ttu-id="58228-204">Al termine della procedura, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="58228-204">After you have completed all of the preceding steps, click **Finish**.</span></span>

<span data-ttu-id="58228-205">Il Toolkit di Azure inizia a distribuire l'app Web in Azure in un contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="58228-205">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="58228-206">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="58228-206">Next steps</span></span>

<span data-ttu-id="58228-207">Per altre risorse per Docker, vedere il [sito Web Docker] ufficiale.</span><span class="sxs-lookup"><span data-stu-id="58228-207">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[sito Web Docker]: https://www.docker.com/
[Docker website]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png