---
title: Installare Azure Toolkit for Eclipse
description: Informazioni su come installare il plug-in Azure Toolkit for Eclipse per creare e distribuire applicazioni cloud in Azure.
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 54f636f1291832702bfed2b49888b531d358cb73
ms.sourcegitcommit: 9c354a65b0f8ad49a528f40ddee647b091f7d246
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/04/2018
---
# <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="e17b1-103">Installare Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="e17b1-103">Install the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="e17b1-104">Azure Toolkit for Eclipse offre modelli e funzionalità che permettono di creare, sviluppare, testare e distribuire con facilità applicazioni cloud in Azure dall'ambiente di sviluppo di Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e17b1-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy cloud  applications to Azure from the Eclipse development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e17b1-105">Azure Toolkit for Eclipse è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente:</span><span class="sxs-lookup"><span data-stu-id="e17b1-105">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="e17b1-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="e17b1-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="e17b1-107">La procedura seguente mostra come installare il Toolkit di Azure per Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e17b1-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="e17b1-108">Per installare il Toolkit di Azure per Eclipse</span><span class="sxs-lookup"><span data-stu-id="e17b1-108">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="e17b1-109">Avviare Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e17b1-109">Start Eclipse.</span></span>

1. <span data-ttu-id="e17b1-110">Fare clic sul menu **Help** (Guida), quindi su **Install New Software** (Installa nuovo software), come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e17b1-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![Installare il Toolkit di Azure per Eclipse.][01]

1. <span data-ttu-id="e17b1-112">Nella finestra di dialogo **Available Software** (Software disponibile), nella casella di testo **Work with** (Usa), digitare `http://dl.microsoft.com/eclipse/` e quindi premere il tasto **Invio**.</span><span class="sxs-lookup"><span data-stu-id="e17b1-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="e17b1-113">Nel riquadro **Nome**, controllare il **Toolkit di Azure per Eclipse**, e deselezionare **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto**.</span><span class="sxs-lookup"><span data-stu-id="e17b1-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="e17b1-114">Verrà visualizzata una schermata simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="e17b1-114">Your screen should appear similar to the following:</span></span>
   
   ![Installare il Toolkit di Azure per Eclipse.][02]

1. <span data-ttu-id="e17b1-116">Espandendo **Azure Toolkit for Eclipse**, viene visualizzato un elenco di componenti che verranno installati, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e17b1-116">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="e17b1-117">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="e17b1-117">Feature</span></span> | <span data-ttu-id="e17b1-118">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="e17b1-118">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="e17b1-119">**Plug-in di Application Insights per Java**</span><span class="sxs-lookup"><span data-stu-id="e17b1-119">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="e17b1-120">Consente di usare servizi di analisi e di registrazione dei dati di telemetria di Azure per le applicazioni e le istanze del server.</span><span class="sxs-lookup"><span data-stu-id="e17b1-120">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="e17b1-121">**Plug-in Azure Common**</span><span class="sxs-lookup"><span data-stu-id="e17b1-121">**Azure Common Plugin**</span></span> | <span data-ttu-id="e17b1-122">Offre le funzionalità comuni necessarie per gli altri componenti del toolkit.</span><span class="sxs-lookup"><span data-stu-id="e17b1-122">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="e17b1-123">**Azure Container Tools for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="e17b1-123">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="e17b1-124">Consente di compilare e distribuire un file WAR come contenitore Docker in un computer Docker.</span><span class="sxs-lookup"><span data-stu-id="e17b1-124">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="e17b1-125">**Azure Containers for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="e17b1-125">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="e17b1-126">Consente di distribuire un file WAR o un elemento JAR come contenitore Docker in una macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e17b1-126">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="e17b1-127">**Azure Explorer per Eclipse**</span><span class="sxs-lookup"><span data-stu-id="e17b1-127">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="e17b1-128">Offre un'interfaccia di tipo Esplora risorse per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="e17b1-128">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="e17b1-129">**Microsoft JDBC Driver 6.1 per SQL Server**</span><span class="sxs-lookup"><span data-stu-id="e17b1-129">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="e17b1-130">Fornisce l'API JDBC per SQL Server e il database SQL di Microsoft Azure per Java Platform Enterprise Edition 8.</span><span class="sxs-lookup"><span data-stu-id="e17b1-130">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="e17b1-131">**Pacchetto per Librerie di Microsoft Azure per Java**</span><span class="sxs-lookup"><span data-stu-id="e17b1-131">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="e17b1-132">Fornisce le API per accedere ai servizi di Microsoft Azure, ad esempio l'archiviazione, il bus di servizio, il runtime del servizio e così via.</span><span class="sxs-lookup"><span data-stu-id="e17b1-132">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="e17b1-133">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e17b1-133">Click **Next**.</span></span> <span data-ttu-id="e17b1-134">(Se si verificano ritardi insoliti durante l'installazione del toolkit, assicurarsi che **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto** sia deselezionato.)</span><span class="sxs-lookup"><span data-stu-id="e17b1-134">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="e17b1-135">Nella finestra di dialogo **Install Details** fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="e17b1-135">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![Verificare i dettagli di installazione][03]

1. <span data-ttu-id="e17b1-137">Nella finestra di dialogo **Esaminare licenze** , rivedere le condizioni dei contratti di licenza.</span><span class="sxs-lookup"><span data-stu-id="e17b1-137">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="e17b1-138">Se si accettano le condizioni dei contratti di licenza, fare clic su **I accept the terms of the license agreements** (Accetto le condizioni dei contratti di licenza) e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="e17b1-138">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="e17b1-139">(I passaggi rimanenti suppongono che le condizioni dei contratti di licenza siano state accettate.</span><span class="sxs-lookup"><span data-stu-id="e17b1-139">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="e17b1-140">Se non si accettano le condizioni dei contratti di licenza, uscire dal processo di installazione.)</span><span class="sxs-lookup"><span data-stu-id="e17b1-140">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![Esaminare licenze][04]
   
   <span data-ttu-id="e17b1-142">Eclipse scarica e installa i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="e17b1-142">Eclipse will download and install the requisite packages.</span></span>
   
   ![Stato dell'installazione][05]

1. <span data-ttu-id="e17b1-144">Se viene richiesto di riavviare Eclipse per completare l'installazione, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e17b1-144">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![Riavviare il prompt dei comandi][06]

## <a name="next-steps"></a><span data-ttu-id="e17b1-146">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e17b1-146">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
