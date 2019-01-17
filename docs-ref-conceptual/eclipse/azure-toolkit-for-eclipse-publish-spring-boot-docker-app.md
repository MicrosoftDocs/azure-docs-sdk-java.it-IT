---
title: Pubblicare un'app Spring Boot come contenitore Docker usando il Toolkit di Azure per Eclipse
description: Informazioni su come pubblicare un'app Web in Microsoft Azure come contenitore Docker usando il Toolkit di Azure per Eclipse.
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
ms.openlocfilehash: c116e0712afd8e48983f946f43eddfd0c79c0ba8
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48898916"
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="dcded-103">Pubblicare un'app Spring Boot come contenitore Docker usando il Toolkit di Azure per Eclipse</span><span class="sxs-lookup"><span data-stu-id="dcded-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="dcded-104">[Spring Framework] è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="dcded-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="dcded-105">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="dcded-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="dcded-106">[Docker] è una soluzione open source che consente agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni in esecuzione nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="dcded-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="dcded-107">Questa esercitazione illustra i passaggi per distribuire un'applicazione Spring Boot come contenitore Docker in Microsoft Azure usando il Toolkit di Azure per Eclipse.</span><span class="sxs-lookup"><span data-stu-id="dcded-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="dcded-108">Clonare il repository Docker Spring Boot</span><span class="sxs-lookup"><span data-stu-id="dcded-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="dcded-109">Importare il repository pubblico</span><span class="sxs-lookup"><span data-stu-id="dcded-109">Import the public repository</span></span>

<span data-ttu-id="dcded-110">I passaggi seguenti illustrano la clonazione del repository Docker Spring Boot nel computer locale tramite IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="dcded-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="dcded-111">Se si intende usare una riga di comando, vedere [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS] (Distribuire un'applicazione Spring Boot in Linux nel servizio Azure Container).</span><span class="sxs-lookup"><span data-stu-id="dcded-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="dcded-112">Aprire Eclipse.</span><span class="sxs-lookup"><span data-stu-id="dcded-112">Open Eclipse.</span></span>

1. <span data-ttu-id="dcded-113">Fare clic su **File** > **Import** (Importa).</span><span class="sxs-lookup"><span data-stu-id="dcded-113">Click **File** > **Import**.</span></span>

   ![Opzione Import (Importa) del menu File][CL01]

1. <span data-ttu-id="dcded-115">Quando viene visualizzata la finestra di dialogo **Import** (Importa):</span><span class="sxs-lookup"><span data-stu-id="dcded-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="dcded-116">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-116">a.</span></span> <span data-ttu-id="dcded-117">Espandere **Git**.</span><span class="sxs-lookup"><span data-stu-id="dcded-117">Expand **Git**.</span></span>

   <span data-ttu-id="dcded-118">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-118">b.</span></span> <span data-ttu-id="dcded-119">Selezionare **Projects from Git** (Progetti da Git).</span><span class="sxs-lookup"><span data-stu-id="dcded-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="dcded-120">c.</span><span class="sxs-lookup"><span data-stu-id="dcded-120">c.</span></span> <span data-ttu-id="dcded-121">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="dcded-121">Click **Next**.</span></span>

   ![Finestra di dialogo Import (Importa)][CL02]

1. <span data-ttu-id="dcded-123">Nella pagina **Select Repository Source** (Seleziona origine repository):</span><span class="sxs-lookup"><span data-stu-id="dcded-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="dcded-124">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-124">a.</span></span> <span data-ttu-id="dcded-125">Selezionare **Clone URI** (Clona URI).</span><span class="sxs-lookup"><span data-stu-id="dcded-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="dcded-126">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-126">b.</span></span> <span data-ttu-id="dcded-127">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="dcded-127">Click **Next**.</span></span>

   ![Selezionare la pagina origine del repository][CL03]

