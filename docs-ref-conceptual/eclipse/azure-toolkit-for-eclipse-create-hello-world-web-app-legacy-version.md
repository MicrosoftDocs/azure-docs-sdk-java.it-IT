---
title: ''
description: Questa esercitazione descrive come usare Azure Toolkit for Eclipse 3.0.6 (o versioni precedenti) per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 896e7eff389bc7d3ac119d315c50aae505a381da
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090804"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-eclipse"></a><span data-ttu-id="0f1b3-102">Creare un'app Web Hello World per Azure con il toolkit legacy per Eclipse</span><span class="sxs-lookup"><span data-stu-id="0f1b3-102">Create a Hello World web app for Azure using the legacy toolkit for Eclipse</span></span>

<span data-ttu-id="0f1b3-103">Questa esercitazione descrive come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Toolkit di Azure per Eclipse] 3.0.6 (o versioni precedenti).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-103">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using version 3.0.6 (or earlier) of the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="0f1b3-104">Per una versione di questo articolo che fa uso di [Azure Toolkit per IntelliJ], vedere [Creare un'app Web Hello World per Azure con IntelliJ][intellij-hello-world].</span><span class="sxs-lookup"><span data-stu-id="0f1b3-104">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="0f1b3-105">Azure Toolkit for Eclipse è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-105">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="0f1b3-106">Questo articolo descrive come creare un'app Web Hello World usando la versione 3.0.6 (o versioni precedenti) di Azure Toolkit for Eclipse.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-106">This article illustrates creating a Hello World web app by using version 3.0.6 (or earlier) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="0f1b3-107">Se si usa la versione 3.0.7 (o versioni successive) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con Eclipse][Updated Version].</span><span class="sxs-lookup"><span data-stu-id="0f1b3-107">If you are using the version 3.0.7 (or later) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse][Updated Version].</span></span>
>

<span data-ttu-id="0f1b3-108">Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-108">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Anteprima dell'app Hello World][01]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="0f1b3-110">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="0f1b3-110">Create a new web app project</span></span>

1. <span data-ttu-id="0f1b3-111">Avviare Eclipse e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for Eclipse][eclipse-sign-in-instructions].</span><span class="sxs-lookup"><span data-stu-id="0f1b3-111">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="0f1b3-112">Fare clic su **File**, **New** (Nuovo) e quindi su **Dynamic Web Project** (Progetto Web dinamico).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-112">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="0f1b3-113">Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...**, espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-113">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

2. <span data-ttu-id="0f1b3-114">Ai fini di questa esercitazione, denominare il progetto **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-114">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="0f1b3-115">L'aspetto della schermata sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-115">Your screen will appear similar to the following:</span></span>
   
   ![Creazione di un nuovo progetto Web dinamico][02]

3. <span data-ttu-id="0f1b3-117">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-117">Click **Finish**.</span></span>

4. <span data-ttu-id="0f1b3-118">Nella visualizzazione **Project Explorer** (Esplora progetti) di Eclipse espandere **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-118">Within Eclipse's **Project Explorer** view, expand **MyWebApp**.</span></span> <span data-ttu-id="0f1b3-119">Fare clic con il pulsante destro del mouse su **WebContent**, scegliere **New** e quindi fare clic su **JSP File**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-119">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

5. <span data-ttu-id="0f1b3-120">Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-120">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

6. <span data-ttu-id="0f1b3-121">Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html)** (Nuovo file JSP - html), quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-121">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

7. <span data-ttu-id="0f1b3-122">Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="0f1b3-122">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="0f1b3-123">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-123">within the existing `<body>` element.</span></span> <span data-ttu-id="0f1b3-124">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-124">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="0f1b3-125">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-125">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="0f1b3-126">Distribuire l'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="0f1b3-126">Deploy your web app to Azure</span></span>

