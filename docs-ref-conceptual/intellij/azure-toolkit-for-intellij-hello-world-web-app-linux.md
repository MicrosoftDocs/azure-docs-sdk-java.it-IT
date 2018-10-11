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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892592"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="62632-103">Distribuire un'app Web Hello World in un contenitore Linux sul cloud tramite Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="62632-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="62632-104">I contenitori [Docker] sono un metodo molto diffuso per distribuire applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="62632-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="62632-105">L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server.</span><span class="sxs-lookup"><span data-stu-id="62632-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="62632-106">Azure Toolkit for IntelliJ semplifica questo processo per gli sviluppatori Java aggiungendo funzionalità per la distribuzione di contenitori in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="62632-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="62632-107">Questo articolo indica i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in un contenitore Linux in Azure usando Azure Toolkit for IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="62632-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="62632-108">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="62632-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="62632-109">Per completare i passaggi di questa esercitazione, è necessario configurare [Docker] in modo da esporre il daemon sulla porta 2375 senza TLS.</span><span class="sxs-lookup"><span data-stu-id="62632-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="62632-110">È possibile configurare questa impostazione durante l'installazione di Docker o tramite il menu delle impostazioni di Docker.</span><span class="sxs-lookup"><span data-stu-id="62632-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Menu delle impostazioni di Docker][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="62632-112">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="62632-112">Create a new web app project</span></span>

1. <span data-ttu-id="62632-113">Avviare IntelliJ e accedere all'account Azure seguendo i passaggi contenuti nell'articolo [Istruzioni di accesso per Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions).</span><span class="sxs-lookup"><span data-stu-id="62632-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="62632-114">Scegliere **New** (Nuovo) dal menu **File** e quindi fare clic su **Project** (Progetto).</span><span class="sxs-lookup"><span data-stu-id="62632-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![Creare un nuovo progetto][file-new-project]

1. <span data-ttu-id="62632-116">Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven**, **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="62632-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![Scegliere l'app Web di sistema Maven][maven-archetype-webapp]
   
1. <span data-ttu-id="62632-118">Specificare i valori per **GroupId** e **ArtifactId** per l'app Web e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="62632-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![Specificare GroupId e ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="62632-120">Personalizzare qualsiasi impostazione per Maven o accettare quelle predefinite e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="62632-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![Specificare le impostazioni per Maven][maven-options]

1. <span data-ttu-id="62632-122">Specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="62632-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![Specificare il nome del progetto][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="62632-124">Creare un registro contenitori di Azure da usare come registro Docker privato</span><span class="sxs-lookup"><span data-stu-id="62632-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="62632-125">La procedura seguente illustra come usare il portale di Azure per creare un Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="62632-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="62632-126">Per usare l'interfaccia della riga di comando di Azure invece del portale di Azure, seguire i passaggi in [Creare un registro contenitori Docker privato usando l'interfaccia della riga di comando di Azure 2.0][Create Docker Registry using Azure CLI].</span><span class="sxs-lookup"><span data-stu-id="62632-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="62632-127">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="62632-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="62632-128">Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="62632-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="62632-129">Fare clic sull'icona di menu **+ Nuovo**, su **Contenitori** e quindi su **Registro contenitori di Azure**.</span><span class="sxs-lookup"><span data-stu-id="62632-129">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Creare un nuovo Registro contenitori di Azure][AR01]

1. <span data-ttu-id="62632-131">Quando viene visualizzata la pagina delle informazioni per il modello di Registro contenitori di Azure, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="62632-131">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Creare un nuovo Registro contenitori di Azure][AR02]

1. <span data-ttu-id="62632-133">Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="62632-133">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Configurare le impostazioni del Registro contenitori di Azure][AR03]

