---
title: Creare un'app Web di base di Azure con Eclipse
description: Questa esercitazione descrive come usare il toolkit di Azure per Eclipse per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: dcab31ad99fb79a91374d95c2b8b0d9d10346f5f
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="b77ef-103">Creare un'app Web di base di Azure con Eclipse</span><span class="sxs-lookup"><span data-stu-id="b77ef-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="b77ef-104">Questa esercitazione descrive come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Azure Toolkit per Eclipse].</span><span class="sxs-lookup"><span data-stu-id="b77ef-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a Web App by using the [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="b77ef-105">Per semplicità è riportato un esempio JSP di base, ma è possibile adottare una procedura simile anche per un servlet Java, per quanto riguarda la distribuzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="b77ef-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="b77ef-106">Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b77ef-106">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Anteprima dell'app Hello World][01]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-create-a-hello-world-application"></a><span data-ttu-id="b77ef-108">Creare un'applicazione Hello World JSP</span><span class="sxs-lookup"><span data-stu-id="b77ef-108">To create a Hello World application</span></span>
<span data-ttu-id="b77ef-109">Creare innanzitutto un progetto Java.</span><span class="sxs-lookup"><span data-stu-id="b77ef-109">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="b77ef-110">Avviare Eclipse e nel menu fare clic su **File**, **New** e quindi su **Dynamic Web Project**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-110">Start Eclipse, and at the menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="b77ef-111">Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...**, espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-111">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

2. <span data-ttu-id="b77ef-112">Ai fini di questa esercitazione, denominare il progetto **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-112">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="b77ef-113">L'aspetto della schermata sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b77ef-113">Your screen will appear similar to the following:</span></span>
   
   ![Creazione di un nuovo progetto Web dinamico][02]

3. <span data-ttu-id="b77ef-115">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-115">Click **Finish**.</span></span>

4. <span data-ttu-id="b77ef-116">Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse espandere **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-116">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="b77ef-117">Fare clic con il pulsante destro del mouse su **WebContent**, scegliere **New** e quindi fare clic su **JSP File**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-117">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

5. <span data-ttu-id="b77ef-118">Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="b77ef-118">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

6. <span data-ttu-id="b77ef-119">Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html)** (Nuovo file JSP - html), quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="b77ef-119">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

7. <span data-ttu-id="b77ef-120">Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="b77ef-120">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="b77ef-121">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="b77ef-121">within the existing `<body>` element.</span></span> <span data-ttu-id="b77ef-122">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b77ef-122">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="b77ef-123">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="b77ef-123">Save index.jsp.</span></span>

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a><span data-ttu-id="b77ef-124">Per distribuire l'applicazione in un contenitore di app Web di Azure</span><span class="sxs-lookup"><span data-stu-id="b77ef-124">To deploy your application to an Azure Web App Container</span></span>
<span data-ttu-id="b77ef-125">Esistono diversi modi con cui è possibile distribuire un'applicazione Web Java in Azure.</span><span class="sxs-lookup"><span data-stu-id="b77ef-125">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="b77ef-126">Questa esercitazione descrive uno dei modi più semplici: l'applicazione viene distribuita in un contenitore di app Web di Azure senza richiedere tipi di progetto specifici o strumenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="b77ef-126">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="b77ef-127">JDK e il software del contenitore Web vengono forniti automaticamente da Azure senza la necessità di eseguire alcun caricamento. È sufficiente disporre dell'app Web Java.</span><span class="sxs-lookup"><span data-stu-id="b77ef-127">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="b77ef-128">Di conseguenza, il processo di pubblicazione per l'applicazione richiederà alcuni secondi, non minuti.</span><span class="sxs-lookup"><span data-stu-id="b77ef-128">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="b77ef-129">In Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-129">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>

2. <span data-ttu-id="b77ef-130">Dal menu di scelta rapida selezionare **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come App Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="b77ef-130">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
   ![Publish as Azure Web App][03]
   
   <span data-ttu-id="b77ef-132">In alternativa, mentre in Project Explorer (Esplora progetti) è selezionato il progetto di applicazione Web, è possibile fare clic sul pulsante a discesa **Publish** (Pubblica) nella barra degli strumenti e selezionare **Publish as Azure Web App** (Pubblica come App Web di Azure) da qui:</span><span class="sxs-lookup"><span data-stu-id="b77ef-132">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
   ![Publish as Azure Web App][14]