<span data-ttu-id="0f1b3-127">Esistono diversi modi con cui è possibile distribuire un'applicazione Web Java in Azure.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-127">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="0f1b3-128">Questa esercitazione descrive uno dei modi più semplici: l'applicazione viene distribuita in un contenitore di app Web di Azure senza richiedere tipi di progetto specifici o strumenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-128">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="0f1b3-129">JDK e il software del contenitore Web vengono forniti automaticamente da Azure senza la necessità di eseguire alcun caricamento. È sufficiente disporre dell'app Web Java.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-129">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="0f1b3-130">Di conseguenza, il processo di pubblicazione per l'applicazione richiederà alcuni secondi, non minuti.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-130">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="0f1b3-131">In Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-131">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>

2. <span data-ttu-id="0f1b3-132">Dal menu di scelta rapida selezionare **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come App Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-132">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
   ![Publish as Azure Web App][03]
   
   <span data-ttu-id="0f1b3-134">In alternativa, mentre in Project Explorer (Esplora progetti) è selezionato il progetto di applicazione Web, è possibile fare clic sul pulsante a discesa **Publish** (Pubblica) nella barra degli strumenti e selezionare **Publish as Azure Web App** (Pubblica come App Web di Azure) da qui:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-134">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
   ![Publish as Azure Web App][14]

3. <span data-ttu-id="0f1b3-136">Se non si è già eseguito l'accesso ad Azure da Eclipse, verrà richiesto di accedere all'account Azure:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-136">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
   ![Finestra di dialogo di accesso di Azure][04]
   
   <span data-ttu-id="0f1b3-138">Se si hanno più account Azure, durante il processo di accesso alcune richieste, all'apparenza identiche, possono essere visualizzate più volte, ognuna per un account diverso.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-138">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="0f1b3-139">In questo caso, continuare a seguire le istruzioni di accesso.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-139">When this happens, continue following the sign in instructions.</span></span>

4. <span data-ttu-id="0f1b3-140">Dopo aver eseguito l'accesso all'account Azure, nella finestra di dialogo **Manage Subscriptions** (Gestisci sottoscrizioni) viene visualizzato un elenco delle sottoscrizioni associate alle credenziali usate.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-140">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="0f1b3-141">Se sono elencate più sottoscrizioni e se ne vogliono usare solo alcune, è possibile deselezionare le sottoscrizioni che non si intende usare.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-141">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="0f1b3-142">Dopo aver selezionato le sottoscrizioni, fare clic su **Close**(Chiudi).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-142">When you have selected your subscriptions, click **Close**.</span></span>
   
   ![Finestra di dialogo Gestisci sottoscrizioni][05]

5. <span data-ttu-id="0f1b3-144">Nella finestra di dialogo **Deploy to Azure Web App Container** (Distribuisci in un contenitore app Web di Azure) sono visualizzati tutti i contenitori di app Web creati in precedenza. Se non è stato creato alcun contenitore, l'elenco appare vuoto.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-144">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][06]

