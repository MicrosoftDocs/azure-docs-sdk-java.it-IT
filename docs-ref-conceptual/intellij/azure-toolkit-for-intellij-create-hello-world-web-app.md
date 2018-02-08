---
title: Creare un'app Web Hello World per Azure con IntelliJ
description: Questa esercitazione spiega come usare Azure Toolkit per IntelliJ per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: cc68e16a6940a1f0f2b08f0b63c90c58ec6dbc4e
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a>Creare un'app Web Hello World per Azure con IntelliJ

Questa esercitazione spiega come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Azure Toolkit per IntelliJ].

> [!NOTE]
>
> Per una versione di questo articolo che fa uso di [Azure Toolkit for Eclipse], vedere [Creare un'app Web Hello World per Azure con Eclipse][eclipse-hello-world].
>

> [!IMPORTANT]
> 
> Azure Toolkit for IntelliJ è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso. Questo articolo descrive la creazione di un'app Web Hello World tramite la versione 3.0.7 (o versioni successive) di Azure Toolkit for IntelliJ. Se si usa la versione 3.0.6 (o versioni precedenti) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con il toolkit legacy per IntelliJ][Legacy Version].
> 

Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:

![Anteprima dell'app Hello World][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>Creare un nuovo progetto di app Web

1. Avviare IntelliJ e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions].

1. Scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).
   
   ![Creare un nuovo progetto][file-new-project]

1. Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven**, **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).
   
   ![Scegliere l'app Web di sistema Maven][maven-archetype-webapp]
   
1. Specificare i valori per **GroupId** e **ArtifactId** per l'app Web e quindi fare clic su **Next** (Avanti).
   
   ![Specificare GroupId e ArtifactId][groupid-and-artifactid]

1. Personalizzare qualsiasi impostazione per Maven o accettare quelle predefinite e quindi fare clic su **Next** (Avanti).
   
   ![Specificare le impostazioni per Maven][maven-options]

1. Specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).
   
   ![Specificare il nome del progetto][project-name]

1. Nella visualizzazione Project Explorer (Esplora progetti) di IntelliJ espandere **src**, **main**, **webapp** e quindi fare doppio clic su **index.jsp**.
   
   ![Apertura della pagina di indice][open-index-page]

1. Quando viene aperto il file index.jsp in IntelliJ, aggiungere testo in modo da visualizzare dinamicamente **Hello World!** all'interno dell'elemento `<body>` esistente. Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![Modifica della pagina di indice][edit-index-page]

1. Salvare index.jsp.

## <a name="deploy-your-web-app-to-azure"></a>Distribuire l'app Web in Azure

1. Nella visualizzazione Project Explorer (Esplora progetti) di IntelliJ fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure**, quindi fare clic su **Run on Web App** (Esegui su app Web).
   
   ![Menu Run on Web App (Esegui su app Web)][run-on-web-app-menu]

1. Nella finestra di dialogo Run on Web App (Esegui su app Web) è possibile scegliere una delle opzioni seguenti:

   * Scegliere un'app esistente (se presente) e quindi fare clic su **Run** (Esegui).

      ![Finestra di dialogo Run on Web App (Esegui su app Web)][run-on-web-app-dialog]

   * Fare clic su **Create New App** (Crea nuova app). Se si sceglie di creare una nuova app Web, specificare le informazioni necessarie per l'app Web e quindi fare clic su **Run** (Esegui).

      ![Create New App (Crea nuova app)][create-new-web-app-dialog]

1. Il toolkit visualizzerà un messaggio di stato dopo aver distribuito l'app Web, che conterrà anche l'URL dell'app Web distribuita.

   ![Distribuzione completata][successfully-deployed]

1. È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.

   ![Collegamento all'app Web][browse-web-app]

1. Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire l'applicazione su Azure facendo clic sulla freccia verde sulla barra degli strumenti. È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Azure Toolkit per IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Panoramica delle App Web]: /azure/app-service/app-service-web-overview
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
