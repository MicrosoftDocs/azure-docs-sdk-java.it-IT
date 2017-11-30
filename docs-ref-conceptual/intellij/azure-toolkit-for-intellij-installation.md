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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: e15a03a7d10d67217565895103e5e58e807a3976
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2017
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="8cf26-103">Installazione del Toolkit di Azure per IntelliJ</span><span class="sxs-lookup"><span data-stu-id="8cf26-103">Installing the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="8cf26-104">Il Toolkit di Azure per IntelliJ offre modelli e funzionalità che consentono di creare, sviluppare, testare e distribuire con facilità applicazioni Azure tramite l'ambiente di sviluppo IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="8cf26-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the IntelliJ IDEA development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="8cf26-105">Il Toolkit di Azure per IntelliJ è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente:</span><span class="sxs-lookup"><span data-stu-id="8cf26-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="8cf26-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="8cf26-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="8cf26-107">Per l'installazione di Azure Toolkit for IntelliJ è possibile procedere in due modi: tramite la finestra di dialogo **Settings** (Impostazioni) o tramite il menu **Configure** (Configura) nella schermata Start.</span><span class="sxs-lookup"><span data-stu-id="8cf26-107">There are two methods of installing the Azure Toolkit for IntelliJ: by using the **Settings** dialog box, and by using the **Configure** menu on the start screen.</span></span> <span data-ttu-id="8cf26-108">Entrambi i metodi di installazione vengono dimostrati nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="8cf26-108">Both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="8cf26-109">Per installare il Toolkit di Azure per IntelliJ dalla finestra di dialogo Impostazioni</span><span class="sxs-lookup"><span data-stu-id="8cf26-109">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="8cf26-110">Avviare IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="8cf26-110">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="8cf26-111">Quando si apre IntelliJ IDEA, fare clic su **File**, quindi fare clic su **Settings** (Impostazioni).</span><span class="sxs-lookup"><span data-stu-id="8cf26-111">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![Aprire la finestra di dialogo Impostazioni di IntelliJ IDEA][01a]

1. <span data-ttu-id="8cf26-113">Nella finestra di dialogo Settings (Impostazioni) fare clic su **Plugins** (Plug-in) e selezionare **Browse repositories** (Sfoglia repository).</span><span class="sxs-lookup"><span data-stu-id="8cf26-113">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![Finestra di dialogo Impostazioni di IntelliJ IDEA][02a]

1. <span data-ttu-id="8cf26-115">Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="8cf26-115">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="8cf26-116">Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).</span><span class="sxs-lookup"><span data-stu-id="8cf26-116">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   <span data-ttu-id="8cf26-118">IntelliJ IDEA mostra lo stato dell'installazione in una finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="8cf26-118">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![Stato dell'installazione][04]

1. <span data-ttu-id="8cf26-120">Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).</span><span class="sxs-lookup"><span data-stu-id="8cf26-120">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![Restart IntelliJ IDEA][05]

1. <span data-ttu-id="8cf26-122">Fare clic su **OK** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="8cf26-122">Click **OK** to close the Settings dialog box.</span></span>
   
   ![Chiudere la finestra di dialogo Impostazioni di IntelliJ IDEA][06]

1. <span data-ttu-id="8cf26-124">Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).</span><span class="sxs-lookup"><span data-stu-id="8cf26-124">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="8cf26-125">1</span><span class="sxs-lookup"><span data-stu-id="8cf26-125">1</span></span>   ![Restart IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="8cf26-127">Per installare il Toolkit di Azure per IntelliJ dalla schermata iniziale</span><span class="sxs-lookup"><span data-stu-id="8cf26-127">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="8cf26-128">Avviare IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="8cf26-128">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="8cf26-129">Nella schermata iniziale di IntelliJ IDEA fare clic su **Configure** (Configura) e selezionare **Plugins** (plug-in).</span><span class="sxs-lookup"><span data-stu-id="8cf26-129">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![Installare i plug-in di IntelliJ IDEA][01b]

1. <span data-ttu-id="8cf26-131">Nella finestra di dialogo **Plugins** (Plug-in) fare clic su **Browse repositories** (Sfoglia repository).</span><span class="sxs-lookup"><span data-stu-id="8cf26-131">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![Sfogliare i repository dei plug-in di IntelliJ IDEA][02b]

1. <span data-ttu-id="8cf26-133">Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="8cf26-133">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="8cf26-134">Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).</span><span class="sxs-lookup"><span data-stu-id="8cf26-134">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   <span data-ttu-id="8cf26-136">IntelliJ IDEA mostrerà lo stato dell'installazione in una finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="8cf26-136">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![Stato dell'installazione][04]

1. <span data-ttu-id="8cf26-138">Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).</span><span class="sxs-lookup"><span data-stu-id="8cf26-138">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![Restart IntelliJ IDEA][05]

1. <span data-ttu-id="8cf26-140">Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).</span><span class="sxs-lookup"><span data-stu-id="8cf26-140">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![Restart IntelliJ IDEA][07]

## <a name="next-steps"></a><span data-ttu-id="8cf26-142">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="8cf26-142">Next steps</span></span>

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