6. <span data-ttu-id="0f1b3-146">Se non è stato creato alcun contenitore di app Web di Azure in precedenza o se si desidera pubblicare l'applicazione in un nuovo contenitore, attenersi alla procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-146">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="0f1b3-147">In caso contrario, selezionare un contenitore di app Web esistente e andare al passaggio 7.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-147">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   <span data-ttu-id="0f1b3-148">a.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-148">a.</span></span> <span data-ttu-id="0f1b3-149">Fare clic su **Nuovo**</span><span class="sxs-lookup"><span data-stu-id="0f1b3-149">Click **New...**</span></span>
      
      ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][15]

   <span data-ttu-id="0f1b3-151">b.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-151">b.</span></span> <span data-ttu-id="0f1b3-152">Verrà visualizzata la finestra di dialogo **New Web App Container** (Nuovo contenitore App Web):</span><span class="sxs-lookup"><span data-stu-id="0f1b3-152">The **New Web App Container** dialog box will be displayed:</span></span>
      
      ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][07a]

   <span data-ttu-id="0f1b3-154">c.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-154">c.</span></span> <span data-ttu-id="0f1b3-155">In **DNS Label** (Etichetta DNS) specificare un'etichetta per il contenitore di app Web. Questa sarà l'etichetta DNS foglia dell'URL dell'host per l'applicazione Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-155">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="0f1b3-156">Si noti che il nome deve essere disponibile e conforme ai requisiti di denominazione delle app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-156">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>

   <span data-ttu-id="0f1b3-157">d.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-157">d.</span></span> <span data-ttu-id="0f1b3-158">Nel menu a discesa **Contenitore Web** selezionare il software appropriato per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-158">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="0f1b3-159">Attualmente è possibile scegliere fra Tomcat 8, Tomcat 7 o Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-159">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="0f1b3-160">Una distribuzione recente del software selezionato verrà fornita da Azure e sarà eseguita in una distribuzione recente di JDK 8 creata da Oracle e fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-160">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="0f1b3-161">e.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-161">e.</span></span> <span data-ttu-id="0f1b3-162">Nel menu a discesa **Subscription** (Sottoscrizione) selezionare la sottoscrizione che si vuole usare per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-162">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="0f1b3-163">f.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-163">f.</span></span> <span data-ttu-id="0f1b3-164">Nel menu a discesa **Resource Group** (Gruppo di risorse) selezionare il gruppo di risorse a cui si vuole associare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-164">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="0f1b3-165">I gruppi di risorse di Azure consentono di raggruppare le risorse correlate in modo che, ad esempio, possano essere eliminate insieme.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-165">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="0f1b3-166">È possibile selezionare un gruppo di risorse esistente, se presente, e andare al passaggio g seguente o usare questa procedura per creare un nuovo gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-166">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
   * <span data-ttu-id="0f1b3-167">Fare clic su **Nuovo**</span><span class="sxs-lookup"><span data-stu-id="0f1b3-167">Click **New...**</span></span>
   * <span data-ttu-id="0f1b3-168">Verrà visualizzata la finestra di dialogo **Nuovo gruppo di risorse** :</span><span class="sxs-lookup"><span data-stu-id="0f1b3-168">The **New Resource Group** dialog box will be displayed:</span></span>
        
       ![Finestra di dialogo Nuovo gruppo di risorse][08]
   * <span data-ttu-id="0f1b3-170">Nella casella di testo **Nome** specificare un nome per il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-170">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
   * <span data-ttu-id="0f1b3-171">Nel menu a discesa **Region** (Area) selezionare il percorso del data center di Azure appropriato per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-171">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
   * <span data-ttu-id="0f1b3-172">FACOLTATIVO: per impostazione predefinita, una distribuzione recente di Java 8 verrà distribuita da Azure automaticamente nel contenitore di app web come JVM.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="0f1b3-173">Tuttavia, è possibile specificare una versione e una distribuzione di JVM diversa, se richiesto dall'app Web.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-173">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="0f1b3-174">Per specificare la versione JDK per l'app Web specifica, fare clic sulla scheda **JDK** e selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-174">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
     * <span data-ttu-id="0f1b3-175">**Deploy the default JDK offered by Azure Web Apps service**(Distribuisci JDK predefinito fornito dal servizio app Web di Azure): questa opzione usa una distribuzione recente di Java 8.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-175">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
     * <span data-ttu-id="0f1b3-176">**Deploy a 3rd party JDK available on Azure**(Distribuisci JDK di terze parti disponibile in Azure): questa opzione consente di scegliere dall'elenco di JDK forniti da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-176">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
     * <span data-ttu-id="0f1b3-177">**Deploy my own JDK from this download location**(Distribuisci JDK personalizzato da questo percorso di download): questa opzione consente di specificare una distribuzione JDK personalizzata, che deve essere compressa come file ZIP e caricata in un percorso di download disponibile pubblicamente o in un account di Archiviazione di Azure per cui si dispone dell'accesso.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-177">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
       ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][07b]

   <span data-ttu-id="0f1b3-179">g.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-179">g.</span></span> <span data-ttu-id="0f1b3-180">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-180">Click **OK**.</span></span>

   <span data-ttu-id="0f1b3-181">h.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-181">h.</span></span> <span data-ttu-id="0f1b3-182">Il menu a discesa **Piano di servizio app** elenca i piani di servizio app associati al gruppo di risorse selezionato.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-182">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="0f1b3-183">I piani di servizio app specificano informazioni come il percorso dell'app Web, il piano tariffario e le dimensioni dell'istanza di calcolo.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-183">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="0f1b3-184">È possibile usare un singolo piano di servizio app per più app Web. Per questo motivo viene gestito separatamente da una distribuzione di app Web specifica.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-184">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:
      
      * <span data-ttu-id="0f1b3-185">Fare clic su **Nuovo**</span><span class="sxs-lookup"><span data-stu-id="0f1b3-185">Click **New...**</span></span>
      * <span data-ttu-id="0f1b3-186">Verrà visualizzata la finestra di dialogo **Nuovo piano di servizio app** :</span><span class="sxs-lookup"><span data-stu-id="0f1b3-186">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Finestra di dialogo New App Service Plan (Nuovo piano di servizio app)][09]
      * <span data-ttu-id="0f1b3-188">Nella casella di testo **Nome** specificare un nome per il nuovo piano di servizio app.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-188">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="0f1b3-189">Nel menu a discesa **Località** selezionare la località del data center di Azure appropriata per il piano.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-189">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="0f1b3-190">Nel menu a discesa **Piano tariffario** selezionare la tariffa appropriata per il piano.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-190">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="0f1b3-191">Ai fini del test è possibile scegliere **Gratuito**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-191">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="0f1b3-192">Nel menu a discesa **Dimensioni istanza** selezionare le dimensioni dell'istanza appropriate per il piano.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-192">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="0f1b3-193">Ai fini del test è possibile scegliere **Piccolo**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-193">For testing purposes you can choose **Small**.</span></span>

   <span data-ttu-id="0f1b3-194">i.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-194">i.</span></span> <span data-ttu-id="0f1b3-195">Dopo aver completato tutti i passaggi precedenti, la finestra di dialogo New Web App Container dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-195">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][10]

   <span data-ttu-id="0f1b3-197">j.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-197">j.</span></span> <span data-ttu-id="0f1b3-198">Fare clic su **OK** per completare la creazione del nuovo contenitore di app Web.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-198">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="0f1b3-199">Attendere alcuni secondi che venga aggiornato l'elenco dei contenitori di app Web. Il contenitore di app Web appena creato risulterà selezionato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-199">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

