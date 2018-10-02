---
title: CI/CD per applicazioni MicroProfile con Azure DevOps
description: Informazioni su come configurare un ciclo di rilascio CI/CD per distribuire un'applicazione MicroProfile in un'istanza di app Web per contenitori di Azure usando Azure DevOps
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: c2b6bf3370982d26d8d23fede370e0105a70b734
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506435"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a>CI/CD per applicazioni MicroProfile con Azure DevOps

Questa esercitazione illustra come gli sviluppatori Java EE possono configurare facilmente un ciclo di rilascio CI/CD per distribuire le proprie applicazioni [MicroProfile](http://microprofile.io) in un'app Web per contenitori di Azure usando Azure DevOps (denominato formalmente VSTS).  In questo esempio si userà un'applicazione MicroProfile che usa un'immagine di base di [Payara Micro](https://www.payara.fish/payara_micro).   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
Il processo di inserimento in un contenitore di Azure DevOps verrà avviato compilando un'immagine Docker ed eseguendo il push dell'immagine del contenitore in un registro contenitori di Azure.  Verrà quindi completato con una pipeline di versione di Azure DevOps per distribuire l'immagine del contenitore in un'app Web.

## <a name="prerequisites"></a>Prerequisiti
- Copiare e salvare l'URL GIT da [GitHub](https://github.com/Azure-Samples/microprofile-hello-azure)
- Eseguire la registrazione o l'accesso all'account [Azure DevOps](https://dev.azure.com)
- Creare un nuovo [progetto DevOps di Azure](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) e usare l'URL GIT precedente per **importare un repository**
- Creare un [registro contenitori di Azure](https://azure.microsoft.com/en-us/services/container-registry)
- Creare un'app web per contenitori di Azure
> [!NOTE]
>
> Selezionare "Avvio rapido" in Impostazioni contenitore durante il provisioning dell'istanza di app Web


## <a name="create-a-build-definition"></a>Creare una definizione di compilazione

La definizione di compilazione in Azure DevOps esegue automaticamente tutte le attività della compilazione a ogni commit nell'applicazione di origine dell'applicazione Java EE.  In questo esempio, Azure DevOps userà Maven per compilare il progetto Java MicroProfile.

1. Fare clic sulla scheda "Compilazione e versione" nella parte superiore della pagina del progetto DevOps di Azure.  Selezionare quindi il collegamento **Compilazioni**. 

<img src="media/VSTS/Buid-and-Release1.png">

2. Fare clic sul pulsante **Nuova pipeline** e quindi su **Continua** per iniziare a definire le attività di compilazione.
3. Selezionare "Maven" nell'elenco di modelli e quindi fare clic sul pulsante **Applica** per compilare il progetto Java.
4. Usare il menu a discesa per il campo Pool di agenti per selezionare l'opzione **Hosted Linux Preview** (Anteprima Linux ospitata).
> [!NOTE]
>
> In questo modo si indica ad Azure DevOps il server di compilazione da usare.  È possibile usare un server di compilazione personalizzato privato.

5. Per configurare la compilazione per l'integrazione continua, selezionare la scheda **Trigger** e quindi la casella di controllo **Abilita l'integrazione continua**.  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. Selezionare la scheda **Attività** per tornare alla pagina principale della pipeline di compilazione.
7. Usare il menu a discesa **Salva e accoda** per selezionare l'opzione **Salva**.
 

## <a name="create-a-docker-build-image"></a>Creare un'immagine di compilazione Docker

In questa attività, Azure DevOps usa un Dockerfile con un'immagine di base di Payara Micro per creare un'immagine Docker.  

1. Selezionare la scheda **Attività** per tornare alla pagina principale della pipeline di compilazione.
2. Fare clic sull'icona **+** per aggiungere una nuova attività alla definizione di compilazione.
 
<img src="media/VSTS/Tasks-add4.png">
 
3. Selezionare "Docker" nell'elenco di modelli e quindi fare clic sul pulsante **Aggiungi**.
4. Immettere un nome descrittivo nel campo **Nome visualizzato**.
5. Verificare che nel menu a discesa per **Tipo di registro contenitori** sia selezionata l'opzione **Registro contenitori di Azure**.
> [!NOTE]
>
>  Se si usa l'hub Docker o un altro registro, selezionare invece "Registro contenitori".  Fare quindi clic sul pulsante "+ Nuovo" per specificare le relative credenziali e informazioni di connessione. Ignorare quindi la sezione Comandi e continuare.

6. Usare il menu a discesa **Sottoscrizione di Azure** per selezionare l'ID sottoscrizione di Azure.  Fare quindi clic sul pulsante **Autorizza**.
7. Nel menu a discesa **Registro contenitori di Azure** selezionare il nome del registro creato in Azure.
8. Verificare che nel menu a discesa **Comando** sia selezionata l'opzione **build**.
9. Per **Dockerfile** fare clic sull'icona per lo spostamento tra i percorsi accanto alla casella di testo per selezionare il Dockerfile del progetto GitHub.  Fare quindi clic sul pulsante **OK**.

<img src="media/VSTS/Dockerfile5.png">

10. In **Nome immagine** selezionare la casella di controllo **Includi tag latest**. 
11. Usare il menu a discesa **Salva e accoda** per selezionare l'opzione **Salva**.

## <a name="push-docker-image-to-acr"></a>Eseguire il push dell'immagine Docker in Registro contenitori di Azure

In questa attività, Azure DevOps eseguirà il push dell'immagine Docker nel registro contenitori di Azure,  che verrà usato per eseguire l'applicazione per le API MicroProfile come app Web Java in contenitore.

1. Dato che si usa Docker in Azure DevOps, creare un nuovo modello Docker ripetendo i passaggi da 1 a 7 riportati sopra nella sezione **Creare un'immagine di compilazione Docker**.
2. Selezionare **push** nel menu a discesa **Comando**.
3. Fare clic sulla scheda **Salva e accoda** e quindi selezionare l'opzione **Salva e accoda**.
4. Verificare che nella finestra popup sia selezionata l'opzione **Hosted Linux Preview** (Anteprima Linux ospitata) per Pool di agenti.  Fare quindi clic sul pulsante **Salva e accoda**.
5. Fare clic sul numero di build per verificare che la pipeline di compilazione per il progetto Java sia stata completata correttamente.

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a>Creare una definizione di versione per un'app Java

La pipeline di versione in Azure DevOps attiva automaticamente la distribuzione degli artefatti di compilazione in un ambiente di destinazione come Azure non appena viene completato il processo di compilazione.   La pipeline di versione può essere creata per ambienti di sviluppo, di test, di staging o di produzione.

1. Fare clic sulla scheda "Compilazione e versione" nella parte superiore della pagina del progetto DevOps di Azure.  Selezionare quindi il collegamento **Versioni**.

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. Fare clic sul pulsante "Nuova pipeline".
3. Selezionare **Distribuisci un'app Java in Servizio app di Azure** nell'elenco di modelli e quindi fare clic sul pulsante **Applica**.

<img src="media/VSTS/deploy-java-template8.png">
 
4. Impostare un **nome di fase**, ad esempio Sviluppo, Test, Staging o Produzione.  Fare quindi clic sul pulsante **X** per chiudere la finestra popup.
5. Fare clic sul pulsante **+ Aggiungi** nella sezione Artefatti.  Gli artefatti della definizione di compilazione verranno così collegati a questa definizione di versione.  
6. Usare il menu a discesa per **Origine (pipeline di compilazione)** per selezionare la definizione di compilazione. Fare quindi clic sul pulsante **Aggiungi** per continuare.

<img src="media/VSTS/add-artifact9.png">
 
7. Fare clic sulla scheda **Attività** della pipeline.  Selezionare quindi il nome della fase.
 
<img src="media/VSTS/release-stage10.png">

8. Usare il menu a discesa **Sottoscrizione di Azure** per selezionare l'ID sottoscrizione di Azure.
9. Selezionare **Linux App** (App Linux) nel menu a discesa **Tipo di app**.
10. Selezionare il nome dell'istanza di app Web per contenitori creata in precedenza nel menu a discesa **Nome del servizio app**.
11. Immettere il nome del registro contenitori di Azure nel campo **Registro o spazio dei nomi**.  Ad esempio, **registropersonale.azure.io**.
12. Immettere il nome del registro nel campo **Repository**.
13. Fare clic su **Deploy Azure App Service** (Distribuisci servizio app di Azure).  Immettere il tag dell'immagine del contenitore nella casella di testo **Tag**. 
14. Fare clic su **Esegui su agente**.  Selezionare **Hosted Linux Preview** (Anteprima Linux ospitata) nel menu a discesa Pool di agenti. 

## <a name="setup-environment-variables"></a>Configurare le variabili di ambiente

1. Fare clic sulla scheda **Variabili**.  Fare quindi clic sul pulsante **+ Aggiungi** per definire le variabili di ambiente.
2. Aggiungere il nome e i valori delle variabili per l'URL, il nome utente e la password del registro contenitori.   Per motivi di sicurezza, fare clic sull'icona a forma di lucchetto per mantenere nascosta la password.

Ad esempio: 
- registry.password
- registry.url
- registry.username

<img src="media/VSTS/environment-variables12.png">

3. Fare clic sulla scheda **Attività** per tornare alla pagina principale della definizione della pipeline di versione.
4. Fare clic su **Deploy Azure App Service** (Distribuisci servizio app di Azure). 
5. Espandere la sezione **Impostazioni applicazione e configurazione** e quindi fare clic sull'icona del percorso di spostamento per il campo **Impostazioni app** per aggiungere le variabili di ambiente per la connessione al registro contenitori durante la distribuzione.
6. Fare clic sul pulsante "+ Aggiungi" per definire le impostazioni seguenti dell'app e assegnare le variabili di ambiente.
- DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)
- DOCKER_REGISTRY_SERVER_URL = $(registry.url)
- DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)

<img src="media/VSTS/environment-variables14.png">
 
7. Fare clic sul pulsante **OK** per continuare.

## <a name="setup-continious-deployment--deploy-java-application"></a>Configurare la distribuzione continua e distribuire l'applicazione Java

1. Per abilitare la distribuzione continua, fare clic sulla scheda **Pipeline**.
2. Nella sezione Artefatti fare clic sull'icona a forma di fulmine.  Impostare quindi **Trigger di distribuzione continua** su Abilitato.

<img src="media/VSTS/release-enable-CD.png">
 
3. Fare clic sul pulsante **Salva** e quindi sul pulsante **OK**. 
4. Fare clic sul menu a discesa **+ Versione** e quindi selezionare il collegamento **Crea una versione**.
5. Usare il menu a discesa **Fasi per la modifica di un trigger da automatizzato a manuale** per selezionare la casella di controllo per il nome della fase.
6. Fare clic sul pulsante **Crea** per continuare.
7. Fare clic sul numero di versione.  Passare quindi il puntatore del mouse sul nome della fase e fare clic su **Distribuisci**.
8. Fare quindi clic sul pulsante **Distribuisci** nella finestra popup per avviare il processo di distribuzione in Azure.


## <a name="test-the-java-web-application"></a>Testare l'applicazione Web Java
1. Eseguire l'URL dell'app Web in un Web browser:  
https://{nome-servizio-app}.azurewebsites.net/api/hello

 
<img src="media/VSTS/web-app16.png">

2. Nella pagina Web verrà visualizzato **Hello Azure!**
 
<img src="media/VSTS/web-api17.png">
