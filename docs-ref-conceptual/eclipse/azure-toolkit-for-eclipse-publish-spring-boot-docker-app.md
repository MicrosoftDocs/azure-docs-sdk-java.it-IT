---
title: Pubblicare un'app Spring Boot come contenitore Docker usando il Toolkit di Azure per Eclipse
description: Informazioni su come pubblicare un'app Web in Microsoft Azure come contenitore Docker usando il Toolkit di Azure per Eclipse.
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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: a2aa6b0aa0689893143073be94539345e229e5f2
ms.sourcegitcommit: 062e07cbd42cda74f02c82b933ce90da646a50a0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="e200b-103">Pubblicare un'app Spring Boot come contenitore Docker usando il Toolkit di Azure per Eclipse</span><span class="sxs-lookup"><span data-stu-id="e200b-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="e200b-104">[Spring Framework] è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="e200b-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="e200b-105">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="e200b-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="e200b-106">[Docker] è una soluzione open source che consente agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni in esecuzione nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="e200b-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="e200b-107">Questa esercitazione illustra i passaggi per distribuire un'applicazione Spring Boot come contenitore Docker in Microsoft Azure usando il Toolkit di Azure per Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e200b-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="e200b-108">Clonare il repository Docker Spring Boot</span><span class="sxs-lookup"><span data-stu-id="e200b-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="e200b-109">Importare il repository pubblico</span><span class="sxs-lookup"><span data-stu-id="e200b-109">Import the public repository</span></span>

<span data-ttu-id="e200b-110">I passaggi seguenti illustrano la clonazione del repository Docker Spring Boot nel computer locale tramite IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="e200b-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="e200b-111">Se si intende usare una riga di comando, vedere [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS] (Distribuire un'applicazione Spring Boot in Linux nel servizio contenitore di Azure).</span><span class="sxs-lookup"><span data-stu-id="e200b-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="e200b-112">Aprire Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e200b-112">Open Eclipse.</span></span>

1. <span data-ttu-id="e200b-113">Fare clic su **File** > **Import** (Importa).</span><span class="sxs-lookup"><span data-stu-id="e200b-113">Click **File** > **Import**.</span></span>

   ![Opzione Import (Importa) del menu File][CL01]

1. <span data-ttu-id="e200b-115">Quando viene visualizzata la finestra di dialogo **Import** (Importa):</span><span class="sxs-lookup"><span data-stu-id="e200b-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="e200b-116">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-116">a.</span></span> <span data-ttu-id="e200b-117">Espandere **Git**.</span><span class="sxs-lookup"><span data-stu-id="e200b-117">Expand **Git**.</span></span>

   <span data-ttu-id="e200b-118">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-118">b.</span></span> <span data-ttu-id="e200b-119">Selezionare **Projects from Git** (Progetti da Git).</span><span class="sxs-lookup"><span data-stu-id="e200b-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="e200b-120">c.</span><span class="sxs-lookup"><span data-stu-id="e200b-120">c.</span></span> <span data-ttu-id="e200b-121">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e200b-121">Click **Next**.</span></span>

   ![Finestra di dialogo Import (Importa)][CL02]

1. <span data-ttu-id="e200b-123">Nella pagina **Select Repository Source** (Seleziona origine repository):</span><span class="sxs-lookup"><span data-stu-id="e200b-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="e200b-124">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-124">a.</span></span> <span data-ttu-id="e200b-125">Selezionare **Clone URI** (Clona URI).</span><span class="sxs-lookup"><span data-stu-id="e200b-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="e200b-126">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-126">b.</span></span> <span data-ttu-id="e200b-127">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e200b-127">Click **Next**.</span></span>

   ![Selezionare la pagina origine del repository][CL03]

