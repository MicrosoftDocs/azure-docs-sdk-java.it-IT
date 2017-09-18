---
title: Installazione del Toolkit di Azure per IntelliJ
description: Informazioni su come installare il Toolkit di Azure per IntelliJ.
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 03767edf0b879e469dc6d47444fb862e6654d2f1
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2017
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="4f8c3-103">Installazione del Toolkit di Azure per IntelliJ</span><span class="sxs-lookup"><span data-stu-id="4f8c3-103">Installing the Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="4f8c3-104">Il Toolkit di Azure per IntelliJ offre modelli e funzionalità che consentono di creare, sviluppare, testare e distribuire con facilità applicazioni Azure tramite l'ambiente di sviluppo IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the IntelliJ IDEA development environment.</span></span> <span data-ttu-id="4f8c3-105">Il Toolkit di Azure per IntelliJ è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8c3-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span>

<span data-ttu-id="4f8c3-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="4f8c3-106"><https://github.com/microsoft/azure-tools-for-java></span></span>

<span data-ttu-id="4f8c3-107">Esistono due metodi di installazione del Toolkit di Azure per IntelliJ: dalla finestra di dialogo Impostazioni e dal menu Configurazione nella schermata iniziale. Entrambi i metodi di installazione verranno illustrati nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-107">There are two methods of installing the Azure Toolkit for IntelliJ, from the Settings dialog box and from the Configure menu on the start screen; both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="4f8c3-108">Per installare il Toolkit di Azure per IntelliJ dalla finestra di dialogo Impostazioni</span><span class="sxs-lookup"><span data-stu-id="4f8c3-108">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="4f8c3-109">Avviare IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-109">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="4f8c3-110">Quando si apre IntelliJ IDEA, fare clic su **File**, quindi fare clic su **Settings** (Impostazioni).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-110">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![Aprire la finestra di dialogo Impostazioni di IntelliJ IDEA][01a]

1. <span data-ttu-id="4f8c3-112">Nella finestra di dialogo Settings (Impostazioni) fare clic su **Plugins** (Plug-in) e selezionare **Browse repositories** (Sfoglia repository).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-112">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![Finestra di dialogo Impostazioni di IntelliJ IDEA][02a]

1. <span data-ttu-id="4f8c3-114">Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-114">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="4f8c3-115">Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-115">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   <span data-ttu-id="4f8c3-117">IntelliJ IDEA mostra lo stato dell'installazione in una finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-117">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![Stato dell'installazione][04]

1. <span data-ttu-id="4f8c3-119">Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-119">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![Restart IntelliJ IDEA][05]

1. <span data-ttu-id="4f8c3-121">Fare clic su **OK** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-121">Click **OK** to close the Settings dialog box.</span></span>
   
   ![Chiudere la finestra di dialogo Impostazioni di IntelliJ IDEA][06]

1. <span data-ttu-id="4f8c3-123">Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-123">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="4f8c3-124">1</span><span class="sxs-lookup"><span data-stu-id="4f8c3-124">1</span></span>   ![Restart IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="4f8c3-126">Per installare il Toolkit di Azure per IntelliJ dalla schermata iniziale</span><span class="sxs-lookup"><span data-stu-id="4f8c3-126">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="4f8c3-127">Avviare IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-127">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="4f8c3-128">Nella schermata iniziale di IntelliJ IDEA fare clic su **Configure** (Configura) e selezionare **Plugins** (plug-in).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-128">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![Installare i plug-in di IntelliJ IDEA][01b]

1. <span data-ttu-id="4f8c3-130">Nella finestra di dialogo **Plugins** (Plug-in) fare clic su **Browse repositories** (Sfoglia repository).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-130">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![Sfogliare i repository dei plug-in di IntelliJ IDEA][02b]

1. <span data-ttu-id="4f8c3-132">Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-132">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="4f8c3-133">Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-133">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   <span data-ttu-id="4f8c3-135">IntelliJ IDEA mostrerà lo stato dell'installazione in una finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="4f8c3-135">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![Stato dell'installazione][04]

1. <span data-ttu-id="4f8c3-137">Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-137">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![Restart IntelliJ IDEA][05]

1. <span data-ttu-id="4f8c3-139">Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).</span><span class="sxs-lookup"><span data-stu-id="4f8c3-139">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![Restart IntelliJ IDEA][07]

## <a name="next-steps"></a><span data-ttu-id="4f8c3-141">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4f8c3-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

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
