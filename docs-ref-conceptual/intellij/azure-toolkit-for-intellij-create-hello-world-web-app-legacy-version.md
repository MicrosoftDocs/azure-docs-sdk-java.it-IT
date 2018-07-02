---
title: Creare un'app Web Hello World per Azure con il toolkit legacy per IntelliJ
description: Questa esercitazione descrive come usare Azure Toolkit for IntelliJ 3.0.6 (o versioni precedenti) per creare un'app Web Hello World per Azure.
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: efc1dffa248987772827bbe7bc0caa9f10a0b4ef
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090784"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-intellij"></a>Creare un'app Web Hello World per Azure con il toolkit legacy per IntelliJ

Questa esercitazione descrive come creare e distribuire un'applicazione Hello World di base in Azure come app Web usando [Azure Toolkit per IntelliJ] 3.0.6 (o versioni precedenti).

> [!NOTE]
>
> Per una versione di questo articolo che fa uso di [Toolkit di Azure per Eclipse], vedere [Creare un'app Web Hello World per Azure con Eclipse][eclipse-hello-world].
>

> [!IMPORTANT]
> 
> Azure Toolkit for IntelliJ è stato aggiornato ad agosto 2017, introducendo un flusso di lavoro diverso. Questo articolo descrive come creare un'app Web Hello World usando la versione 3.0.6 (o versioni precedenti) di Azure Toolkit for IntelliJ. Se si usa la versione 3.0.7 (o versioni successive) del toolkit, è necessario seguire la procedura illustrata in [Creare un'app Web Hello World per Azure con IntelliJ][Updated Version].
>

Al termine di questa esercitazione, l'applicazione visualizzata in un browser Web avrà un aspetto simile al seguente:

![Anteprima dell'app Hello World][01]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>Creare un nuovo progetto di app Web

1. Avviare IntelliJ e accedere all'account Azure seguendo le istruzioni contenute nell'articolo [Istruzioni di accesso ad Azure per Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions].

1. Scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).
   
   ![File > New Project (Nuovo progetto)][02]

2. Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Java**, **Web Application** (Applicazione Web) e quindi fare clic su **New** (Nuovo) per aggiungere un SDK di progetto.
   
   ![Finestra di dialogo Nuovo progetto][03a]
   
3. Nella finestra di dialogo Select Home Directory for JDK (Selezionare la home directory per JDK), selezionare la cartella in cui è installato JDK e quindi fare clic su **OK**. Fare clic su **Next** (Avanti) nella finestra di dialogo New Project (Nuovo progetto) per continuare.
   
   ![Specificare la home directory di JDK][03b]

4. Ai fini di questa esercitazione, denominare il progetto **Java-Web-App-On-Azure**, quindi fare clic su **Finish** (Fine).
   
   ![Finestra di dialogo Nuovo progetto][04]

5. Nella visualizzazione Project Explorer di IntelliJ espandere **Java-Web-App-On-Azure** e **web**, quindi fare doppio clic su **index.jsp**.
   
   ![Pagina di indice aperta][05c]

6. Quando viene aperto il file index.jsp in IntelliJ, aggiungere testo in modo da visualizzare dinamicamente **Hello World!** all'interno dell'elemento `<body>` esistente. Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. Salvare index.jsp.

## <a name="deploy-your-web-app-to-azure"></a>Distribuire l'app Web in Azure

Esistono diversi modi per distribuire un'applicazione Web Java in Azure. Questa esercitazione descrive uno dei modi più semplici: l'applicazione viene distribuita in un contenitore di app Web di Azure senza richiedere tipi di progetto specifici o strumenti aggiuntivi. JDK e il software del contenitore Web vengono forniti automaticamente da Azure senza la necessità di eseguire alcun caricamento. È sufficiente disporre dell'app Web Java. Di conseguenza, il processo di pubblicazione per l'applicazione richiederà alcuni secondi, non minuti.

Prima di pubblicare l'applicazione è necessario configurare le impostazioni del modulo. A tale scopo, seguire questa procedura:

1. In Project Explorer di IntelliJ fare clic con il pulsante destro del mouse sul progetto **Java-Web-App-On-Azure** . Quando viene visualizzato il menu di scelta rapida, fare clic su **Open Module Settings** (Apri impostazioni modulo).

   ![Open Module Settings (Apri impostazioni modulo)][05a]

2. Nella finestra di dialogo Project Structure (Struttura progetto):

   a. Fare clic su **Artifacts** (Elementi) nell'elenco **Project Settings** (Impostazioni progetto).

   b. Modificare il nome dell'elemento nella casella **Nome** in modo che non contenga caratteri speciali o spazi vuoti; questa operazione è necessaria perché il nome verrà usato nell'URI (Uniform Resource Identifier).

   c. Modificare il valore di **Type** (Tipo) in **Web Application: Archive** (Applicazione Web: archivio).

   d. Fare clic su **OK** per chiudere la finestra di dialogo Project Structure (Struttura progetto).

   ![Open Module Settings (Apri impostazioni modulo)][05b]

Dopo aver configurato le impostazioni del modulo è possibile pubblicare l'applicazione in Azure con la procedura seguente:

1. In Project Explorer di IntelliJ fare clic con il pulsante destro del mouse sul progetto **Java-Web-App-On-Azure** . Dal menu di scelta rapida visualizzato scegliere **Azure** e fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).
   
   ![Menu di scelta rapida per la pubblicazione in Azure][06]

2. Se non è già stato eseguito l'accesso ad Azure da IntelliJ, verrà chiesto di accedere all'account Azure. In caso di più account Azure, durante il processo di accesso potrebbero essere visualizzati più volte alcuni prompt apparentemente identici. In questo caso, continuare a seguire le istruzioni di accesso.
   
   ![Finestra di accesso di Azure][07]

3. Dopo aver eseguito l'accesso all'account Azure, nella finestra di dialogo **Gestisci sottoscrizioni** viene visualizzato un elenco delle sottoscrizioni associate alle credenziali usate. Se sono elencate più sottoscrizioni e se ne vogliono usare solo alcune, è possibile deselezionare le sottoscrizioni che non si intende usare. Dopo aver selezionato le sottoscrizioni, fare clic su **Close**(Chiudi).
   
   ![Gestisci sottoscrizioni][08]

4. Nella finestra di dialogo **Deploy to Azure Web App Container** (Distribuisci in un contenitore app Web di Azure) sono visualizzati tutti i contenitori di app Web creati in precedenza. Se non è stato creato alcun contenitore, l'elenco appare vuoto.
   
   ![Contenitori di app][09]

