---
title: ''
description: Questa esercitazione descrive come usare Azure Toolkit for Eclipse 3.0.6 (o versioni precedenti) per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/13/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: b05dcd52f36524ab17652f83c6ced4006f874365
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338715"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-eclipse"></a>Creare un'app Web Hello World per Azure con il toolkit legacy per Eclipse

Questa esercitazione descrive come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Azure Toolkit for Eclipse] 3.0.6 (o versioni precedenti).

> [!NOTE]
>
> Per una versione di questo articolo che fa uso di [Azure Toolkit for IntelliJ], vedere [Creare un'app Web Hello World per Azure con IntelliJ][intellij-hello-world].
>

> [!IMPORTANT]
> 
> Azure Toolkit for Eclipse è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso. Questo articolo descrive come creare un'app Web Hello World usando la versione 3.0.6 (o versioni precedenti) di Azure Toolkit for Eclipse. Se si usa la versione 3.0.7 (o versioni successive) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con Eclipse][Updated Version].
>

Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:

![Anteprima dell'app Hello World][01]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>Creare un nuovo progetto di app Web

1. Avviare Eclipse e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for Eclipse][eclipse-sign-in-instructions].

1. Fare clic su **File**, **New** (Nuovo) e quindi su **Dynamic Web Project** (Progetto Web dinamico). Se **Dynamic Web Project** non è elencato tra i progetti disponibili dopo aver fatto clic su **File** e **New**, fare clic su **File**, **New**, **Project...**, espandere **Web**, fare clic su **Dynamic Web Project** e fare clic su **Next**.

2. Ai fini di questa esercitazione, denominare il progetto **MyWebApp**. L'aspetto della schermata sarà simile al seguente:
   
   ![Creazione di un nuovo progetto Web dinamico][02]

3. Fare clic su **Fine**.

4. Nella visualizzazione **Project Explorer** (Esplora progetti) di Eclipse espandere **MyWebApp**. Fare clic con il pulsante destro del mouse su **WebContent**, scegliere **New** e quindi fare clic su **JSP File**.

5. Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).

6. Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html)** (Nuovo file JSP - html), quindi fare clic su **Finish** (Fine).

7. Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!** all'interno dell'elemento `<body>` esistente. Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. Salvare index.jsp.

## <a name="deploy-your-web-app-to-azure"></a>Distribuire l'app Web in Azure

Esistono diversi modi con cui è possibile distribuire un'applicazione Web Java in Azure. Questa esercitazione descrive uno dei modi più semplici: l'applicazione viene distribuita in un contenitore di app Web di Azure senza richiedere tipi di progetto specifici o strumenti aggiuntivi. JDK e il software del contenitore Web vengono forniti automaticamente da Azure senza la necessità di eseguire alcun caricamento. È sufficiente disporre dell'app Web Java. Di conseguenza, il processo di pubblicazione per l'applicazione richiederà alcuni secondi, non minuti.

1. In Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **MyWebApp**.

2. Dal menu di scelta rapida selezionare **Azure**, quindi fare clic su **Publish as Azure Web App** (Pubblica come App Web di Azure).
   
   ![Publish as Azure Web App][03]
   
   In alternativa, mentre in Project Explorer (Esplora progetti) è selezionato il progetto di applicazione Web, è possibile fare clic sul pulsante a discesa **Publish** (Pubblica) nella barra degli strumenti e selezionare **Publish as Azure Web App** (Pubblica come App Web di Azure) da qui:
   
   ![Publish as Azure Web App][14]

3. Se non si è già eseguito l'accesso ad Azure da Eclipse, verrà richiesto di accedere all'account Azure:
   
   ![Finestra di dialogo di accesso di Azure][04]
   
   Se si hanno più account Azure, durante il processo di accesso alcune richieste, all'apparenza identiche, possono essere visualizzate più volte, ognuna per un account diverso. In questo caso, continuare a seguire le istruzioni di accesso.

4. Dopo aver eseguito l'accesso all'account Azure, nella finestra di dialogo **Manage Subscriptions** (Gestisci sottoscrizioni) viene visualizzato un elenco delle sottoscrizioni associate alle credenziali usate. Se sono elencate più sottoscrizioni e se ne vogliono usare solo alcune, è possibile deselezionare le sottoscrizioni che non si intende usare. Dopo aver selezionato le sottoscrizioni, fare clic su **Close**(Chiudi).
   
   ![Finestra di dialogo Gestisci sottoscrizioni][05]