3. <span data-ttu-id="b77ef-134">Se non si è già eseguito l'accesso ad Azure da Eclipse, verrà richiesto di accedere all'account Azure:</span><span class="sxs-lookup"><span data-stu-id="b77ef-134">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
   ![Finestra di dialogo di accesso di Azure][04]
   
   <span data-ttu-id="b77ef-136">Se si hanno più account Azure, durante il processo di accesso alcune richieste, all'apparenza identiche, possono essere visualizzate più volte, ognuna per un account diverso.</span><span class="sxs-lookup"><span data-stu-id="b77ef-136">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="b77ef-137">In questo caso, continuare a seguire le istruzioni di accesso.</span><span class="sxs-lookup"><span data-stu-id="b77ef-137">When this happens, continue following the sign in instructions.</span></span>

4. <span data-ttu-id="b77ef-138">Dopo aver eseguito l'accesso all'account Azure, nella finestra di dialogo **Manage Subscriptions** (Gestisci sottoscrizioni) viene visualizzato un elenco delle sottoscrizioni associate alle credenziali usate.</span><span class="sxs-lookup"><span data-stu-id="b77ef-138">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="b77ef-139">Se sono elencate più sottoscrizioni e se ne vogliono usare solo alcune, è possibile deselezionare le sottoscrizioni che non si intende usare.</span><span class="sxs-lookup"><span data-stu-id="b77ef-139">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="b77ef-140">Dopo aver selezionato le sottoscrizioni, fare clic su **Close**(Chiudi).</span><span class="sxs-lookup"><span data-stu-id="b77ef-140">When you have selected your subscriptions, click **Close**.</span></span>
   
   ![Finestra di dialogo Gestisci sottoscrizioni][05]

5. <span data-ttu-id="b77ef-142">Nella finestra di dialogo **Deploy to Azure Web App Container** (Distribuisci in un contenitore app Web di Azure) sono visualizzati tutti i contenitori di app Web creati in precedenza. Se non è stato creato alcun contenitore, l'elenco appare vuoto.</span><span class="sxs-lookup"><span data-stu-id="b77ef-142">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][06]

