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
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a>Creare un'app Web Hello World per Servizio app di Azure con Eclipse

Usando il plug-in open source [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse), è possibile creare e distribuire in pochi minuti un'applicazione Hello World di base come app Web in Servizio app di Azure.

> [!NOTE]
>
> Se si preferisce usare IntelliJ IDEA, vedere l'[esercitazione simile per IntelliJ][intellij-hello-world].
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> Dopo aver completato questa esercitazione, non dimenticare di pulire le risorse. In tal modo, con l'esecuzione di questa guida non si supererà la quota dell'account gratuito.
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>Installazione e accesso

1. Trascinare il pulsante seguente nell'area di lavoro di Eclipse in esecuzione per installare il plug-in Azure Toolkit for Eclipse ([altre opzioni di installazione](azure-toolkit-for-eclipse-installation.md)).

    [![Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace")

1. Per accedere all'account Azure, fare clic su **Tools** (Strumenti), su **Azure** e quindi su **Sign In** (Accedi).
   ![Menu di Eclipse per l'accesso ad Azure][I01]

1. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ([altre opzioni di accesso](azure-toolkit-for-eclipse-sign-in-instructions.md)).

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

   ![Finestra di dialogo di accesso ad Azure][I03]

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

   ![Accesso al dispositivo nel browser][I04]

1. Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="creating-web-app-project"></a>Creazione del progetto di app Web

1. Fare clic su **File**, **New** (Nuovo) e quindi su **Dynamic Web Project** (Progetto Web dinamico). Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...** , espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.

   ![Creazione di un nuovo progetto Web dinamico][file-new-dynamic-web-project]

2. Ai fini di questa esercitazione, denominare il progetto **MyWebApp**. L'aspetto della schermata sarà simile al seguente:
   
   ![Proprietà del nuovo progetto Web dinamico][dynamic-web-project-properties]

3. Fare clic su **Fine**.

4. Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse espandere **MyWebApp**. Fare clic con il pulsante destro del mouse su **WebContent**, scegliere **New** e quindi fare clic su **JSP File**.

   ![Creare un nuovo file JSP][create-new-jsp-file]

5. Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).

   ![Finestra di dialogo New JSP File (Nuovo file JSP)][new-jsp-file-dialog]

6. Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html)** (Nuovo file JSP - html), quindi fare clic su **Finish** (Fine).

   ![Selezionare il modello JSP][select-jsp-template]

7. Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!** all'interno dell'elemento `<body>` esistente. Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. Salvare index.jsp.

## <a name="deploying-web-app-to-azure"></a>Distribuzione dell'app Web in Azure

1. Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).
   
   ![Publish as Azure Web App][publish-as-azure-web-app]

1. Quando viene visualizzata la finestra di dialogo **Deploy Web App** (Distribuisci app Web) è possibile scegliere una delle opzioni seguenti:

   * Selezionare un'app Web esistente, se presente.

      ![Selezionare un servizio app][select-app-service]

   * Fare clic su **Create New App** (Crea nuova app).

      ![Creare un servizio app][create-app-service]

      Specificare le informazioni necessarie per l'app Web nella finestra di dialogo **Create App Service** (Crea servizio app) e quindi fare clic su **Create** (Crea).

      In questa finestra di dialogo è possibile configurare l'ambiente di runtime, le impostazioni dell'app, il piano di servizio e il gruppo di risorse.

      ![Finestra di dialogo Crea servizio app][create-app-service-dialog]

1. Selezionare l'app Web e quindi fare clic su **Deploy** (Distribuisci).

   ![Distribuire il servizio app][deploy-app-service]

1. Dopo aver distribuito l'app Web, il toolkit visualizza lo stato come **Pubblicato** sotto la scheda **Log attività di Azure**, come collegamento ipertestuale all'URL dell'app Web distribuita.

   ![Stato della pubblicazione][publish-status]

1. È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.

   ![Collegamento all'app Web][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a>Pulizia delle risorse

1. Dopo aver pubblicato l'app Web in Azure, è possibile gestirla facendo clic con il pulsante destro del mouse in Azure Explorer e scegliendo una delle opzioni del menu di scelta rapida. È ad esempio possibile **eliminare** l'app Web per pulire le risorse per l'esercitazione.

   ![Gestire il servizio app][manage-app-service]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
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