5. Nella finestra di dialogo **Deploy to Azure Web App Container** (Distribuisci in un contenitore app Web di Azure) sono visualizzati tutti i contenitori di app Web creati in precedenza. Se non è stato creato alcun contenitore, l'elenco appare vuoto.
   
   ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][06]

6. Se non è stato creato alcun contenitore di app Web di Azure in precedenza o se si desidera pubblicare l'applicazione in un nuovo contenitore, attenersi alla procedura seguente. In caso contrario, selezionare un contenitore di app Web esistente e andare al passaggio 7.
   
   a. Fare clic su **Nuovo**
      
      ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][15]

   b. Verrà visualizzata la finestra di dialogo **New Web App Container** (Nuovo contenitore App Web):
      
      ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][07a]

   c. In **DNS Label** (Etichetta DNS) specificare un'etichetta per il contenitore di app Web. Questa sarà l'etichetta DNS foglia dell'URL dell'host per l'applicazione Web in Azure. Si noti che il nome deve essere disponibile e conforme ai requisiti di denominazione delle app Web di Azure.

   d. Nel menu a discesa **Contenitore Web** selezionare il software appropriato per l'applicazione.
      
      Attualmente è possibile scegliere fra Tomcat 8, Tomcat 7 o Jetty 9. Una distribuzione recente del software selezionato verrà offerta da Azure ed eseguita in una distribuzione recente di JDK fornita da Azure.

   e. Nel menu a discesa **Subscription** (Sottoscrizione) selezionare la sottoscrizione che si vuole usare per la distribuzione.

   f. Nel menu a discesa **Resource Group** (Gruppo di risorse) selezionare il gruppo di risorse a cui si vuole associare l'app Web. I gruppi di risorse di Azure consentono di raggruppare le risorse correlate in modo che, ad esempio, possano essere eliminate insieme.
      
      È possibile selezionare un gruppo di risorse esistente, se presente, e andare al passaggio g seguente o usare questa procedura per creare un nuovo gruppo di risorse:
      
   * Fare clic su **Nuovo**
   * Verrà visualizzata la finestra di dialogo **Nuovo gruppo di risorse** :
        
       ![Finestra di dialogo Nuovo gruppo di risorse][08]
   * Nella casella di testo **Nome** specificare un nome per il nuovo gruppo di risorse.
   * Nel menu a discesa **Region** (Area) selezionare il percorso del data center di Azure appropriato per il gruppo di risorse.
   * FACOLTATIVO: per impostazione predefinita, una distribuzione recente di Java 8 verrà distribuita da Azure automaticamente nel contenitore di app web come JVM. Tuttavia, è possibile specificare una versione e una distribuzione di JVM diversa, se richiesto dall'app Web. Per specificare la versione JDK per l'app Web specifica, fare clic sulla scheda **JDK** e selezionare una delle opzioni seguenti:
     * **Deploy the default JDK offered by Azure Web Apps service** (Distribuisci JDK predefinito fornito dal servizio app Web di Azure): questa opzione distribuirà una distribuzione recente di Java.
     * **Deploy a 3rd party JDK available on Azure**(Distribuisci JDK di terze parti disponibile in Azure): questa opzione consente di scegliere dall'elenco di JDK forniti da Microsoft Azure.
     * **Deploy my own JDK from this download location**(Distribuisci JDK personalizzato da questo percorso di download): questa opzione consente di specificare una distribuzione JDK personalizzata, che deve essere compressa come file ZIP e caricata in un percorso di download disponibile pubblicamente o in un account di Archiviazione di Azure per cui si dispone dell'accesso.
          
       ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][07b]

   g. Fare clic su **OK**.

   h. Il menu a discesa **Piano di servizio app** elenca i piani di servizio app associati al gruppo di risorse selezionato. I piani di servizio app specificano informazioni come il percorso dell'app Web, il piano tariffario e le dimensioni dell'istanza di calcolo. È possibile usare un singolo piano di servizio app per più app Web. Per questo motivo viene gestito separatamente da una distribuzione di app Web specifica.
      
       You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:
      
      * Fare clic su **Nuovo**
      * Verrà visualizzata la finestra di dialogo **Nuovo piano di servizio app** :
        
          ![Finestra di dialogo New App Service Plan (Nuovo piano di servizio app)][09]
      * Nella casella di testo **Nome** specificare un nome per il nuovo piano di servizio app.
      * Nel menu a discesa **Località** selezionare la località del data center di Azure appropriata per il piano.
      * Nel menu a discesa **Piano tariffario** selezionare la tariffa appropriata per il piano. Ai fini del test è possibile scegliere **Gratuito**.
      * Nel menu a discesa **Dimensioni istanza** selezionare le dimensioni dell'istanza appropriate per il piano. Ai fini del test è possibile scegliere **Piccolo**.

   i. Dopo aver completato tutti i passaggi precedenti, la finestra di dialogo New Web App Container dovrebbe essere simile alla seguente:
      
      ![Finestra di dialogo New Web App Container (Nuovo contenitore App Web)][10]

   j. Fare clic su **OK** per completare la creazione del nuovo contenitore di app Web.
       
      Attendere alcuni secondi che venga aggiornato l'elenco dei contenitori di app Web. Il contenitore di app Web appena creato risulterà selezionato nell'elenco.

