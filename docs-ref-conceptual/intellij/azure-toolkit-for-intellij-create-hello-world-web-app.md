---
title: Creare un'app Web di base di Azure in IntelliJ
description: Questa esercitazione spiega come usare Azure Toolkit per IntelliJ per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 323ce74f2d4dad10460a0c2bc0fb59f2bbdbbf3c
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="fd83d-103">Creare un'app Web di base di Azure in IntelliJ</span><span class="sxs-lookup"><span data-stu-id="fd83d-103">Create a basic Azure web app in IntelliJ</span></span>

<span data-ttu-id="fd83d-104">Questa esercitazione spiega come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Azure Toolkit per IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="fd83d-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
> 
> <span data-ttu-id="fd83d-105">Azure Toolkit for IntelliJ è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso.</span><span class="sxs-lookup"><span data-stu-id="fd83d-105">The Azure Toolkit for IntelliJ was updated in August 2017, with a different workflow.</span></span> <span data-ttu-id="fd83d-106">Tenendo presente questo aspetto, questo articolo contiene due sezioni diverse:</span><span class="sxs-lookup"><span data-stu-id="fd83d-106">With that in mind, this article contains two different sections:</span></span>
>
> * <span data-ttu-id="fd83d-107">La prima sezione descrive la creazione di un'app Web Hello World tramite la versione di agosto 2017 (o successive) di Azure Toolkit for IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="fd83d-107">The first section illustrates creating a Hello World web app by using the August 2017 (or later) version of the Azure Toolkit for IntelliJ.</span></span>
>
> * <span data-ttu-id="fd83d-108">La seconda sezione di questo articolo descrive la creazione di un'app Web Hello World tramite la versione di aprile 2017 (o precedenti) del toolkit.</span><span class="sxs-lookup"><span data-stu-id="fd83d-108">The second section of this article demonstrates creating a Hello World web app by using the April 2017 (or earlier) version of the toolkit.</span></span>
> 

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-hello-world-web-app-by-using-the-version-307-or-later-toolkit"></a><span data-ttu-id="fd83d-109">Creare un'app Web Hello World con il toolkit versione 3.0.7 o successive</span><span class="sxs-lookup"><span data-stu-id="fd83d-109">Create a Hello World web app by using the version 3.0.7 or later toolkit</span></span>

### <a name="create-a-new-web-app-project"></a><span data-ttu-id="fd83d-110">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="fd83d-110">Create a new web app project</span></span>

1. <span data-ttu-id="fd83d-111">Avviare IntelliJ e accedere all'account Azure seguendo i passaggi contenuti nell'articolo [Istruzioni di accesso per Azure Toolkit for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="fd83d-111">Start IntelliJ and sign-in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="fd83d-112">Scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).</span><span class="sxs-lookup"><span data-stu-id="fd83d-112">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![Creare un nuovo progetto][file-new-project]

