---
title: Pubblicare un contenitore Docker usando Azure Toolkit for IntelliJ
description: Informazioni su come pubblicare un'app Web in Microsoft Azure come contenitore Docker usando il Toolkit di Azure per IntelliJ.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 05fb81466202547cb1bad34caae0f94f16a9d21b
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893652"
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>Pubblicare un'app Web come contenitore Docker usando Azure Toolkit for IntelliJ

I contenitori Docker sono un metodo molto diffuso per la distribuzione di applicazioni Web. L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server. Azure Toolkit for IntelliJ semplifica questo processo per gli sviluppatori Java aggiungendo la funzionalità *Publish as Docker Container* (Pubblica come contenitore Docker) per la distribuzione in Microsoft Azure. Questo articolo illustra la procedura da seguire per pubblicare le applicazioni in Azure come contenitori Docker.

> [!NOTE]
>
> Altre informazioni su Docker sono disponibili nel [sito Web Docker].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Pubblicare l'app Web in Azure usando un contenitore Docker

> [!NOTE]
> * Per pubblicare l'app Web, è necessario creare un elemento pronto per la distribuzione. Per altre informazioni, vedere la sezione [Altre informazioni sulla creazione di elementi](#artifacts).
>
> * Dopo aver completato la distribuzione guidata almeno una volta, la maggior parte delle impostazioni viene usata come impostazioni predefinite all'esecuzione successiva.
>

1. Aprire il progetto dell'app Web in IntelliJ.

2. Per avviare la procedura guidata **Publish as Docker Container** (Pubblica come contenitore Docker), eseguire una di queste operazioni:

   * Fare clic sul progetto con il pulsante destro del mouse nella finestra degli strumenti **Project** (Progetto), fare clic su **Azure** e quindi fare clic su **Publish as Docker Container** (Pubblica come contenitore Docker):

      ![Comando Publish as Docker Container (Pubblica come contenitore Docker)][PUB01]

   * Sulla barra degli strumenti di IntelliJ fare clic sul pulsante **Publish Group** (Pubblica gruppo) e quindi fare clic su **Publish as Docker Container** (Pubblica come contenitore Docker):

      ![Comando Publish as Docker Container][PUB02] (Pubblica come contenitore Docker)  
    Verrà visualizzata la procedura guidata **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure).

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB03]

3. Nella finestra **Type an image name, select the artifact's path and check a Docker host to be used** (Digitare un nome immagine, scegliere il percorso dell'elemento e selezionare un host Docker da usare) seguire questa procedura: 

   a. Nella casella **Docker image name** (Nome immagine Docker) immettere un nome univoco per l'host Docker. La procedura guidata crea automaticamente un nome, ma è possibile modificarlo. 

   b. Nell'area **Hosts** (Host) vengono visualizzati tutti gli host Docker già creati. Eseguire una di queste operazioni: 
   * Se è disponibile un host Docker esistente, è possibile distribuirvi l'app Web.
   * Per creare un host Docker, fare clic sul segno più verde (**+**).  
     Verrà visualizzata la finestra di dialogo **Create Docker Host** (Crea host Docker). 

     ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB04a]

