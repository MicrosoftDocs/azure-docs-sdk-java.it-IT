---
title: Pubblicare un'app Spring Boot come contenitore Docker usando il Toolkit di Azure per IntelliJ
description: Informazioni su come pubblicare un'app Web in Microsoft Azure come contenitore Docker usando il Toolkit di Azure per IntelliJ.
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm
ms.openlocfilehash: 19621d0b780cf0607171fd8c6d46aaf17d57cc49
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="63693-103">Pubblicare un'app Spring Boot come contenitore Docker usando il Toolkit di Azure per IntelliJ</span><span class="sxs-lookup"><span data-stu-id="63693-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="63693-104">[Spring Framework] è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="63693-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="63693-105">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="63693-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="63693-106">[Docker] è una soluzione open source che consente agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni in esecuzione nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="63693-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="63693-107">Questa esercitazione illustra i passaggi per distribuire un'applicazione Spring Boot come contenitore Docker in Microsoft Azure usando il Toolkit di Azure per IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="63693-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="63693-108">Clonare il repository predefinito per Docker Spring Boot</span><span class="sxs-lookup"><span data-stu-id="63693-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="63693-109">I passaggi seguenti illustrano la clonazione del repository Docker Spring Boot tramite IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="63693-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="63693-110">Se si intende usare una riga di comando, vedere [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS] (Distribuire un'applicazione Spring Boot in Linux nel servizio contenitore di Azure).</span><span class="sxs-lookup"><span data-stu-id="63693-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="63693-111">Aprire IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="63693-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="63693-112">Nella schermata iniziale scegliere l'opzione **GitHub** dall'elenco **Check out from version control** (Estrai da controllo della versione).</span><span class="sxs-lookup"><span data-stu-id="63693-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![Opzione GitHub per il controllo della versione][CL01]

