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
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>Creare un'app Web Hello World per Servizio app di Azure con IntelliJ

Usando il plug-in open source [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053), è possibile creare e distribuire in pochi minuti un'applicazione Hello World di base come app Web in Servizio app di Azure.

> [!NOTE]
>
> Se si preferisce usare Eclipse, vedere l'[esercitazione simile per Eclipse][eclipse-hello-world].
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> Dopo aver completato questa esercitazione, non dimenticare di pulire le risorse. In tal modo, con l'esecuzione di questa guida non si supererà la quota dell'account gratuito.
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>Installazione e accesso

1. Nella finestra di dialogo Settings/Preferences (Impostazioni/Preferenze) (CTRL+ALT+S) di IntelliJ IDEA selezionare **Plugins**. Quindi individuare **Azure Toolkit for IntelliJ** nel **Marketplace** e fare clic su **Installa**. Dopo l'installazione, fare clic su **Riavvia** per attivare il plug-in. 

   ![Plug-in Azure Toolkit for IntelliJ in Marketplace][marketplace]

2. Per accedere all'account Azure, aprire **Azure Explorer** sulla barra laterale e quindi fare clic su **Azure Sign In** (Accesso ad Azure) sulla barra superiore. Oppure scegliere **Tools/Azure/Azure Sign in** (Strumenti/Azure/Accesso ad Azure) dal menu IDEA.

   ![Comando di accesso ad Azure in IntelliJ][I01]

3. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ([altre opzioni di accesso](azure-toolkit-for-intellij-sign-in-instructions.md)).

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

4. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

   ![Finestra di dialogo di accesso ad Azure][I03]

5. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

   ![Accesso al dispositivo nel browser][I04]

6. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="creating-web-app-project"></a>Creazione del progetto di app Web

1. In IntelliJ scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).

   ![Creare un nuovo progetto][file-new-project]

2. Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven**, **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).

   ![Scegliere l'app Web di sistema Maven][maven-archetype-webapp]

3. Specificare i valori per **GroupId** e **ArtifactId** per l'app Web e quindi fare clic su **Next** (Avanti).

   ![Specificare GroupId e ArtifactId][groupid-and-artifactid]

4. Personalizzare qualsiasi impostazione per Maven o accettare quelle predefinite e quindi fare clic su **Next** (Avanti).

   ![Specificare le impostazioni per Maven][maven-options]

5. Specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).

   ![Specificare il nome del progetto][project-name]

6. Nella visualizzazione Project Explorer (Esplora progetto) aprire e modificare il file **src/main/webapp/index.jsp** come indicato di seguito e quindi **salvare le modifiche**:

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![Modifica della pagina di indice][edit-index-page]

## <a name="deploying-web-app-to-azure"></a>Distribuzione dell'app Web in Azure

1. Nella visualizzazione Project Explorer fare clic con il pulsante destro del mouse sul progetto, espandere **Azure** e quindi fare clic su **Deploy to Azure** (Distribuisci in Azure).

   ![Menu Deploy to Azure][deploy-to-azure-menu]

1. Nella finestra di dialogo Deploy to Azure è possibile distribuire l'applicazione direttamente in un'app Web Tomcat esistente, se disponibile; in caso contrario, sarà necessario crearne prima una nuova.
   1. Fare clic sul collegamento **No Available webapp, click to create a new one** (Nessuna app Web disponibile, fare clic per crearne una nuova) per creare una nuova app Web oppure scegliere **Create New WebApp** (Crea nuova app Web) dal menu a discesa WebApp (App Web) se sono disponibili app Web nella sottoscrizione.

      ![Finestra di dialogo Deploy to Azure][deploy-to-azure-dialog]

   1. Nella finestra di dialogo popup scegliere **TOMCAT 8.5-jre8** come contenitore Web e specificare le altre informazioni necessarie, quindi fare clic su **OK** per creare l'app Web.

      ![Create New App (Crea nuova app)][create-new-web-app-dialog]

   1. Scegliere l'app Web dal menu WebApp e quindi fare clic su **Run** (Esegui). In alternativa, è possibile iniziare da qui se si vuole eseguire la distribuzione in un'app Web esistente.

      ![Distribuire in un'app Web esistente][deploy-to-existing-webapp]

1. Dopo la distribuzione dell'app Web, il toolkit visualizzerà un messaggio di stato insieme all'URL dell'app Web distribuita, se l'operazione è riuscita.

   ![Distribuzione completata][successfully-deployed]

1. È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.

   ![Collegamento all'app Web][browse-web-app]

## <a name="managing-deploy-configurations"></a>Gestione delle configurazioni della distribuzione

1. Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire la distribuzione facendo clic sull'icona della freccia verde sulla barra degli strumenti. È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a>Pulizia delle risorse

1. Eliminazione delle app Web in Azure Explorer

     ![Pulire le risorse][clean-resources]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
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