4. Nella finestra **Configure the new virtual machine** (Configura la nuova macchina virtuale) specificare le informazioni seguenti sull'host Docker. La procedura guidata genera automaticamente la maggior parte delle informazioni, ma è possibile modificarle. 

   a. Nella casella **Name** (Nome) immettere un nome univoco per l'host Docker. Questo non è lo stesso nome immagine Docker specificato in precedenza. 
    
   b. Nella casella **Subscription** (Sottoscrizione) immettere la sottoscrizione di Azure usata per l'host. 
      
   c. Nella casella **Region** (Area) immettere l'area geografica in cui si trova l'host.
      
   d. Nella scheda **OS and Size** (Sistema operativo e dimensioni) seguire questa procedura:      
      * **Host OS** (Sistema operativo host): specificare il sistema operativo della macchina virtuale in cui è presente l'host. 
      * **Size** (Dimensioni): immettere le dimensioni della macchina virtuale per l'host.   
       
   e. Nella scheda **Resource Group** (Gruppo di risorse) selezionare una delle opzioni seguenti:      
      * **New resource group** (Nuovo gruppo di risorse): consente di creare un nuovo gruppo di risorse per l'host.
      * **Existing resource group** (Gruppo di risorse esistente): consente di specificare un gruppo di risorse dal proprio account Azure. 
       
   f. Nella scheda **Network** (Rete) selezionare una delle opzioni seguenti:      
      * **New virtual network** (Nuova rete virtuale): consente di creare una nuova rete virtuale per l'host.
      * **Existing virtual network** (Rete virtuale esistente): consente di specificare una rete virtuale esistente dal proprio account Azure. 
       
   g. Nella scheda **Storage** (Archiviazione) selezionare una delle opzioni seguenti:      
      * **New storage account** (Nuovo account di archiviazione): consente di creare un nuovo account di archiviazione per l'host.
      * **Existing storage account** (Account di archiviazione esistente): consente di specificare un account di archiviazione esistente dal proprio account Azure.
       
5. Fare clic su **Avanti**.  
     Verrà visualizzata la finestra **Configure log in credentials and port settings** (Configurare le credenziali di accesso e le impostazioni delle porte).

      ![Finestra Configure log in credentials and port settings (Configurare le credenziali di accesso e le impostazioni delle porte)][PUB05]

6. Selezionare una delle opzioni seguenti:

   * **Import credentials from Azure Key Vault** (Importa credenziali da Azure Key Vault): consente di specificare un set di credenziali salvato in precedenza e archiviato nella sottoscrizione di Azure.

       > [!NOTE]
       > Un insieme di credenziali delle chiavi di Azure creato con un'entità servizio o un account specifico non è automaticamente accessibile da un altro account o entità servizio che condivide la sottoscrizione. Per consentire a un altro account oppure a un'altra entità servizio di usare l'insieme di credenziali delle chiavi, è necessario usare il portale di Azure per aggiungere l'account o l'entità servizio.

   * **New log in credentials** (Nuove credenziali di accesso): consente di creare un nuovo set di credenziali di accesso. Se si seleziona questa opzione, seguire questa procedura:

     a. Nella scheda **VM Credentials** (Credenziali macchina virtuale) fornire le informazioni seguenti per le credenziali di accesso alla macchina virtuale dell'host Docker:

     * **Username** (Nome utente): immettere il nome utente per le credenziali di accesso alla macchina virtuale.

     * **Password** e **Confirm** (Conferma): immettere la password per le credenziali di accesso alla macchina virtuale.

     * **SSH**: immettere le impostazioni Secure Shell (SSH) per l'host Docker. È possibile selezionare una delle opzioni seguenti:

     * **None** (Nessuna): specifica che la macchina virtuale non consente connessioni SSH.

     * **Auto-generate** (Genera automaticamente): crea automaticamente le impostazioni necessarie per la connessione tramite SSH.

     * **Import from directory**: (Importa da directory): permette di specificare una directory contenente un insieme di impostazioni SSH salvate in precedenza. La directory deve contenere i due file seguenti:

         * *id_rsa*: contiene l'identificazione RSA per un utente.

         * *id_rsa.pub*: contiene la chiave pubblica RSA usata per l'autenticazione.

     b. Nella scheda **Docker Daemon Access** (Accesso daemon Docker) specificare le informazioni seguenti:

     ![Creare un host Docker][PUB06]
    
     * **Docker Daemon port** (Porta Daemon Docker): immettere la porta TCP univoca per l'host Docker.
    
     * **Sicurezza TLS**: immettere le impostazioni Transport Layer Security per l'host Docker. È possibile scegliere tra le opzioni seguenti:
    
     * **None** (Nessuna): specifica che la macchina virtuale non consente connessioni TLS.
        
     * **Auto-generate** (Genera automaticamente): crea automaticamente le impostazioni necessarie per la connessione tramite TLS.
        
     * **Import from directory** (Importa da directory): specifica una directory che contiene un set di impostazioni TLS salvate in precedenza. La directory deve contenere i sei file seguenti:
        
         * *ca.pem* e *ca-key.pem*: contengono il certificato e la chiave pubblica per l'Autorità di certificazione TLS.
            
         * *cert.pem* e *key.pem*: contengono il certificato client e la chiave pubblica da usare per l'autenticazione TLS.
            
         * *server.pem* e *server-key.pem*: contengono il certificato client e la chiave pubblica usati per l'autenticazione TLS.

