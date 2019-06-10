---
title: Installazione del Toolkit di Azure per IntelliJ
description: Informazioni su come installare il plug-in Azure Toolkit for IntelliJ per creare e distribuire applicazioni cloud in Azure.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 86cef07873ae7a2ba75aab1044fe4d241cd5b13e
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270876"
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="89381-103">Installazione del Toolkit di Azure per IntelliJ</span><span class="sxs-lookup"><span data-stu-id="89381-103">Installing the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="89381-104">Azure Toolkit for IntelliJ offre modelli e funzionalità che consentono di creare, sviluppare, testare e distribuire con facilità applicazioni cloud in Azure tramite l'ambiente di sviluppo IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="89381-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy cloud application to Azure using the IntelliJ IDEA development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="89381-105">Il Toolkit di Azure per IntelliJ è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente:</span><span class="sxs-lookup"><span data-stu-id="89381-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

<span data-ttu-id="89381-106">Per l'installazione di Azure Toolkit for IntelliJ è possibile procedere in due modi: tramite la finestra di dialogo **Settings** (Impostazioni) o tramite il menu **Configure** (Configura) nella schermata Start.</span><span class="sxs-lookup"><span data-stu-id="89381-106">There are two methods of installing the Azure Toolkit for IntelliJ: by using the **Settings** dialog box, and by using the **Configure** menu on the start screen.</span></span> <span data-ttu-id="89381-107">Entrambi i metodi di installazione vengono dimostrati nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="89381-107">Both installation methods will be demonstrated in the following steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89381-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="89381-108">Prerequisites</span></span>

<span data-ttu-id="89381-109">Azure Toolkit for IntelliJ richiede i componenti software seguenti:</span><span class="sxs-lookup"><span data-stu-id="89381-109">The Azure Toolkit for IntelliJ requires to the following software components:</span></span>

* <span data-ttu-id="89381-110">Java Development Kit (JDK) 8 o versione successiva installato, ad esempio: [OpenJDK](https://openjdk.java.net/) o [JDK Oracle](https://www.oracle.com/technetwork/java/javase/downloads/index.html)</span><span class="sxs-lookup"><span data-stu-id="89381-110">An Java Development Kit (JDK) 8+ installed, for example: [OpenJDK](https://openjdk.java.net/) or [Oracle JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)</span></span>
* <span data-ttu-id="89381-111">[IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition o Community Edition installato</span><span class="sxs-lookup"><span data-stu-id="89381-111">An [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition or Community Edition installed</span></span>

> [!NOTE]
> 
> <span data-ttu-id="89381-112">Nella pagina [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) nel repository dei plug-in JetBrains sono elencate le build compatibili con il toolkit.</span><span class="sxs-lookup"><span data-stu-id="89381-112">The [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) page at the JetBrains Plugin Repository lists the builds that are compatible with the toolkit.</span></span>
> 

<!--
> [!IMPORTANT]
> 
> If you are using the Azure Toolkit for IntelliJ on Windows, the toolkit requires installing the Azure SDK 2.9.6 or later in order to use the Azure emulator. You have two options for installing the Azure SDK:
> 
> * You can download and install the Azure SDK by using the [Web Platform Installer (WebPI)](http://go.microsoft.com/fwlink/?LinkID=252838).
> * If you do not have the Azure SDK installed when you create your first Azure deployment project, you will be prompted to automatically download install the requisite version of the Azure SDK.
> 
> Note that the Azure SDK is only required on Windows.
> 
-->


## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="89381-113">Per installare il Toolkit di Azure per IntelliJ dalla finestra di dialogo Impostazioni</span><span class="sxs-lookup"><span data-stu-id="89381-113">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="89381-114">Avviare IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="89381-114">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="89381-115">Quando si apre IntelliJ IDEA, fare clic su **File**, quindi fare clic su **Settings** (Impostazioni).</span><span class="sxs-lookup"><span data-stu-id="89381-115">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![Aprire la finestra di dialogo Impostazioni di IntelliJ IDEA][01a]

1. <span data-ttu-id="89381-117">Nella finestra di dialogo Settings (Impostazioni) fare clic su **Plugins** (Plug-in) e selezionare **Browse repositories** (Sfoglia repository).</span><span class="sxs-lookup"><span data-stu-id="89381-117">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![Finestra di dialogo Impostazioni di IntelliJ IDEA][02a]

1. <span data-ttu-id="89381-119">Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="89381-119">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="89381-120">Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).</span><span class="sxs-lookup"><span data-stu-id="89381-120">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   <span data-ttu-id="89381-122">IntelliJ IDEA mostra lo stato dell'installazione in una finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="89381-122">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![Stato dell'installazione][04]

1. <span data-ttu-id="89381-124">Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).</span><span class="sxs-lookup"><span data-stu-id="89381-124">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![Restart IntelliJ IDEA][05]

1. <span data-ttu-id="89381-126">Fare clic su **OK** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="89381-126">Click **OK** to close the Settings dialog box.</span></span>
   
   ![Chiudere la finestra di dialogo Impostazioni di IntelliJ IDEA][06]

1. <span data-ttu-id="89381-128">Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).</span><span class="sxs-lookup"><span data-stu-id="89381-128">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="89381-129">1</span><span class="sxs-lookup"><span data-stu-id="89381-129">1</span></span>   ![Restart IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="89381-131">Per installare il Toolkit di Azure per IntelliJ dalla schermata iniziale</span><span class="sxs-lookup"><span data-stu-id="89381-131">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="89381-132">Avviare IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="89381-132">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="89381-133">Nella schermata iniziale di IntelliJ IDEA fare clic su **Configure** (Configura) e selezionare **Plugins** (plug-in).</span><span class="sxs-lookup"><span data-stu-id="89381-133">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![Installare i plug-in di IntelliJ IDEA][01b]

1. <span data-ttu-id="89381-135">Nella finestra di dialogo **Plugins** (Plug-in) fare clic su **Browse repositories** (Sfoglia repository).</span><span class="sxs-lookup"><span data-stu-id="89381-135">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![Sfogliare i repository dei plug-in di IntelliJ IDEA][02b]

1. <span data-ttu-id="89381-137">Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="89381-137">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="89381-138">Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).</span><span class="sxs-lookup"><span data-stu-id="89381-138">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   <span data-ttu-id="89381-140">IntelliJ IDEA mostrerà lo stato dell'installazione in una finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="89381-140">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![Stato dell'installazione][04]

1. <span data-ttu-id="89381-142">Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).</span><span class="sxs-lookup"><span data-stu-id="89381-142">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![Restart IntelliJ IDEA][05]

1. <span data-ttu-id="89381-144">Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).</span><span class="sxs-lookup"><span data-stu-id="89381-144">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![Restart IntelliJ IDEA][07]

## <a name="next-steps"></a><span data-ttu-id="89381-146">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="89381-146">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[01a]: media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