6. <span data-ttu-id="b77ef-144">Se non è stato creato alcun contenitore di app Web di Azure in precedenza o se si desidera pubblicare l'applicazione in un nuovo contenitore, attenersi alla procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="b77ef-144">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="b77ef-145">In caso contrario, selezionare un contenitore di app Web esistente e andare al passaggio 7.</span><span class="sxs-lookup"><span data-stu-id="b77ef-145">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   <span data-ttu-id="b77ef-146">a.</span><span class="sxs-lookup"><span data-stu-id="b77ef-146">a.</span></span> <span data-ttu-id="b77ef-147">Fare clic su **Nuovo**</span><span class="sxs-lookup"><span data-stu-id="b77ef-147">Click **New...**</span></span>
      
      ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][15]

   <span data-ttu-id="b77ef-149">b.</span><span class="sxs-lookup"><span data-stu-id="b77ef-149">b.</span></span> <span data-ttu-id="b77ef-150">Verrà visualizzata la finestra di dialogo **New Web App Container** (Nuovo contenitore App Web):</span><span class="sxs-lookup"><span data-stu-id="b77ef-150">The **New Web App Container** dialog box will be displayed:</span></span>
      
      ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][07a]

   <span data-ttu-id="b77ef-152">c.</span><span class="sxs-lookup"><span data-stu-id="b77ef-152">c.</span></span> <span data-ttu-id="b77ef-153">In **DNS Label** (Etichetta DNS) specificare un'etichetta per il contenitore di app Web. Questa sarà l'etichetta DNS foglia dell'URL dell'host per l'applicazione Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="b77ef-153">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="b77ef-154">Si noti che il nome deve essere disponibile e conforme ai requisiti di denominazione delle app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="b77ef-154">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>

   <span data-ttu-id="b77ef-155">d.</span><span class="sxs-lookup"><span data-stu-id="b77ef-155">d.</span></span> <span data-ttu-id="b77ef-156">Nel menu a discesa **Contenitore Web** selezionare il software appropriato per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b77ef-156">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="b77ef-157">Attualmente è possibile scegliere fra Tomcat 8, Tomcat 7 o Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="b77ef-157">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="b77ef-158">Una distribuzione recente del software selezionato verrà fornita da Azure e sarà eseguita in una distribuzione recente di JDK 8 creata da Oracle e fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="b77ef-158">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="b77ef-159">e.</span><span class="sxs-lookup"><span data-stu-id="b77ef-159">e.</span></span> <span data-ttu-id="b77ef-160">Nel menu a discesa **Subscription** (Sottoscrizione) selezionare la sottoscrizione che si vuole usare per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="b77ef-160">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="b77ef-161">f.</span><span class="sxs-lookup"><span data-stu-id="b77ef-161">f.</span></span> <span data-ttu-id="b77ef-162">Nel menu a discesa **Resource Group** (Gruppo di risorse) selezionare il gruppo di risorse a cui si vuole associare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="b77ef-162">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="b77ef-163">I gruppi di risorse di Azure consentono di raggruppare le risorse correlate in modo che, ad esempio, possano essere eliminate insieme.</span><span class="sxs-lookup"><span data-stu-id="b77ef-163">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="b77ef-164">È possibile selezionare un gruppo di risorse esistente, se presente, e andare al passaggio g seguente o usare questa procedura per creare un nuovo gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="b77ef-164">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="b77ef-165">Fare clic su **Nuovo**</span><span class="sxs-lookup"><span data-stu-id="b77ef-165">Click **New...**</span></span>
      * <span data-ttu-id="b77ef-166">Verrà visualizzata la finestra di dialogo **Nuovo gruppo di risorse** :</span><span class="sxs-lookup"><span data-stu-id="b77ef-166">The **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Finestra di dialogo Nuovo gruppo di risorse][08]
      * <span data-ttu-id="b77ef-168">Nella casella di testo **Nome** specificare un nome per il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="b77ef-168">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="b77ef-169">Nel menu a discesa **Region** (Area) selezionare il percorso del data center di Azure appropriato per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="b77ef-169">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="b77ef-170">FACOLTATIVO: per impostazione predefinita, una distribuzione recente di Java 8 verrà distribuita da Azure automaticamente nel contenitore di app web come JVM.</span><span class="sxs-lookup"><span data-stu-id="b77ef-170">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="b77ef-171">Tuttavia, è possibile specificare una versione e una distribuzione di JVM diversa, se richiesto dall'app Web.</span><span class="sxs-lookup"><span data-stu-id="b77ef-171">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="b77ef-172">Per specificare la versione JDK per l'app Web specifica, fare clic sulla scheda **JDK** e selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b77ef-172">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
         * <span data-ttu-id="b77ef-173">**Deploy the default JDK offered by Azure Web Apps service**(Distribuisci JDK predefinito fornito dal servizio app Web di Azure): questa opzione usa una distribuzione recente di Java 8.</span><span class="sxs-lookup"><span data-stu-id="b77ef-173">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
         * <span data-ttu-id="b77ef-174">**Deploy a 3rd party JDK available on Azure**(Distribuisci JDK di terze parti disponibile in Azure): questa opzione consente di scegliere dall'elenco di JDK forniti da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b77ef-174">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
         * <span data-ttu-id="b77ef-175">**Deploy my own JDK from this download location**(Distribuisci JDK personalizzato da questo percorso di download): questa opzione consente di specificare una distribuzione JDK personalizzata, che deve essere compressa come file ZIP e caricata in un percorso di download disponibile pubblicamente o in un account di Archiviazione di Azure per cui si dispone dell'accesso.</span><span class="sxs-lookup"><span data-stu-id="b77ef-175">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
         ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][07b]

   <span data-ttu-id="b77ef-177">g.</span><span class="sxs-lookup"><span data-stu-id="b77ef-177">g.</span></span> <span data-ttu-id="b77ef-178">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-178">Click **OK**.</span></span>

   <span data-ttu-id="b77ef-179">h.</span><span class="sxs-lookup"><span data-stu-id="b77ef-179">h.</span></span> <span data-ttu-id="b77ef-180">Il menu a discesa **Piano di servizio app** elenca i piani di servizio app associati al gruppo di risorse selezionato.</span><span class="sxs-lookup"><span data-stu-id="b77ef-180">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="b77ef-181">I piani di servizio app specificano informazioni come il percorso dell'app Web, il piano tariffario e le dimensioni dell'istanza di calcolo.</span><span class="sxs-lookup"><span data-stu-id="b77ef-181">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="b77ef-182">È possibile usare un singolo piano di servizio app per più app Web. Per questo motivo viene gestito separatamente da una distribuzione di app Web specifica.</span><span class="sxs-lookup"><span data-stu-id="b77ef-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:
      
      * <span data-ttu-id="b77ef-183">Fare clic su **Nuovo**</span><span class="sxs-lookup"><span data-stu-id="b77ef-183">Click **New...**</span></span>
      * <span data-ttu-id="b77ef-184">Verrà visualizzata la finestra di dialogo **Nuovo piano di servizio app** :</span><span class="sxs-lookup"><span data-stu-id="b77ef-184">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Finestra di dialogo New App Service Plan (Nuovo piano di servizio app)][09]
      * <span data-ttu-id="b77ef-186">Nella casella di testo **Nome** specificare un nome per il nuovo piano di servizio app.</span><span class="sxs-lookup"><span data-stu-id="b77ef-186">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="b77ef-187">Nel menu a discesa **Località** selezionare la località del data center di Azure appropriata per il piano.</span><span class="sxs-lookup"><span data-stu-id="b77ef-187">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="b77ef-188">Nel menu a discesa **Piano tariffario** selezionare la tariffa appropriata per il piano.</span><span class="sxs-lookup"><span data-stu-id="b77ef-188">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="b77ef-189">Ai fini del test è possibile scegliere **Gratuito**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-189">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="b77ef-190">Nel menu a discesa **Dimensioni istanza** selezionare le dimensioni dell'istanza appropriate per il piano.</span><span class="sxs-lookup"><span data-stu-id="b77ef-190">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="b77ef-191">Ai fini del test è possibile scegliere **Piccolo**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-191">For testing purposes you can choose **Small**.</span></span>

   <span data-ttu-id="b77ef-192">i.</span><span class="sxs-lookup"><span data-stu-id="b77ef-192">i.</span></span> <span data-ttu-id="b77ef-193">Dopo aver completato tutti i passaggi precedenti, la finestra di dialogo New Web App Container dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="b77ef-193">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][10]

   <span data-ttu-id="b77ef-195">j.</span><span class="sxs-lookup"><span data-stu-id="b77ef-195">j.</span></span> <span data-ttu-id="b77ef-196">Fare clic su **OK** per completare la creazione del nuovo contenitore di app Web.</span><span class="sxs-lookup"><span data-stu-id="b77ef-196">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="b77ef-197">Attendere alcuni secondi che venga aggiornato l'elenco dei contenitori di app Web. Il contenitore di app Web appena creato risulterà selezionato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="b77ef-197">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

