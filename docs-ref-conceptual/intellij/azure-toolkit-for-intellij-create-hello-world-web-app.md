---
title: Creare un'app Web Hello World per Servizio app di Azure con IntelliJ
description: Questa esercitazione spiega come usare Azure Toolkit per IntelliJ per creare un'app Web Hello World per Azure.
services: app-service
keywords: java, intellij, app web, servizio app di azure, hello world, avvio rapido
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
ms.openlocfilehash: ae0749ce1ddab971f1a83e2e5e58492fd8ccb287
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626110"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a><span data-ttu-id="b70d5-104">Creare un'app Web Hello World per Servizio app di Azure con IntelliJ</span><span class="sxs-lookup"><span data-stu-id="b70d5-104">Create a Hello World web app for Azure App Service using IntelliJ</span></span>

<span data-ttu-id="b70d5-105">Usando il plug-in open source [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053), è possibile creare e distribuire in pochi minuti un'applicazione Hello World di base come app Web in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="b70d5-105">Using open sourced [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="b70d5-106">Se si preferisce usare Eclipse, vedere l'[esercitazione simile per Eclipse][eclipse-hello-world].</span><span class="sxs-lookup"><span data-stu-id="b70d5-106">If you prefer using Eclipse, check out our [similar tutorial for Eclipse][eclipse-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="b70d5-107">Dopo aver completato questa esercitazione, non dimenticare di pulire le risorse.</span><span class="sxs-lookup"><span data-stu-id="b70d5-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="b70d5-108">In tal modo, con l'esecuzione di questa guida non si supererà la quota dell'account gratuito.</span><span class="sxs-lookup"><span data-stu-id="b70d5-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="b70d5-109">Installazione e accesso</span><span class="sxs-lookup"><span data-stu-id="b70d5-109">Installation and Sign-in</span></span>

1. <span data-ttu-id="b70d5-110">Nella finestra di dialogo Settings/Preferences (Impostazioni/Preferenze) (CTRL+ALT+S) di IntelliJ IDEA selezionare **Plugins**.</span><span class="sxs-lookup"><span data-stu-id="b70d5-110">In IntelliJ IDEA's Settings/Preferences dialog (Ctrl+Alt+S), select **Plugins**.</span></span> <span data-ttu-id="b70d5-111">Quindi individuare **Azure Toolkit for IntelliJ** nel **Marketplace** e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="b70d5-111">Then, find the **Azure Toolkit for IntelliJ** in the **Marketplace** and click **Install**.</span></span> <span data-ttu-id="b70d5-112">Dopo l'installazione, fare clic su **Riavvia** per attivare il plug-in.</span><span class="sxs-lookup"><span data-stu-id="b70d5-112">After installed, click **Restart** to activate the plugin.</span></span> 

   ![Plug-in Azure Toolkit for IntelliJ in Marketplace][marketplace]

2. <span data-ttu-id="b70d5-114">Per accedere all'account Azure, aprire **Azure Explorer** sulla barra laterale e quindi fare clic su **Azure Sign In** (Accesso ad Azure) sulla barra superiore. Oppure scegliere **Tools/Azure/Azure Sign in** (Strumenti/Azure/Accesso ad Azure) dal menu IDEA.</span><span class="sxs-lookup"><span data-stu-id="b70d5-114">To sign in to your Azure account, open sidebar **Azure Explorer**, and then click the **Azure Sign In** icon in the bar on top (or from IDEA menu **Tools/Azure/Azure Sign in**).</span></span>

   ![Comando di accesso ad Azure in IntelliJ][I01]

3. <span data-ttu-id="b70d5-116">Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ([altre opzioni di accesso](azure-toolkit-for-intellij-sign-in-instructions.md)).</span><span class="sxs-lookup"><span data-stu-id="b70d5-116">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign in options](azure-toolkit-for-intellij-sign-in-instructions.md)).</span></span>

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

4. <span data-ttu-id="b70d5-118">Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).</span><span class="sxs-lookup"><span data-stu-id="b70d5-118">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![Finestra di dialogo di accesso ad Azure][I03]

5. <span data-ttu-id="b70d5-120">Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="b70d5-120">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![Accesso al dispositivo nel browser][I04]

6. <span data-ttu-id="b70d5-122">Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b70d5-122">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="b70d5-124">Creazione del progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="b70d5-124">Creating web app project</span></span>

1. <span data-ttu-id="b70d5-125">In IntelliJ scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).</span><span class="sxs-lookup"><span data-stu-id="b70d5-125">In IntelliJ, click the **File** menu, then click **New**, and then click **Project**.</span></span>

   ![Creare un nuovo progetto][file-new-project]

