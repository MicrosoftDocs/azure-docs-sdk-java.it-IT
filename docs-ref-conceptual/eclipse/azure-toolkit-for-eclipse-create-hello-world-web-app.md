---
title: Creare un'app Web Hello World per Azure con Eclipse
description: Questa esercitazione descrive come usare il toolkit di Azure per Eclipse per creare un'app Web Hello World per Azure.
services: app-service
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
ms.openlocfilehash: 5e025c90c2619ec72ffddf5815fd49c3ac59c00f
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893092"
---
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a><span data-ttu-id="6087b-103">Creare un'app Web Hello World per Azure con Eclipse</span><span class="sxs-lookup"><span data-stu-id="6087b-103">Create a Hello World web app for Azure using Eclipse</span></span>

<span data-ttu-id="6087b-104">Questa esercitazione descrive come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Toolkit di Azure per Eclipse].</span><span class="sxs-lookup"><span data-stu-id="6087b-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="6087b-105">Per una versione di questo articolo che fa uso di [Toolkit di Azure per IntelliJ], vedere [Creare un'app Web Hello World per Azure con IntelliJ][intellij-hello-world].</span><span class="sxs-lookup"><span data-stu-id="6087b-105">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="6087b-106">Azure Toolkit for Eclipse è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso.</span><span class="sxs-lookup"><span data-stu-id="6087b-106">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="6087b-107">Questo articolo descrive come creare un'app Web Hello World usando la versione 3.0.7 (o versioni successive) di Azure Toolkit for Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6087b-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="6087b-108">Se si usa la versione 3.0.6 (o versioni precedenti) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con il toolkit legacy per Eclipse][Legacy Version].</span><span class="sxs-lookup"><span data-stu-id="6087b-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="6087b-109">Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6087b-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Anteprima dell'app Hello World][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="6087b-111">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="6087b-111">Create a new web app project</span></span>

1. <span data-ttu-id="6087b-112">Avviare Eclipse e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for Eclipse][eclipse-sign-in-instructions].</span><span class="sxs-lookup"><span data-stu-id="6087b-112">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="6087b-113">Fare clic su **File**, **New** (Nuovo) e quindi su **Dynamic Web Project** (Progetto Web dinamico).</span><span class="sxs-lookup"><span data-stu-id="6087b-113">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="6087b-114">Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...**, espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.</span><span class="sxs-lookup"><span data-stu-id="6087b-114">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![Creazione di un nuovo progetto Web dinamico][file-new-dynamic-web-project]

2. <span data-ttu-id="6087b-116">Ai fini di questa esercitazione, denominare il progetto **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="6087b-116">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="6087b-117">L'aspetto della schermata sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6087b-117">Your screen will appear similar to the following:</span></span>
   
   ![Proprietà del nuovo progetto Web dinamico][dynamic-web-project-properties]

3. <span data-ttu-id="6087b-119">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="6087b-119">Click **Finish**.</span></span>

4. <span data-ttu-id="6087b-120">Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse espandere **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="6087b-120">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="6087b-121">Fare clic con il pulsante destro del mouse su **WebContent**, scegliere **New** e quindi fare clic su **JSP File**.</span><span class="sxs-lookup"><span data-stu-id="6087b-121">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![Creare un nuovo file JSP][create-new-jsp-file]

5. <span data-ttu-id="6087b-123">Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="6087b-123">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![Finestra di dialogo New JSP File (Nuovo file JSP)][new-jsp-file-dialog]

6. <span data-ttu-id="6087b-125">Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html)** (Nuovo file JSP - html), quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="6087b-125">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![Selezionare il modello JSP][select-jsp-template]

7. <span data-ttu-id="6087b-127">Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="6087b-127">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="6087b-128">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="6087b-128">within the existing `<body>` element.</span></span> <span data-ttu-id="6087b-129">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="6087b-129">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="6087b-130">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="6087b-130">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="6087b-131">Distribuire l'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="6087b-131">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="6087b-132">Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="6087b-132">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![Publish as Azure Web App][publish-as-azure-web-app]

1. <span data-ttu-id="6087b-134">Quando viene visualizzata la finestra di dialogo **Deploy Web App** (Distribuisci app Web) è possibile scegliere una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6087b-134">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="6087b-135">Selezionare un'app Web esistente, se presente.</span><span class="sxs-lookup"><span data-stu-id="6087b-135">Select an existing web app if one exists.</span></span>

      ![Selezionare un servizio app][select-app-service]

   * <span data-ttu-id="6087b-137">Fare clic su **Create New App** (Crea nuova app).</span><span class="sxs-lookup"><span data-stu-id="6087b-137">Click **Create New Web App**.</span></span>

      ![Creare un servizio app][create-app-service]

      <span data-ttu-id="6087b-139">Specificare le informazioni necessarie per l'app Web nella finestra di dialogo **Create App Service** (Crea servizio app) e quindi fare clic su **Create** (Crea).</span><span class="sxs-lookup"><span data-stu-id="6087b-139">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      ![Finestra di dialogo Crea servizio app][create-app-service-dialog]

1. <span data-ttu-id="6087b-141">Selezionare l'app Web e quindi fare clic su **Deploy** (Distribuisci).</span><span class="sxs-lookup"><span data-stu-id="6087b-141">Select your web app and then click **Deploy**.</span></span>

   ![Distribuire il servizio app][deploy-app-service]

1. <span data-ttu-id="6087b-143">Dopo aver distribuito l'app Web, il toolkit visualizza lo stato come **Pubblicato** sotto la scheda **Log attività di Azure**, come collegamento ipertestuale all'URL dell'app Web distribuita.</span><span class="sxs-lookup"><span data-stu-id="6087b-143">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![Stato della pubblicazione][publish-status]

1. <span data-ttu-id="6087b-145">È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.</span><span class="sxs-lookup"><span data-stu-id="6087b-145">You can browse to your web app using the link provided in the status message.</span></span>

   ![Collegamento all'app Web][browse-web-app]

1. <span data-ttu-id="6087b-147">Dopo aver pubblicato l'app Web in Azure, è possibile gestirla facendo clic su di essa con il pulsante destro del mouse e selezionando una delle opzioni del menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6087b-147">After you have published your web to Azure, you can manage your app by right-clicking on it and selecting one of the options on the context menu.</span></span> <span data-ttu-id="6087b-148">Ad esempio, è possibile **avviare**, **arrestare** o **eliminare** l'app Web.</span><span class="sxs-lookup"><span data-stu-id="6087b-148">For example, you can **Start**, **Stop**, or **Delete** your web app.</span></span>

   ![Gestire il servizio app][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="6087b-150">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6087b-150">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="6087b-151">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="6087b-151">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Toolkit di Azure per Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Toolkit di Azure per IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->

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