1. <span data-ttu-id="63693-114">Immettere le proprie credenziali se viene richiesto di accedere.</span><span class="sxs-lookup"><span data-stu-id="63693-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="63693-115">Se si usano un nome utente e una password per accedere a GitHub:</span><span class="sxs-lookup"><span data-stu-id="63693-115">If you are using a username/password to log in to GitHub:</span></span> 

      ![Finestra di dialogo per l'immissione del nome utente e password GitHub][CL02a]

   * <span data-ttu-id="63693-117">Se si usa un token per l'accesso a GitHub:</span><span class="sxs-lookup"><span data-stu-id="63693-117">If you are using a token to log in to GitHub:</span></span> 

      ![Finestra di dialogo per l'immissione di un token GitHub][CL02b]

1. <span data-ttu-id="63693-119">Immettere **https://github.com/spring-guides/gs-spring-boot-docker.git** per l'URL del repository, specificare il percorso locale e le informazioni sulla cartella e quindi fare clic su **Clone** (Clona).</span><span class="sxs-lookup"><span data-stu-id="63693-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Finestra di dialogo per la clonazione del repository][CL03]

1. <span data-ttu-id="63693-121">Scegliere **No** quando viene richiesto di creare un progetto IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="63693-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![Rifiuto di creare un progetto IntelliJ][CL04]

1. <span data-ttu-id="63693-123">Nella home page fare clic su **Import Project** (Importa progetto).</span><span class="sxs-lookup"><span data-stu-id="63693-123">On the welcome page, click **Import Project**.</span></span>

   ![Selezione del progetto da importare][CL05]

1. <span data-ttu-id="63693-125">Individuare il percorso in cui è stato clonato il repository Spring Boot, evidenziare la cartella **Complete** (Completo) sotto la radice e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="63693-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![Selezionare una cartella per l'importazione][CL06]

1. <span data-ttu-id="63693-127">Quando richiesto, scegliere **Create project from existing sources** (Crea progetto da origini esistenti).</span><span class="sxs-lookup"><span data-stu-id="63693-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![Opzione per creare un progetto da origini esistenti][CL07]

1. <span data-ttu-id="63693-129">Specificare il nome del progetto o accettare il valore predefinito, verificare il percorso corretto della cartella **Complete** (Completo) e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![Specificare il nome del progetto][CL08]

1. <span data-ttu-id="63693-131">Personalizzare le directory per l'importazione e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Scegliere le directory][CL09]

1. <span data-ttu-id="63693-133">Esaminare le librerie da importare e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-133">Review the libraries to import, and then click **Next**.</span></span>

   ![Esaminare le librerie del progetto][CL10]

1. <span data-ttu-id="63693-135">Esaminare la struttura del modulo e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-135">Review the module structure, and then click **Next**.</span></span>

   ![Esaminare la struttura del modulo][CL11]

1. <span data-ttu-id="63693-137">Specificare il JDK e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-137">Specify your JDK, and then click **Next**.</span></span>

   ![Specificare un JDK][CL12]

1. <span data-ttu-id="63693-139">Fare clic su **Finish**.</span><span class="sxs-lookup"><span data-stu-id="63693-139">Click **Finish**.</span></span>

   ![Pulsante Fine][CL13]

<span data-ttu-id="63693-141">IntelliJ importa l'app Spring Boot come un progetto e visualizza la struttura al termine dell'importazione.</span><span class="sxs-lookup"><span data-stu-id="63693-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![App Spring Boot in IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="63693-143">Compilare l'app Spring Boot</span><span class="sxs-lookup"><span data-stu-id="63693-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="63693-144">Compilare l'app con Maven POM</span><span class="sxs-lookup"><span data-stu-id="63693-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="63693-145">Se non è già aperta, aprire la finestra degli strumenti di Maven.</span><span class="sxs-lookup"><span data-stu-id="63693-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="63693-146">Fare clic su **Visualizza** > **Finestre degli strumenti** > **Maven Projects** (Progetti Maven).</span><span class="sxs-lookup"><span data-stu-id="63693-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Comandi Finestre degli strumenti e Maven Projects (Progetti Maven)][BU01]

1. <span data-ttu-id="63693-148">Nella finestra degli strumenti di Maven fare clic con il pulsante destro del mouse su **package** (pacchetto) e scegliere **Run Maven Build** (Esegui compilazione Maven).</span><span class="sxs-lookup"><span data-stu-id="63693-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="63693-149">Se il progetto Maven non viene visualizzato automaticamente, fare clic sull'icona **Reimport** (Reimporta) sulla barra degli strumenti di Maven.</span><span class="sxs-lookup"><span data-stu-id="63693-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![Comando Run Maven Build (Esegui compilazione Maven)][BU02]

1. <span data-ttu-id="63693-151">Una volta creata correttamente l'app Spring Boot, IntelliJ visualizzerà un messaggio **BUILD SUCCESS** (COMPILAZIONE COMPLETATA).</span><span class="sxs-lookup"><span data-stu-id="63693-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![Messaggio BUILD SUCCESS (COMPILAZIONE COMPLETATA)][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="63693-153">Creare un elemento pronto per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="63693-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="63693-154">Per pubblicare l'app Spring Boot, è necessario creare un elemento pronto per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="63693-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="63693-155">Seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="63693-155">Use the following steps:</span></span>

1. <span data-ttu-id="63693-156">Aprire il progetto dell'app Web in IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="63693-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="63693-157">Fare clic su **File** e quindi fare clic su **Project Structure** (Struttura del progetto).</span><span class="sxs-lookup"><span data-stu-id="63693-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Comando Project Structure (Struttura del progetto)][ART01]

1. <span data-ttu-id="63693-159">Fare clic sul simbolo più di colore verde (**+**) per aggiungere un elemento, fare clic su **JAR** e quindi fare clic su **Empty** (Vuoto).</span><span class="sxs-lookup"><span data-stu-id="63693-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Aggiungere un elemento][ART02]

1. <span data-ttu-id="63693-161">Assegnare un nome all'elemento assicurandosi di non aggiungere l'estensione ".jar" e quindi specificare la cartella di destinazione per l'output di Maven.</span><span class="sxs-lookup"><span data-stu-id="63693-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![Specificare le proprietà dell'elemento][ART03]

1. <span data-ttu-id="63693-163">Creare un manifesto per l'elemento (facoltativo):</span><span class="sxs-lookup"><span data-stu-id="63693-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="63693-164">a.</span><span class="sxs-lookup"><span data-stu-id="63693-164">a.</span></span> <span data-ttu-id="63693-165">Fare clic su **Create Manifest** (Crea manifesto).</span><span class="sxs-lookup"><span data-stu-id="63693-165">Click **Create Manifest**.</span></span>

      ![Fare clic sul pulsante Create Manifest (Crea manifesto).][ART04a]

   <span data-ttu-id="63693-167">b.</span><span class="sxs-lookup"><span data-stu-id="63693-167">b.</span></span> <span data-ttu-id="63693-168">Scegliere il percorso predefinito per l'elemento e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="63693-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![Specificare il percorso dell'elemento][ART04b]

   <span data-ttu-id="63693-170">c.</span><span class="sxs-lookup"><span data-stu-id="63693-170">c.</span></span> <span data-ttu-id="63693-171">Fare clic sul pulsante con i puntini di sospensione **...** per individuare la classe principale.</span><span class="sxs-lookup"><span data-stu-id="63693-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![Individuare la classe principale][ART04c]

   <span data-ttu-id="63693-173">d.</span><span class="sxs-lookup"><span data-stu-id="63693-173">d.</span></span> <span data-ttu-id="63693-174">Scegliere la classe principale e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="63693-174">Choose your main class, and then click **OK**.</span></span>

      ![Specificare la classe principale][ART04d]