7. <span data-ttu-id="b77ef-198">A questo punto si è pronti completare la distribuzione iniziale dell'app Web in Azure:</span><span class="sxs-lookup"><span data-stu-id="b77ef-198">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
   ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][11]
   
   <span data-ttu-id="b77ef-200">Fare clic su **OK** per distribuire l'applicazione Java nel contenitore di App Web selezionato.</span><span class="sxs-lookup"><span data-stu-id="b77ef-200">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
   <span data-ttu-id="b77ef-201">Per impostazione predefinita, l'applicazione verrà distribuita come sottodirectory del server applicazioni.</span><span class="sxs-lookup"><span data-stu-id="b77ef-201">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="b77ef-202">Se si vuole distribuire l'applicazione come applicazione radice, selezionare la casella di controllo **Deploy to root** (Distribuisci a radice) prima di fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-202">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>

8. <span data-ttu-id="b77ef-203">Verrà quindi aperta la visualizzazione **Azure Activity Log** (Log attività di Azure) in cui è indicato lo stato della distribuzione dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="b77ef-203">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![Azure Activity Log][12]
   
   <span data-ttu-id="b77ef-205">Il processo di distribuzione dell'app Web in Azure dovrebbe richiedere solo alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="b77ef-205">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="b77ef-206">Quando l'applicazione è pronta, viene visualizzato un collegamento denominato **Published** in the **Status** (Stato).</span><span class="sxs-lookup"><span data-stu-id="b77ef-206">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="b77ef-207">Quando si fa clic sul collegamento, viene visualizzata la home page dell'app Web distribuita.</span><span class="sxs-lookup"><span data-stu-id="b77ef-207">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="b77ef-208">Aggiornamento dell'app Web</span><span class="sxs-lookup"><span data-stu-id="b77ef-208">Updating your web app</span></span>
