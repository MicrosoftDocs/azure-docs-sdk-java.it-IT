---
title: Creare un'app Web Hello World per Servizio app di Azure con Eclipse
description: Questa esercitazione descrive come usare il toolkit di Azure per Eclipse per creare un'app Web Hello World per Azure.
services: app-service
keywords: java, eclipse, app web, servizio app di azure, hello world, avvio rapido
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 7e88298afaf0b4601d85d6063b7096c79e677421
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625925"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a><span data-ttu-id="00391-104">Creare un'app Web Hello World per Servizio app di Azure con Eclipse</span><span class="sxs-lookup"><span data-stu-id="00391-104">Create a Hello World web app for Azure App Service using Eclipse</span></span>

<span data-ttu-id="00391-105">Usando il plug-in open source [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse), è possibile creare e distribuire in pochi minuti un'applicazione Hello World di base come app Web in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="00391-105">Using open sourced [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="00391-106">Se si preferisce usare IntelliJ IDEA, vedere l'[esercitazione simile per IntelliJ][intellij-hello-world].</span><span class="sxs-lookup"><span data-stu-id="00391-106">If you prefer using IntelliJ IDEA, check out our [similar tutorial for IntelliJ][intellij-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="00391-107">Dopo aver completato questa esercitazione, non dimenticare di pulire le risorse.</span><span class="sxs-lookup"><span data-stu-id="00391-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="00391-108">In tal modo, con l'esecuzione di questa guida non si supererà la quota dell'account gratuito.</span><span class="sxs-lookup"><span data-stu-id="00391-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="00391-109">Installazione e accesso</span><span class="sxs-lookup"><span data-stu-id="00391-109">Installation and sign-in</span></span>

1. <span data-ttu-id="00391-110">Trascinare il pulsante seguente nell'area di lavoro di Eclipse in esecuzione per installare il plug-in Azure Toolkit for Eclipse ([altre opzioni di installazione](azure-toolkit-for-eclipse-installation.md)).</span><span class="sxs-lookup"><span data-stu-id="00391-110">Drag the following button to your running Eclipse workspace to install the Azure Toolkit for Eclipse plugin ([other installation options](azure-toolkit-for-eclipse-installation.md)).</span></span>

    <span data-ttu-id="00391-111">[![Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace")</span><span class="sxs-lookup"><span data-stu-id="00391-111">[![Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client")</span></span>

1. <span data-ttu-id="00391-112">Per accedere all'account Azure, fare clic su **Tools** (Strumenti), su **Azure** e quindi su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="00391-112">To sign in to your Azure account, click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="00391-113">![Menu di Eclipse per l'accesso ad Azure][I01]</span><span class="sxs-lookup"><span data-stu-id="00391-113">![Eclipse Menu for Azure Sign In][I01]</span></span>

1. <span data-ttu-id="00391-114">Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ([altre opzioni di accesso](azure-toolkit-for-eclipse-sign-in-instructions.md)).</span><span class="sxs-lookup"><span data-stu-id="00391-114">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign-in options](azure-toolkit-for-eclipse-sign-in-instructions.md)).</span></span>

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

1. <span data-ttu-id="00391-116">Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).</span><span class="sxs-lookup"><span data-stu-id="00391-116">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![Finestra di dialogo di accesso ad Azure][I03]

1. <span data-ttu-id="00391-118">Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="00391-118">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![Accesso al dispositivo nel browser][I04]

1. <span data-ttu-id="00391-120">Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="00391-120">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="00391-122">Creazione del progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="00391-122">Creating web app project</span></span>

1. <span data-ttu-id="00391-123">Fare clic su **File**, **New** (Nuovo) e quindi su **Dynamic Web Project** (Progetto Web dinamico).</span><span class="sxs-lookup"><span data-stu-id="00391-123">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="00391-124">Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...** , espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.</span><span class="sxs-lookup"><span data-stu-id="00391-124">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![Creazione di un nuovo progetto Web dinamico][file-new-dynamic-web-project]

2. <span data-ttu-id="00391-126">Ai fini di questa esercitazione, denominare il progetto **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="00391-126">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="00391-127">L'aspetto della schermata sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="00391-127">Your screen will appear similar to the following:</span></span>
   
   ![Proprietà del nuovo progetto Web dinamico][dynamic-web-project-properties]

3. <span data-ttu-id="00391-129">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="00391-129">Click **Finish**.</span></span>

4. <span data-ttu-id="00391-130">Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse espandere **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="00391-130">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="00391-131">Fare clic con il pulsante destro del mouse su **WebContent**, scegliere **New** e quindi fare clic su **JSP File**.</span><span class="sxs-lookup"><span data-stu-id="00391-131">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![Creare un nuovo file JSP][create-new-jsp-file]