7. <span data-ttu-id="0f1b3-200">A questo punto si è pronti completare la distribuzione iniziale dell'app Web in Azure:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-200">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
   ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][11]
   
   <span data-ttu-id="0f1b3-202">Fare clic su **OK** per distribuire l'applicazione Java nel contenitore di App Web selezionato.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-202">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
   <span data-ttu-id="0f1b3-203">Per impostazione predefinita, l'applicazione verrà distribuita come sottodirectory del server applicazioni.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-203">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="0f1b3-204">Se si vuole distribuire l'applicazione come applicazione radice, selezionare la casella di controllo **Deploy to root** (Distribuisci a radice) prima di fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-204">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>

8. <span data-ttu-id="0f1b3-205">Verrà quindi aperta la visualizzazione **Azure Activity Log** (Log attività di Azure) in cui è indicato lo stato della distribuzione dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-205">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![Azure Activity Log][12]
   
   <span data-ttu-id="0f1b3-207">Il processo di distribuzione dell'app Web in Azure dovrebbe richiedere solo alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-207">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="0f1b3-208">Quando l'applicazione è pronta, viene visualizzato un collegamento denominato **Published** in the **Status** (Stato).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-208">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="0f1b3-209">Quando si fa clic sul collegamento, viene visualizzata la home page dell'app Web distribuita.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-209">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="0f1b3-210">Aggiornamento dell'app Web</span><span class="sxs-lookup"><span data-stu-id="0f1b3-210">Updating your web app</span></span>

<span data-ttu-id="0f1b3-211">L'aggiornamento di un'app Web di Azure in esecuzione è un processo semplice e rapido. Sono disponibili due opzioni per l'aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-211">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="0f1b3-212">È possibile aggiornare la distribuzione di un'app Web Java esistente.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-212">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="0f1b3-213">È possibile pubblicare un'applicazione Java aggiuntiva nello stesso contenitore di app Web.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-213">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="0f1b3-214">In entrambi i casi, il processo è identico e richiede solo pochi secondi:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-214">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="0f1b3-215">In Project Explorer in Eclipse, fare clic con il pulsante destro del mouse sull'applicazione Java che si desidera aggiornare o aggiungere a un contenitore di app Web esistente.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-215">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>

