---
title: Creare un'app Web Hello World per Azure con IntelliJ
description: Questa esercitazione spiega come usare Azure Toolkit per IntelliJ per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 7055751d1b1c37e019ef4ed59f1710ce6905e9f8
ms.sourcegitcommit: a108a82414bd35be896e3c4e7047f5eb7b1518cb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489639"
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a><span data-ttu-id="c1e2b-103">Creare un'app Web Hello World per Azure con IntelliJ</span><span class="sxs-lookup"><span data-stu-id="c1e2b-103">Create a Hello World web app for Azure using IntelliJ</span></span>

<span data-ttu-id="c1e2b-104">Questa esercitazione spiega come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Azure Toolkit for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="c1e2b-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
>
> <span data-ttu-id="c1e2b-105">Per una versione di questo articolo che fa uso di [Azure Toolkit for Eclipse], vedere [Creare un'app Web Hello World per Azure con Eclipse][eclipse-hello-world].</span><span class="sxs-lookup"><span data-stu-id="c1e2b-105">For a version of this article that uses the [Azure Toolkit for Eclipse], see [Create a Hello World web app for Azure using Eclipse][eclipse-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="c1e2b-106">Azure Toolkit for IntelliJ è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-106">The Azure Toolkit for IntelliJ was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="c1e2b-107">Questo articolo descrive la creazione di un'app Web Hello World tramite la versione 3.0.7 (o versioni successive) di Azure Toolkit for IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="c1e2b-108">Se si usa la versione 3.0.6 (o versioni precedenti) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con il toolkit legacy per IntelliJ][Legacy Version].</span><span class="sxs-lookup"><span data-stu-id="c1e2b-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in IntelliJ using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="c1e2b-109">Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c1e2b-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Anteprima dell'app Hello World][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="c1e2b-111">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="c1e2b-111">Create a new web app project</span></span>

1. <span data-ttu-id="c1e2b-112">Avviare IntelliJ e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions].</span><span class="sxs-lookup"><span data-stu-id="c1e2b-112">Start IntelliJ, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="c1e2b-113">Scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-113">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![Creare un nuovo progetto][file-new-project]

1. <span data-ttu-id="c1e2b-115">Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven**, **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-115">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![Scegliere l'app Web di sistema Maven][maven-archetype-webapp]
   
1. <span data-ttu-id="c1e2b-117">Specificare i valori per **GroupId** e **ArtifactId** per l'app Web e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-117">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![Specificare GroupId e ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="c1e2b-119">Personalizzare qualsiasi impostazione per Maven o accettare quelle predefinite e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-119">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![Specificare le impostazioni per Maven][maven-options]

1. <span data-ttu-id="c1e2b-121">Specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-121">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![Specificare il nome del progetto][project-name]

1. <span data-ttu-id="c1e2b-123">Nella visualizzazione Project Explorer (Esplora progetti) di IntelliJ espandere **src**, **main**, **webapp** e quindi fare doppio clic su **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-123">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![Apertura della pagina di indice][open-index-page]

1. <span data-ttu-id="c1e2b-125">Quando viene aperto il file index.jsp in IntelliJ, aggiungere testo in modo da visualizzare dinamicamente **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="c1e2b-125">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="c1e2b-126">all'interno dell'elemento `<body>` esistente.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-126">within the existing `<body>` element.</span></span> <span data-ttu-id="c1e2b-127">Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c1e2b-127">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![Modifica della pagina di indice][edit-index-page]

1. <span data-ttu-id="c1e2b-129">Salvare index.jsp.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-129">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="c1e2b-130">Distribuire l'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="c1e2b-130">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="c1e2b-131">Nella visualizzazione Project Explorer (Esplora progetti) di IntelliJ fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Run on Web App** (Esegui su app Web).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-131">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![Menu Run on Web App (Esegui su app Web)][run-on-web-app-menu]

1. <span data-ttu-id="c1e2b-133">Nella finestra di dialogo Run on Web App (Esegui su app Web) è possibile scegliere una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1e2b-133">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="c1e2b-134">Scegliere un'app esistente (se presente) e quindi fare clic su **Run** (Esegui).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-134">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![Finestra di dialogo Run on Web App (Esegui su app Web)][run-on-web-app-dialog]

   * <span data-ttu-id="c1e2b-136">Fare clic su **Create New Web App** (Crea una nuova app Web) dal menu a discesa App Web.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-136">Click **Create New Web App** from WebApp dropdown.</span></span> <span data-ttu-id="c1e2b-137">Se si sceglie di creare una nuova app Web, specificare le informazioni necessarie per l'app Web e quindi fare clic su **Run** (Esegui) dopo aver creato l'app Web.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-137">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run** after web app creation.</span></span>

      ![Create New App (Crea nuova app)][create-new-web-app-dialog]

1. <span data-ttu-id="c1e2b-139">Il toolkit visualizzerà un messaggio di stato dopo aver distribuito l'app Web, che conterrà anche l'URL dell'app Web distribuita.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-139">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![Distribuzione completata][successfully-deployed]

1. <span data-ttu-id="c1e2b-141">È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-141">You can browse to your web app using the link provided in the status message.</span></span>

   ![Collegamento all'app Web][browse-web-app]

1. <span data-ttu-id="c1e2b-143">Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire l'applicazione su Azure facendo clic sulla freccia verde sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-143">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="c1e2b-144">È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).</span><span class="sxs-lookup"><span data-stu-id="c1e2b-144">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. <span data-ttu-id="c1e2b-146">Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1e2b-146">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="c1e2b-148">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c1e2b-148">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="c1e2b-149">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="c1e2b-149">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

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