<span data-ttu-id="b77ef-209">L'aggiornamento di un'app Web di Azure in esecuzione è un processo semplice e rapido. Sono disponibili due opzioni per l'aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="b77ef-209">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="b77ef-210">È possibile aggiornare la distribuzione di un'app Web Java esistente.</span><span class="sxs-lookup"><span data-stu-id="b77ef-210">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="b77ef-211">È possibile pubblicare un'applicazione Java aggiuntiva nello stesso contenitore di app Web.</span><span class="sxs-lookup"><span data-stu-id="b77ef-211">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="b77ef-212">In entrambi i casi, il processo è identico e richiede solo pochi secondi:</span><span class="sxs-lookup"><span data-stu-id="b77ef-212">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="b77ef-213">In Project Explorer in Eclipse, fare clic con il pulsante destro del mouse sull'applicazione Java che si desidera aggiornare o aggiungere a un contenitore di app Web esistente.</span><span class="sxs-lookup"><span data-stu-id="b77ef-213">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>

2. <span data-ttu-id="b77ef-214">Dal menu di scelta rapida visualizzato selezionare **Azure**, quindi **Publish as Azure Web App** (Pubblica come App Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="b77ef-214">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>

3. <span data-ttu-id="b77ef-215">Poiché è già stato effettuato l'accesso in precedenza, verrà visualizzato un elenco dei contenitori di app Web esistenti.</span><span class="sxs-lookup"><span data-stu-id="b77ef-215">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="b77ef-216">Selezionare il contenitore in cui si vuole pubblicare o ripubblicare l'applicazione Java e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-216">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="b77ef-217">Pochi secondi dopo nella visualizzazione **Azure Activity Log** (Log attività di Azure) la distribuzione aggiornata apparirà come **Published** (Pubblicata) e sarà possibile verificare l'applicazione aggiornata in un browser Web.</span><span class="sxs-lookup"><span data-stu-id="b77ef-217">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="b77ef-218">Avvio, arresto o riavvio di un'app Web esistente</span><span class="sxs-lookup"><span data-stu-id="b77ef-218">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="b77ef-219">Per avviare o arrestare un contenitore di app Web di Azure esistente, incluse tutte le applicazioni Java in esso distribuite, è possibile usare la visualizzazione **Azure Explorer** .</span><span class="sxs-lookup"><span data-stu-id="b77ef-219">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="b77ef-220">Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **Window** (Finestra) in Eclipse, quindi su **Show View** (Mostra visualizzazione), **Other** (Altro), **Azure** e infine su **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b77ef-220">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="b77ef-221">Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="b77ef-221">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="b77ef-222">Quando appare la visualizzazione **Azure Explorer** , per avviare o arrestare l'app Web seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="b77ef-222">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="b77ef-223">Espandere il nodo **Azure** .</span><span class="sxs-lookup"><span data-stu-id="b77ef-223">Expand the **Azure** node.</span></span>

2. <span data-ttu-id="b77ef-224">Espandere il nodo **Web Apps** (App Web).</span><span class="sxs-lookup"><span data-stu-id="b77ef-224">Expand the **Web Apps** node.</span></span> 

3. <span data-ttu-id="b77ef-225">Fare clic con il pulsante destro del mouse sull'app Web desiderata.</span><span class="sxs-lookup"><span data-stu-id="b77ef-225">Right-click the desired Web App.</span></span>

4. <span data-ttu-id="b77ef-226">Quando viene visualizzato il menu di scelta rapida, fare clic su **Start** (Avvia), **Stop** (Arresta) o **Restart** (Riavvia).</span><span class="sxs-lookup"><span data-stu-id="b77ef-226">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="b77ef-227">Si noti che le opzioni di menu sono sensibili al contesto, quindi è possibile arrestare solo un'app Web in esecuzione o avviare un'app Web al momento non in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b77ef-227">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![Arresto di un'app Web esistente][13]

## <a name="next-steps"></a><span data-ttu-id="b77ef-229">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b77ef-229">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="b77ef-230">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="b77ef-230">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit per Eclipse]: azure-toolkit-for-eclipse.md
[Panoramica delle App Web]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
