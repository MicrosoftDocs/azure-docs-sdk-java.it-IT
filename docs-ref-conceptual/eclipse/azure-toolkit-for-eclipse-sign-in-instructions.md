---
title: Istruzioni di accesso per Azure Toolkit for Eclipse
description: Informazioni su come accedere a Microsoft Azure con il Toolkit di Azure per Eclipse.
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
ms.openlocfilehash: b4b13de38913ae6e7ae2bb09210ac742efc9d0ad
ms.sourcegitcommit: d18d9dce22b7f7af178f756bd341433d24e3c3b5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66575314"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="43a5d-103">Istruzioni di accesso per Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="43a5d-103">Sign-in instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="43a5d-104">Il Toolkit di Azure per Eclipse consente di accedere all'account Azure in due metodi diversi:</span><span class="sxs-lookup"><span data-stu-id="43a5d-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  - [<span data-ttu-id="43a5d-105">Accedere all'account Azure tramite accesso dispositivo</span><span class="sxs-lookup"><span data-stu-id="43a5d-105">Sign in to your Azure account by Device Login</span></span>](#sign-in-to-your-azure-account-by-device-login)
  - [<span data-ttu-id="43a5d-106">Accedere all'account Azure tramite entità servizio</span><span class="sxs-lookup"><span data-stu-id="43a5d-106">Sign in to your Azure account by Service Principal</span></span>](#sign-in-to-your-azure-account-by-service-principal)

<span data-ttu-id="43a5d-107">Sono anche disponibili metodi di [**disconnessione**](#sign-out-of-your-azure-account).</span><span class="sxs-lookup"><span data-stu-id="43a5d-107">[**Sign out**](#sign-out-of-your-azure-account) methods are also provided.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a><span data-ttu-id="43a5d-108">Accedere all'account Azure tramite accesso dispositivo</span><span class="sxs-lookup"><span data-stu-id="43a5d-108">Sign in to your Azure account by Device Login</span></span>

<span data-ttu-id="43a5d-109">Per accedere ad Azure tramite accesso dispositivo, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="43a5d-109">To sign in Azure by device login, do the following:</span></span>

1. <span data-ttu-id="43a5d-110">Aprire il progetto con Eclipse.</span><span class="sxs-lookup"><span data-stu-id="43a5d-110">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="43a5d-111">Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="43a5d-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="43a5d-112">![Menu di Eclipse per l'accesso ad Azure][I01]</span><span class="sxs-lookup"><span data-stu-id="43a5d-112">![Eclipse Menu for Azure Sign In][I01]</span></span>

3. <span data-ttu-id="43a5d-113">Nella finestra **Azure Sign In** (Accesso ad Azure), selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="43a5d-113">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in**.</span></span>

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

4. <span data-ttu-id="43a5d-115">Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).</span><span class="sxs-lookup"><span data-stu-id="43a5d-115">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![Finestra di dialogo di accesso ad Azure][I03]

> [!NOTE]
>
> <span data-ttu-id="43a5d-117">Se il browser non si apre, configurare Eclipse per l'uso di un browser esterno, come Internet Explorer, Firefox o Chrome:</span><span class="sxs-lookup"><span data-stu-id="43a5d-117">If the browser doesn't open, configure Eclipse to use an external browser like Internet Explorer, Firefox, or Chrome:</span></span>
>
> 1. <span data-ttu-id="43a5d-118">Aprire Preferences -> General -> Web Browser -> Use external web browser in Eclipse (Preferenze -> Generale -> Web Browser -> Usa Web browser esterno in Eclipse)</span><span class="sxs-lookup"><span data-stu-id="43a5d-118">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="43a5d-119">Selezionare il browser che si preferisce usare</span><span class="sxs-lookup"><span data-stu-id="43a5d-119">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="43a5d-120">Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="43a5d-120">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![Accesso al dispositivo nel browser][I04]

6. <span data-ttu-id="43a5d-122">Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="43a5d-122">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a><span data-ttu-id="43a5d-124">Accedere all'account Azure tramite entità servizio</span><span class="sxs-lookup"><span data-stu-id="43a5d-124">Sign in to your Azure account by Service Principal</span></span>

<span data-ttu-id="43a5d-125">La sezione seguente illustra come creare un file di credenziali che contiene i dati dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="43a5d-125">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="43a5d-126">Dopo avere completato questo processo, Eclipse usa il file di credenziali per l'accesso automatico ad Azure quando si apre il progetto.</span><span class="sxs-lookup"><span data-stu-id="43a5d-126">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure when open your project.</span></span>

1. <span data-ttu-id="43a5d-127">Aprire il progetto con Eclipse.</span><span class="sxs-lookup"><span data-stu-id="43a5d-127">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="43a5d-128">Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="43a5d-128">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="43a5d-129">![Comando di accesso ad Azure in Eclipse][A01]</span><span class="sxs-lookup"><span data-stu-id="43a5d-129">![The Eclipse Azure Sign In command][A01]</span></span>

3. <span data-ttu-id="43a5d-130">Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Service Principal** (Entità servizio).</span><span class="sxs-lookup"><span data-stu-id="43a5d-130">In the **Azure Sign In** window, select **Service Principal**.</span></span> <span data-ttu-id="43a5d-131">Se il file di autenticazione dell'entità servizio non è già disponibile, fare clic su **New** (Nuovo) per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="43a5d-131">If you do not have the service principal authentication file yet, click **New** to create one.</span></span> <span data-ttu-id="43a5d-132">Altrimenti è possibile fare clic su **Browse** (Sfoglia) per aprirlo e procedere con il passaggio 8.</span><span class="sxs-lookup"><span data-stu-id="43a5d-132">Otherwise you can click **Browse** to open it and jump to step 8.</span></span>

   ![Finestra di accesso ad Azure con l'entità servizio selezionata][A02]

4. <span data-ttu-id="43a5d-134">Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).</span><span class="sxs-lookup"><span data-stu-id="43a5d-134">Click **Copy&Open** in **Azure Device Login** dialog.</span></span>

   ![Finestra di dialogo di accesso ad Azure][A08]

> [!NOTE]
>
> <span data-ttu-id="43a5d-136">Se il browser non si apre, configurare Eclipse per l'uso di un browser esterno, come IE o Chrome:</span><span class="sxs-lookup"><span data-stu-id="43a5d-136">If the browser doesn't open, configure eclipse to use an external browser like IE or Chrome:</span></span>
>
> 1. <span data-ttu-id="43a5d-137">Aprire Preferences -> General -> Web Browser -> Use external web browser in Eclipse (Preferenze -> Generale -> Web Browser -> Usa Web browser esterno in Eclipse)</span><span class="sxs-lookup"><span data-stu-id="43a5d-137">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="43a5d-138">Selezionare il browser che si preferisce usare</span><span class="sxs-lookup"><span data-stu-id="43a5d-138">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="43a5d-139">Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="43a5d-139">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![Accesso al dispositivo nel browser][A03]

6. <span data-ttu-id="43a5d-141">Nella finestra **Create authentication files** (Crea file di autenticazione) selezionare le sottoscrizioni da usare, scegliere la directory di destinazione e quindi fare cli su **Start** (Avvio).</span><span class="sxs-lookup"><span data-stu-id="43a5d-141">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Finestra di creazione dei file di autenticazione][A04]

7. <span data-ttu-id="43a5d-143">Nella finestra di dialogo **Service Principal Creation Status** (Stato creazione entità servizio) fare clic su **OK** al termine della creazione dei file.</span><span class="sxs-lookup"><span data-stu-id="43a5d-143">In the **Service Principal Creation Status** dialog box, click **OK** after your files have been created successfully.</span></span>

   ![Finestra di dialogo sullo stato di creazione dell'entità servizio][A05]

8. <span data-ttu-id="43a5d-145">L'indirizzo del file creato verrà inserito automaticamente nella finestra **Azure Sign In** (Accesso ad Azure). Fare clic su **Sign in** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="43a5d-145">Address of the created file will be automatically filled in the **Azure Sign In** window, now click **Sign in**.</span></span>

   ![Finestra di dialogo di accesso ad Azure][A06]

9. <span data-ttu-id="43a5d-147">Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="43a5d-147">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][A07]

## <a name="sign-out-of-your-azure-account"></a><span data-ttu-id="43a5d-149">Disconnettersi dall'account Azure</span><span class="sxs-lookup"><span data-stu-id="43a5d-149">Sign out of your Azure account</span></span>

<span data-ttu-id="43a5d-150">Dopo avere configurato l'account usando i passaggi precedenti, l'accesso verrà eseguito automaticamente ogni volta che si avvia Eclipse.</span><span class="sxs-lookup"><span data-stu-id="43a5d-150">After you have configured your account by preceding steps, you will be automatically signed in each time you start Eclipse.</span></span> <span data-ttu-id="43a5d-151">Se tuttavia ci si vuole disconnettere dall'account Azure, eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a5d-151">However, if you want to sign out of your Azure account, use the following steps.</span></span>

1. <span data-ttu-id="43a5d-152">In Eclipse fare clic su **Tools** (Strumenti), su **Azure** e quindi su **Sign Out** (Disconnetti).</span><span class="sxs-lookup"><span data-stu-id="43a5d-152">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Menu di Eclipse per la disconnessione da Azure][L01]

2. <span data-ttu-id="43a5d-154">Quando viene visualizzata la finestra di dialogo **Azure Sign Out** (Disconnessione da Azure), fare clic su **Yes** (Sì).</span><span class="sxs-lookup"><span data-stu-id="43a5d-154">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Finestra di dialogo di disconnessione][L02]

## <a name="next-steps"></a><span data-ttu-id="43a5d-156">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="43a5d-156">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

[A01]: media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