2. <span data-ttu-id="b70d5-127">Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven**, **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="b70d5-127">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>

   ![Scegliere l'app Web di sistema Maven][maven-archetype-webapp]

3. <span data-ttu-id="b70d5-129">Specificare i valori per **GroupId** e **ArtifactId** per l'app Web e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="b70d5-129">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>

   ![Specificare GroupId e ArtifactId][groupid-and-artifactid]

4. <span data-ttu-id="b70d5-131">Personalizzare qualsiasi impostazione per Maven o accettare quelle predefinite e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="b70d5-131">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>

   ![Specificare le impostazioni per Maven][maven-options]

5. <span data-ttu-id="b70d5-133">Specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="b70d5-133">Specify your project name and location, and then click **Finish**.</span></span>

   ![Specificare il nome del progetto][project-name]

6. <span data-ttu-id="b70d5-135">Nella visualizzazione Project Explorer (Esplora progetto) aprire e modificare il file **src/main/webapp/index.jsp** come indicato di seguito e quindi **salvare le modifiche**:</span><span class="sxs-lookup"><span data-stu-id="b70d5-135">Under Project Explorer view, open and edit the file **src/main/webapp/index.jsp** as following and **save the changes**:</span></span>

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![Modifica della pagina di indice][edit-index-page]

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="b70d5-137">Distribuzione dell'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="b70d5-137">Deploying web app to Azure</span></span>

1. <span data-ttu-id="b70d5-138">Nella visualizzazione Project Explorer fare clic con il pulsante destro del mouse sul progetto, espandere **Azure** e quindi fare clic su **Deploy to Azure** (Distribuisci in Azure).</span><span class="sxs-lookup"><span data-stu-id="b70d5-138">Under Project Explorer view, right-click your project, expand **Azure**, then click **Deploy to Azure**.</span></span>

   ![Menu Deploy to Azure][deploy-to-azure-menu]

1. <span data-ttu-id="b70d5-140">Nella finestra di dialogo Deploy to Azure è possibile distribuire l'applicazione direttamente in un'app Web Tomcat esistente, se disponibile; in caso contrario, sarà necessario crearne prima una nuova.</span><span class="sxs-lookup"><span data-stu-id="b70d5-140">In the Deploy to Azure dialog box, you can directly deploy the application to an existing Tomcat webapp if you already have one, otherwise you should create a new one first.</span></span>
   1. <span data-ttu-id="b70d5-141">Fare clic sul collegamento **No Available webapp, click to create a new one** (Nessuna app Web disponibile, fare clic per crearne una nuova) per creare una nuova app Web oppure scegliere **Create New WebApp** (Crea nuova app Web) dal menu a discesa WebApp (App Web) se sono disponibili app Web nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="b70d5-141">Click the link **No Available webapp, click to create a new one** to crete a new web app, you could choose **Create New WebApp** from WebApp dropdown if there are existing webapps in your subscription.</span></span>

      ![Finestra di dialogo Deploy to Azure][deploy-to-azure-dialog]

   1. <span data-ttu-id="b70d5-143">Nella finestra di dialogo popup scegliere **TOMCAT 8.5-jre8** come contenitore Web e specificare le altre informazioni necessarie, quindi fare clic su **OK** per creare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="b70d5-143">In the pop-up dialog box, chose **TOMCAT 8.5-jre8** as Web Container and specify other required information, then click **OK** to create the webapp.</span></span>

      ![Create New App (Crea nuova app)][create-new-web-app-dialog]

   1. <span data-ttu-id="b70d5-145">Scegliere l'app Web dal menu WebApp e quindi fare clic su **Run** (Esegui). In alternativa, è possibile iniziare da qui se si vuole eseguire la distribuzione in un'app Web esistente.</span><span class="sxs-lookup"><span data-stu-id="b70d5-145">Choose the web app from WebApp drop down, and then click **Run**.(You could start from here if you want deploy to an existing webapp)</span></span>

      ![Distribuire in un'app Web esistente][deploy-to-existing-webapp]

1. <span data-ttu-id="b70d5-147">Dopo la distribuzione dell'app Web, il toolkit visualizzerà un messaggio di stato insieme all'URL dell'app Web distribuita, se l'operazione è riuscita.</span><span class="sxs-lookup"><span data-stu-id="b70d5-147">The toolkit will display a status message when it has successfully deployed your web app, along with the URL of your deployed web app if succeed.</span></span>

   ![Distribuzione completata][successfully-deployed]

1. <span data-ttu-id="b70d5-149">È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.</span><span class="sxs-lookup"><span data-stu-id="b70d5-149">You can browse to your web app using the link provided in the status message.</span></span>

   ![Collegamento all'app Web][browse-web-app]

## <a name="managing-deploy-configurations"></a><span data-ttu-id="b70d5-151">Gestione delle configurazioni della distribuzione</span><span class="sxs-lookup"><span data-stu-id="b70d5-151">Managing deploy configurations</span></span>

1. <span data-ttu-id="b70d5-152">Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire la distribuzione facendo clic sull'icona della freccia verde sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="b70d5-152">After you have published your web app, your settings will be saved as the default, and you can run the deployment by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="b70d5-153">È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).</span><span class="sxs-lookup"><span data-stu-id="b70d5-153">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. <span data-ttu-id="b70d5-155">Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b70d5-155">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a><span data-ttu-id="b70d5-157">Pulizia delle risorse</span><span class="sxs-lookup"><span data-stu-id="b70d5-157">Cleaning up resources</span></span>

1. <span data-ttu-id="b70d5-158">Eliminazione delle app Web in Azure Explorer</span><span class="sxs-lookup"><span data-stu-id="b70d5-158">Deleting Web Apps in Azure Explorer</span></span>

     ![Pulire le risorse][clean-resources]

## <a name="next-steps"></a><span data-ttu-id="b70d5-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b70d5-160">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="b70d5-161">Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].</span><span class="sxs-lookup"><span data-stu-id="b70d5-161">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

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
[marketplace]:./media/azure-toolkit-for-intellij-create-hello-world-web-app/marketplace.png
[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/clean-resource.png
[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png
