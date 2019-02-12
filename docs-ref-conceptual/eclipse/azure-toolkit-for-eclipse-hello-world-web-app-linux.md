---
title: Distribuire un'app Web Hello World in esecuzione in un contenitore Linux nel cloud con Azure Toolkit for Eclipse
description: Eseguire un'app Web Hello World di base in un contenitore Linux e distribuirla nel cloud con Azure Toolkit for Eclipse.
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649247"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="4fe06-103">Distribuire un'app Web Hello World in un contenitore Linux nel cloud con Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="4fe06-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="4fe06-104">I contenitori [Docker] sono un metodo molto diffuso per distribuire applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="4fe06-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="4fe06-105">L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server.</span><span class="sxs-lookup"><span data-stu-id="4fe06-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="4fe06-106">Azure Toolkit for Eclipse semplifica questo processo per gli sviluppatori Java aggiungendo funzionalità per la distribuzione di contenitori in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4fe06-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="4fe06-107">Questo articolo illustra i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in un contenitore Linux in Azure usando Azure Toolkit for Eclipse.</span><span class="sxs-lookup"><span data-stu-id="4fe06-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* <span data-ttu-id="4fe06-108">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="4fe06-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4fe06-109">Per completare i passaggi di questa esercitazione, è necessario configurare [Docker] in modo da esporre il daemon sulla porta 2375 senza TLS.</span><span class="sxs-lookup"><span data-stu-id="4fe06-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="4fe06-110">È possibile configurare questa impostazione durante l'installazione di Docker o tramite il menu delle impostazioni di Docker.</span><span class="sxs-lookup"><span data-stu-id="4fe06-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Menu delle impostazioni di Docker][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="4fe06-112">Creare un nuovo progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="4fe06-112">Create a new web app project</span></span>

1. <span data-ttu-id="4fe06-113">Avviare Eclipse e accedere al proprio account Azure usando la procedura descritta nelle [istruzioni di accesso per Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).</span><span class="sxs-lookup"><span data-stu-id="4fe06-113">Start Eclipse and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="4fe06-114">Fare clic sul menu **File**, quindi su **New** (Nuovo) e infine su **Dynamic Web Project** (Progetto Web dinamico).</span><span class="sxs-lookup"><span data-stu-id="4fe06-114">Click the **File** menu, then click **New**, and then click **Dynamic Web Project**.</span></span>
   
   ![Creare un nuovo progetto][file-new-project]