1. <span data-ttu-id="63693-176">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="63693-176">Click **OK**.</span></span>

   ![Chiudere la finestra di dialogo Project Structure (Struttura progetto)][ART05]

> [!NOTE]
> <span data-ttu-id="63693-178">Per altre informazioni sulla creazione di elementi in IntelliJ, vedere [Configuring Artifacts] (Configurazione di elementi) sul sito Web JetBrains.</span><span class="sxs-lookup"><span data-stu-id="63693-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="63693-179">Compilare l'elemento per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="63693-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="63693-180">Fare clic su **Build** (Compila) e quindi su **Artifacts** (Elementi).</span><span class="sxs-lookup"><span data-stu-id="63693-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Comando Build Artifacts (Compila elementi)][BU04]

1. <span data-ttu-id="63693-182">Quando viene visualizzato il menu di scelta rapida **Build Artifact** (Compila elemento), fare clic su **Build** (Compila).</span><span class="sxs-lookup"><span data-stu-id="63693-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Menu di scelta rapida Build Artifact (Compila elemento)][BU05]

<span data-ttu-id="63693-184">IntelliJ visualizzerà l'elemento completato per l'app Spring Boot nella finestra degli strumenti del progetto.</span><span class="sxs-lookup"><span data-stu-id="63693-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![Elemento creato][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="63693-186">Pubblicare l'app Web in Azure usando un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="63693-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="63693-187">Se non è stato eseguito l'accesso all'account Azure, eseguire la procedura descritta in [Istruzioni di accesso per Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="63693-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="63693-188">Nella finestra degli strumenti di Esplora progetti fare clic con il pulsante destro del mouse sul progetto e quindi selezionare **Azure** > **Publish as Docker Container** (Pubblica come contenitore Docker).</span><span class="sxs-lookup"><span data-stu-id="63693-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Comando Publish as Docker Container (Pubblica come contenitore Docker)][PU01]