1. <span data-ttu-id="62632-135">Una volta creato il registro contenitori, passare al registro contenitori stesso nel portale di Azure e quindi fare clic su **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="62632-135">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="62632-136">Prendere nota del nome utente e della password per i passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="62632-136">Take note of the username and password for the next steps.</span></span>

   ![Chiavi di accesso al Registro contenitori di Azure][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="62632-138">Distribuire l'app Web in un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="62632-138">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="62632-139">Fare clic con il pulsante destro del mouse sul progetto in Project Explorer (Esplora progetti) e scegliere **Azure**, quindi fare clic su **Add Docker Support** (Aggiungi supporto per Docker).</span><span class="sxs-lookup"><span data-stu-id="62632-139">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="62632-140">Verrà automaticamente creato un file Docker con una configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="62632-140">This will automatically create a Docker file with a default configuration.</span></span>

   ![Aggiungere il supporto di Docker][add-docker-support]

1. <span data-ttu-id="62632-142">Dopo aver aggiunto il supporto per Docker, fare clic con il pulsante destro del mouse in Project Explorer (Esplora progetti), scegliere **Azure** e quindi fare clic su **Run on Web App (Linux)** (Esegui su app Web - Linux).</span><span class="sxs-lookup"><span data-stu-id="62632-142">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App (Linux)**.</span></span>

   ![Run on Web App (Linux) (Esegui su app Web - Linux)][run-on-web-app-linux]

1. <span data-ttu-id="62632-144">Quando viene visualizzata la finestra di dialogo **Run on Web App (Linux)** (Esegui su app Web - Linux), immettere le informazioni necessarie:</span><span class="sxs-lookup"><span data-stu-id="62632-144">When the **Run on Web App (Linux)** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="62632-145">**Name** (Nome): specifica il nome descrittivo visualizzato in Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="62632-145">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="62632-146">**Server URL** (URL server): specifica l'URL per il registro contenitori creato nella sezione precedente di questo articolo. In genere viene usata questa sintassi: "*registro*.azurecr.io".</span><span class="sxs-lookup"><span data-stu-id="62632-146">**Server URL**: This specifies the URL for your container registry from the previous section of this article; typically this will use the following syntax: "*registry*.azurecr.io".</span></span> 

   * <span data-ttu-id="62632-147">**Username** (Nome utente) e **Password**: specificano le chiavi di accesso per il registro contenitori creato nella sezione precedente di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="62632-147">**Username** and **Password**: Specifies the access keys for your container registry from the previous section of this article.</span></span> 

   * <span data-ttu-id="62632-148">**Image and tag** (Immagine e tag): specifica il nome dell'immagine del contenitore. In genere viene usata questa sintassi: "*registro*.azurecr.io/*nomeapp*:latest", dove:</span><span class="sxs-lookup"><span data-stu-id="62632-148">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="62632-149">*registro* è il registro contenitori creato nella sezione precedente di questo articolo</span><span class="sxs-lookup"><span data-stu-id="62632-149">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="62632-150">*nomeapp* è il nome dell'app Web</span><span class="sxs-lookup"><span data-stu-id="62632-150">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="62632-151">**Use Existing Web App** (Usa app Web esistente) o **Create New Web App** (Crea nuova app Web): specifica se il contenitore verrà distribuito in un'app Web esistente o se creare una nuova app Web.</span><span class="sxs-lookup"><span data-stu-id="62632-151">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> 

   * <span data-ttu-id="62632-152">**Resource Group** (Gruppo di risorse): specifica se usare un gruppo di risorse esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="62632-152">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="62632-153">**App Service Plan** (Piano di servizio app): specifica se usare un piano di servizio app esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="62632-153">**App Service Plan**: Specifies whether you willuse an existing or create a new app service plan.</span></span> 

1. <span data-ttu-id="62632-154">Al termine della configurazione delle impostazioni indicate sopra, fare clic su **Run** (Esegui).</span><span class="sxs-lookup"><span data-stu-id="62632-154">When you have finished configuring the settings listed above, click **Run**.</span></span>

   ![Crea app Web][create-web-app]

1. <span data-ttu-id="62632-156">Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire l'applicazione su Azure facendo clic sulla freccia verde sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="62632-156">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="62632-157">È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).</span><span class="sxs-lookup"><span data-stu-id="62632-157">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![Menu Edit Configurations (Modifica configurazioni)][edit-configuration-menu]

1. <span data-ttu-id="62632-159">Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="62632-159">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![Finestra di dialogo Edit Configurations (Modifica configurazioni)][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="62632-161">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="62632-161">Next steps</span></span>

<span data-ttu-id="62632-162">Per altre risorse per Docker, vedere il [sito Web Docker][Docker] ufficiale.</span><span class="sxs-lookup"><span data-stu-id="62632-162">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Portale di Azure]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
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
