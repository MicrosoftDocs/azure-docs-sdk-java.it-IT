---
title: Istruzioni di accesso per Azure Toolkit for IntelliJ
description: Informazioni su come accedere a Microsoft Azure con Azure Toolkit for IntelliJ.
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
ms.openlocfilehash: 4e24dac285fa38bad4293f1cce2830242c2fe151
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954392"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="ec53f-103">Istruzioni di accesso per Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="ec53f-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="ec53f-104">Azure Toolkit for IntelliJ consente di accedere all'account Azure in due metodi diversi:</span><span class="sxs-lookup"><span data-stu-id="ec53f-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="ec53f-105">**Automated** (Automatico): si crea un file di credenziali che è possibile usare per accedere automaticamente all'account Azure.</span><span class="sxs-lookup"><span data-stu-id="ec53f-105">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>
  * <span data-ttu-id="ec53f-106">**Interactive** (Interattivo): immettere le credenziali di Azure ogni volta che si accede all'account Azure.</span><span class="sxs-lookup"><span data-stu-id="ec53f-106">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>

<span data-ttu-id="ec53f-107">Le sezioni seguenti descrivono come usare ogni metodo.</span><span class="sxs-lookup"><span data-stu-id="ec53f-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="ec53f-108">Accedere automaticamente all'account Azure</span><span class="sxs-lookup"><span data-stu-id="ec53f-108">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="ec53f-109">La sezione seguente illustra come creare un file di credenziali che contiene i dati dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="ec53f-109">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="ec53f-110">Dopo avere completato questo processo, Eclipse usa il file di credenziali per l'accesso ad Azure ogni volta che si apre il progetto.</span><span class="sxs-lookup"><span data-stu-id="ec53f-110">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="ec53f-111">Aprire il progetto con IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ec53f-111">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="ec53f-112">Fare clic su **Tools** (Strumenti), puntare su **Azure** e quindi fare clic su **Azure Sign In** (Accesso ad Azure).</span><span class="sxs-lookup"><span data-stu-id="ec53f-112">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Comando di accesso ad Azure in IntelliJ][A01]

1. <span data-ttu-id="ec53f-114">Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Automated** (Automatico) e quindi fare clic su **New** (Nuovo).</span><span class="sxs-lookup"><span data-stu-id="ec53f-114">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![Finestra di accesso ad Azure con opzione automatica selezionata][A02]

1. <span data-ttu-id="ec53f-116">Nella finestra **Azure Log In** (Accedi ad Azure) immettere le credenziali di Azure e quindi fare clic su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="ec53f-116">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Finestra di dialogo di accesso ad Azure][A03]

1. <span data-ttu-id="ec53f-118">Nella finestra **Create authentication files** (Crea file di autenticazione) selezionare le sottoscrizioni da usare, scegliere la directory di destinazione e quindi fare cli su **Start** (Avvio).</span><span class="sxs-lookup"><span data-stu-id="ec53f-118">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Finestra di creazione dei file di autenticazione][A04]

1. <span data-ttu-id="ec53f-120">Nella finestra di dialogo **Service Principal Creation Status** (Stato creazione entità servizio) fare clic su **OK** al termine della creazione dei file.</span><span class="sxs-lookup"><span data-stu-id="ec53f-120">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![Finestra di dialogo sullo stato di creazione dell'entità servizio][A05]

1. <span data-ttu-id="ec53f-122">Nella finestra **Azure Sign In** (Accesso ad Azure) fare clic su **Sign in** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="ec53f-122">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Finestra di dialogo di accesso ad Azure][A06]

1. <span data-ttu-id="ec53f-124">Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec53f-124">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="ec53f-126">Disconnettersi dall'account di Azure dopo che l'accesso è stato in modo automatico</span><span class="sxs-lookup"><span data-stu-id="ec53f-126">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="ec53f-127">Dopo avere configurato l'account usando i passaggi precedenti, Azure Toolkit esegue automaticamente la connessione all'account Azure a ogni riavvio di IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ec53f-127">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="ec53f-128">Per disconnettersi dall'account Azure e impedire l'accesso automatico eseguito da Azure Toolkit, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="ec53f-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="ec53f-129">In IntelliJ IDEA fare clic su **Tools** (Strumenti), puntare su **Azure** e quindi fare clic su **Azure Sign Out** (Disconnessione da Azure).</span><span class="sxs-lookup"><span data-stu-id="ec53f-129">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Comando di disconnessione da Azure in IntelliJ][L01]