1. <span data-ttu-id="63693-190">Quando viene visualizzata la finestra di dialogo **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure), vengono mostrati tutti gli host Docker esistenti.</span><span class="sxs-lookup"><span data-stu-id="63693-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="63693-191">Se si sceglie di eseguire la distribuzione in un host esistente, è possibile procedere al passaggio 4.</span><span class="sxs-lookup"><span data-stu-id="63693-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="63693-192">In alternativa, per creare un host seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="63693-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="63693-193">a.</span><span class="sxs-lookup"><span data-stu-id="63693-193">a.</span></span> <span data-ttu-id="63693-194">Fare clic sul simbolo più di colore verde (**+**).</span><span class="sxs-lookup"><span data-stu-id="63693-194">Click the green plus (**+**) symbol.</span></span>

      ![Aggiungere un nuovo host Docker][PU02]

   <span data-ttu-id="63693-196">b.</span><span class="sxs-lookup"><span data-stu-id="63693-196">b.</span></span> <span data-ttu-id="63693-197">Quando viene visualizzata la finestra di dialogo **Create Docker Host** (Crea host Docker), è possibile scegliere di accettare le impostazioni predefinite oppure specificare impostazioni personalizzate per il nuovo host Docker.</span><span class="sxs-lookup"><span data-stu-id="63693-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="63693-198">Per una descrizione dettagliata delle varie impostazioni vedere [Pubblicare un'app Web come contenitore Docker usando il Toolkit di Azure per IntelliJ][Publish Container with Azure Toolkit]. Dopo aver specificato le impostazioni da usare, fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Specificare le opzioni per l'host Docker][PU03a]

   <span data-ttu-id="63693-200">c.</span><span class="sxs-lookup"><span data-stu-id="63693-200">c.</span></span> <span data-ttu-id="63693-201">È possibile scegliere di usare le credenziali di accesso esistenti da un insieme di credenziali delle chiavi di Azure oppure immettere nuove credenziali di accesso per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="63693-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="63693-202">Dopo aver specificato le opzioni, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="63693-202">Click **Finish** when you have specified your options.</span></span>

      ![Specificare le credenziali per l'host Docker][PU03b]

1. <span data-ttu-id="63693-204">Selezionare l'host Docker e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="63693-204">Select your Docker host, and then click **Next**.</span></span>

   ![Selezionare l'host Docker da usare][PU04]

1. <span data-ttu-id="63693-206">Nell'ultima pagina della finestra di dialogo **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure) specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="63693-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="63693-207">a.</span><span class="sxs-lookup"><span data-stu-id="63693-207">a.</span></span> <span data-ttu-id="63693-208">È possibile scegliere di specificare un nome personalizzato per il contenitore che ospiterà il contenitore Docker oppure accettare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="63693-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="63693-209">b.</span><span class="sxs-lookup"><span data-stu-id="63693-209">b.</span></span> <span data-ttu-id="63693-210">Immettere le porte TCP per l'host Docker usando la sintassi seguente: *[porta esterna]*:*[porta interna]*.</span><span class="sxs-lookup"><span data-stu-id="63693-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="63693-211">**80:8080** ad esempio specifica una porta esterna 80 e la porta interna predefinita di Spring Boot 8080.</span><span class="sxs-lookup"><span data-stu-id="63693-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="63693-212">Se la porta interna è stata personalizzata, ad esempio modificando il file application.yml, è necessario specificare il numero di porta per il corretto funzionamento del routing in Azure.</span><span class="sxs-lookup"><span data-stu-id="63693-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="63693-213">c.</span><span class="sxs-lookup"><span data-stu-id="63693-213">c.</span></span> <span data-ttu-id="63693-214">Dopo aver configurato queste opzioni, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="63693-214">After you have configured these options, click **Finish**.</span></span>

   ![Distribuire un contenitore Docker in Azure][PU05]

1. <span data-ttu-id="63693-216">Al termine della pubblicazione da parte del Toolkit di Azure, nel Log attività di Azure viene indicato lo stato **Pubblicato**.</span><span class="sxs-lookup"><span data-stu-id="63693-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Distribuzione dell'host Docker completata][PU06]

## <a name="next-steps"></a><span data-ttu-id="63693-218">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="63693-218">Next steps</span></span>

<span data-ttu-id="63693-219">Per informazioni su altri metodi per la creazione di app Spring Boot tramite IntelliJ, vedere [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) (Creazione di progetti Spring Boot) nel sito Web JetBrains.</span><span class="sxs-lookup"><span data-stu-id="63693-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html (Configurazione di elementi)
[Deploy Spring Boot on Linux in ACS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
