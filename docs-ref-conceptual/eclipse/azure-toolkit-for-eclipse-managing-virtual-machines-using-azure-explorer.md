---
title: Gestire macchine virtuali con Azure Explorer per Eclipse
description: Informazioni su come gestire macchine virtuali di Azure con Azure Explorer per Eclipse.
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
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: f7526a49e8ba6432d6bf6af54a725a285a61fc07
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="55882-103">Gestire macchine virtuali con Azure Explorer per Eclipse</span><span class="sxs-lookup"><span data-stu-id="55882-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="55882-104">Incluso in Toolkit di Azure per Eclipse, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione delle macchine virtuali con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di Eclipse.</span><span class="sxs-lookup"><span data-stu-id="55882-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="55882-105">Creare una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="55882-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="55882-106">Per creare una macchina virtuale con Azure Explorer, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="55882-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="55882-107">Accedere al proprio account Azure seguendo le [Istruzioni di accesso ad Azure per Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="55882-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span>

1. <span data-ttu-id="55882-108">Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Macchine virtuali** e quindi fare clic su **Crea macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="55882-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   ![Comando Crea macchina virtuale][CR01]  

   <span data-ttu-id="55882-110">Viene visualizzata la procedura guidata **Creare una nuova macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="55882-110">The **Create new Virtual Machine** wizard opens.</span></span>

1. <span data-ttu-id="55882-111">Nella finestra **Scegliere una sottoscrizione** selezionare la sottoscrizione e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="55882-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![Finestra di dialogo Scegliere una sottoscrizione][CR02]

1. <span data-ttu-id="55882-113">Nella finestra **Selezionare un'immagine di macchina virtuale** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="55882-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="55882-114">**Località**: specifica dove verrà creata la macchina virtuale, ad esempio *Stati Uniti occidentali*.</span><span class="sxs-lookup"><span data-stu-id="55882-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="55882-115">**Autore**: specifica l'autore che ha creato l'immagine che verrà usata per creare la macchina virtuale, ad esempio *Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="55882-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="55882-116">**Offerta**: specifica la macchina virtuale offerta da usare dall'autore selezionato, ad esempio *JDK*.</span><span class="sxs-lookup"><span data-stu-id="55882-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="55882-117">**SKU**: specifica la SKU da usare nell'offerta selezionata, ad esempio *JDK_8*.</span><span class="sxs-lookup"><span data-stu-id="55882-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="55882-118">**Versione #**(N. versione): specifica la versione della SKU selezionata.</span><span class="sxs-lookup"><span data-stu-id="55882-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![Finestra Selezionare un'immagine di macchina virtuale][CR03]

1. <span data-ttu-id="55882-120">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="55882-120">Click **Next**.</span></span>

1. <span data-ttu-id="55882-121">Nella finestra **Impostazioni di base della macchina virtuale** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="55882-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="55882-122">**Nome macchina virtuale**: specifica il nome della nuova macchina virtuale, che deve iniziare con una lettera e contenere solo lettere, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="55882-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="55882-123">**Dimensioni**: specifica il numero di core e la quantità di memoria da allocare per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="55882-124">**Nome utente**: specifica l'account amministratore da creare per la gestione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="55882-125">**Password** e **Conferma**: specifica la password per l'account di amministratore.</span><span class="sxs-lookup"><span data-stu-id="55882-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![Finestra Impostazioni di base della macchina virtuale][CR04]

1. <span data-ttu-id="55882-127">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="55882-127">Click **Next**.</span></span>

1. <span data-ttu-id="55882-128">Nella finestra **Crea un nuovo account di archiviazione** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="55882-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="55882-129">**Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="55882-130">Selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="55882-130">Select one of the following options:</span></span>
      * <span data-ttu-id="55882-131">**Crea nuovo**: specifica che si intende creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="55882-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="55882-132">**Usa esistente**: specifica che si vuole selezionare un gruppo di risorse già associato all'account di Azure.</span><span class="sxs-lookup"><span data-stu-id="55882-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

      ![Finestra di dialogo Crea un nuovo account di archiviazione][CR05]

   * <span data-ttu-id="55882-134">**Account di archiviazione**: specifica l'account di archiviazione da usare per archiviare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="55882-135">È possibile usare un account di archiviazione esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="55882-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="55882-136">**Rete virtuale** e **Subnet**: specifica la rete virtuale e la subnet a cui si connetterà la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="55882-137">È possibile usare una subnet e una rete esistente oppure creare una rete e una subnet nuove.</span><span class="sxs-lookup"><span data-stu-id="55882-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="55882-138">Se si seleziona **Crea nuovo**, verrà visualizzata la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="55882-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![Finestra di dialogo Crea una nuova rete virtuale][CR06]

1. <span data-ttu-id="55882-140">Nella finestra **Risorse associate** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="55882-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="55882-141">**Indirizzo IP pubblico**: specifica un indirizzo IP con connessione esterna per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="55882-142">È possibile scegliere di creare un nuovo indirizzo IP o, se la macchina virtuale non avrà un indirizzo IP pubblico, è possibile selezionare **(Nessuno)**.</span><span class="sxs-lookup"><span data-stu-id="55882-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="55882-143">**Gruppo di sicurezza di rete**: specifica un firewall di rete facoltativo per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="55882-144">È possibile selezionare un firewall esistente oppure, se la macchina virtuale non usa un firewall di rete, è possibile selezionare **(Nessuno)**.</span><span class="sxs-lookup"><span data-stu-id="55882-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="55882-145">**Set di disponibilità**: specifica un set di disponibilità facoltativo a cui può appartenere la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55882-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="55882-146">È possibile selezionare un set di disponibilità esistente, creare un nuovo set di disponibilità o, se la macchina virtuale non apparterrà a un set di disponibilità, selezionare **(Nessuno)**.</span><span class="sxs-lookup"><span data-stu-id="55882-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![Finestra Risorse associate][CR07]

1. <span data-ttu-id="55882-148">Fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="55882-148">Click **Finish**.</span></span>  

   <span data-ttu-id="55882-149">La nuova macchina virtuale viene visualizzata nella finestra dello strumento Azure Explorer.</span><span class="sxs-lookup"><span data-stu-id="55882-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

   ![Nuova macchina virtuale][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="55882-151">Riavvio di una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="55882-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="55882-152">Per riavviare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="55882-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="55882-153">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Riavvia**.</span><span class="sxs-lookup"><span data-stu-id="55882-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![Comando di riavvio della macchina virtuale][RE01]

1. <span data-ttu-id="55882-155">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="55882-155">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma del riavvio][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="55882-157">Arrestare una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="55882-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="55882-158">Per arrestare una macchina virtuale in esecuzione con Azure Explorer in Eclipse, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="55882-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="55882-159">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="55882-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![Comando di arresto della macchina virtuale][SH01]

1. <span data-ttu-id="55882-161">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="55882-161">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma dell'arresto della macchina virtuale][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="55882-163">Eliminare una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="55882-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="55882-164">Per eliminare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="55882-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="55882-165">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="55882-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![Comando di eliminazione della macchina virtuale][DE01]

1. <span data-ttu-id="55882-167">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="55882-167">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma dell'eliminazione della macchina virtuale][DE02]

## <a name="next-steps"></a><span data-ttu-id="55882-169">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="55882-169">Next steps</span></span>

<span data-ttu-id="55882-170">Per altre informazioni sulle dimensioni e sui prezzi delle macchine virtuali in Azure, vedere i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="55882-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="55882-171">Dimensioni delle macchine virtuali in Azure</span><span class="sxs-lookup"><span data-stu-id="55882-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="55882-172">[Dimensioni per le macchine virtuali Windows in Azure]</span><span class="sxs-lookup"><span data-stu-id="55882-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="55882-173">[Dimensioni delle macchine virtuali Linux in Azure]</span><span class="sxs-lookup"><span data-stu-id="55882-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="55882-174">Prezzi delle macchine virtuali in Azure</span><span class="sxs-lookup"><span data-stu-id="55882-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="55882-175">[Prezzi delle macchine virtuali in Windows]</span><span class="sxs-lookup"><span data-stu-id="55882-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="55882-176">[Prezzi delle macchine virtuali in Linux]</span><span class="sxs-lookup"><span data-stu-id="55882-176">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Dimensioni per le macchine virtuali Windows in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Dimensioni delle macchine virtuali Linux in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Prezzi delle macchine virtuali in Windows]: /pricing/details/virtual-machines/windows/
[Prezzi delle macchine virtuali in Linux]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png