1. <span data-ttu-id="ec53f-131">Nella finestra di conferma **Azure Sign Out** (Disconnessione da Azure) fare clic su **Yes** (Sì).</span><span class="sxs-lookup"><span data-stu-id="ec53f-131">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma della disconnessione da Azure][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="ec53f-133">Accedere al proprio account Azure automaticamente usando un file di credenziali esistente</span><span class="sxs-lookup"><span data-stu-id="ec53f-133">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="ec53f-134">Se si esegue la disconnessione dall'account Azure quando si usa IntelliJ IDEA, è necessario usare un file esistente di credenziali per accedere di nuovo automaticamente all'account.</span><span class="sxs-lookup"><span data-stu-id="ec53f-134">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="ec53f-135">Per configurare Azure Toolkit for Eclipse in modo da usare un file di credenziali esistente, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="ec53f-135">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="ec53f-136">Aprire il progetto con IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ec53f-136">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="ec53f-137">Fare clic su **Tools** (Strumenti), puntare su **Azure** e quindi fare clic su **Azure Sign In** (Accesso ad Azure).</span><span class="sxs-lookup"><span data-stu-id="ec53f-137">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Comando di accesso ad Azure in IntelliJ][A01]

1. <span data-ttu-id="ec53f-139">Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Automated** (Automatico) e quindi fare clic su **Browse** (Sfoglia).</span><span class="sxs-lookup"><span data-stu-id="ec53f-139">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![Finestra di accesso ad Azure con opzione automatica selezionata][A02]

1. <span data-ttu-id="ec53f-141">Nella finestra di dialogo **Select Authenticated File** (Seleziona file autenticazione) selezionare un file di credenziali creato in precedenza e quindi fare clic su **Select** (Seleziona).</span><span class="sxs-lookup"><span data-stu-id="ec53f-141">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![Finestra di dialogo di selezione del file di autenticazione][A08]

1. <span data-ttu-id="ec53f-143">Nella finestra **Azure Sign In** (Accesso ad Azure) fare clic su **Sign in** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="ec53f-143">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Finestra di accesso ad Azure con opzione automatica selezionata][A06]

1. <span data-ttu-id="ec53f-145">Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec53f-145">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][A07]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="ec53f-147">Accedere all'account Azure in modo interattivo</span><span class="sxs-lookup"><span data-stu-id="ec53f-147">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="ec53f-148">Per accedere ad Azure immettendo manualmente le credenziali, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="ec53f-148">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="ec53f-149">Aprire il progetto con IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ec53f-149">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="ec53f-150">Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Azure Sign In** (Accesso ad Azure).</span><span class="sxs-lookup"><span data-stu-id="ec53f-150">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Comando di accesso ad Azure in IntelliJ][I01]

1. <span data-ttu-id="ec53f-152">Quando viene visualizzata la finestra **Azure Sign In** (Accesso ad Azure), selezionare **Interactive** (Interattivo) e quindi fare clic su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="ec53f-152">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![Finestra di accesso in Azure con opzione interattiva selezionata][I02]

1. <span data-ttu-id="ec53f-154">Nella finestra di dialogo **Azure Log In** (Accedi ad Azure) immettere le credenziali di Azure e quindi fare clic su **Sign In** (Accedi).</span><span class="sxs-lookup"><span data-stu-id="ec53f-154">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Finestra di dialogo di accesso ad Azure][I03]

1. <span data-ttu-id="ec53f-156">Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec53f-156">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Finestra di dialogo Seleziona sottoscrizioni][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="ec53f-158">Disconnettersi dall'account di Azure dopo che l'accesso è stato eseguito in modo interattivo</span><span class="sxs-lookup"><span data-stu-id="ec53f-158">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="ec53f-159">Dopo avere configurato l'account usando i passaggi precedenti, la disconnessione dall'account Azure verrà eseguita automaticamente a ogni riavvio di IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ec53f-159">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="ec53f-160">Se tuttavia si vuole disconnettersi dall'account di Azure *senza* riavviare IntelliJ IDEA, eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="ec53f-160">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="ec53f-161">In IntelliJ IDEA fare clic su **Tools** (Strumenti), puntare su **Azure** e quindi fare clic su **Azure Sign Out** (Disconnessione da Azure).</span><span class="sxs-lookup"><span data-stu-id="ec53f-161">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Comando di disconnessione da Azure in IntelliJ][L01]

1. <span data-ttu-id="ec53f-163">Nella finestra di conferma **Azure Sign Out** (Disconnessione da Azure) fare clic su **Yes** (Sì).</span><span class="sxs-lookup"><span data-stu-id="ec53f-163">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma della disconnessione da Azure][L02]

## <a name="next-steps"></a><span data-ttu-id="ec53f-165">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="ec53f-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