1. <span data-ttu-id="fd83d-114">Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven**, **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="fd83d-114">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![Scegliere l'app Web di sistema Maven][maven-archetype-webapp]
   
1. <span data-ttu-id="fd83d-116">Specificare i valori per **GroupId** e **ArtifactId** per l'app Web e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="fd83d-116">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![Specificare GroupId e ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="fd83d-118">Personalizzare qualsiasi impostazione per Maven o accettare quelle predefinite e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="fd83d-118">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![Specificare le impostazioni per Maven][maven-options]

1. <span data-ttu-id="fd83d-120">Specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="fd83d-120">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![Specificare il nome del progetto][project-name]

1. <span data-ttu-id="fd83d-122">Nella visualizzazione Project Explorer (Esplora progetti) di IntelliJ espandere **src**, **main**, **webapp** e quindi fare doppio clic su **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-122">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![Apertura della pagina di indice][open-index-page]

1. <span data-ttu-id="fd83d-124">Quando viene aperto il file index.jsp in IntelliJ, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="fd83d-124">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="fd83d-125">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="fd83d-125">within the existing `<body>` element.</span></span> <span data-ttu-id="fd83d-126">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fd83d-126">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![Modifica della pagina di indice][edit-index-page]

1. <span data-ttu-id="fd83d-128">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="fd83d-128">Save index.jsp.</span></span>

### <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="fd83d-129">Distribuire l'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="fd83d-129">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="fd83d-130">Nella visualizzazione Project Explorer (Esplora progetti) di IntelliJ fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Run on Web App** (Esegui su app Web).</span><span class="sxs-lookup"><span data-stu-id="fd83d-130">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![Menu Run on Web App (Esegui su app Web)][run-on-web-app-menu]

1. <span data-ttu-id="fd83d-132">Nella finestra di dialogo Run on Web App (Esegui su app Web) è possibile scegliere una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd83d-132">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="fd83d-133">Scegliere un'app esistente (se presente) e quindi fare clic su **Run** (Esegui).</span><span class="sxs-lookup"><span data-stu-id="fd83d-133">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![Finestra di dialogo Run on Web App (Esegui su app Web)][run-on-web-app-dialog]

   * <span data-ttu-id="fd83d-135">Fare clic su **Create New App** (Crea nuova app).</span><span class="sxs-lookup"><span data-stu-id="fd83d-135">Click **Create New Web App**.</span></span> <span data-ttu-id="fd83d-136">Se si sceglie di creare una nuova app Web, specificare le informazioni necessarie per l'app Web e quindi fare clic su **Run** (Esegui).</span><span class="sxs-lookup"><span data-stu-id="fd83d-136">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run**.</span></span>

      ![Create New App (Crea nuova app)][create-new-web-app-dialog]

1. <span data-ttu-id="fd83d-138">Il toolkit visualizzerà un messaggio di stato dopo aver distribuito l'app Web, che conterrà anche l'URL dell'app Web distribuita.</span><span class="sxs-lookup"><span data-stu-id="fd83d-138">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![Distribuzione completata][successfully-deployed]

1. <span data-ttu-id="fd83d-140">È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.</span><span class="sxs-lookup"><span data-stu-id="fd83d-140">You can browse to your web app using the link provided in the status message.</span></span>

   ![Collegamento all'app Web][browse-web-app]

1. <span data-ttu-id="fd83d-142">Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire l'applicazione su Azure facendo clic sulla freccia verde sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="fd83d-142">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="fd83d-143">È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).</span><span class="sxs-lookup"><span data-stu-id="fd83d-143">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. <span data-ttu-id="fd83d-145">Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-145">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

<hr />

## <a name="create-a-hello-world-web-app-by-using-the-version-306-or-earlier-toolkit"></a><span data-ttu-id="fd83d-147">Creare un'app Web Hello World con il toolkit versione 3.0.6 o precedenti</span><span class="sxs-lookup"><span data-stu-id="fd83d-147">Create a Hello World web app by using the version 3.0.6 or earlier toolkit</span></span>

### <a name="create-a-new-web-app-project"></a><span data-ttu-id="fd83d-148">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="fd83d-148">Create a new web app project</span></span>

1. <span data-ttu-id="fd83d-149">Avviare IntelliJ e fare clic sul menu **File**, quindi su **New** (Nuovo) e su **Project** (Progetto).</span><span class="sxs-lookup"><span data-stu-id="fd83d-149">Start IntelliJ and click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![File > New Project (Nuovo progetto)][02]

2. <span data-ttu-id="fd83d-151">Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Java**, **Web Application** (Applicazione Web) e quindi fare clic su **New** (Nuovo) per aggiungere un SDK di progetto.</span><span class="sxs-lookup"><span data-stu-id="fd83d-151">In the **New Project** dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
   ![Finestra di dialogo Nuovo progetto][03a]
   
3. <span data-ttu-id="fd83d-153">Nella finestra di dialogo Select Home Directory for JDK (Selezionare la home directory per JDK), selezionare la cartella in cui è installato JDK e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-153">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="fd83d-154">Fare clic su **Next** (Avanti) nella finestra di dialogo New Project (Nuovo progetto) per continuare.</span><span class="sxs-lookup"><span data-stu-id="fd83d-154">Click **Next** in the New Project dialog box to continue.</span></span>
   
   ![Specificare la home directory di JDK][03b]

4. <span data-ttu-id="fd83d-156">Ai fini di questa esercitazione, denominare il progetto **Java-Web-App-On-Azure**, quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="fd83d-156">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
   ![Finestra di dialogo Nuovo progetto][04]

5. <span data-ttu-id="fd83d-158">Nella visualizzazione Project Explorer di IntelliJ espandere **Java-Web-App-On-Azure** e **web**, quindi fare doppio clic su **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-158">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
   ![Pagina di indice aperta][05c]

6. <span data-ttu-id="fd83d-160">Quando viene aperto il file index.jsp in IntelliJ, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="fd83d-160">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="fd83d-161">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="fd83d-161">within the existing `<body>` element.</span></span> <span data-ttu-id="fd83d-162">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fd83d-162">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. <span data-ttu-id="fd83d-163">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="fd83d-163">Save index.jsp.</span></span>

### <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="fd83d-164">Distribuire l'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="fd83d-164">Deploy your web app to Azure</span></span>
<span data-ttu-id="fd83d-165">Esistono diversi modi per distribuire un'applicazione Web Java in Azure.</span><span class="sxs-lookup"><span data-stu-id="fd83d-165">There are several ways by which you can deploy a Java web app to Azure.</span></span> <span data-ttu-id="fd83d-166">Questa esercitazione descrive uno dei modi più semplici: l'applicazione viene distribuita in un contenitore di app Web di Azure senza richiedere tipi di progetto specifici o strumenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="fd83d-166">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="fd83d-167">JDK e il software del contenitore Web vengono forniti automaticamente da Azure senza la necessità di eseguire alcun caricamento. È sufficiente disporre dell'app Web Java.</span><span class="sxs-lookup"><span data-stu-id="fd83d-167">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="fd83d-168">Di conseguenza, il processo di pubblicazione per l'applicazione richiederà alcuni secondi, non minuti.</span><span class="sxs-lookup"><span data-stu-id="fd83d-168">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="fd83d-169">Prima di pubblicare l'applicazione è necessario configurare le impostazioni del modulo.</span><span class="sxs-lookup"><span data-stu-id="fd83d-169">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="fd83d-170">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="fd83d-170">To do so, use the following steps:</span></span>

1. <span data-ttu-id="fd83d-171">In Project Explorer di IntelliJ fare clic con il pulsante destro del mouse sul progetto **Java-Web-App-On-Azure** .</span><span class="sxs-lookup"><span data-stu-id="fd83d-171">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="fd83d-172">Quando viene visualizzato il menu di scelta rapida, fare clic su **Open Module Settings** (Apri impostazioni modulo).</span><span class="sxs-lookup"><span data-stu-id="fd83d-172">When the context menu appears, click **Open Module Settings**.</span></span>

   ![Open Module Settings (Apri impostazioni modulo)][05a]

2. <span data-ttu-id="fd83d-174">Nella finestra di dialogo Project Structure (Struttura progetto):</span><span class="sxs-lookup"><span data-stu-id="fd83d-174">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="fd83d-175">a.</span><span class="sxs-lookup"><span data-stu-id="fd83d-175">a.</span></span> <span data-ttu-id="fd83d-176">Fare clic su **Artifacts** (Elementi) nell'elenco **Project Settings** (Impostazioni progetto).</span><span class="sxs-lookup"><span data-stu-id="fd83d-176">Click **Artifacts** in the list of **Project Settings**.</span></span>

   <span data-ttu-id="fd83d-177">b.</span><span class="sxs-lookup"><span data-stu-id="fd83d-177">b.</span></span> <span data-ttu-id="fd83d-178">Modificare il nome dell'elemento nella casella **Nome** in modo che non contenga caratteri speciali o spazi vuoti; questa operazione è necessaria perché il nome verrà usato nell'URI (Uniform Resource Identifier).</span><span class="sxs-lookup"><span data-stu-id="fd83d-178">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>

   <span data-ttu-id="fd83d-179">c.</span><span class="sxs-lookup"><span data-stu-id="fd83d-179">c.</span></span> <span data-ttu-id="fd83d-180">Modificare il valore di **Type** (Tipo) in **Web Application: Archive** (Applicazione Web: archivio).</span><span class="sxs-lookup"><span data-stu-id="fd83d-180">Change the **Type** to **Web Application: Archive**.</span></span>

   <span data-ttu-id="fd83d-181">d.</span><span class="sxs-lookup"><span data-stu-id="fd83d-181">d.</span></span> <span data-ttu-id="fd83d-182">Fare clic su **OK** per chiudere la finestra di dialogo Project Structure (Struttura progetto).</span><span class="sxs-lookup"><span data-stu-id="fd83d-182">Click **OK** to close the Project Structure dialog box.</span></span>

   ![Open Module Settings (Apri impostazioni modulo)][05b]

<span data-ttu-id="fd83d-184">Dopo aver configurato le impostazioni del modulo è possibile pubblicare l'applicazione in Azure con la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="fd83d-184">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="fd83d-185">In Project Explorer di IntelliJ fare clic con il pulsante destro del mouse sul progetto **Java-Web-App-On-Azure** .</span><span class="sxs-lookup"><span data-stu-id="fd83d-185">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="fd83d-186">Dal menu di scelta rapida visualizzato scegliere **Azure** e fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="fd83d-186">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
   ![Menu di scelta rapida per la pubblicazione in Azure][06]

2. <span data-ttu-id="fd83d-188">Se non è già stato eseguito l'accesso ad Azure da IntelliJ, verrà chiesto di accedere all'account Azure.</span><span class="sxs-lookup"><span data-stu-id="fd83d-188">If you have not already signed in to Azure from IntelliJ, you will be prompted to sign in to your Azure account.</span></span> <span data-ttu-id="fd83d-189">In caso di più account Azure, durante il processo di accesso potrebbero essere visualizzati più volte alcuni prompt apparentemente identici.</span><span class="sxs-lookup"><span data-stu-id="fd83d-189">(If you have multiple Azure accounts, some of the prompts during the sign-in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="fd83d-190">In questo caso, continuare a seguire le istruzioni di accesso.</span><span class="sxs-lookup"><span data-stu-id="fd83d-190">When this happens, continue to follow the sign-in instructions.)</span></span>
   
   ![Finestra di accesso di Azure][07]

3. <span data-ttu-id="fd83d-192">Dopo aver eseguito l'accesso all'account Azure, nella finestra di dialogo **Gestisci sottoscrizioni** viene visualizzato un elenco delle sottoscrizioni associate alle credenziali usate.</span><span class="sxs-lookup"><span data-stu-id="fd83d-192">After you have successfully signed in to your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="fd83d-193">Se sono elencate più sottoscrizioni e se ne vogliono usare solo alcune, è possibile deselezionare le sottoscrizioni che non si intende usare. Dopo aver selezionato le sottoscrizioni, fare clic su **Close**(Chiudi).</span><span class="sxs-lookup"><span data-stu-id="fd83d-193">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
   ![Gestisci sottoscrizioni][08]

4. <span data-ttu-id="fd83d-195">Nella finestra di dialogo **Deploy to Azure Web App Container** (Distribuisci in un contenitore app Web di Azure) sono visualizzati tutti i contenitori di app Web creati in precedenza. Se non è stato creato alcun contenitore, l'elenco appare vuoto.</span><span class="sxs-lookup"><span data-stu-id="fd83d-195">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![Contenitori di app][09]

5. <span data-ttu-id="fd83d-197">Se non è stato creato alcun contenitore di app Web di Azure in precedenza o se si desidera pubblicare l'applicazione in un nuovo contenitore, attenersi alla procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="fd83d-197">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="fd83d-198">In caso contrario, selezionare un contenitore di app Web esistente e andare al passaggio 6.</span><span class="sxs-lookup"><span data-stu-id="fd83d-198">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   <span data-ttu-id="fd83d-199">a.</span><span class="sxs-lookup"><span data-stu-id="fd83d-199">a.</span></span> <span data-ttu-id="fd83d-200">Fare clic sul pulsante **+**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-200">Click the **+** sign.</span></span>
      
      ![Aggiungere un contenitore di app][10]

   <span data-ttu-id="fd83d-202">b.</span><span class="sxs-lookup"><span data-stu-id="fd83d-202">b.</span></span> <span data-ttu-id="fd83d-203">Viene visualizzata la finestra di dialogo **New Web App Container** (Nuovo contenitore app Web), che verrà usata in diversi passaggi della procedura.</span><span class="sxs-lookup"><span data-stu-id="fd83d-203">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
      ![Nuovo contenitore di app][11a]
   
   <span data-ttu-id="fd83d-205">c.</span><span class="sxs-lookup"><span data-stu-id="fd83d-205">c.</span></span> <span data-ttu-id="fd83d-206">In **DNS Label** (Etichetta DNS) specificare un'etichetta per il contenitore di app Web. Questa sarà l'etichetta DNS foglia dell'URL dell'host per l'applicazione Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="fd83d-206">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="fd83d-207">Si noti che il nome deve essere disponibile e conforme ai requisiti di denominazione delle app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="fd83d-207">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>

   <span data-ttu-id="fd83d-208">d.</span><span class="sxs-lookup"><span data-stu-id="fd83d-208">d.</span></span> <span data-ttu-id="fd83d-209">Nel menu a discesa **Contenitore Web** selezionare il software appropriato per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fd83d-209">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="fd83d-210">Attualmente è possibile scegliere fra Tomcat 8, Tomcat 7 o Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="fd83d-210">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="fd83d-211">Una distribuzione recente del software selezionato verrà fornita da Azure e sarà eseguita in una distribuzione recente di JDK 8 creata da Oracle e fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="fd83d-211">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="fd83d-212">e.</span><span class="sxs-lookup"><span data-stu-id="fd83d-212">e.</span></span> <span data-ttu-id="fd83d-213">Nel menu a discesa **Subscription** (Sottoscrizione) selezionare la sottoscrizione che si vuole usare per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="fd83d-213">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="fd83d-214">f.</span><span class="sxs-lookup"><span data-stu-id="fd83d-214">f.</span></span> <span data-ttu-id="fd83d-215">Nel menu a discesa **Resource Group** (Gruppo di risorse) selezionare il gruppo di risorse a cui si vuole associare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="fd83d-215">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="fd83d-216">I gruppi di risorse di Azure consentono di raggruppare le risorse correlate in modo che, ad esempio, possano essere eliminate insieme.</span><span class="sxs-lookup"><span data-stu-id="fd83d-216">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="fd83d-217">È possibile selezionare un gruppo di risorse esistente, se presente, e andare al passaggio g seguente o usare questa procedura per creare un nuovo gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="fd83d-217">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="fd83d-218">Selezionare **&lt;&lt; Create new Resource Group &gt;&gt;** (Crea nuovo gruppo di risorse) nel menu a discesa **Resource Group** (Gruppo di risorse).</span><span class="sxs-lookup"><span data-stu-id="fd83d-218">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="fd83d-219">Verrà visualizzata la finestra di dialogo **Nuovo gruppo di risorse** :</span><span class="sxs-lookup"><span data-stu-id="fd83d-219">The **New Resource Group** dialog box will be displayed:</span></span>
        
         ![Nuovo gruppo di risorse][12]

      * <span data-ttu-id="fd83d-221">Nella casella di testo **Nome** specificare un nome per il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="fd83d-221">In the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="fd83d-222">Nel menu a discesa **Area** selezionare la località del data center di Azure appropriata per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="fd83d-222">In the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="fd83d-223">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-223">Click **OK**.</span></span>

   <span data-ttu-id="fd83d-224">g.</span><span class="sxs-lookup"><span data-stu-id="fd83d-224">g.</span></span> <span data-ttu-id="fd83d-225">Il menu a discesa **Piano di servizio app** elenca i piani di servizio app associati al gruppo di risorse selezionato.</span><span class="sxs-lookup"><span data-stu-id="fd83d-225">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="fd83d-226">Un piano di servizio app specifica informazioni quali il percorso dell'app Web, il piano tariffario e le dimensioni dell'istanza di calcolo.</span><span class="sxs-lookup"><span data-stu-id="fd83d-226">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="fd83d-227">È possibile usare un singolo piano di servizio app per più app Web. Per questo motivo viene gestito separatamente da una distribuzione di app Web specifica.</span><span class="sxs-lookup"><span data-stu-id="fd83d-227">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
      <span data-ttu-id="fd83d-228">È possibile selezionare un piano di servizio app esistente, se presente, e andare al passaggio h seguente o usare questa procedura per creare un nuovo piano di servizio app:</span><span class="sxs-lookup"><span data-stu-id="fd83d-228">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="fd83d-229">Selezionare **&lt;&lt; Create new App Service Plan &gt;&gt;** (Crea nuovo piano di servizio app) nel menu a discesa **App Service Plan** (Piano di servizio app).</span><span class="sxs-lookup"><span data-stu-id="fd83d-229">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="fd83d-230">Verrà visualizzata la finestra di dialogo **Nuovo piano di servizio app** :</span><span class="sxs-lookup"><span data-stu-id="fd83d-230">The **New App Service Plan** dialog box will be displayed:</span></span>
        
         ![Nuovo piano di servizio app][13]

      * <span data-ttu-id="fd83d-232">Nella casella di testo **Nome** specificare un nome per il nuovo piano di servizio app.</span><span class="sxs-lookup"><span data-stu-id="fd83d-232">In the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="fd83d-233">Nel menu a discesa **Località** selezionare la località del data center di Azure appropriata per il piano.</span><span class="sxs-lookup"><span data-stu-id="fd83d-233">In the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="fd83d-234">Nel menu a discesa **Piano tariffario** selezionare la tariffa appropriata per il piano.</span><span class="sxs-lookup"><span data-stu-id="fd83d-234">In the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="fd83d-235">Ai fini del test è possibile scegliere **Gratuito**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-235">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="fd83d-236">Nel menu a discesa **Dimensioni istanza** selezionare le dimensioni dell'istanza appropriate per il piano.</span><span class="sxs-lookup"><span data-stu-id="fd83d-236">In the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="fd83d-237">Ai fini del test è possibile scegliere **Piccolo**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-237">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="fd83d-238">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-238">Click **OK**.</span></span>

   <span data-ttu-id="fd83d-239">h.</span><span class="sxs-lookup"><span data-stu-id="fd83d-239">h.</span></span> <span data-ttu-id="fd83d-240">(Facoltativo) Per impostazione predefinita, una distribuzione recente di Java 8 verrà distribuita automaticamente da Azure nel contenitore di app Web come JVM.</span><span class="sxs-lookup"><span data-stu-id="fd83d-240">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="fd83d-241">È tuttavia possibile selezionare una versione e una distribuzione di JVM diversa.</span><span class="sxs-lookup"><span data-stu-id="fd83d-241">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="fd83d-242">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="fd83d-242">To do so, use the following steps:</span></span>
      
      * <span data-ttu-id="fd83d-243">Fare clic sulla scheda **JDK** nella finestra di dialogo **New Web App Container** (Nuovo contenitore app Web).</span><span class="sxs-lookup"><span data-stu-id="fd83d-243">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="fd83d-244">È possibile scegliere una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd83d-244">You can choose from one of the following options:</span></span>
        
         * <span data-ttu-id="fd83d-245">Distribuire il JDK predefinito offerto da Azure</span><span class="sxs-lookup"><span data-stu-id="fd83d-245">Deploy the default JDK which is offered by Azure</span></span>
         * <span data-ttu-id="fd83d-246">Distribuire un JDK di terze parti da un elenco a discesa di altri JDK disponibili in Azure</span><span class="sxs-lookup"><span data-stu-id="fd83d-246">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
         * <span data-ttu-id="fd83d-247">Distribuire un JDK personalizzato, che deve essere compresso come file ZIP e disponibile pubblicamente o nell'account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="fd83d-247">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
      ![Scheda JDK della finestra di dialogo New Web App Container (Nuovo contenitore app Web)][11b]

   <span data-ttu-id="fd83d-249">i.</span><span class="sxs-lookup"><span data-stu-id="fd83d-249">i.</span></span> <span data-ttu-id="fd83d-250">Dopo aver completato tutti i passaggi precedenti, la finestra di dialogo New Web App Container dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="fd83d-250">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![Nuovo contenitore di app][14]
   
   <span data-ttu-id="fd83d-252">j.</span><span class="sxs-lookup"><span data-stu-id="fd83d-252">j.</span></span> <span data-ttu-id="fd83d-253">Fare clic su **OK** per completare la creazione del nuovo contenitore di app Web.</span><span class="sxs-lookup"><span data-stu-id="fd83d-253">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="fd83d-254">Attendere alcuni secondi che venga aggiornato l'elenco dei contenitori di app Web. Il contenitore di app Web appena creato risulterà selezionato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="fd83d-254">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

6. <span data-ttu-id="fd83d-255">A questo punto è possibile completare la distribuzione iniziale dell'app Web in Azure. Fare clic su **OK** per distribuire l'applicazione Java nel contenitore di app Web selezionato.</span><span class="sxs-lookup"><span data-stu-id="fd83d-255">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="fd83d-256">Per impostazione predefinita, l'applicazione verrà distribuita come sottodirectory del server applicazioni.</span><span class="sxs-lookup"><span data-stu-id="fd83d-256">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="fd83d-257">Se si vuole distribuire l'applicazione come applicazione radice, selezionare la casella di controllo **Deploy to root** (Distribuisci a radice) prima di fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-257">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
   ![Distribuire in Azure][15]

7. <span data-ttu-id="fd83d-259">Verrà quindi aperta la visualizzazione **Azure Activity Log** (Log attività di Azure) in cui è indicato lo stato della distribuzione dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="fd83d-259">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![Indicatore di stato][16]
   
   <span data-ttu-id="fd83d-261">Il processo di distribuzione dell'app Web in Azure dovrebbe richiedere solo alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="fd83d-261">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="fd83d-262">Quando l'applicazione è pronta, viene visualizzato un collegamento con valore **Pubblicato** nella colonna **Stato** .</span><span class="sxs-lookup"><span data-stu-id="fd83d-262">When your application is ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="fd83d-263">Quando si fa clic sul collegamento, si passa alla home page dell'app Web distribuita oppure è possibile seguire la procedura indicata nella sezione seguente per passare all'app Web.</span><span class="sxs-lookup"><span data-stu-id="fd83d-263">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

### <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="fd83d-264">Passaggio all'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="fd83d-264">Browsing to your Web App on Azure</span></span>
<span data-ttu-id="fd83d-265">Per trovare l'app Web in Azure, è possibile usare la visualizzazione **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-265">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="fd83d-266">Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **View** (Visualizza) in IntelliJ, quindi su **Tool Windows** (Finestre degli strumenti) e su **Service Explorer**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-266">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="fd83d-267">Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="fd83d-267">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="fd83d-268">Quando viene aperta la visualizzazione **Azure Explorer**, seguire questa procedura per passare all'app Web:</span><span class="sxs-lookup"><span data-stu-id="fd83d-268">When the **Azure Explorer** view is displayed, follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="fd83d-269">Espandere il nodo **Azure** .</span><span class="sxs-lookup"><span data-stu-id="fd83d-269">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="fd83d-270">Espandere il nodo **Web Apps** (App Web).</span><span class="sxs-lookup"><span data-stu-id="fd83d-270">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="fd83d-271">Fare clic con il pulsante destro del mouse sull'app Web desiderata.</span><span class="sxs-lookup"><span data-stu-id="fd83d-271">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="fd83d-272">Quando viene visualizzato il menu di scelta rapida, fare clic su **Open in Browser**(Apri nel browser).</span><span class="sxs-lookup"><span data-stu-id="fd83d-272">When the context menu appears, click **Open in Browser**.</span></span>
   
   ![Sfogliare l'app Web][17]

### <a name="updating-your-web-app"></a><span data-ttu-id="fd83d-274">Aggiornamento dell'app Web</span><span class="sxs-lookup"><span data-stu-id="fd83d-274">Updating your web app</span></span>
<span data-ttu-id="fd83d-275">L'aggiornamento di un'app Web di Azure in esecuzione è un processo semplice e rapido. Sono disponibili due opzioni per l'aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="fd83d-275">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="fd83d-276">È possibile aggiornare la distribuzione di un'app Web Java esistente.</span><span class="sxs-lookup"><span data-stu-id="fd83d-276">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="fd83d-277">È possibile pubblicare un'applicazione Java aggiuntiva nello stesso contenitore di app Web.</span><span class="sxs-lookup"><span data-stu-id="fd83d-277">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="fd83d-278">In entrambi i casi, il processo è identico e richiede solo pochi secondi:</span><span class="sxs-lookup"><span data-stu-id="fd83d-278">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="fd83d-279">In Project Explorer di IntelliJ fare clic con il pulsante destro del mouse sull'applicazione Java che si vuole aggiornare o aggiungere a un contenitore di app Web esistente.</span><span class="sxs-lookup"><span data-stu-id="fd83d-279">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="fd83d-280">Dal menu di scelta rapida visualizzato selezionare **Azure**, quindi **Publish as Azure Web App** (Pubblica come App Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="fd83d-280">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="fd83d-281">Poiché è già stato effettuato l'accesso in precedenza, verrà visualizzato un elenco dei contenitori di app Web esistenti.</span><span class="sxs-lookup"><span data-stu-id="fd83d-281">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="fd83d-282">Selezionare il contenitore in cui si vuole pubblicare o ripubblicare l'applicazione Java e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-282">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="fd83d-283">Pochi secondi dopo nella visualizzazione **Azure Activity Log** (Log attività di Azure) la distribuzione aggiornata apparirà come **Published** (Pubblicata) e sarà possibile verificare l'applicazione aggiornata in un browser Web.</span><span class="sxs-lookup"><span data-stu-id="fd83d-283">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

### <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="fd83d-284">Avvio, arresto o riavvio di un'app Web esistente</span><span class="sxs-lookup"><span data-stu-id="fd83d-284">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="fd83d-285">Per avviare o arrestare un contenitore di app Web di Azure esistente, incluse tutte le applicazioni Java in esso distribuite, è possibile usare la visualizzazione **Azure Explorer** .</span><span class="sxs-lookup"><span data-stu-id="fd83d-285">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="fd83d-286">Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **View** (Visualizza) in IntelliJ, quindi su **Tool Windows** (Finestre degli strumenti) e su **Service Explorer**.</span><span class="sxs-lookup"><span data-stu-id="fd83d-286">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="fd83d-287">Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="fd83d-287">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="fd83d-288">Quando viene aperta la visualizzazione **Azure Explorer**, seguire questa procedura per avviare o arrestare l'app Web:</span><span class="sxs-lookup"><span data-stu-id="fd83d-288">When the **Azure Explorer** view is displayed, follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="fd83d-289">Espandere il nodo **Azure** .</span><span class="sxs-lookup"><span data-stu-id="fd83d-289">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="fd83d-290">Espandere il nodo **Web Apps** (App Web).</span><span class="sxs-lookup"><span data-stu-id="fd83d-290">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="fd83d-291">Fare clic con il pulsante destro del mouse sull'app Web desiderata.</span><span class="sxs-lookup"><span data-stu-id="fd83d-291">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="fd83d-292">Quando viene visualizzato il menu di scelta rapida, fare clic su **Start** (Avvia), **Stop** (Arresta) o **Restart** (Riavvia).</span><span class="sxs-lookup"><span data-stu-id="fd83d-292">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="fd83d-293">Si noti che le opzioni di menu sono sensibili al contesto, quindi è possibile arrestare solo un'app Web in esecuzione o avviare un'app Web al momento non in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fd83d-293">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![Arrestare l'app Web][18]

## <a name="next-steps"></a><span data-ttu-id="fd83d-295">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fd83d-295">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="fd83d-296">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="fd83d-296">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit per IntelliJ]: azure-toolkit-for-intellij.md
[Panoramica delle App Web]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/18-Stop-Web-App.png

[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[run-on-web-app-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[run-on-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