1. <span data-ttu-id="e200b-129">Nella pagina **Select Repository Source** (Seleziona origine repository):</span><span class="sxs-lookup"><span data-stu-id="e200b-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="e200b-130">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-130">a.</span></span> <span data-ttu-id="e200b-131">Per **URI**, immettere `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="e200b-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="e200b-132">Questo passaggio inserisce automaticamente i valori corretti nei campi **Host** e **Repository path** (Percorso repository).</span><span class="sxs-lookup"><span data-stu-id="e200b-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="e200b-133">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-133">b.</span></span> <span data-ttu-id="e200b-134">Il repository Spring Boot è pubblico, quindi non è necessario immettere il nome utente e la password Git.</span><span class="sxs-lookup"><span data-stu-id="e200b-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="e200b-135">c.</span><span class="sxs-lookup"><span data-stu-id="e200b-135">c.</span></span> <span data-ttu-id="e200b-136">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e200b-136">Click **Next**.</span></span>

   ![Pagina del repository Git di origine][CL04]

1. <span data-ttu-id="e200b-138">Nella pagina **Branch selection** (Selezione ramo) fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="e200b-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![Pagina Branch Selection (Selezione ramo)][CL05]

1. <span data-ttu-id="e200b-140">Nella pagina **Local Destination** (Destinazione locale):</span><span class="sxs-lookup"><span data-stu-id="e200b-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="e200b-141">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-141">a.</span></span> <span data-ttu-id="e200b-142">Specificare la cartella locale per il repository locale.</span><span class="sxs-lookup"><span data-stu-id="e200b-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="e200b-143">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-143">b.</span></span> <span data-ttu-id="e200b-144">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e200b-144">Click **Next**.</span></span>

   ![Pagina Local Destination (Destinazione locale)][CL06]

1. <span data-ttu-id="e200b-146">Nella pagina **Select a wizard to use for importing projects (Selezionare una procedura guidata per l'importazione di progetti)**:</span><span class="sxs-lookup"><span data-stu-id="e200b-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="e200b-147">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-147">a.</span></span> <span data-ttu-id="e200b-148">Selezionare **Import as a general project** (Importa come progetto generale).</span><span class="sxs-lookup"><span data-stu-id="e200b-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="e200b-149">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-149">b.</span></span> <span data-ttu-id="e200b-150">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e200b-150">Click **Next**.</span></span>

   ![Pagina Select a wizard to use for importing projects (Selezionare una procedura guidata per l'importazione di progetti)][CL07]

1. <span data-ttu-id="e200b-152">Nella pagina **Import Projects** (Importa progetti):</span><span class="sxs-lookup"><span data-stu-id="e200b-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="e200b-153">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-153">a.</span></span> <span data-ttu-id="e200b-154">Specificare il nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="e200b-154">Specify your project name.</span></span>
   
   <span data-ttu-id="e200b-155">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-155">b.</span></span> <span data-ttu-id="e200b-156">Fare clic su **Finish**.</span><span class="sxs-lookup"><span data-stu-id="e200b-156">Click **Finish**.</span></span>

   ![Pagina Import Projects (Importa progetti)][CL08]

1. <span data-ttu-id="e200b-158">Dopo aver clonato il repository, tutti i file vengono elencati in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e200b-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![Repository locale][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="e200b-160">Creare un progetto Maven dal repository locale</span><span class="sxs-lookup"><span data-stu-id="e200b-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="e200b-161">Il repository Docker Spring Boot contiene un progetto Maven completato che verrà usato per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e200b-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="e200b-162">Fare clic su **File** > **Import** (Importa).</span><span class="sxs-lookup"><span data-stu-id="e200b-162">Click **File** > **Import**.</span></span>

   ![Importare i comandi del menu File][CL01]

1. <span data-ttu-id="e200b-164">Quando viene visualizzata la finestra di dialogo **Import** (Importa):</span><span class="sxs-lookup"><span data-stu-id="e200b-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="e200b-165">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-165">a.</span></span> <span data-ttu-id="e200b-166">Espandere **Maven**.</span><span class="sxs-lookup"><span data-stu-id="e200b-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="e200b-167">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-167">b.</span></span> <span data-ttu-id="e200b-168">Selezionare **Existing Maven Projects** (Progetti Maven esistenti).</span><span class="sxs-lookup"><span data-stu-id="e200b-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="e200b-169">c.</span><span class="sxs-lookup"><span data-stu-id="e200b-169">c.</span></span> <span data-ttu-id="e200b-170">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e200b-170">Click **Next**.</span></span>

   ![Finestra di dialogo Import (Importa)][MV01]

1. <span data-ttu-id="e200b-172">Nella pagina **Maven Projects** (Progetti Maven):</span><span class="sxs-lookup"><span data-stu-id="e200b-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="e200b-173">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-173">a.</span></span> <span data-ttu-id="e200b-174">Per **Root Directory** (Directory radice) specificare la cartella **complete** (completo) nel repository locale.</span><span class="sxs-lookup"><span data-stu-id="e200b-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="e200b-175">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-175">b.</span></span> <span data-ttu-id="e200b-176">Espandere la sezione **Advanced** (Avanzate) e immettere un nome personalizzato per **Name template** (Modello nome).</span><span class="sxs-lookup"><span data-stu-id="e200b-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="e200b-177">c.</span><span class="sxs-lookup"><span data-stu-id="e200b-177">c.</span></span> <span data-ttu-id="e200b-178">Selezionare la casella per il file **pom.xml** nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e200b-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="e200b-179">d.</span><span class="sxs-lookup"><span data-stu-id="e200b-179">d.</span></span> <span data-ttu-id="e200b-180">Fare clic su **Finish**.</span><span class="sxs-lookup"><span data-stu-id="e200b-180">Click **Finish**.</span></span>

   ![Pagina Maven Projects (Progetti Maven)][MV02]

1. <span data-ttu-id="e200b-182">Dopo aver aperto correttamente il progetto Maven, viene visualizzato un secondo progetto in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e200b-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Progetto Maven locale][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="e200b-184">Compilazione dell'app Spring Boot mediante Maven</span><span class="sxs-lookup"><span data-stu-id="e200b-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="e200b-185">In Project Explorer di Eclipse selezionare il progetto Maven.</span><span class="sxs-lookup"><span data-stu-id="e200b-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="e200b-186">Fare clic su **Run** > **Run As** > **Maven build** (Esegui > Esegui come > Compilazione Maven)</span><span class="sxs-lookup"><span data-stu-id="e200b-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Comandi per esecuzione come compilazione Maven][BU01]

1. <span data-ttu-id="e200b-188">Dopo la compilazione corretta dell'applicazione, viene elencato lo stato nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="e200b-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Completamento della compilazione Maven][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="e200b-190">Pubblicare l'app Web in Azure usando un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="e200b-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="e200b-191">In Project Explorer di Eclipse selezionare il progetto Maven.</span><span class="sxs-lookup"><span data-stu-id="e200b-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="e200b-192">Fare clic sul menu **Publish** (Pubblica) di Azure e quindi su **Publish as Docker container** (Pubblica come contenitore Docker).</span><span class="sxs-lookup"><span data-stu-id="e200b-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Comando Publish as Docker Container (Pubblica come contenitore Docker)][PU01]

1. <span data-ttu-id="e200b-194">Quando viene visualizzata la finestra di dialogo **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure):</span><span class="sxs-lookup"><span data-stu-id="e200b-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="e200b-195">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-195">a.</span></span> <span data-ttu-id="e200b-196">Immettere il nome di un'immagine Docker personalizzata.</span><span class="sxs-lookup"><span data-stu-id="e200b-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="e200b-197">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-197">b.</span></span> <span data-ttu-id="e200b-198">Per **Artifact to deploy** (Elemento da distribuire), specificare il percorso del file **gs-spring-boot-docker-0.1.0.jar** appena compilato.</span><span class="sxs-lookup"><span data-stu-id="e200b-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Specificare le opzioni per Docker][PU02]

   <span data-ttu-id="e200b-200">Verranno visualizzati tutti gli host Docker esistenti.</span><span class="sxs-lookup"><span data-stu-id="e200b-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="e200b-201">Se si sceglie di eseguire la distribuzione in un host esistente, è possibile procedere al passaggio 5.</span><span class="sxs-lookup"><span data-stu-id="e200b-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="e200b-202">In alternativa, per creare un host seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="e200b-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="e200b-203">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-203">a.</span></span> <span data-ttu-id="e200b-204">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="e200b-204">Click **Add**.</span></span>

      ![Aggiungere un nuovo host Docker][PU03]

   <span data-ttu-id="e200b-206">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-206">b.</span></span> <span data-ttu-id="e200b-207">Quando viene visualizzata la finestra di dialogo **Create Docker Host** (Crea host Docker), è possibile scegliere di accettare le impostazioni predefinite oppure specificare impostazioni personalizzate per il nuovo host Docker.</span><span class="sxs-lookup"><span data-stu-id="e200b-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="e200b-208">Per una descrizione dettagliata delle varie impostazioni vedere [Pubblicare un'app Web come contenitore Docker usando il Toolkit di Azure per IntelliJ][Publish Container with Azure Toolkit]. Dopo aver specificato le impostazioni da usare, fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="e200b-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Specificare le opzioni per l'host Docker][PU04]

   <span data-ttu-id="e200b-210">c.</span><span class="sxs-lookup"><span data-stu-id="e200b-210">c.</span></span> <span data-ttu-id="e200b-211">È possibile scegliere di usare le credenziali di accesso esistenti da un insieme di credenziali delle chiavi di Azure oppure immettere nuove credenziali di accesso per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="e200b-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="e200b-212">Dopo aver specificato le opzioni, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="e200b-212">Click **Finish** when you have specified your options.</span></span>

      ![Specificare le credenziali per l'host Docker][PU05]

1. <span data-ttu-id="e200b-214">Selezionare l'host Docker e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="e200b-214">Select your Docker host, and then click **Next**.</span></span>

   ![Selezionare l'host Docker da usare][PU06]

1. <span data-ttu-id="e200b-216">Nell'ultima pagina della finestra di dialogo **Deploying Docker Container on Azure** (Distribuzione contenitore Docker in Azure), specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e200b-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="e200b-217">a.</span><span class="sxs-lookup"><span data-stu-id="e200b-217">a.</span></span> <span data-ttu-id="e200b-218">È possibile scegliere di specificare un nome personalizzato per il contenitore che ospiterà il contenitore Docker oppure accettare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="e200b-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="e200b-219">b.</span><span class="sxs-lookup"><span data-stu-id="e200b-219">b.</span></span> <span data-ttu-id="e200b-220">Immettere le porte TCP per l'host Docker usando la sintassi seguente: *[porta esterna]*:*[porta interna]*.</span><span class="sxs-lookup"><span data-stu-id="e200b-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="e200b-221">**80:8080** ad esempio specifica una porta esterna 80 e la porta interna predefinita di Spring Boot 8080.</span><span class="sxs-lookup"><span data-stu-id="e200b-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="e200b-222">Se la porta interna è stata personalizzata, ad esempio modificando il file application.yml, è necessario specificare il numero di porta per il corretto funzionamento del routing in Azure.</span><span class="sxs-lookup"><span data-stu-id="e200b-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="e200b-223">c.</span><span class="sxs-lookup"><span data-stu-id="e200b-223">c.</span></span> <span data-ttu-id="e200b-224">Dopo aver configurato queste opzioni, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="e200b-224">After you configure these options, click **Finish**.</span></span>

   ![Distribuire un contenitore Docker in Azure][PU07]

1. <span data-ttu-id="e200b-226">Al termine della pubblicazione da parte del Toolkit di Azure, nel Log attività di Azure viene indicato lo stato **Pubblicato**.</span><span class="sxs-lookup"><span data-stu-id="e200b-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Distribuzione dell'host Docker completata][PU08]

## <a name="next-steps"></a><span data-ttu-id="e200b-228">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e200b-228">Next steps</span></span>

<span data-ttu-id="e200b-229">Per altre risorse per Docker, vedere il [sito Web Docker] ufficiale.</span><span class="sxs-lookup"><span data-stu-id="e200b-229">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in AKS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: azure-toolkit-for-eclipse-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
