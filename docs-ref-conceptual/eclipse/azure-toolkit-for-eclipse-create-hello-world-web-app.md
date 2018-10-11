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
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a>Creare un'app Web Hello World per Azure con Eclipse

Questa esercitazione descrive come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Toolkit di Azure per Eclipse].

> [!NOTE]
>
> Per una versione di questo articolo che fa uso di [Toolkit di Azure per IntelliJ], vedere [Creare un'app Web Hello World per Azure con IntelliJ][intellij-hello-world].
>

> [!IMPORTANT]
> 
> Azure Toolkit for Eclipse è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso. Questo articolo descrive come creare un'app Web Hello World usando la versione 3.0.7 (o versioni successive) di Azure Toolkit for Eclipse. Se si usa la versione 3.0.6 (o versioni precedenti) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con il toolkit legacy per Eclipse][Legacy Version].
> 

Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:

![Anteprima dell'app Hello World][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>Creare un nuovo progetto di app Web

1. Avviare Eclipse e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for Eclipse][eclipse-sign-in-instructions].

1. Fare clic su **File**, **New** (Nuovo) e quindi su **Dynamic Web Project** (Progetto Web dinamico). Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...**, espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.

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

## <a name="deploy-your-web-app-to-azure"></a>Distribuire l'app Web in Azure

1. Nella visualizzazione Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).
   
   ![Publish as Azure Web App][publish-as-azure-web-app]

1. Quando viene visualizzata la finestra di dialogo **Deploy Web App** (Distribuisci app Web) è possibile scegliere una delle opzioni seguenti:

   * Selezionare un'app Web esistente, se presente.

      ![Selezionare un servizio app][select-app-service]

   * Fare clic su **Create New App** (Crea nuova app).

      ![Creare un servizio app][create-app-service]

      Specificare le informazioni necessarie per l'app Web nella finestra di dialogo **Create App Service** (Crea servizio app) e quindi fare clic su **Create** (Crea).

      ![Finestra di dialogo Crea servizio app][create-app-service-dialog]

1. Selezionare l'app Web e quindi fare clic su **Deploy** (Distribuisci).

   ![Distribuire il servizio app][deploy-app-service]

1. Dopo aver distribuito l'app Web, il toolkit visualizza lo stato come **Pubblicato** sotto la scheda **Log attività di Azure**, come collegamento ipertestuale all'URL dell'app Web distribuita.

   ![Stato della pubblicazione][publish-status]

1. È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.

   ![Collegamento all'app Web][browse-web-app]

1. Dopo aver pubblicato l'app Web in Azure, è possibile gestirla facendo clic su di essa con il pulsante destro del mouse e selezionando una delle opzioni del menu di scelta rapida. Ad esempio, è possibile **avviare**, **arrestare** o **eliminare** l'app Web.

   ![Gestire il servizio app][manage-app-service]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Toolkit di Azure per Eclipse]: azure-toolkit-for-eclipse.md
[Toolkit di Azure per IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
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
