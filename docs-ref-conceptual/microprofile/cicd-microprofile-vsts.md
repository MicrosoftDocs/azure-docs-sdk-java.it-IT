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
ms.openlocfilehash: 818e37291fa47f99cb161c63a86062bddbf6248c
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799937"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a><span data-ttu-id="738c4-103">CI/CD per applicazioni MicroProfile con Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="738c4-103">CI/CD for MicroProfile applications using Azure DevOps</span></span>

<span data-ttu-id="738c4-104">Questa esercitazione illustra come gli sviluppatori Java EE possono configurare facilmente un ciclo di rilascio CI/CD per distribuire le proprie applicazioni [MicroProfile](http://microprofile.io) in un'app Web per contenitori di Azure usando Azure DevOps (denominato formalmente VSTS).</span><span class="sxs-lookup"><span data-stu-id="738c4-104">This tutorial will show how Java EE developers can easily setup a CI/CD release cycle to deploy their [MicroProfile](http://microprofile.io) applications to an Azure Web App for Containers using Azure DevOps (formally known as VSTS).</span></span>  <span data-ttu-id="738c4-105">In questo esempio si userà un'applicazione MicroProfile che usa un'immagine di base di [Payara Micro](https://www.payara.fish/payara_micro).</span><span class="sxs-lookup"><span data-stu-id="738c4-105">In this example, we’ll be using a MicroProfile application that uses a [Payara Micro](https://www.payara.fish/payara_micro) as a base image.</span></span>   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
<span data-ttu-id="738c4-106">Il processo di inserimento in un contenitore di Azure DevOps verrà avviato compilando un'immagine Docker ed eseguendo il push dell'immagine del contenitore in un registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-106">We will start the Azure DevOps containerize process by building a Docker image and pushing the container image to an Azure Container Register.</span></span>  <span data-ttu-id="738c4-107">Verrà quindi completato con una pipeline di versione di Azure DevOps per distribuire l'immagine del contenitore in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="738c4-107">Then complete with a Azure DevOps release pipeline to deploy the container image to a Web App.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="738c4-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="738c4-108">Prerequisites</span></span>
- <span data-ttu-id="738c4-109">Copiare e salvare l'URL GIT da [GitHub](https://github.com/Azure-Samples/microprofile-hello-azure)</span><span class="sxs-lookup"><span data-stu-id="738c4-109">Copy and save the Git url from [Github](https://github.com/Azure-Samples/microprofile-hello-azure)</span></span>
- <span data-ttu-id="738c4-110">Eseguire la registrazione o l'accesso all'account [Azure DevOps](https://dev.azure.com)</span><span class="sxs-lookup"><span data-stu-id="738c4-110">Register or Log into your [Azure DevOps](https://dev.azure.com) account</span></span>
- <span data-ttu-id="738c4-111">Creare un nuovo [progetto DevOps di Azure](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) e usare l'URL GIT precedente per **importare un repository**</span><span class="sxs-lookup"><span data-stu-id="738c4-111">Create a new [Azure DevOps project](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) and use the above Git url to **import a repository**</span></span>
- <span data-ttu-id="738c4-112">Creare un [registro contenitori di Azure](https://azure.microsoft.com/en-us/services/container-registry)</span><span class="sxs-lookup"><span data-stu-id="738c4-112">Create an [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span></span>
- <span data-ttu-id="738c4-113">Creare un'app web per contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="738c4-113">Create an Azure Web App for Container</span></span>
  > [!NOTE]
  >
  > <span data-ttu-id="738c4-114">Selezionare "Avvio rapido" in Impostazioni contenitore durante il provisioning dell'istanza di app Web</span><span class="sxs-lookup"><span data-stu-id="738c4-114">Select "Quickstart" in the Container Settings when provisioning the Web App instance</span></span>


## <a name="create-a-build-definition"></a><span data-ttu-id="738c4-115">Creare una definizione di compilazione</span><span class="sxs-lookup"><span data-stu-id="738c4-115">Create a Build definition</span></span>

<span data-ttu-id="738c4-116">La definizione di compilazione in Azure DevOps esegue automaticamente tutte le attività della compilazione a ogni commit nell'applicazione di origine dell'applicazione Java EE.</span><span class="sxs-lookup"><span data-stu-id="738c4-116">The build definition in Azure DevOps automatically executes all the tasks in the build each time there’s a commit in Java EE application source application.</span></span>  <span data-ttu-id="738c4-117">In questo esempio, Azure DevOps userà Maven per compilare il progetto Java MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="738c4-117">In this example, Azure DevOps will use Maven to build the Java MicroProfile project.</span></span>

1. <span data-ttu-id="738c4-118">Fare clic sulla scheda "Compilazione e versione" nella parte superiore della pagina del progetto DevOps di Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-118">Click on the "Build and Release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="738c4-119">Selezionare quindi il collegamento **Compilazioni**.</span><span class="sxs-lookup"><span data-stu-id="738c4-119">Then, select the **Builds** link</span></span> 

<img src="media/VSTS/Buid-and-Release1.png">

2. <span data-ttu-id="738c4-120">Fare clic sul pulsante **Nuova pipeline** e quindi su **Continua** per iniziare a definire le attività di compilazione.</span><span class="sxs-lookup"><span data-stu-id="738c4-120">Click on the **New Pipeline** button, and then **Continue** to start defining your build tasks</span></span>
3. <span data-ttu-id="738c4-121">Selezionare "Maven" nell'elenco di modelli e quindi fare clic sul pulsante **Applica** per compilare il progetto Java.</span><span class="sxs-lookup"><span data-stu-id="738c4-121">Select "Maven" from the list of templates, then click on the **Apply** button to build your Java project</span></span>
4. <span data-ttu-id="738c4-122">Usare il menu a discesa per il campo Pool di agenti per selezionare l'opzione **Hosted Linux Preview** (Anteprima Linux ospitata).</span><span class="sxs-lookup"><span data-stu-id="738c4-122">Use the drop-down menu for the Agent pool field to select **Hosted Linux Preview** option.</span></span>
   > [!NOTE]
   >
   > <span data-ttu-id="738c4-123">In questo modo si indica ad Azure DevOps il server di compilazione da usare.</span><span class="sxs-lookup"><span data-stu-id="738c4-123">This informs Azure DevOps which build server to use.</span></span>  <span data-ttu-id="738c4-124">È possibile usare un server di compilazione personalizzato privato.</span><span class="sxs-lookup"><span data-stu-id="738c4-124">You can use your private customized build server</span></span>

5. <span data-ttu-id="738c4-125">Per configurare la compilazione per l'integrazione continua, selezionare la scheda **Trigger** e quindi la casella di controllo **Abilita l'integrazione continua**.</span><span class="sxs-lookup"><span data-stu-id="738c4-125">To configure your build for continuous integration, select the **Triggers** tab and check the **Enable continuous integration** checkbox.</span></span>  

<img src="media/VSTS/Build-Triggers2.png"> 

6. <span data-ttu-id="738c4-126">Selezionare la scheda <strong>Attività</strong> per tornare alla pagina principale della pipeline di compilazione.</span><span class="sxs-lookup"><span data-stu-id="738c4-126">Select the <strong>Tasks</strong> tab to return back to the main build pipeline page</span></span>
7. <span data-ttu-id="738c4-127">Usare il menu a discesa <strong>Salva e accoda</strong> per selezionare l'opzione <strong>Salva</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-127">Use the <strong>Save &amp; queue</strong> drop-down menu to select the <strong>Save</strong> option</span></span>


## <a name="create-a-docker-build-image"></a><span data-ttu-id="738c4-128">Creare un'immagine di compilazione Docker</span><span class="sxs-lookup"><span data-stu-id="738c4-128">Create a Docker Build Image</span></span>

<span data-ttu-id="738c4-129">In questa attività, Azure DevOps usa un Dockerfile con un'immagine di base di Payara Micro per creare un'immagine Docker.</span><span class="sxs-lookup"><span data-stu-id="738c4-129">In this task, Azure DevOps uses a Dockerfile with a base image from Payara Micro to create a Docker image.</span></span>  

1. <span data-ttu-id="738c4-130">Selezionare la scheda **Attività** per tornare alla pagina principale della pipeline di compilazione.</span><span class="sxs-lookup"><span data-stu-id="738c4-130">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
2. <span data-ttu-id="738c4-131">Fare clic sull'icona **+** per aggiungere una nuova attività alla definizione di compilazione.</span><span class="sxs-lookup"><span data-stu-id="738c4-131">Click on the **+** icon to add new task to the build definition</span></span>

<img src="media/VSTS/Tasks-add4.png">

3. <span data-ttu-id="738c4-132">Selezionare &quot;Docker&quot; nell'elenco di modelli e quindi fare clic sul pulsante <strong>Aggiungi</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-132">Select &quot;Docker&quot; from the list of templates, then click on the <strong>Add</strong> button</span></span>
4. <span data-ttu-id="738c4-133">Immettere un nome descrittivo nel campo <strong>Nome visualizzato</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-133">Enter a description name for the <strong>Display name</strong> field</span></span>
5. <span data-ttu-id="738c4-134">Verificare che nel menu a discesa per <strong>Tipo di registro contenitori</strong> sia selezionata l'opzione <strong>Registro contenitori di Azure</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-134">Verify that <strong>Azure Container Registry</strong> is selected in the drop-down menu for <strong>Container registry type</strong>.</span></span>
<span data-ttu-id="738c4-135">&gt;</span><span class="sxs-lookup"><span data-stu-id="738c4-135">&gt;</span></span> [!NOTE]
<span data-ttu-id="738c4-136">&gt; &gt;Se si usa l'hub Docker o un altro registro, selezionare invece &quot;Registro contenitori&quot;.</span><span class="sxs-lookup"><span data-stu-id="738c4-136">&gt; &gt;  If you are using Docker Hub or another registry, select &quot;Container Registry&quot; instead.</span></span>  <span data-ttu-id="738c4-137">Fare quindi clic sul pulsante &quot;+ Nuovo&quot; per specificare le relative credenziali e informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="738c4-137">Then click on the &quot;+ New&quot; button to provide the credentials and connection information for it.</span></span> <span data-ttu-id="738c4-138">Ignorare quindi la sezione Comandi e continuare.</span><span class="sxs-lookup"><span data-stu-id="738c4-138">Then skip to the Commands section to continue.</span></span>

6. <span data-ttu-id="738c4-139">Usare il menu a discesa **Sottoscrizione di Azure** per selezionare l'ID sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-139">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>  <span data-ttu-id="738c4-140">Fare quindi clic sul pulsante **Autorizza**.</span><span class="sxs-lookup"><span data-stu-id="738c4-140">Then click on the **Authorize** button</span></span>
7. <span data-ttu-id="738c4-141">Nel menu a discesa **Registro contenitori di Azure** selezionare il nome del registro creato in Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-141">In the **Azure container registry** drop-down menu, select registry name you created in Azure.</span></span>
8. <span data-ttu-id="738c4-142">Verificare che nel menu a discesa **Comando** sia selezionata l'opzione **build**.</span><span class="sxs-lookup"><span data-stu-id="738c4-142">Verify that **build** option is selected in the **Command** drop-down menu.</span></span>
9. <span data-ttu-id="738c4-143">Per **Dockerfile** fare clic sull'icona per lo spostamento tra i percorsi accanto alla casella di testo per selezionare il Dockerfile del progetto GitHub.</span><span class="sxs-lookup"><span data-stu-id="738c4-143">For the **Dockerfile**, click on the path navigation icon next to the textbox to select the Dockerfile from the github project.</span></span>  <span data-ttu-id="738c4-144">Fare quindi clic sul pulsante **OK**.</span><span class="sxs-lookup"><span data-stu-id="738c4-144">Then click the **OK** button.</span></span>

<img src="media/VSTS/Dockerfile5.png">

10. <span data-ttu-id="738c4-145">In **Nome immagine** selezionare la casella di controllo **Includi tag latest**.</span><span class="sxs-lookup"><span data-stu-id="738c4-145">Under the **Image name**, check the **Include latest tag** checkbox.</span></span> 
11. <span data-ttu-id="738c4-146">Usare il menu a discesa **Salva e accoda** per selezionare l'opzione **Salva**.</span><span class="sxs-lookup"><span data-stu-id="738c4-146">Use the **Save & queue** drop-down menu to select the **Save** option.</span></span>

## <a name="push-docker-image-to-acr"></a><span data-ttu-id="738c4-147">Eseguire il push dell'immagine Docker in Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="738c4-147">Push Docker Image to ACR</span></span>

<span data-ttu-id="738c4-148">In questa attività, Azure DevOps eseguirà il push dell'immagine Docker nel registro contenitori di Azure,</span><span class="sxs-lookup"><span data-stu-id="738c4-148">In this task, Azure DevOps will push the docker image to your Azure Container Registry.</span></span>  <span data-ttu-id="738c4-149">che verrà usato per eseguire l'applicazione per le API MicroProfile come app Web Java in contenitore.</span><span class="sxs-lookup"><span data-stu-id="738c4-149">This will be used to run the MicroProfile API application as a containerized Java web app.</span></span>

1. <span data-ttu-id="738c4-150">Dato che si usa Docker in Azure DevOps, creare un nuovo modello Docker ripetendo i passaggi da 1 a 7 riportati sopra nella sezione **Creare un'immagine di compilazione Docker**.</span><span class="sxs-lookup"><span data-stu-id="738c4-150">Since we are using Docker in Azure DevOps, create a new Docker template by repeating steps 1 - 7 above in the **Create a Docker Build Image** section.</span></span>
2. <span data-ttu-id="738c4-151">Selezionare **push** nel menu a discesa **Comando**.</span><span class="sxs-lookup"><span data-stu-id="738c4-151">Select **push** in the **Command** drop-down menu.</span></span>
3. <span data-ttu-id="738c4-152">Fare clic sulla scheda **Salva e accoda** e quindi selezionare l'opzione **Salva e accoda**.</span><span class="sxs-lookup"><span data-stu-id="738c4-152">Click on the **Save & queue** tab, then select **Save & queue** option.</span></span>
4. <span data-ttu-id="738c4-153">Verificare che nella finestra popup sia selezionata l'opzione **Hosted Linux Preview** (Anteprima Linux ospitata) per Pool di agenti.</span><span class="sxs-lookup"><span data-stu-id="738c4-153">Verify that the **Hosted Linux Preview** is select for the Agent pool on the pop-up window.</span></span>  <span data-ttu-id="738c4-154">Fare quindi clic sul pulsante **Salva e accoda**.</span><span class="sxs-lookup"><span data-stu-id="738c4-154">Then click on the **Save & queue** button.</span></span>
5. <span data-ttu-id="738c4-155">Fare clic sul numero di build per verificare che la pipeline di compilazione per il progetto Java sia stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="738c4-155">Click on the build number to verify that the build pipeline for the Java project completed successfully.</span></span>

<img src="media/VSTS/Build-Number6.png">


## <a name="create-a-release-definition-for-a-java-app"></a><span data-ttu-id="738c4-156">Creare una definizione di versione per un'app Java</span><span class="sxs-lookup"><span data-stu-id="738c4-156">Create a release definition for a Java app</span></span>

<span data-ttu-id="738c4-157">La pipeline di versione in Azure DevOps attiva automaticamente la distribuzione degli artefatti di compilazione in un ambiente di destinazione come Azure non appena viene completato il processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="738c4-157">The release pipeline in Azure DevOps automatically triggers the deployment of build artifacts to a target environment like Azure as soon as the Build process completes successfully.</span></span>   <span data-ttu-id="738c4-158">La pipeline di versione può essere creata per ambienti di sviluppo, di test, di staging o di produzione.</span><span class="sxs-lookup"><span data-stu-id="738c4-158">The release pipeline can be created for dev, test, staging or production environments.</span></span>

1. <span data-ttu-id="738c4-159">Fare clic sulla scheda "Compilazione e versione" nella parte superiore della pagina del progetto DevOps di Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-159">Click on the "Build and release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="738c4-160">Selezionare quindi il collegamento **Versioni**.</span><span class="sxs-lookup"><span data-stu-id="738c4-160">Then, select the **Releases** link.</span></span>

<img src="media/VSTS/Release-new-pipeline7.png">

2. <span data-ttu-id="738c4-161">Fare clic sul pulsante &quot;Nuova pipeline\*\*</span><span class="sxs-lookup"><span data-stu-id="738c4-161">Click on the &quot;New pipeline\*\* button</span></span>
3. <span data-ttu-id="738c4-162">Selezionare <strong>Distribuisci un'app Java in Servizio app di Azure</strong> nell'elenco di modelli e quindi fare clic sul pulsante <strong>Applica</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-162">Select the <strong>Deploy a Java app to Azure App Service</strong> in the list of templates, then click on the <strong>Apply</strong> button.</span></span>

<img src="media/VSTS/deploy-java-template8.png">

4. <span data-ttu-id="738c4-163">Impostare un <strong>nome di fase</strong>, ad esempio Sviluppo, Test, Staging o Produzione.</span><span class="sxs-lookup"><span data-stu-id="738c4-163">Set a <strong>Stage name</strong> (e.g Dev, Test, Staging or Production).</span></span>  <span data-ttu-id="738c4-164">Fare quindi clic sul pulsante <strong>X</strong> per chiudere la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="738c4-164">Then click on the <strong>X</strong> button to close the pop-up window</span></span>
5. <span data-ttu-id="738c4-165">Fare clic sul pulsante <strong>+ Aggiungi</strong> nella sezione Artefatti.</span><span class="sxs-lookup"><span data-stu-id="738c4-165">Click on the <strong>+ Add</strong> button in the Artifacts section.</span></span>  <span data-ttu-id="738c4-166">Gli artefatti della definizione di compilazione verranno così collegati a questa definizione di versione.</span><span class="sxs-lookup"><span data-stu-id="738c4-166">This will link artifacts from the build definition to this release definition.</span></span><br/><span data-ttu-id="738c4-167">6. Usare il menu a discesa per <strong>Origine (pipeline di compilazione)</strong> per selezionare la definizione di compilazione.</span><span class="sxs-lookup"><span data-stu-id="738c4-167">6. Use the drop-down menu for the <strong>Source (build pipeline)</strong> to select your build definition.</span></span> <span data-ttu-id="738c4-168">Fare quindi clic sul pulsante <strong>Aggiungi</strong> per continuare.</span><span class="sxs-lookup"><span data-stu-id="738c4-168">Then click the <strong>Add</strong> button to continue.</span></span>

<img src="media/VSTS/add-artifact9.png">

7. <span data-ttu-id="738c4-169">Fare clic sulla scheda <strong>Attività</strong> della pipeline.</span><span class="sxs-lookup"><span data-stu-id="738c4-169">Click on the <strong>Tasks</strong> tab on the pipeline.</span></span>  <span data-ttu-id="738c4-170">Selezionare quindi il nome della fase.</span><span class="sxs-lookup"><span data-stu-id="738c4-170">Then, select your stage name.</span></span>

<img src="media/VSTS/release-stage10.png">

8. <span data-ttu-id="738c4-171">Usare il menu a discesa **Sottoscrizione di Azure** per selezionare l'ID sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-171">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>
9. <span data-ttu-id="738c4-172">Selezionare **Linux App** (App Linux) nel menu a discesa **Tipo di app**.</span><span class="sxs-lookup"><span data-stu-id="738c4-172">Select **Linux App** from the **App type** drop-down menu</span></span>
10. <span data-ttu-id="738c4-173">Selezionare il nome dell'istanza di app Web per contenitori creata in precedenza nel menu a discesa **Nome del servizio app**.</span><span class="sxs-lookup"><span data-stu-id="738c4-173">Select the name of the Web App for Container instance you created above in the **App service name** drop-down menu</span></span>
11. <span data-ttu-id="738c4-174">Immettere il nome del registro contenitori di Azure nel campo **Registro o spazio dei nomi**.</span><span class="sxs-lookup"><span data-stu-id="738c4-174">Enter the name of your azure container registry in the **Registry or Namespaces** field.</span></span>  <span data-ttu-id="738c4-175">Ad esempio, **registropersonale.azure.io**.</span><span class="sxs-lookup"><span data-stu-id="738c4-175">E.g **myregistry.azure.io**</span></span>
12. <span data-ttu-id="738c4-176">Immettere il nome del registro nel campo **Repository**.</span><span class="sxs-lookup"><span data-stu-id="738c4-176">Enter the registry name in the **Repository** field</span></span>
13. <span data-ttu-id="738c4-177">Fare clic su **Deploy Azure App Service** (Distribuisci servizio app di Azure).</span><span class="sxs-lookup"><span data-stu-id="738c4-177">Click on **Deploy Azure App Service**.</span></span>  <span data-ttu-id="738c4-178">Immettere il tag dell'immagine del contenitore nella casella di testo **Tag**.</span><span class="sxs-lookup"><span data-stu-id="738c4-178">Enter the tag for the container image in the **Tag** textbox</span></span> 
14. <span data-ttu-id="738c4-179">Fare clic su **Esegui su agente**.</span><span class="sxs-lookup"><span data-stu-id="738c4-179">Click on **Run on agent**.</span></span>  <span data-ttu-id="738c4-180">Selezionare **Hosted Linux Preview** (Anteprima Linux ospitata) nel menu a discesa Pool di agenti.</span><span class="sxs-lookup"><span data-stu-id="738c4-180">Select **Hosted Linux Preview** in the Agent pool drop-down menu</span></span> 

## <a name="setup-environment-variables"></a><span data-ttu-id="738c4-181">Configurare le variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="738c4-181">Setup Environment Variables</span></span>

1. <span data-ttu-id="738c4-182">Fare clic sulla scheda **Variabili**.  Fare quindi clic sul pulsante **+ Aggiungi** per definire le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="738c4-182">Click on the **Variables** tab.  Then click on the **+ Add** button to define your environment variables</span></span>
2. <span data-ttu-id="738c4-183">Aggiungere il nome e i valori delle variabili per l'URL, il nome utente e la password del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="738c4-183">Add the variable name and values for your container registry url, username and password.</span></span>   <span data-ttu-id="738c4-184">Per motivi di sicurezza, fare clic sull'icona a forma di lucchetto per mantenere nascosta la password.</span><span class="sxs-lookup"><span data-stu-id="738c4-184">For security, click on the lock icon to keep the password value hidden.</span></span>

<span data-ttu-id="738c4-185">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="738c4-185">For example:</span></span>
- <span data-ttu-id="738c4-186">registry.password</span><span class="sxs-lookup"><span data-stu-id="738c4-186">registry.password</span></span>
- <span data-ttu-id="738c4-187">registry.url</span><span class="sxs-lookup"><span data-stu-id="738c4-187">registry.url</span></span>
- <span data-ttu-id="738c4-188">registry.username</span><span class="sxs-lookup"><span data-stu-id="738c4-188">registry.username</span></span>

<img src="media/VSTS/environment-variables12.png">

3. <span data-ttu-id="738c4-189">Fare clic sulla scheda **Attività** per tornare alla pagina principale della definizione della pipeline di versione.</span><span class="sxs-lookup"><span data-stu-id="738c4-189">Click on the **Tasks** tab to return to the main release pipeline definition page</span></span>
4. <span data-ttu-id="738c4-190">Fare clic su **Deploy Azure App Service** (Distribuisci servizio app di Azure).</span><span class="sxs-lookup"><span data-stu-id="738c4-190">Click on **Deploy Azure App Service**.</span></span> 
5. <span data-ttu-id="738c4-191">Espandere la sezione **Impostazioni applicazione e configurazione** e quindi fare clic sull'icona del percorso di spostamento per il campo **Impostazioni app** per aggiungere le variabili di ambiente per la connessione al registro contenitori durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="738c4-191">Expand the **Application and Configuration Settings** section, then click on the navigation path for the **App Settings** field to add environments variable to connect to the container registry during deployment.</span></span>
6. <span data-ttu-id="738c4-192">Fare clic sul pulsante "+ Aggiungi" per definire le impostazioni seguenti dell'app e assegnare le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="738c4-192">Click on the \*\* + Add\*\* button to define the following app settings and assign the environment variables</span></span>
7. <span data-ttu-id="738c4-193">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span><span class="sxs-lookup"><span data-stu-id="738c4-193">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span></span>
8. <span data-ttu-id="738c4-194">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span><span class="sxs-lookup"><span data-stu-id="738c4-194">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span></span>
9. <span data-ttu-id="738c4-195">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span><span class="sxs-lookup"><span data-stu-id="738c4-195">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span></span>

<img src="media/VSTS/environment-variables14.png">

7. <span data-ttu-id="738c4-196">Fare clic sul pulsante <strong>OK</strong> per continuare.</span><span class="sxs-lookup"><span data-stu-id="738c4-196">Click on the <strong>OK</strong> button to continue</span></span>

## <a name="setup-continious-deployment--deploy-java-application"></a><span data-ttu-id="738c4-197">Configurare la distribuzione continua e distribuire l'applicazione Java</span><span class="sxs-lookup"><span data-stu-id="738c4-197">Setup Continious Deployment & Deploy Java Application</span></span>

1. <span data-ttu-id="738c4-198">Per abilitare la distribuzione continua, fare clic sulla scheda **Pipeline**.</span><span class="sxs-lookup"><span data-stu-id="738c4-198">To enable continuous deployment, click the **Pipelines** tab</span></span>
2. <span data-ttu-id="738c4-199">Nella sezione Artefatti fare clic sull'icona a forma di fulmine.</span><span class="sxs-lookup"><span data-stu-id="738c4-199">In the Artifacts section, click on the lightening icon.</span></span>  <span data-ttu-id="738c4-200">Impostare quindi **Trigger di distribuzione continua** su Abilitato.</span><span class="sxs-lookup"><span data-stu-id="738c4-200">Then set the **Continuous deployment trigger** to Enabled.</span></span>

<img src="media/VSTS/release-enable-CD.png">

3. <span data-ttu-id="738c4-201">Fare clic sul pulsante <strong>Salva</strong> e quindi sul pulsante <strong>OK</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-201">Click on the <strong>Save</strong> button, then the <strong>OK</strong> button</span></span> 
4. <span data-ttu-id="738c4-202">Fare clic sul menu a discesa <strong>+ Versione</strong> e quindi selezionare il collegamento <strong>Crea una versione</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-202">Click on the <strong>+ Release</strong> drop-down menu, then select <strong>Create a release</strong> link</span></span>
5. <span data-ttu-id="738c4-203">Usare il menu a discesa <strong>Fasi per la modifica di un trigger da automatizzato a manuale</strong> per selezionare la casella di controllo per il nome della fase.</span><span class="sxs-lookup"><span data-stu-id="738c4-203">Use the <strong>Stages for a trigger change from automated to manual</strong> drop-down menu to select the checkbox for your stage name</span></span>
6. <span data-ttu-id="738c4-204">Fare clic sul pulsante <strong>Crea</strong> per continuare.</span><span class="sxs-lookup"><span data-stu-id="738c4-204">Click the <strong>Create</strong> button to continue</span></span>
7. <span data-ttu-id="738c4-205">Fare clic sul numero di versione.</span><span class="sxs-lookup"><span data-stu-id="738c4-205">Click on the release number.</span></span>  <span data-ttu-id="738c4-206">Passare quindi il puntatore del mouse sul nome della fase e fare clic su <strong>Distribuisci</strong>.</span><span class="sxs-lookup"><span data-stu-id="738c4-206">Then hover your mouse cursor over the stage name and click on the <strong>Deploy</strong> button</span></span>
8. <span data-ttu-id="738c4-207">Fare quindi clic sul pulsante <strong>Distribuisci</strong> nella finestra popup per avviare il processo di distribuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="738c4-207">The click on the <strong>Deploy</strong> button on the pop-up window to start the deployment process to Azure</span></span>


## <a name="test-the-java-web-application"></a><span data-ttu-id="738c4-208">Testare l'applicazione Web Java</span><span class="sxs-lookup"><span data-stu-id="738c4-208">Test the Java Web Application</span></span>
1. <span data-ttu-id="738c4-209">Eseguire l'URL dell'app Web in un Web browser:</span><span class="sxs-lookup"><span data-stu-id="738c4-209">Run the web app url in web browser:</span></span>  
   <span data-ttu-id="738c4-210">https://{nome-servizio-app}.azurewebsites.net/api/hello</span><span class="sxs-lookup"><span data-stu-id="738c4-210">https://{your-app-service-name}.azurewebsites.net/api/hello</span></span>


<img src="media/VSTS/web-app16.png">

2. <span data-ttu-id="738c4-211">Nella pagina Web verrà visualizzato **Hello Azure!**</span><span class="sxs-lookup"><span data-stu-id="738c4-211">The web page should say **Hello Azure!**</span></span>

<img src="media/VSTS/web-api17.png">