2. <span data-ttu-id="0f1b3-216">Dal menu di scelta rapida visualizzato selezionare **Azure**, quindi **Publish as Azure Web App** (Pubblica come App Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-216">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>

3. <span data-ttu-id="0f1b3-217">Poiché è già stato effettuato l'accesso in precedenza, verrà visualizzato un elenco dei contenitori di app Web esistenti.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-217">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="0f1b3-218">Selezionare il contenitore in cui si vuole pubblicare o ripubblicare l'applicazione Java e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-218">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="0f1b3-219">Pochi secondi dopo nella visualizzazione **Azure Activity Log** (Log attività di Azure) la distribuzione aggiornata apparirà come **Published** (Pubblicata) e sarà possibile verificare l'applicazione aggiornata in un browser Web.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-219">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="0f1b3-220">Avvio, arresto o riavvio di un'app Web esistente</span><span class="sxs-lookup"><span data-stu-id="0f1b3-220">Starting, stopping, or restarting an existing web app</span></span>

<span data-ttu-id="0f1b3-221">Per avviare o arrestare un contenitore di app Web di Azure esistente, incluse tutte le applicazioni Java in esso distribuite, è possibile usare la visualizzazione **Azure Explorer** .</span><span class="sxs-lookup"><span data-stu-id="0f1b3-221">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="0f1b3-222">Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **Window** (Finestra) in Eclipse, quindi su **Show View** (Mostra visualizzazione), **Other** (Altro), **Azure** e infine su **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-222">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="0f1b3-223">Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-223">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="0f1b3-224">Quando appare la visualizzazione **Azure Explorer** , per avviare o arrestare l'app Web seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="0f1b3-224">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="0f1b3-225">Espandere il nodo **Azure** .</span><span class="sxs-lookup"><span data-stu-id="0f1b3-225">Expand the **Azure** node.</span></span>

2. <span data-ttu-id="0f1b3-226">Espandere il nodo **Web Apps** (App Web).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-226">Expand the **Web Apps** node.</span></span> 

3. <span data-ttu-id="0f1b3-227">Fare clic con il pulsante destro del mouse sull'app Web desiderata.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-227">Right-click the desired Web App.</span></span>

4. <span data-ttu-id="0f1b3-228">Quando viene visualizzato il menu di scelta rapida, fare clic su **Start** (Avvia), **Stop** (Arresta) o **Restart** (Riavvia).</span><span class="sxs-lookup"><span data-stu-id="0f1b3-228">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="0f1b3-229">Si noti che le opzioni di menu sono sensibili al contesto, quindi è possibile arrestare solo un'app Web in esecuzione o avviare un'app Web al momento non in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0f1b3-229">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![Arresto di un'app Web esistente][13]

## <a name="next-steps"></a><span data-ttu-id="0f1b3-231">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0f1b3-231">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="0f1b3-232">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="0f1b3-232">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Toolkit di Azure per Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit per IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Panoramica delle App Web]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Updated Version]: azure-toolkit-for-eclipse-create-hello-world-web-app.md
[eclipse-sign-in-instructions]: azure-toolkit-for-eclipse-sign-in-instructions.md


<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/02-Dynamic-Web-Project.png
[03]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/03-Context-Menu.png
[04]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/04-Log-In-Dialog.png
[05]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/05-Manage-Subscriptions-Dialog.png
[06]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/07b-New-Web-App-Container-Dialog.png
[08]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/08-New-Resource-Group-Dialog.png
[09]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/09-New-Service-Plan-Dialog.png
[10]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/11-Completed-Deploy-Dialog.png
[12]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/12-Activity-Log-View.png
[13]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/13-Azure-Explorer-Web-App.png
[14]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/14-publishDropdownButton.png
[15]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/15-New-Azure-Web-Container.png