7. Dopo aver immesso le informazioni richieste, fare clic su **Finish** (Fine).  
    Verrà visualizzata nuovamente la procedura guidata **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure).

   ![Procedura guidata Deploy Docker Container on Azure (Distribuisci contenitore Docker in Azure)][PUB07]

8. Fare clic su **Avanti**.  
    Verrà visualizzata la finestra **Configure the Docker container to be created** (Configurare il contenitore Docker da creare).

   ![Finestra Configure the Docker container to be created (Configurare il contenitore Docker da creare)][PUB08]

9. Nella finestra **Configure the Docker container to be created** (Configurare il contenitore Docker da creare) specificare le informazioni seguenti: 

   a. Nella casella **Docker container name** (Nome contenitore Docker) immettere un nome univoco per il contenitore Docker.

   b. Scegliere una delle immagini Docker seguenti: 

      * **Predefined Docker image** (Immagine Docker predefinita): consente di specificare un'immagine preesistente in Azure. 

        > [!NOTE]
        > L'elenco di immagini Docker in questa casella è costituito da diverse immagini per cui Azure Toolkit è stato configurato per l'applicazione automatica delle patch, in modo che l'elemento venga distribuito automaticamente. 

      * **Custom Dockerfile** (Dockerfile personalizzato): consente di specificare un Dockerfile salvato in precedenza nel computer locale.

        > [!NOTE]
        > Questa è una funzionalità più avanzata per gli sviluppatori che intendono distribuire i propri Dockerfile. È tuttavia compito degli sviluppatori che usano questa opzione garantire che i propri Dockerfile vengano compilati correttamente. Dato che Azure Toolkit non esegue la convalida del contenuto in un Dockerfile personalizzato, in caso di problemi con il Dockerfile la distribuzione può avere esito negativo. Per Azure Toolkit il Dockerfile personalizzato deve contenere un elemento app Web e prova quindi ad aprire una connessione HTTP. Pubblicando un diverso tipo di elemento, gli sviluppatori potrebbero ricevere errori non gravi dopo la distribuzione.

   c. Nella casella **Port settings** (Impostazioni porta) specificare l'associazione univoca della porta TCP per il contenitore Docker. 

10. Al termine della procedura, fare clic su **Finish** (Fine). 

Azure Toolkit inizia a distribuire l'app Web in Azure in un contenitore Docker. A meno che non sia stato configurato IntelliJ per la distribuzione in background, viene visualizzato l'indicatore di stato **Deploying to Azure** (Distribuzione in Azure). 

![Indicatore di stato della distribuzione][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Altre informazioni sulla creazione di elementi

Per creare un elemento pronto per la distribuzione, seguire questa procedura:

1. Aprire il progetto dell'app Web in IntelliJ.

2. Fare clic su **File** e quindi fare clic su **Project Structure** (Struttura del progetto).

   ![Comando Project Structure (Struttura del progetto)][ART01]

3. Per aggiungere un elemento, fare clic sul simbolo più di colore verde (**+**) e quindi scegliere **Web Application: Archive** (Applicazione Web: archivio).

   ![Comando "Web Application: Archive" (Applicazione Web: archivio)][ART02]

4. Nella casella **Name** (Nome) immettere un nome per l'elemento, senza l'estensione *war*, quindi fare clic su **OK**.

   ![Casella Name (Nome) dell'elemento][ART03]

Per altre informazioni sulla creazione di elementi in IntelliJ, vedere [Configuring Artifacts] (Configurazione di elementi) sul sito Web JetBrains.

## <a name="next-steps"></a>Passaggi successivi

Per altre risorse per Docker, vedere il [sito Web Docker] ufficiale.

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Sito Web Docker]: https://www.docker.com/
[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html (Configurazione di elementi)

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
