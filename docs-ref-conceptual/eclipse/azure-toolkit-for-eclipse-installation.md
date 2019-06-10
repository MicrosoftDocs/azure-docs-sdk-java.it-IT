---
title: Installare il Toolkit di Azure per Eclipse.
description: Informazioni su come installare il plug-in Azure Toolkit for Eclipse per creare e distribuire applicazioni cloud in Azure.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 8e6630f7e019d950249e7e84024ac800a0f2f136
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270840"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="2488e-103">Installare il Toolkit di Azure per Eclipse.</span><span class="sxs-lookup"><span data-stu-id="2488e-103">Installing the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="2488e-104">È possibile installare Azure Toolkit for Eclipse in due modi:</span><span class="sxs-lookup"><span data-stu-id="2488e-104">There are two ways to install Azure Toolkit for Eclipse:</span></span>

  - [<span data-ttu-id="2488e-105">Eclipse Marketplace</span><span class="sxs-lookup"><span data-stu-id="2488e-105">Eclipse marketplace</span></span>](#eclipse-marketplace)
  - [<span data-ttu-id="2488e-106">Installazione di nuovo software</span><span class="sxs-lookup"><span data-stu-id="2488e-106">Install new software</span></span>](#install-new-software)

> [!NOTE] 
> 
> <span data-ttu-id="2488e-107">Azure Toolkit for Eclipse è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente:</span><span class="sxs-lookup"><span data-stu-id="2488e-107">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [azure-toolkit-for-eclipse-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="eclipse-marketplace"></a><span data-ttu-id="2488e-108">Eclipse Marketplace</span><span class="sxs-lookup"><span data-stu-id="2488e-108">Eclipse marketplace</span></span>

1. <span data-ttu-id="2488e-109">Trascinare il pulsante seguente nell'area di lavoro di Eclipse in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2488e-109">Drag the following button to your running Eclipse workspace.</span></span>

    <span data-ttu-id="2488e-110">[![Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace")</span><span class="sxs-lookup"><span data-stu-id="2488e-110">[![Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client")</span></span>

2. <span data-ttu-id="2488e-111">Altrimenti, è anche possibile cercare e installare il **plug-in Azure Toolkit for Eclipse** tramite la **Guida di Eclipse Marketplace**.</span><span class="sxs-lookup"><span data-stu-id="2488e-111">Otherwise, it is also possible to search and install the **Azure Toolkit for Eclipse plugin** at **Help/Eclipse Marketplace**.</span></span>

    ![Marketplace](./media/azure-toolkit-for-eclipse-installation/marketplace.png)

## <a name="install-new-software"></a><span data-ttu-id="2488e-113">Installazione di nuovo software</span><span class="sxs-lookup"><span data-stu-id="2488e-113">Install new software</span></span>

1. <span data-ttu-id="2488e-114">Avviare Eclipse.</span><span class="sxs-lookup"><span data-stu-id="2488e-114">Start Eclipse.</span></span>

1. <span data-ttu-id="2488e-115">Fare clic sul menu **Help** (Guida), quindi su **Install New Software** (Installa nuovo software), come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2488e-115">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>

   ![Installare il Toolkit di Azure per Eclipse.][01]

1. <span data-ttu-id="2488e-117">Nella finestra di dialogo **Available Software** (Software disponibile), nella casella di testo **Work with** (Usa), digitare `http://dl.microsoft.com/eclipse/` e quindi premere il tasto **Invio**.</span><span class="sxs-lookup"><span data-stu-id="2488e-117">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="2488e-118">Nel riquadro **Nome**, controllare il **Toolkit di Azure per Java** e deselezionare **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto**.</span><span class="sxs-lookup"><span data-stu-id="2488e-118">In the **Name** pane, check **Azure Toolkit for Java**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="2488e-119">Verrà visualizzata una schermata simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="2488e-119">Your screen should appear similar to the following:</span></span>

   ![Installare il Toolkit di Azure per Eclipse.][02]

1. <span data-ttu-id="2488e-121">Espandendo **Azure Toolkit for Eclipse**, viene visualizzato un elenco di componenti che verranno installati, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2488e-121">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="2488e-122">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="2488e-122">Feature</span></span> | <span data-ttu-id="2488e-123">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="2488e-123">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="2488e-124">**Plug-in di Application Insights per Java**</span><span class="sxs-lookup"><span data-stu-id="2488e-124">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="2488e-125">Consente di usare servizi di analisi e di registrazione dei dati di telemetria di Azure per le applicazioni e le istanze del server.</span><span class="sxs-lookup"><span data-stu-id="2488e-125">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="2488e-126">**Plug-in Azure Common**</span><span class="sxs-lookup"><span data-stu-id="2488e-126">**Azure Common Plugin**</span></span> | <span data-ttu-id="2488e-127">Offre le funzionalità comuni necessarie per gli altri componenti del toolkit.</span><span class="sxs-lookup"><span data-stu-id="2488e-127">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="2488e-128">**Azure Container Tools for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="2488e-128">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="2488e-129">Consente di compilare e distribuire un file WAR come contenitore Docker in un computer Docker.</span><span class="sxs-lookup"><span data-stu-id="2488e-129">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="2488e-130">**Azure Containers for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="2488e-130">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="2488e-131">Consente di distribuire un file WAR o un elemento JAR come contenitore Docker in una macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2488e-131">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="2488e-132">**Azure Explorer per Eclipse**</span><span class="sxs-lookup"><span data-stu-id="2488e-132">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="2488e-133">Offre un'interfaccia di tipo Esplora risorse per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="2488e-133">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="2488e-134">**Microsoft JDBC Driver 6.1 per SQL Server**</span><span class="sxs-lookup"><span data-stu-id="2488e-134">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="2488e-135">Fornisce l'API JDBC per SQL Server e il database SQL di Microsoft Azure per Java Platform Enterprise Edition 8.</span><span class="sxs-lookup"><span data-stu-id="2488e-135">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="2488e-136">**Pacchetto per Librerie di Microsoft Azure per Java**</span><span class="sxs-lookup"><span data-stu-id="2488e-136">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="2488e-137">Fornisce le API per accedere ai servizi di Microsoft Azure, ad esempio l'archiviazione, il bus di servizio, il runtime del servizio e così via.</span><span class="sxs-lookup"><span data-stu-id="2488e-137">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="2488e-138">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2488e-138">Click **Next**.</span></span> <span data-ttu-id="2488e-139">(Se si verificano ritardi insoliti durante l'installazione del toolkit, assicurarsi che **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto** sia deselezionato.)</span><span class="sxs-lookup"><span data-stu-id="2488e-139">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="2488e-140">Nella finestra di dialogo **Install Details** fare clic su **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="2488e-140">In the **Install Details** dialog, click **Next**.</span></span>

   ![Verificare i dettagli di installazione][03]

1. <span data-ttu-id="2488e-142">Nella finestra di dialogo **Esaminare licenze** , rivedere le condizioni dei contratti di licenza.</span><span class="sxs-lookup"><span data-stu-id="2488e-142">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="2488e-143">Se si accettano le condizioni dei contratti di licenza, fare clic su **I accept the terms of the license agreements** (Accetto le condizioni dei contratti di licenza) e quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="2488e-143">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="2488e-144">(I passaggi rimanenti suppongono che le condizioni dei contratti di licenza siano state accettate.</span><span class="sxs-lookup"><span data-stu-id="2488e-144">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="2488e-145">Se non si accettano le condizioni dei contratti di licenza, uscire dal processo di installazione.)</span><span class="sxs-lookup"><span data-stu-id="2488e-145">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>

   ![Esaminare licenze][04]

   <span data-ttu-id="2488e-147">Eclipse scarica e installa i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="2488e-147">Eclipse will download and install the requisite packages.</span></span>

   ![Stato dell'installazione][05]

1. <span data-ttu-id="2488e-149">Se viene richiesto di riavviare Eclipse per completare l'installazione, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="2488e-149">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>

   ![Riavviare il prompt dei comandi][06]

## <a name="next-steps"></a><span data-ttu-id="2488e-151">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2488e-151">Next steps</span></span>

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