5. <span data-ttu-id="00391-133">Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="00391-133">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![Finestra di dialogo New JSP File (Nuovo file JSP)][new-jsp-file-dialog]

6. <span data-ttu-id="00391-135">Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html)** (Nuovo file JSP - html), quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="00391-135">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![Selezionare il modello JSP][select-jsp-template]

7. <span data-ttu-id="00391-137">Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="00391-137">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="00391-138">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="00391-138">within the existing `<body>` element.</span></span> <span data-ttu-id="00391-139">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="00391-139">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="00391-140">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="00391-140">Save index.jsp.</span></span>

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="00391-141">Distribuzione dell'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="00391-141">Deploying web app to Azure</span></span>

1. <span data-ttu-id="00391-142">Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="00391-142">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![Publish as Azure Web App][publish-as-azure-web-app]

1. <span data-ttu-id="00391-144">Quando viene visualizzata la finestra di dialogo **Deploy Web App** (Distribuisci app Web) è possibile scegliere una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="00391-144">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="00391-145">Selezionare un'app Web esistente, se presente.</span><span class="sxs-lookup"><span data-stu-id="00391-145">Select an existing web app if one exists.</span></span>

      ![Selezionare un servizio app][select-app-service]

   * <span data-ttu-id="00391-147">Fare clic su **Create New App** (Crea nuova app).</span><span class="sxs-lookup"><span data-stu-id="00391-147">Click **Create New Web App**.</span></span>

      ![Creare un servizio app][create-app-service]

      <span data-ttu-id="00391-149">Specificare le informazioni necessarie per l'app Web nella finestra di dialogo **Create App Service** (Crea servizio app) e quindi fare clic su **Create** (Crea).</span><span class="sxs-lookup"><span data-stu-id="00391-149">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      <span data-ttu-id="00391-150">In questa finestra di dialogo è possibile configurare l'ambiente di runtime, le impostazioni dell'app, il piano di servizio e il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="00391-150">Here you can configure the runtime environment, app settings, service plan and resource group.</span></span>

      ![Finestra di dialogo Crea servizio app][create-app-service-dialog]

1. <span data-ttu-id="00391-152">Selezionare l'app Web e quindi fare clic su **Deploy** (Distribuisci).</span><span class="sxs-lookup"><span data-stu-id="00391-152">Select your web app and then click **Deploy**.</span></span>

   ![Distribuire il servizio app][deploy-app-service]

1. <span data-ttu-id="00391-154">Dopo aver distribuito l'app Web, il toolkit visualizza lo stato come **Pubblicato** sotto la scheda **Log attività di Azure**, come collegamento ipertestuale all'URL dell'app Web distribuita.</span><span class="sxs-lookup"><span data-stu-id="00391-154">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![Stato della pubblicazione][publish-status]

1. <span data-ttu-id="00391-156">È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.</span><span class="sxs-lookup"><span data-stu-id="00391-156">You can browse to your web app using the link provided in the status message.</span></span>

   ![Collegamento all'app Web][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a><span data-ttu-id="00391-158">Pulizia delle risorse</span><span class="sxs-lookup"><span data-stu-id="00391-158">Cleaning up resources</span></span>

1. <span data-ttu-id="00391-159">Dopo aver pubblicato l'app Web in Azure, è possibile gestirla facendo clic con il pulsante destro del mouse in Azure Explorer e scegliendo una delle opzioni del menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="00391-159">After you have published your web app to Azure, you can manage it by right-clicking in Azure Explorer and selecting one of the options in the context menu.</span></span> <span data-ttu-id="00391-160">È ad esempio possibile **eliminare** l'app Web per pulire le risorse per l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="00391-160">For example, you can **Delete** your web app here to clean up the resource for this tutorial.</span></span>

   ![Gestire il servizio app][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="00391-162">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="00391-162">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="00391-163">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="00391-163">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->
[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

[browse-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/browse-web-app.png
[file-new-dynamic-web-project]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/file-new-dynamic-web-project.png
[dynamic-web-project-properties]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/dynamic-web-project-properties.png
[create-new-jsp-file]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-new-jsp-file.png
[new-jsp-file-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/new-jsp-file-dialog.png
[select-jsp-template]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-jsp-template.png
[publish-as-azure-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-as-azure-web-app.png
[deploy-web-app-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-web-app-dialog.png
[select-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-app-service.png
[create-app-service-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service-dialog.png
[publish-status]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-status.png
[create-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service.png
[deploy-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-app-service.png
[manage-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/manage-app-service.png
