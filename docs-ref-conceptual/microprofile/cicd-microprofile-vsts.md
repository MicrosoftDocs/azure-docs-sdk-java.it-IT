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
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a><span data-ttu-id="d1c06-103">CI/CD per applicazioni MicroProfile con Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="d1c06-103">CI/CD for MicroProfile applications using Azure DevOps</span></span>

<span data-ttu-id="d1c06-104">Questa esercitazione illustra come gli sviluppatori Java EE possono configurare facilmente un ciclo di rilascio CI/CD per distribuire le proprie applicazioni [MicroProfile](http://microprofile.io) in un'app Web per contenitori di Azure usando Azure DevOps (denominato formalmente VSTS).</span><span class="sxs-lookup"><span data-stu-id="d1c06-104">This tutorial will show how Java EE developers can easily setup a CI/CD release cycle to deploy their [MicroProfile](http://microprofile.io) applications to an Azure Web App for Containers using Azure DevOps (formally known as VSTS).</span></span>  <span data-ttu-id="d1c06-105">In questo esempio si userà un'applicazione MicroProfile che usa un'immagine di base di [Payara Micro](https://www.payara.fish/payara_micro).</span><span class="sxs-lookup"><span data-stu-id="d1c06-105">In this example, we’ll be using a MicroProfile application that uses a [Payara Micro](https://www.payara.fish/payara_micro) as a base image.</span></span>   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
<span data-ttu-id="d1c06-106">Il processo di inserimento in un contenitore di Azure DevOps verrà avviato compilando un'immagine Docker ed eseguendo il push dell'immagine del contenitore in un registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-106">We will start the Azure DevOps containerize process by building a Docker image and pushing the container image to an Azure Container Register.</span></span>  <span data-ttu-id="d1c06-107">Verrà quindi completato con una pipeline di versione di Azure DevOps per distribuire l'immagine del contenitore in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="d1c06-107">Then complete with a Azure DevOps release pipeline to deploy the container image to a Web App.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1c06-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d1c06-108">Prerequisites</span></span>
- <span data-ttu-id="d1c06-109">Copiare e salvare l'URL GIT da [GitHub](https://github.com/Azure-Samples/microprofile-hello-azure)</span><span class="sxs-lookup"><span data-stu-id="d1c06-109">Copy and save the Git url from [Github](https://github.com/Azure-Samples/microprofile-hello-azure)</span></span>
- <span data-ttu-id="d1c06-110">Eseguire la registrazione o l'accesso all'account [Azure DevOps](https://dev.azure.com)</span><span class="sxs-lookup"><span data-stu-id="d1c06-110">Register or Log into your [Azure DevOps](https://dev.azure.com) account</span></span>
- <span data-ttu-id="d1c06-111">Creare un nuovo [progetto DevOps di Azure](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) e usare l'URL GIT precedente per **importare un repository**</span><span class="sxs-lookup"><span data-stu-id="d1c06-111">Create a new [Azure DevOps project](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) and use the above Git url to **import a repository**</span></span>
- <span data-ttu-id="d1c06-112">Creare un [registro contenitori di Azure](https://azure.microsoft.com/en-us/services/container-registry)</span><span class="sxs-lookup"><span data-stu-id="d1c06-112">Create an [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span></span>
- <span data-ttu-id="d1c06-113">Creare un'app web per contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="d1c06-113">Create an Azure Web App for Container</span></span>
> [!NOTE]
>
> <span data-ttu-id="d1c06-114">Selezionare "Avvio rapido" in Impostazioni contenitore durante il provisioning dell'istanza di app Web</span><span class="sxs-lookup"><span data-stu-id="d1c06-114">Select "Quickstart" in the Container Settings when provisioning the Web App instance</span></span>


## <a name="create-a-build-definition"></a><span data-ttu-id="d1c06-115">Creare una definizione di compilazione</span><span class="sxs-lookup"><span data-stu-id="d1c06-115">Create a Build definition</span></span>

<span data-ttu-id="d1c06-116">La definizione di compilazione in Azure DevOps esegue automaticamente tutte le attività della compilazione a ogni commit nell'applicazione di origine dell'applicazione Java EE.</span><span class="sxs-lookup"><span data-stu-id="d1c06-116">The build definition in Azure DevOps automatically executes all the tasks in the build each time there’s a commit in Java EE application source application.</span></span>  <span data-ttu-id="d1c06-117">In questo esempio, Azure DevOps userà Maven per compilare il progetto Java MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="d1c06-117">In this example, Azure DevOps will use Maven to build the Java MicroProfile project.</span></span>

1. <span data-ttu-id="d1c06-118">Fare clic sulla scheda "Compilazione e versione" nella parte superiore della pagina del progetto DevOps di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-118">Click on the "Build and Release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="d1c06-119">Selezionare quindi il collegamento **Compilazioni**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-119">Then, select the **Builds** link</span></span> 

<img src="media/VSTS/Buid-and-Release1.png">

2. <span data-ttu-id="d1c06-120">Fare clic sul pulsante **Nuova pipeline** e quindi su **Continua** per iniziare a definire le attività di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-120">Click on the **New Pipeline** button, and then **Continue** to start defining your build tasks</span></span>
3. <span data-ttu-id="d1c06-121">Selezionare "Maven" nell'elenco di modelli e quindi fare clic sul pulsante **Applica** per compilare il progetto Java.</span><span class="sxs-lookup"><span data-stu-id="d1c06-121">Select "Maven" from the list of templates, then click on the **Apply** button to build your Java project</span></span>
4. <span data-ttu-id="d1c06-122">Usare il menu a discesa per il campo Pool di agenti per selezionare l'opzione **Hosted Linux Preview** (Anteprima Linux ospitata).</span><span class="sxs-lookup"><span data-stu-id="d1c06-122">Use the drop-down menu for the Agent pool field to select **Hosted Linux Preview** option.</span></span>
> [!NOTE]
>
> <span data-ttu-id="d1c06-123">In questo modo si indica ad Azure DevOps il server di compilazione da usare.</span><span class="sxs-lookup"><span data-stu-id="d1c06-123">This informs Azure DevOps which build server to use.</span></span>  <span data-ttu-id="d1c06-124">È possibile usare un server di compilazione personalizzato privato.</span><span class="sxs-lookup"><span data-stu-id="d1c06-124">You can use your private customized build server</span></span>

5. <span data-ttu-id="d1c06-125">Per configurare la compilazione per l'integrazione continua, selezionare la scheda **Trigger** e quindi la casella di controllo **Abilita l'integrazione continua**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-125">To configure your build for continuous integration, select the **Triggers** tab and check the **Enable continuous integration** checkbox.</span></span>  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. <span data-ttu-id="d1c06-126">Selezionare la scheda **Attività** per tornare alla pagina principale della pipeline di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-126">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
7. <span data-ttu-id="d1c06-127">Usare il menu a discesa **Salva e accoda** per selezionare l'opzione **Salva**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-127">Use the **Save & queue** drop-down menu to select the **Save** option</span></span>
 

## <a name="create-a-docker-build-image"></a><span data-ttu-id="d1c06-128">Creare un'immagine di compilazione Docker</span><span class="sxs-lookup"><span data-stu-id="d1c06-128">Create a Docker Build Image</span></span>

<span data-ttu-id="d1c06-129">In questa attività, Azure DevOps usa un Dockerfile con un'immagine di base di Payara Micro per creare un'immagine Docker.</span><span class="sxs-lookup"><span data-stu-id="d1c06-129">In this task, Azure DevOps uses a Dockerfile with a base image from Payara Micro to create a Docker image.</span></span>  

1. <span data-ttu-id="d1c06-130">Selezionare la scheda **Attività** per tornare alla pagina principale della pipeline di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-130">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
2. <span data-ttu-id="d1c06-131">Fare clic sull'icona **+** per aggiungere una nuova attività alla definizione di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-131">Click on the **+** icon to add new task to the build definition</span></span>
 
<img src="media/VSTS/Tasks-add4.png">
 
3. <span data-ttu-id="d1c06-132">Selezionare "Docker" nell'elenco di modelli e quindi fare clic sul pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-132">Select "Docker" from the list of templates, then click on the **Add** button</span></span>
4. <span data-ttu-id="d1c06-133">Immettere un nome descrittivo nel campo **Nome visualizzato**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-133">Enter a description name for the **Display name** field</span></span>
5. <span data-ttu-id="d1c06-134">Verificare che nel menu a discesa per **Tipo di registro contenitori** sia selezionata l'opzione **Registro contenitori di Azure**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-134">Verify that **Azure Container Registry** is selected in the drop-down menu for **Container registry type**.</span></span>
> [!NOTE]
>
>  <span data-ttu-id="d1c06-135">Se si usa l'hub Docker o un altro registro, selezionare invece "Registro contenitori".</span><span class="sxs-lookup"><span data-stu-id="d1c06-135">If you are using Docker Hub or another registry, select "Container Registry" instead.</span></span>  <span data-ttu-id="d1c06-136">Fare quindi clic sul pulsante "+ Nuovo" per specificare le relative credenziali e informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-136">Then click on the "+ New" button to provide the credentials and connection information for it.</span></span> <span data-ttu-id="d1c06-137">Ignorare quindi la sezione Comandi e continuare.</span><span class="sxs-lookup"><span data-stu-id="d1c06-137">Then skip to the Commands section to continue.</span></span>

6. <span data-ttu-id="d1c06-138">Usare il menu a discesa **Sottoscrizione di Azure** per selezionare l'ID sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-138">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>  <span data-ttu-id="d1c06-139">Fare quindi clic sul pulsante **Autorizza**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-139">Then click on the **Authorize** button</span></span>
7. <span data-ttu-id="d1c06-140">Nel menu a discesa **Registro contenitori di Azure** selezionare il nome del registro creato in Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-140">In the **Azure container registry** drop-down menu, select registry name you created in Azure.</span></span>
8. <span data-ttu-id="d1c06-141">Verificare che nel menu a discesa **Comando** sia selezionata l'opzione **build**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-141">Verify that **build** option is selected in the **Command** drop-down menu.</span></span>
9. <span data-ttu-id="d1c06-142">Per **Dockerfile** fare clic sull'icona per lo spostamento tra i percorsi accanto alla casella di testo per selezionare il Dockerfile del progetto GitHub.</span><span class="sxs-lookup"><span data-stu-id="d1c06-142">For the **Dockerfile**, click on the path navigation icon next to the textbox to select the Dockerfile from the github project.</span></span>  <span data-ttu-id="d1c06-143">Fare quindi clic sul pulsante **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-143">Then click the **OK** button.</span></span>

<img src="media/VSTS/Dockerfile5.png">

10. <span data-ttu-id="d1c06-144">In **Nome immagine** selezionare la casella di controllo **Includi tag latest**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-144">Under the **Image name**, check the **Include latest tag** checkbox.</span></span> 
11. <span data-ttu-id="d1c06-145">Usare il menu a discesa **Salva e accoda** per selezionare l'opzione **Salva**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-145">Use the **Save & queue** drop-down menu to select the **Save** option.</span></span>

## <a name="push-docker-image-to-acr"></a><span data-ttu-id="d1c06-146">Eseguire il push dell'immagine Docker in Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="d1c06-146">Push Docker Image to ACR</span></span>

<span data-ttu-id="d1c06-147">In questa attività, Azure DevOps eseguirà il push dell'immagine Docker nel registro contenitori di Azure,</span><span class="sxs-lookup"><span data-stu-id="d1c06-147">In this task, Azure DevOps will push the docker image to your Azure Container Registry.</span></span>  <span data-ttu-id="d1c06-148">che verrà usato per eseguire l'applicazione per le API MicroProfile come app Web Java in contenitore.</span><span class="sxs-lookup"><span data-stu-id="d1c06-148">This will be used to run the MicroProfile API application as a containerized Java web app.</span></span>

1. <span data-ttu-id="d1c06-149">Dato che si usa Docker in Azure DevOps, creare un nuovo modello Docker ripetendo i passaggi da 1 a 7 riportati sopra nella sezione **Creare un'immagine di compilazione Docker**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-149">Since we are using Docker in Azure DevOps, create a new Docker template by repeating steps 1 - 7 above in the **Create a Docker Build Image** section.</span></span>
2. <span data-ttu-id="d1c06-150">Selezionare **push** nel menu a discesa **Comando**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-150">Select **push** in the **Command** drop-down menu.</span></span>
3. <span data-ttu-id="d1c06-151">Fare clic sulla scheda **Salva e accoda** e quindi selezionare l'opzione **Salva e accoda**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-151">Click on the **Save & queue** tab, then select **Save & queue** option.</span></span>
4. <span data-ttu-id="d1c06-152">Verificare che nella finestra popup sia selezionata l'opzione **Hosted Linux Preview** (Anteprima Linux ospitata) per Pool di agenti.</span><span class="sxs-lookup"><span data-stu-id="d1c06-152">Verify that the **Hosted Linux Preview** is select for the Agent pool on the pop-up window.</span></span>  <span data-ttu-id="d1c06-153">Fare quindi clic sul pulsante **Salva e accoda**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-153">Then click on the **Save & queue** button.</span></span>
5. <span data-ttu-id="d1c06-154">Fare clic sul numero di build per verificare che la pipeline di compilazione per il progetto Java sia stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="d1c06-154">Click on the build number to verify that the build pipeline for the Java project completed successfully.</span></span>

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a><span data-ttu-id="d1c06-155">Creare una definizione di versione per un'app Java</span><span class="sxs-lookup"><span data-stu-id="d1c06-155">Create a release definition for a Java app</span></span>

<span data-ttu-id="d1c06-156">La pipeline di versione in Azure DevOps attiva automaticamente la distribuzione degli artefatti di compilazione in un ambiente di destinazione come Azure non appena viene completato il processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-156">The release pipeline in Azure DevOps automatically triggers the deployment of build artifacts to a target environment like Azure as soon as the Build process completes successfully.</span></span>   <span data-ttu-id="d1c06-157">La pipeline di versione può essere creata per ambienti di sviluppo, di test, di staging o di produzione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-157">The release pipeline can be created for dev, test, staging or production environments.</span></span>

1. <span data-ttu-id="d1c06-158">Fare clic sulla scheda "Compilazione e versione" nella parte superiore della pagina del progetto DevOps di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-158">Click on the "Build and release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="d1c06-159">Selezionare quindi il collegamento **Versioni**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-159">Then, select the **Releases** link.</span></span>

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. <span data-ttu-id="d1c06-160">Fare clic sul pulsante "Nuova pipeline".</span><span class="sxs-lookup"><span data-stu-id="d1c06-160">Click on the "New pipeline\*\* button</span></span>
3. <span data-ttu-id="d1c06-161">Selezionare **Distribuisci un'app Java in Servizio app di Azure** nell'elenco di modelli e quindi fare clic sul pulsante **Applica**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-161">Select the **Deploy a Java app to Azure App Service** in the list of templates, then click on the **Apply** button.</span></span>

<img src="media/VSTS/deploy-java-template8.png">
 
4. <span data-ttu-id="d1c06-162">Impostare un **nome di fase**, ad esempio Sviluppo, Test, Staging o Produzione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-162">Set a **Stage name** (e.g Dev, Test, Staging or Production).</span></span>  <span data-ttu-id="d1c06-163">Fare quindi clic sul pulsante **X** per chiudere la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="d1c06-163">Then click on the **X** button to close the pop-up window</span></span>
5. <span data-ttu-id="d1c06-164">Fare clic sul pulsante **+ Aggiungi** nella sezione Artefatti.</span><span class="sxs-lookup"><span data-stu-id="d1c06-164">Click on the **+ Add** button in the Artifacts section.</span></span>  <span data-ttu-id="d1c06-165">Gli artefatti della definizione di compilazione verranno così collegati a questa definizione di versione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-165">This will link artifacts from the build definition to this release definition.</span></span>  
6. <span data-ttu-id="d1c06-166">Usare il menu a discesa per **Origine (pipeline di compilazione)** per selezionare la definizione di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-166">Use the drop-down menu for the **Source (build pipeline)** to select your build definition.</span></span> <span data-ttu-id="d1c06-167">Fare quindi clic sul pulsante **Aggiungi** per continuare.</span><span class="sxs-lookup"><span data-stu-id="d1c06-167">Then click the **Add** button to continue.</span></span>

<img src="media/VSTS/add-artifact9.png">
 
7. <span data-ttu-id="d1c06-168">Fare clic sulla scheda **Attività** della pipeline.</span><span class="sxs-lookup"><span data-stu-id="d1c06-168">Click on the **Tasks** tab on the pipeline.</span></span>  <span data-ttu-id="d1c06-169">Selezionare quindi il nome della fase.</span><span class="sxs-lookup"><span data-stu-id="d1c06-169">Then, select your stage name.</span></span>
 
<img src="media/VSTS/release-stage10.png">

8. <span data-ttu-id="d1c06-170">Usare il menu a discesa **Sottoscrizione di Azure** per selezionare l'ID sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-170">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>
9. <span data-ttu-id="d1c06-171">Selezionare **Linux App** (App Linux) nel menu a discesa **Tipo di app**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-171">Select **Linux App** from the **App type** drop-down menu</span></span>
10. <span data-ttu-id="d1c06-172">Selezionare il nome dell'istanza di app Web per contenitori creata in precedenza nel menu a discesa **Nome del servizio app**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-172">Select the name of the Web App for Container instance you created above in the **App service name** drop-down menu</span></span>
11. <span data-ttu-id="d1c06-173">Immettere il nome del registro contenitori di Azure nel campo **Registro o spazio dei nomi**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-173">Enter the name of your azure container registry in the **Registry or Namespaces** field.</span></span>  <span data-ttu-id="d1c06-174">Ad esempio, **registropersonale.azure.io**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-174">E.g **myregistry.azure.io**</span></span>
12. <span data-ttu-id="d1c06-175">Immettere il nome del registro nel campo **Repository**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-175">Enter the registry name in the **Repository** field</span></span>
13. <span data-ttu-id="d1c06-176">Fare clic su **Deploy Azure App Service** (Distribuisci servizio app di Azure).</span><span class="sxs-lookup"><span data-stu-id="d1c06-176">Click on **Deploy Azure App Service**.</span></span>  <span data-ttu-id="d1c06-177">Immettere il tag dell'immagine del contenitore nella casella di testo **Tag**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-177">Enter the tag for the container image in the **Tag** textbox</span></span> 
14. <span data-ttu-id="d1c06-178">Fare clic su **Esegui su agente**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-178">Click on **Run on agent**.</span></span>  <span data-ttu-id="d1c06-179">Selezionare **Hosted Linux Preview** (Anteprima Linux ospitata) nel menu a discesa Pool di agenti.</span><span class="sxs-lookup"><span data-stu-id="d1c06-179">Select **Hosted Linux Preview** in the Agent pool drop-down menu</span></span> 

## <a name="setup-environment-variables"></a><span data-ttu-id="d1c06-180">Configurare le variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d1c06-180">Setup Environment Variables</span></span>

1. <span data-ttu-id="d1c06-181">Fare clic sulla scheda **Variabili**.  Fare quindi clic sul pulsante **+ Aggiungi** per definire le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d1c06-181">Click on the **Variables** tab.  Then click on the **+ Add** button to define your environment variables</span></span>
2. <span data-ttu-id="d1c06-182">Aggiungere il nome e i valori delle variabili per l'URL, il nome utente e la password del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="d1c06-182">Add the variable name and values for your container registry url, username and password.</span></span>   <span data-ttu-id="d1c06-183">Per motivi di sicurezza, fare clic sull'icona a forma di lucchetto per mantenere nascosta la password.</span><span class="sxs-lookup"><span data-stu-id="d1c06-183">For security, click on the lock icon to keep the password value hidden.</span></span>

<span data-ttu-id="d1c06-184">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="d1c06-184">For example:</span></span>
- <span data-ttu-id="d1c06-185">registry.password</span><span class="sxs-lookup"><span data-stu-id="d1c06-185">registry.password</span></span>
- <span data-ttu-id="d1c06-186">registry.url</span><span class="sxs-lookup"><span data-stu-id="d1c06-186">registry.url</span></span>
- <span data-ttu-id="d1c06-187">registry.username</span><span class="sxs-lookup"><span data-stu-id="d1c06-187">registry.username</span></span>

<img src="media/VSTS/environment-variables12.png">

3. <span data-ttu-id="d1c06-188">Fare clic sulla scheda **Attività** per tornare alla pagina principale della definizione della pipeline di versione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-188">Click on the **Tasks** tab to return to the main release pipeline definition page</span></span>
4. <span data-ttu-id="d1c06-189">Fare clic su **Deploy Azure App Service** (Distribuisci servizio app di Azure).</span><span class="sxs-lookup"><span data-stu-id="d1c06-189">Click on **Deploy Azure App Service**.</span></span> 
5. <span data-ttu-id="d1c06-190">Espandere la sezione **Impostazioni applicazione e configurazione** e quindi fare clic sull'icona del percorso di spostamento per il campo **Impostazioni app** per aggiungere le variabili di ambiente per la connessione al registro contenitori durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-190">Expand the **Application and Configuration Settings** section, then click on the navigation path for the **App Settings** field to add environments variable to connect to the container registry during deployment.</span></span>
6. <span data-ttu-id="d1c06-191">Fare clic sul pulsante "+ Aggiungi" per definire le impostazioni seguenti dell'app e assegnare le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d1c06-191">Click on the \*\* + Add\*\* button to define the following app settings and assign the environment variables</span></span>
- <span data-ttu-id="d1c06-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span><span class="sxs-lookup"><span data-stu-id="d1c06-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span></span>
- <span data-ttu-id="d1c06-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span><span class="sxs-lookup"><span data-stu-id="d1c06-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span></span>
- <span data-ttu-id="d1c06-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span><span class="sxs-lookup"><span data-stu-id="d1c06-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span></span>

<img src="media/VSTS/environment-variables14.png">
 
7. <span data-ttu-id="d1c06-195">Fare clic sul pulsante **OK** per continuare.</span><span class="sxs-lookup"><span data-stu-id="d1c06-195">Click on the **OK** button to continue</span></span>

## <a name="setup-continious-deployment--deploy-java-application"></a><span data-ttu-id="d1c06-196">Configurare la distribuzione continua e distribuire l'applicazione Java</span><span class="sxs-lookup"><span data-stu-id="d1c06-196">Setup Continious Deployment & Deploy Java Application</span></span>

1. <span data-ttu-id="d1c06-197">Per abilitare la distribuzione continua, fare clic sulla scheda **Pipeline**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-197">To enable continuous deployment, click the **Pipelines** tab</span></span>
2. <span data-ttu-id="d1c06-198">Nella sezione Artefatti fare clic sull'icona a forma di fulmine.</span><span class="sxs-lookup"><span data-stu-id="d1c06-198">In the Artifacts section, click on the lightening icon.</span></span>  <span data-ttu-id="d1c06-199">Impostare quindi **Trigger di distribuzione continua** su Abilitato.</span><span class="sxs-lookup"><span data-stu-id="d1c06-199">Then set the **Continuous deployment trigger** to Enabled.</span></span>

<img src="media/VSTS/release-enable-CD.png">
 
3. <span data-ttu-id="d1c06-200">Fare clic sul pulsante **Salva** e quindi sul pulsante **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-200">Click on the **Save** button, then the **OK** button</span></span> 
4. <span data-ttu-id="d1c06-201">Fare clic sul menu a discesa **+ Versione** e quindi selezionare il collegamento **Crea una versione**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-201">Click on the **+ Release** drop-down menu, then select **Create a release** link</span></span>
5. <span data-ttu-id="d1c06-202">Usare il menu a discesa **Fasi per la modifica di un trigger da automatizzato a manuale** per selezionare la casella di controllo per il nome della fase.</span><span class="sxs-lookup"><span data-stu-id="d1c06-202">Use the **Stages for a trigger change from automated to manual** drop-down menu to select the checkbox for your stage name</span></span>
6. <span data-ttu-id="d1c06-203">Fare clic sul pulsante **Crea** per continuare.</span><span class="sxs-lookup"><span data-stu-id="d1c06-203">Click the **Create** button to continue</span></span>
7. <span data-ttu-id="d1c06-204">Fare clic sul numero di versione.</span><span class="sxs-lookup"><span data-stu-id="d1c06-204">Click on the release number.</span></span>  <span data-ttu-id="d1c06-205">Passare quindi il puntatore del mouse sul nome della fase e fare clic su **Distribuisci**.</span><span class="sxs-lookup"><span data-stu-id="d1c06-205">Then hover your mouse cursor over the stage name and click on the **Deploy** button</span></span>
8. <span data-ttu-id="d1c06-206">Fare quindi clic sul pulsante **Distribuisci** nella finestra popup per avviare il processo di distribuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="d1c06-206">The click on the **Deploy** button on the pop-up window to start the deployment process to Azure</span></span>


## <a name="test-the-java-web-application"></a><span data-ttu-id="d1c06-207">Testare l'applicazione Web Java</span><span class="sxs-lookup"><span data-stu-id="d1c06-207">Test the Java Web Application</span></span>
1. <span data-ttu-id="d1c06-208">Eseguire l'URL dell'app Web in un Web browser:</span><span class="sxs-lookup"><span data-stu-id="d1c06-208">Run the web app url in web browser:</span></span>  
<span data-ttu-id="d1c06-209">https://{nome-servizio-app}.azurewebsites.net/api/hello</span><span class="sxs-lookup"><span data-stu-id="d1c06-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span></span>

 
<img src="media/VSTS/web-app16.png">

2. <span data-ttu-id="d1c06-210">Nella pagina Web verrà visualizzato **Hello Azure!**</span><span class="sxs-lookup"><span data-stu-id="d1c06-210">The web page should say **Hello Azure!**</span></span>
 
<img src="media/VSTS/web-api17.png">