1. <span data-ttu-id="dcded-129">Nella pagina **Select Repository Source** (Seleziona origine repository):</span><span class="sxs-lookup"><span data-stu-id="dcded-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="dcded-130">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-130">a.</span></span> <span data-ttu-id="dcded-131">Per **URI**, immettere `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="dcded-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="dcded-132">Questo passaggio inserisce automaticamente i valori corretti nei campi **Host** e **Repository path** (Percorso repository).</span><span class="sxs-lookup"><span data-stu-id="dcded-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="dcded-133">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-133">b.</span></span> <span data-ttu-id="dcded-134">Il repository Spring Boot è pubblico, quindi non è necessario immettere il nome utente e la password Git.</span><span class="sxs-lookup"><span data-stu-id="dcded-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="dcded-135">c.</span><span class="sxs-lookup"><span data-stu-id="dcded-135">c.</span></span> <span data-ttu-id="dcded-136">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="dcded-136">Click **Next**.</span></span>

   ![Pagina del repository Git di origine][CL04]

1. <span data-ttu-id="dcded-138">Nella pagina **Branch selection** (Selezione ramo) fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="dcded-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![Pagina Branch Selection (Selezione ramo)][CL05]

1. <span data-ttu-id="dcded-140">Nella pagina **Local Destination** (Destinazione locale):</span><span class="sxs-lookup"><span data-stu-id="dcded-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="dcded-141">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-141">a.</span></span> <span data-ttu-id="dcded-142">Specificare la cartella locale per il repository locale.</span><span class="sxs-lookup"><span data-stu-id="dcded-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="dcded-143">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-143">b.</span></span> <span data-ttu-id="dcded-144">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="dcded-144">Click **Next**.</span></span>

   ![Pagina Local Destination (Destinazione locale)][CL06]

1. <span data-ttu-id="dcded-146">Nella pagina **Select a wizard to use for importing projects (Selezionare una procedura guidata per l'importazione di progetti)**:</span><span class="sxs-lookup"><span data-stu-id="dcded-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="dcded-147">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-147">a.</span></span> <span data-ttu-id="dcded-148">Selezionare **Import as a general project** (Importa come progetto generale).</span><span class="sxs-lookup"><span data-stu-id="dcded-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="dcded-149">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-149">b.</span></span> <span data-ttu-id="dcded-150">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="dcded-150">Click **Next**.</span></span>

   ![Pagina Select a wizard to use for importing projects (Selezionare una procedura guidata per l'importazione di progetti)][CL07]

1. <span data-ttu-id="dcded-152">Nella pagina **Import Projects** (Importa progetti):</span><span class="sxs-lookup"><span data-stu-id="dcded-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="dcded-153">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-153">a.</span></span> <span data-ttu-id="dcded-154">Specificare il nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="dcded-154">Specify your project name.</span></span>
   
   <span data-ttu-id="dcded-155">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-155">b.</span></span> <span data-ttu-id="dcded-156">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="dcded-156">Click **Finish**.</span></span>

   ![Pagina Import Projects (Importa progetti)][CL08]

1. <span data-ttu-id="dcded-158">Dopo aver clonato il repository, tutti i file vengono elencati in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="dcded-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![Repository locale][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="dcded-160">Creare un progetto Maven dal repository locale</span><span class="sxs-lookup"><span data-stu-id="dcded-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="dcded-161">Il repository Docker Spring Boot contiene un progetto Maven completato che verrà usato per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="dcded-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="dcded-162">Fare clic su **File** > **Import** (Importa).</span><span class="sxs-lookup"><span data-stu-id="dcded-162">Click **File** > **Import**.</span></span>

   ![Importare i comandi del menu File][CL01]

1. <span data-ttu-id="dcded-164">Quando viene visualizzata la finestra di dialogo **Import** (Importa):</span><span class="sxs-lookup"><span data-stu-id="dcded-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="dcded-165">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-165">a.</span></span> <span data-ttu-id="dcded-166">Espandere **Maven**.</span><span class="sxs-lookup"><span data-stu-id="dcded-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="dcded-167">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-167">b.</span></span> <span data-ttu-id="dcded-168">Selezionare **Existing Maven Projects** (Progetti Maven esistenti).</span><span class="sxs-lookup"><span data-stu-id="dcded-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="dcded-169">c.</span><span class="sxs-lookup"><span data-stu-id="dcded-169">c.</span></span> <span data-ttu-id="dcded-170">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="dcded-170">Click **Next**.</span></span>

   ![Finestra di dialogo Import (Importa)][MV01]

1. <span data-ttu-id="dcded-172">Nella pagina **Maven Projects** (Progetti Maven):</span><span class="sxs-lookup"><span data-stu-id="dcded-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="dcded-173">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-173">a.</span></span> <span data-ttu-id="dcded-174">Per **Root Directory** (Directory radice) specificare la cartella **complete** (completo) nel repository locale.</span><span class="sxs-lookup"><span data-stu-id="dcded-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="dcded-175">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-175">b.</span></span> <span data-ttu-id="dcded-176">Espandere la sezione **Advanced** (Avanzate) e immettere un nome personalizzato per **Name template** (Modello nome).</span><span class="sxs-lookup"><span data-stu-id="dcded-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="dcded-177">c.</span><span class="sxs-lookup"><span data-stu-id="dcded-177">c.</span></span> <span data-ttu-id="dcded-178">Selezionare la casella per il file **pom.xml** nel progetto.</span><span class="sxs-lookup"><span data-stu-id="dcded-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="dcded-179">d.</span><span class="sxs-lookup"><span data-stu-id="dcded-179">d.</span></span> <span data-ttu-id="dcded-180">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="dcded-180">Click **Finish**.</span></span>

   ![Pagina Maven Projects (Progetti Maven)][MV02]

1. <span data-ttu-id="dcded-182">Dopo aver aperto correttamente il progetto Maven, viene visualizzato un secondo progetto in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="dcded-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Progetto Maven locale][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="dcded-184">Compilazione dell'app Spring Boot mediante Maven</span><span class="sxs-lookup"><span data-stu-id="dcded-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="dcded-185">In Project Explorer di Eclipse selezionare il progetto Maven.</span><span class="sxs-lookup"><span data-stu-id="dcded-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="dcded-186">Fare clic su **Run** > **Run As** > **Maven build** (Esegui > Esegui come > Compilazione Maven)</span><span class="sxs-lookup"><span data-stu-id="dcded-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Comandi per esecuzione come compilazione Maven][BU01]

1. <span data-ttu-id="dcded-188">Dopo la compilazione corretta dell'applicazione, viene elencato lo stato nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="dcded-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Completamento della compilazione Maven][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="dcded-190">Pubblicare l'app Web in Azure usando un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="dcded-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="dcded-191">In Project Explorer di Eclipse selezionare il progetto Maven.</span><span class="sxs-lookup"><span data-stu-id="dcded-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="dcded-192">Fare clic sul menu **Publish** (Pubblica) di Azure e quindi su **Publish as Docker container** (Pubblica come contenitore Docker).</span><span class="sxs-lookup"><span data-stu-id="dcded-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Comando Publish as Docker Container (Pubblica come contenitore Docker)][PU01]

1. <span data-ttu-id="dcded-194">Quando viene visualizzata la finestra di dialogo **Deploy Docker Container on Azure** (Distribuisci contenitore Docker in Azure):</span><span class="sxs-lookup"><span data-stu-id="dcded-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="dcded-195">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-195">a.</span></span> <span data-ttu-id="dcded-196">Immettere il nome di un'immagine Docker personalizzata.</span><span class="sxs-lookup"><span data-stu-id="dcded-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="dcded-197">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-197">b.</span></span> <span data-ttu-id="dcded-198">Per **Artifact to deploy** (Elemento da distribuire), specificare il percorso del file **gs-spring-boot-docker-0.1.0.jar** appena compilato.</span><span class="sxs-lookup"><span data-stu-id="dcded-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Specificare le opzioni per Docker][PU02]

   <span data-ttu-id="dcded-200">Verranno visualizzati tutti gli host Docker esistenti.</span><span class="sxs-lookup"><span data-stu-id="dcded-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="dcded-201">Se si sceglie di eseguire la distribuzione in un host esistente, è possibile procedere al passaggio 5.</span><span class="sxs-lookup"><span data-stu-id="dcded-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="dcded-202">In alternativa, per creare un host seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="dcded-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="dcded-203">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-203">a.</span></span> <span data-ttu-id="dcded-204">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="dcded-204">Click **Add**.</span></span>

      ![Aggiungere un nuovo host Docker][PU03]

   <span data-ttu-id="dcded-206">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-206">b.</span></span> <span data-ttu-id="dcded-207">Quando viene visualizzata la finestra di dialogo **Create Docker Host** (Crea host Docker), è possibile scegliere di accettare le impostazioni predefinite oppure specificare impostazioni personalizzate per il nuovo host Docker.</span><span class="sxs-lookup"><span data-stu-id="dcded-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="dcded-208">Per una descrizione dettagliata delle varie impostazioni vedere [Pubblicare un'app Web come contenitore Docker usando il Toolkit di Azure per IntelliJ][Publish Container with Azure Toolkit]. Dopo aver specificato le impostazioni da usare, fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="dcded-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Specificare le opzioni per l'host Docker][PU04]

   <span data-ttu-id="dcded-210">c.</span><span class="sxs-lookup"><span data-stu-id="dcded-210">c.</span></span> <span data-ttu-id="dcded-211">È possibile scegliere di usare le credenziali di accesso esistenti da un insieme di credenziali delle chiavi di Azure oppure immettere nuove credenziali di accesso per l'host Docker.</span><span class="sxs-lookup"><span data-stu-id="dcded-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="dcded-212">Dopo aver specificato le opzioni, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="dcded-212">Click **Finish** when you have specified your options.</span></span>

      ![Specificare le credenziali per l'host Docker][PU05]

1. <span data-ttu-id="dcded-214">Selezionare l'host Docker e quindi fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="dcded-214">Select your Docker host, and then click **Next**.</span></span>

   ![Selezionare l'host Docker da usare][PU06]

1. <span data-ttu-id="dcded-216">Nell'ultima pagina della finestra di dialogo **Deploying Docker Container on Azure** (Distribuzione contenitore Docker in Azure), specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dcded-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="dcded-217">a.</span><span class="sxs-lookup"><span data-stu-id="dcded-217">a.</span></span> <span data-ttu-id="dcded-218">È possibile scegliere di specificare un nome personalizzato per il contenitore che ospiterà il contenitore Docker oppure accettare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="dcded-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="dcded-219">b.</span><span class="sxs-lookup"><span data-stu-id="dcded-219">b.</span></span> <span data-ttu-id="dcded-220">Immettere le porte TCP per l'host Docker usando la sintassi seguente: *[porta esterna]*:*[porta interna]*.</span><span class="sxs-lookup"><span data-stu-id="dcded-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="dcded-221">**80:8080** ad esempio specifica una porta esterna 80 e la porta interna predefinita di Spring Boot 8080.</span><span class="sxs-lookup"><span data-stu-id="dcded-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="dcded-222">Se la porta interna è stata personalizzata, ad esempio modificando il file application.yml, è necessario specificare il numero di porta per il corretto funzionamento del routing in Azure.</span><span class="sxs-lookup"><span data-stu-id="dcded-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="dcded-223">c.</span><span class="sxs-lookup"><span data-stu-id="dcded-223">c.</span></span> <span data-ttu-id="dcded-224">Dopo aver configurato queste opzioni, fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="dcded-224">After you configure these options, click **Finish**.</span></span>

   ![Distribuire un contenitore Docker in Azure][PU07]

1. <span data-ttu-id="dcded-226">Al termine della pubblicazione da parte del Toolkit di Azure, nel Log attività di Azure viene indicato lo stato **Pubblicato**.</span><span class="sxs-lookup"><span data-stu-id="dcded-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Distribuzione dell'host Docker completata][PU08]

## <a name="next-steps"></a><span data-ttu-id="dcded-228">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dcded-228">Next steps</span></span>

<span data-ttu-id="dcded-229">Per altre risorse per Docker, vedere il [sito Web Docker](https://www.docker.com/) ufficiale.</span><span class="sxs-lookup"><span data-stu-id="dcded-229">For additional resources for Docker, see the official [Docker website](https://www.docker.com/).</span></span>

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