5. Se non è stato creato alcun contenitore di app Web di Azure in precedenza o se si desidera pubblicare l'applicazione in un nuovo contenitore, attenersi alla procedura seguente. In caso contrario, selezionare un contenitore di app Web esistente e andare al passaggio 6.
   
   a. Fare clic sul pulsante **+**.
      
      ![Aggiungere un contenitore di app][10]

   b. Viene visualizzata la finestra di dialogo **New Web App Container** (Nuovo contenitore app Web), che verrà usata in diversi passaggi della procedura.
      
      ![Nuovo contenitore di app][11a]
   
   c. In **DNS Label** (Etichetta DNS) specificare un'etichetta per il contenitore di app Web. Questa sarà l'etichetta DNS foglia dell'URL dell'host per l'applicazione Web in Azure. Si noti che il nome deve essere disponibile e conforme ai requisiti di denominazione delle app Web di Azure.

   d. Nel menu a discesa **Contenitore Web** selezionare il software appropriato per l'applicazione.
      
      Attualmente è possibile scegliere fra Tomcat 8, Tomcat 7 o Jetty 9. Una distribuzione recente del software selezionato verrà fornita da Azure e sarà eseguita in una distribuzione recente di JDK 8 creata da Oracle e fornita da Azure.

   e. Nel menu a discesa **Subscription** (Sottoscrizione) selezionare la sottoscrizione che si vuole usare per la distribuzione.

   f. Nel menu a discesa **Resource Group** (Gruppo di risorse) selezionare il gruppo di risorse a cui si vuole associare l'app Web. I gruppi di risorse di Azure consentono di raggruppare le risorse correlate in modo che, ad esempio, possano essere eliminate insieme.
      
      È possibile selezionare un gruppo di risorse esistente, se presente, e andare al passaggio g seguente o usare questa procedura per creare un nuovo gruppo di risorse:
      
      * Selezionare **&lt;&lt; Create new Resource Group &gt;&gt;** (Crea nuovo gruppo di risorse) nel menu a discesa **Resource Group** (Gruppo di risorse).
      * Verrà visualizzata la finestra di dialogo **Nuovo gruppo di risorse** :
        
         ![Nuovo gruppo di risorse][12]

      * Nella casella di testo **Nome** specificare un nome per il nuovo gruppo di risorse.
      * Nel menu a discesa **Area** selezionare la località del data center di Azure appropriata per il gruppo di risorse.
      * Fare clic su **OK**.

   g. Il menu a discesa **Piano di servizio app** elenca i piani di servizio app associati al gruppo di risorse selezionato. Un piano di servizio app specifica informazioni quali il percorso dell'app Web, il piano tariffario e le dimensioni dell'istanza di calcolo. È possibile usare un singolo piano di servizio app per più app Web. Per questo motivo viene gestito separatamente da una distribuzione di app Web specifica.
      
      È possibile selezionare un piano di servizio app esistente, se presente, e andare al passaggio h seguente o usare questa procedura per creare un nuovo piano di servizio app:
      
      * Selezionare **&lt;&lt; Create new App Service Plan &gt;&gt;** (Crea nuovo piano di servizio app) nel menu a discesa **App Service Plan** (Piano di servizio app).
      * Verrà visualizzata la finestra di dialogo **Nuovo piano di servizio app** :
        
         ![Nuovo piano di servizio app][13]

      * Nella casella di testo **Nome** specificare un nome per il nuovo piano di servizio app.
      * Nel menu a discesa **Località** selezionare la località del data center di Azure appropriata per il piano.
      * Nel menu a discesa **Piano tariffario** selezionare la tariffa appropriata per il piano. Ai fini del test è possibile scegliere **Gratuito**.
      * Nel menu a discesa **Dimensioni istanza** selezionare le dimensioni dell'istanza appropriate per il piano. Ai fini del test è possibile scegliere **Piccolo**.
      * Fare clic su **OK**.

   h. (Facoltativo) Per impostazione predefinita, una distribuzione recente di Java 8 verrà distribuita automaticamente da Azure nel contenitore di app Web come JVM. È tuttavia possibile selezionare una versione e una distribuzione di JVM diversa. A tale scopo, seguire questa procedura:
      
   * Fare clic sulla scheda **JDK** nella finestra di dialogo **New Web App Container** (Nuovo contenitore app Web).
   * È possibile scegliere una delle opzioni seguenti:
        
      * Distribuire il JDK predefinito offerto da Azure
      * Distribuire un JDK di terze parti da un elenco a discesa di altri JDK disponibili in Azure
      * Distribuire un JDK personalizzato, che deve essere compresso come file ZIP e disponibile pubblicamente o nell'account di archiviazione di Azure
        
     ![Scheda JDK della finestra di dialogo New Web App Container (Nuovo contenitore app Web)][11b]

   i. Dopo aver completato tutti i passaggi precedenti, la finestra di dialogo New Web App Container dovrebbe essere simile alla seguente:
      
      ![Nuovo contenitore di app][14]
   
   j. Fare clic su **OK** per completare la creazione del nuovo contenitore di app Web.
       
      Attendere alcuni secondi che venga aggiornato l'elenco dei contenitori di app Web. Il contenitore di app Web appena creato risulterà selezionato nell'elenco.

6. A questo punto è possibile completare la distribuzione iniziale dell'app Web in Azure. Fare clic su **OK** per distribuire l'applicazione Java nel contenitore di app Web selezionato. Per impostazione predefinita, l'applicazione verrà distribuita come sottodirectory del server applicazioni. Se si vuole distribuire l'applicazione come applicazione radice, selezionare la casella di controllo **Deploy to root** (Distribuisci a radice) prima di fare clic su **OK**.
   
   ![Distribuire in Azure][15]