1. <span data-ttu-id="4fe06-116">Nella finestra di dialogo **New Dynamic Web Project** (Nuovo progetto Web dinamico) specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="4fe06-116">In the **New Dynamic Web Project** dialog box, specify your project name and location, and then click **Finish**.</span></span>
   
   ![Specificare il nome del progetto][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="4fe06-118">Creare un'istanza di Registro Azure Container da usare come registro Docker privato</span><span class="sxs-lookup"><span data-stu-id="4fe06-118">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="4fe06-119">La procedura seguente illustra come usare il portale di Azure per creare un'istanza di Registro Azure Container.</span><span class="sxs-lookup"><span data-stu-id="4fe06-119">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4fe06-120">Per usare l'interfaccia della riga di comando di Azure invece del portale di Azure, seguire i passaggi in [Creare un registro contenitori Docker privato usando l'interfaccia della riga di comando di Azure 2.0][Create Docker Registry using Azure CLI].</span><span class="sxs-lookup"><span data-stu-id="4fe06-120">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="4fe06-121">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="4fe06-121">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="4fe06-122">Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="4fe06-122">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="4fe06-123">Fare clic sull'icona di menu **+ Crea una risorsa**, quindi su **Contenitori** e infine su **Registro Container**.</span><span class="sxs-lookup"><span data-stu-id="4fe06-123">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![Creare una nuova istanza di Registro Azure Container][create-container-registry-01]

1. <span data-ttu-id="4fe06-125">Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4fe06-125">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Configurare le impostazioni di Registro Azure Container][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="4fe06-127">Distribuire l'app Web in un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="4fe06-127">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="4fe06-128">Fare clic con il pulsante destro del mouse sul progetto in Project Explorer (Esplora progetti) e scegliere **Azure**, quindi fare clic su **Add Docker Support** (Aggiungi supporto per Docker).</span><span class="sxs-lookup"><span data-stu-id="4fe06-128">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="4fe06-129">Verrà automaticamente creato un file Docker con una configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4fe06-129">This will automatically create a Docker file with a default configuration.</span></span>

   ![Aggiungere il supporto di Docker][add-docker-support]

1. <span data-ttu-id="4fe06-131">Dopo aver aggiunto il supporto per Docker, fare clic con il pulsante destro del mouse in Project Explorer (Esplora progetti), scegliere **Azure** e quindi fare clic su **Publish to Web App for Containers** (Pubblica in app Web per contenitori).</span><span class="sxs-lookup"><span data-stu-id="4fe06-131">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Publish to Web App for Containers**.</span></span>

   ![Pubblicazione in un'app Web per contenitori][run-on-web-app-for-containers]

1. <span data-ttu-id="4fe06-133">Quando viene visualizzata la finestra di dialogo **Run on Web App for Containers** (Esegui in app Web per contenitori), inserire le informazioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="4fe06-133">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="4fe06-134">**Docker File** (File Docker): specifica il percorso del file Docker che è stato creato quando è stato aggiunto il supporto per Docker al progetto.</span><span class="sxs-lookup"><span data-stu-id="4fe06-134">**Docker File**: This specifies the path to the Docker file that was created when you added Docker support to your project.</span></span> 

   * <span data-ttu-id="4fe06-135">**Container Registry** (Registro Container): scegliere dal menu di scelta rapida l'istanza di Registro Container creata nella sezione precedente di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="4fe06-135">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="4fe06-136">I campi **Server URL** (URL server), **Username** (Nome utente) e **Password** verranno popolati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4fe06-136">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="4fe06-137">**Image and tag** (Immagine e tag): specifica il nome dell'immagine del contenitore. In genere viene usata la sintassi "*registro*.azurecr.io/*nomeapp*:latest", dove:</span><span class="sxs-lookup"><span data-stu-id="4fe06-137">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="4fe06-138">*registro* è il registro contenitori creato nella sezione precedente di questo articolo</span><span class="sxs-lookup"><span data-stu-id="4fe06-138">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="4fe06-139">*nomeapp* è il nome dell'app Web</span><span class="sxs-lookup"><span data-stu-id="4fe06-139">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="4fe06-140">**Web App for Container** (App Web per contenitore): scegliere **Use Existing** (Usa esistente) o **Create New** (Crea nuova) per specificare se il contenitore verrà distribuito in un'app Web esistente o ne verrà creata una nuova.</span><span class="sxs-lookup"><span data-stu-id="4fe06-140">**Web App for Container**: Choose **Use Existing** or **Create New** to specify whether you will deploy your container to an existing web app or create a new web app.</span></span>  <span data-ttu-id="4fe06-141">Con il valore specificato in **App name** (Nome app) verrà creato l'URL dell'app Web, ad esempio *wingtiptoys.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="4fe06-141">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="4fe06-142">**Resource Group** (Gruppo di risorse): specifica se usare un gruppo di risorse esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="4fe06-142">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="4fe06-143">**App Service Plan** (Piano di servizio app): specifica se usare un piano di servizio app esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="4fe06-143">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![Esecuzione in un'app Web per contenitori][run-on-web-app-linux]

1. <span data-ttu-id="4fe06-145">Al termine della configurazione delle impostazioni indicate sopra, fare clic su **OK** per pubblicare l'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="4fe06-145">When you have finished configuring the settings listed above, click **OK** to publish your web app to Azure.</span></span>

1. <span data-ttu-id="4fe06-146">Dopo la pubblicazione dell'app Web, è possibile passare all'URL specificato in precedenza per l'app Web, ad esempio *wingtiptoys.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="4fe06-146">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![Passaggio all'app Web][browsing-to-web-app]

## <a name="next-steps"></a><span data-ttu-id="4fe06-148">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4fe06-148">Next steps</span></span>

<span data-ttu-id="4fe06-149">Per altre risorse per Docker, vedere il [sito Web Docker][Docker] ufficiale.</span><span class="sxs-lookup"><span data-stu-id="4fe06-149">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

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

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