7. A questo punto si è pronti completare la distribuzione iniziale dell'app Web in Azure:
   
   ![Finestra di dialogo Deploy to Azure Web App Container (Distribuisci in un contenitore app Web di Azure)][11]
   
   Fare clic su **OK** per distribuire l'applicazione Java nel contenitore di App Web selezionato.
   
   Per impostazione predefinita, l'applicazione verrà distribuita come sottodirectory del server applicazioni. Se si vuole distribuire l'applicazione come applicazione radice, selezionare la casella di controllo **Deploy to root** (Distribuisci a radice) prima di fare clic su **OK**.

8. Verrà quindi aperta la visualizzazione **Azure Activity Log** (Log attività di Azure) in cui è indicato lo stato della distribuzione dell'app Web.
   
   ![Azure Activity Log][12]
   
   Il processo di distribuzione dell'app Web in Azure dovrebbe richiedere solo alcuni secondi. Quando l'applicazione è pronta, viene visualizzato un collegamento denominato **Published** in the **Status** (Stato). Quando si fa clic sul collegamento, viene visualizzata la home page dell'app Web distribuita.

## <a name="updating-your-web-app"></a>Aggiornamento dell'app Web

L'aggiornamento di un'app Web di Azure in esecuzione è un processo semplice e rapido. Sono disponibili due opzioni per l'aggiornamento:

* È possibile aggiornare la distribuzione di un'app Web Java esistente.
* È possibile pubblicare un'applicazione Java aggiuntiva nello stesso contenitore di app Web.

In entrambi i casi, il processo è identico e richiede solo pochi secondi:

1. In Project Explorer in Eclipse, fare clic con il pulsante destro del mouse sull'applicazione Java che si desidera aggiornare o aggiungere a un contenitore di app Web esistente.

2. Dal menu di scelta rapida visualizzato selezionare **Azure**, quindi **Publish as Azure Web App** (Pubblica come App Web di Azure).

3. Poiché è già stato effettuato l'accesso in precedenza, verrà visualizzato un elenco dei contenitori di app Web esistenti. Selezionare il contenitore in cui si vuole pubblicare o ripubblicare l'applicazione Java e fare clic su **OK**.

Pochi secondi dopo nella visualizzazione **Azure Activity Log** (Log attività di Azure) la distribuzione aggiornata apparirà come **Published** (Pubblicata) e sarà possibile verificare l'applicazione aggiornata in un browser Web.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Avvio, arresto o riavvio di un'app Web esistente

Per avviare o arrestare un contenitore di app Web di Azure esistente, incluse tutte le applicazioni Java in esso distribuite, è possibile usare la visualizzazione **Azure Explorer** .

Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **Window** (Finestra) in Eclipse, quindi su **Show View** (Mostra visualizzazione), **Other** (Altro), **Azure** e infine su **Azure Explorer**. Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.

Quando appare la visualizzazione **Azure Explorer** , per avviare o arrestare l'app Web seguire questa procedura: 

1. Espandere il nodo **Azure** .

2. Espandere il nodo **Web Apps** (App Web). 

3. Fare clic con il pulsante destro del mouse sull'app Web desiderata.

4. Quando viene visualizzato il menu di scelta rapida, fare clic su **Start** (Avvia), **Stop** (Arresta) o **Restart** (Riavvia). Si noti che le opzioni di menu sono sensibili al contesto, quindi è possibile arrestare solo un'app Web in esecuzione o avviare un'app Web al momento non in esecuzione.
   
   ![Arresto di un'app Web esistente][13]

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