7. Verrà quindi aperta la visualizzazione **Azure Activity Log** (Log attività di Azure) in cui è indicato lo stato della distribuzione dell'app Web.
   
   ![Indicatore di stato][16]
   
   Il processo di distribuzione dell'app Web in Azure dovrebbe richiedere solo alcuni secondi. Quando l'applicazione è pronta, viene visualizzato un collegamento con valore **Pubblicato** nella colonna **Stato** . Quando si fa clic sul collegamento, si passa alla home page dell'app Web distribuita oppure è possibile seguire la procedura indicata nella sezione seguente per passare all'app Web.

## <a name="browsing-to-your-web-app-on-azure"></a>Passaggio all'app Web in Azure

Per trovare l'app Web in Azure, è possibile usare la visualizzazione **Azure Explorer**.

Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **View** (Visualizza) in IntelliJ, quindi su **Tool Windows** (Finestre degli strumenti) e su **Service Explorer**. Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.

Quando viene aperta la visualizzazione **Azure Explorer**, seguire questa procedura per passare all'app Web: 

1. Espandere il nodo **Azure** .
2. Espandere il nodo **Web Apps** (App Web). 
3. Fare clic con il pulsante destro del mouse sull'app Web desiderata.
4. Quando viene visualizzato il menu di scelta rapida, fare clic su **Open in Browser**(Apri nel browser).
   
   ![Sfogliare l'app Web][17]

## <a name="updating-your-web-app"></a>Aggiornamento dell'app Web

L'aggiornamento di un'app Web di Azure in esecuzione è un processo semplice e rapido. Sono disponibili due opzioni per l'aggiornamento:

* È possibile aggiornare la distribuzione di un'app Web Java esistente.
* È possibile pubblicare un'applicazione Java aggiuntiva nello stesso contenitore di app Web.

In entrambi i casi, il processo è identico e richiede solo pochi secondi:

1. In Project Explorer di IntelliJ fare clic con il pulsante destro del mouse sull'applicazione Java che si vuole aggiornare o aggiungere a un contenitore di app Web esistente.
2. Dal menu di scelta rapida visualizzato selezionare **Azure**, quindi **Publish as Azure Web App** (Pubblica come App Web di Azure).
3. Poiché è già stato effettuato l'accesso in precedenza, verrà visualizzato un elenco dei contenitori di app Web esistenti. Selezionare il contenitore in cui si vuole pubblicare o ripubblicare l'applicazione Java e fare clic su **OK**.

Pochi secondi dopo nella visualizzazione **Azure Activity Log** (Log attività di Azure) la distribuzione aggiornata apparirà come **Published** (Pubblicata) e sarà possibile verificare l'applicazione aggiornata in un browser Web.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Avvio, arresto o riavvio di un'app Web esistente

Per avviare o arrestare un contenitore di app Web di Azure esistente, incluse tutte le applicazioni Java in esso distribuite, è possibile usare la visualizzazione **Azure Explorer** .

Se la visualizzazione **Azure Explorer** non è già aperta, aprirla facendo clic sul menu **View** (Visualizza) in IntelliJ, quindi su **Tool Windows** (Finestre degli strumenti) e su **Service Explorer**. Se non è già stato eseguito l'accesso in precedenza, verrà richiesto di accedere.

Quando viene aperta la visualizzazione **Azure Explorer**, seguire questa procedura per avviare o arrestare l'app Web: 

1. Espandere il nodo **Azure** .
2. Espandere il nodo **Web Apps** (App Web). 
3. Fare clic con il pulsante destro del mouse sull'app Web desiderata.
4. Quando viene visualizzato il menu di scelta rapida, fare clic su **Start** (Avvia), **Stop** (Arresta) o **Restart** (Riavvia). Si noti che le opzioni di menu sono sensibili al contesto, quindi è possibile arrestare solo un'app Web in esecuzione o avviare un'app Web al momento non in esecuzione.
   
   ![Arrestare l'app Web][18]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Azure Toolkit per IntelliJ]: azure-toolkit-for-intellij.md
[Toolkit di Azure per Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Panoramica delle App Web]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Updated Version]: azure-toolkit-for-intellij-create-hello-world-web-app.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/18-Stop-Web-App.png
