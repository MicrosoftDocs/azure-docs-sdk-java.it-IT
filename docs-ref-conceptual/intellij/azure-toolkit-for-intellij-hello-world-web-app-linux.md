---
title: Distribuire un'app Web Hello World in esecuzione in un contenitore Linux sul cloud tramite Azure Toolkit for IntelliJ
description: Eseguire un'app Web Hello World di base in un contenitore Linux e distribuirla sul cloud tramite Azure Toolkit for IntelliJ.
services: app-service\web
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
ms.openlocfilehash: d281f37b027d4011ea2e3106990c5e45b69ebc88
ms.sourcegitcommit: 798f4d4199d3be9fc5c9f8bf7a754d7393de31ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33887094"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-intellij"></a>Distribuire un'app Web Hello World in un contenitore Linux sul cloud tramite Azure Toolkit for IntelliJ

I contenitori [Docker] sono un metodo molto diffuso per distribuire applicazioni Web. L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server. Azure Toolkit for IntelliJ semplifica questo processo per gli sviluppatori Java aggiungendo funzionalità per la distribuzione di contenitori in Microsoft Azure.

Questo articolo indica i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in un contenitore Linux in Azure usando Azure Toolkit for IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* Un client [Docker].

> [!NOTE]
>
> Per completare i passaggi di questa esercitazione, è necessario configurare [Docker] in modo da esporre il daemon sulla porta 2375 senza TLS. È possibile configurare questa impostazione durante l'installazione di Docker o tramite il menu delle impostazioni di Docker.
>
> ![Menu delle impostazioni di Docker][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>Creare un nuovo progetto di app Web

1. Avviare IntelliJ e accedere all'account Azure seguendo i passaggi contenuti nell'articolo [Istruzioni di accesso per Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions).

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

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Creare un registro contenitori di Azure da usare come registro Docker privato

La procedura seguente illustra come usare il portale di Azure per creare un Registro contenitori di Azure.

> [!NOTE]
>
> Per usare l'interfaccia della riga di comando di Azure invece del portale di Azure, seguire i passaggi in [Creare un registro contenitori Docker privato usando l'interfaccia della riga di comando di Azure 2.0][Create Docker Registry using Azure CLI].
>

1. Aprire il [portale di Azure] ed effettuare l'accesso.

   Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.

1. Fare clic sull'icona di menu **+ Nuovo**, su **Contenitori** e quindi su **Registro contenitori di Azure**.
   
   ![Creare un nuovo Registro contenitori di Azure][AR01]

1. Quando viene visualizzata la pagina delle informazioni per il modello di Registro contenitori di Azure, fare clic su **Crea**. 

   ![Creare un nuovo Registro contenitori di Azure][AR02]

1. Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.

   ![Configurare le impostazioni del Registro contenitori di Azure][AR03]

1. Una volta creato il registro contenitori, passare al registro contenitori stesso nel portale di Azure e quindi fare clic su **Chiavi di accesso**. Prendere nota del nome utente e della password per i passaggi successivi.

   ![Chiavi di accesso al Registro contenitori di Azure][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a>Distribuire l'app Web in un contenitore Docker

1. Fare clic con il pulsante destro del mouse sul progetto in Project Explorer (Esplora progetti) e scegliere **Azure**, quindi fare clic su **Add Docker Support** (Aggiungi supporto per Docker).

   Verrà automaticamente creato un file Docker con una configurazione predefinita.

   ![Aggiungere il supporto di Docker][add-docker-support]

1. Dopo aver aggiunto il supporto per Docker, fare clic con il pulsante destro del mouse in Project Explorer (Esplora progetti), scegliere **Azure** e quindi fare clic su **Run on Web App (Linux)** (Esegui su app Web - Linux).

   ![Run on Web App (Linux) (Esegui su app Web - Linux)][run-on-web-app-linux]

1. Quando viene visualizzata la finestra di dialogo **Run on Web App (Linux)** (Esegui su app Web - Linux), immettere le informazioni necessarie:

   * **Name** (Nome): specifica il nome descrittivo visualizzato in Azure Toolkit. 

   * **Server URL** (URL server): specifica l'URL per il registro contenitori creato nella sezione precedente di questo articolo. In genere viene usata questa sintassi: "*registro*.azurecr.io". 

   * **Username** (Nome utente) e **Password**: specificano le chiavi di accesso per il registro contenitori creato nella sezione precedente di questo articolo. 

   * **Image and tag** (Immagine e tag): specifica il nome dell'immagine del contenitore. In genere viene usata questa sintassi: "*registro*.azurecr.io/*nomeapp*:latest", dove: 
      * *registro* è il registro contenitori creato nella sezione precedente di questo articolo 
      * *nomeapp* è il nome dell'app Web 

   * **Use Existing Web App** (Usa app Web esistente) o **Create New Web App** (Crea nuova app Web): specifica se il contenitore verrà distribuito in un'app Web esistente o se creare una nuova app Web. 

   * **Resource Group** (Gruppo di risorse): specifica se usare un gruppo di risorse esistente o crearne uno nuovo. 

   * **App Service Plan** (Piano di servizio app): specifica se usare un piano di servizio app esistente o crearne uno nuovo. 

1. Al termine della configurazione delle impostazioni indicate sopra, fare clic su **Run** (Esegui).

   ![Crea app Web][create-web-app]

1. Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire l'applicazione su Azure facendo clic sulla freccia verde sulla barra degli strumenti. È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

## <a name="next-steps"></a>Passaggi successivi

Per altre risorse per Docker, vedere il [sito Web Docker][Docker] ufficiale.

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[portale di Azure]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
