---
title: Gestire macchine virtuali con Azure Explorer per Eclipse
description: Informazioni su come gestire macchine virtuali di Azure con Azure Explorer per Eclipse.
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/13/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 7f3b4a6adb4234bbf11f0f7cddafbe99fa0ff3df
ms.sourcegitcommit: 24f037d133875f86761ec893dfa74e723de040b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636697"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="0d42b-103">Gestire macchine virtuali con Azure Explorer per Eclipse</span><span class="sxs-lookup"><span data-stu-id="0d42b-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="0d42b-104">Incluso in Toolkit di Azure per Eclipse, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione delle macchine virtuali con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di Eclipse.</span><span class="sxs-lookup"><span data-stu-id="0d42b-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="0d42b-105">Creare una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="0d42b-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="0d42b-106">Per creare una macchina virtuale con Azure Explorer, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="0d42b-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="0d42b-107">Accedere al proprio account Azure seguendo [Istruzioni di accesso per Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).</span><span class="sxs-lookup"><span data-stu-id="0d42b-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).</span></span>

2. <span data-ttu-id="0d42b-108">Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Macchine virtuali** e quindi fare clic su **Crea macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   ![Comando Crea macchina virtuale][CR01]  

   <span data-ttu-id="0d42b-110">Viene visualizzata la procedura guidata **Creare una nuova macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="0d42b-111">Nella finestra **Scegliere una sottoscrizione** selezionare la sottoscrizione e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![Finestra di dialogo Scegliere una sottoscrizione][CR02]

4. <span data-ttu-id="0d42b-113">Nella finestra **Selezionare un'immagine di macchina virtuale** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0d42b-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="0d42b-114">**Località**: specifica dove verrà creata la macchina virtuale, ad esempio *West US* (Stati Uniti occidentali).</span><span class="sxs-lookup"><span data-stu-id="0d42b-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="0d42b-115">**Publisher** (Editore): specifica l'editore che ha creato l'immagine che verrà usata per creare la macchina virtuale, ad esempio *Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="0d42b-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="0d42b-116">**Offer** (Offerta): specifica l'offerta di macchina virtuale dell'editore selezionato da usare, ad esempio *JDK*.</span><span class="sxs-lookup"><span data-stu-id="0d42b-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="0d42b-117">**Sku**: specifica lo SKU dell'offerta selezionata da usare, ad esempio *JDK_8*.</span><span class="sxs-lookup"><span data-stu-id="0d42b-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="0d42b-118">**Version #** (N. versione): specifica la versione dello SKU selezionato da usare.</span><span class="sxs-lookup"><span data-stu-id="0d42b-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![Finestra Selezionare un'immagine di macchina virtuale][CR03]

5. <span data-ttu-id="0d42b-120">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-120">Click **Next**.</span></span>

6. <span data-ttu-id="0d42b-121">Nella finestra **Impostazioni di base della macchina virtuale** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0d42b-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="0d42b-122">**Virtual Machine Name** (Nome macchina virtuale): specifica il nome della nuova macchina virtuale, che deve iniziare con una lettera e contenere solo lettere, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="0d42b-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="0d42b-123">**Dimensione**: specifica il numero di core e la quantità di memoria da allocare per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="0d42b-124">**User name** (Nome utente): specifica l'account amministratore da creare per la gestione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="0d42b-125">**Password** e **Confirm** (Conferma): specificano la password per l'account amministratore.</span><span class="sxs-lookup"><span data-stu-id="0d42b-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![Finestra Impostazioni di base della macchina virtuale][CR04]

7. <span data-ttu-id="0d42b-127">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-127">Click **Next**.</span></span>

8. <span data-ttu-id="0d42b-128">Nella finestra **Crea un nuovo account di archiviazione** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0d42b-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="0d42b-129">**Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="0d42b-130">Selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0d42b-130">Select one of the following options:</span></span>
     * <span data-ttu-id="0d42b-131">**Crea nuovo**: specifica che si vuole creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="0d42b-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
     * <span data-ttu-id="0d42b-132">**Use existing** (Usa esistente): specifica che si vuole selezionare un gruppo di risorse già associato all'account Azure.</span><span class="sxs-lookup"><span data-stu-id="0d42b-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

       ![Finestra di dialogo Crea un nuovo account di archiviazione][CR05]

   * <span data-ttu-id="0d42b-134">**Account di archiviazione**: specifica l'account di archiviazione da usare per archiviare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="0d42b-135">È possibile usare un account di archiviazione esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="0d42b-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="0d42b-136">**Virtual Network** (Rete virtuale) e **Subnet**: specificano la rete virtuale e la subnet a cui si connetterà la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="0d42b-137">È possibile usare una subnet e una rete esistente oppure creare una rete e una subnet nuove.</span><span class="sxs-lookup"><span data-stu-id="0d42b-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="0d42b-138">Se si seleziona **Crea nuovo**, verrà visualizzata la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="0d42b-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![Finestra di dialogo Crea una nuova rete virtuale][CR06]

9. <span data-ttu-id="0d42b-140">Nella finestra **Risorse associate** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0d42b-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="0d42b-141">**Indirizzo IP pubblico**: specifica un indirizzo IP con accesso all'esterno per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="0d42b-142">È possibile scegliere di creare un nuovo indirizzo IP o, se la macchina virtuale non avrà un indirizzo IP pubblico, è possibile selezionare **(Nessuno)**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="0d42b-143">**Network security group** (Gruppo di sicurezza di rete): specifica un firewall di rete facoltativo per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="0d42b-144">È possibile selezionare un firewall esistente oppure, se la macchina virtuale non usa un firewall di rete, è possibile selezionare **(Nessuno)**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="0d42b-145">**Set di disponibilità**: specifica un set di disponibilità facoltativo a cui può appartenere la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="0d42b-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="0d42b-146">È possibile selezionare un set di disponibilità esistente, creare un nuovo set di disponibilità o, se la macchina virtuale non apparterrà a un set di disponibilità, selezionare **(Nessuno)**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![Finestra Risorse associate][CR07]

10. <span data-ttu-id="0d42b-148">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-148">Click **Finish**.</span></span>  

    <span data-ttu-id="0d42b-149">La nuova macchina virtuale viene visualizzata nella finestra dello strumento Azure Explorer.</span><span class="sxs-lookup"><span data-stu-id="0d42b-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

    ![Nuova macchina virtuale][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="0d42b-151">Riavvio di una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="0d42b-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="0d42b-152">Per riavviare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="0d42b-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="0d42b-153">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Riavvia**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![Comando di riavvio della macchina virtuale][RE01]

1. <span data-ttu-id="0d42b-155">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-155">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma del riavvio][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="0d42b-157">Arrestare una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="0d42b-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="0d42b-158">Per arrestare una macchina virtuale in esecuzione con Azure Explorer in Eclipse, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="0d42b-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="0d42b-159">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![Comando di arresto della macchina virtuale][SH01]

1. <span data-ttu-id="0d42b-161">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-161">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma dell'arresto della macchina virtuale][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="0d42b-163">Eliminare una macchina virtuale in Eclipse</span><span class="sxs-lookup"><span data-stu-id="0d42b-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="0d42b-164">Per eliminare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="0d42b-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="0d42b-165">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![Comando di eliminazione della macchina virtuale][DE01]

1. <span data-ttu-id="0d42b-167">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="0d42b-167">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma dell'eliminazione della macchina virtuale][DE02]

## <a name="next-steps"></a><span data-ttu-id="0d42b-169">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0d42b-169">Next steps</span></span>

<span data-ttu-id="0d42b-170">Per altre informazioni sulle dimensioni e sui prezzi delle macchine virtuali in Azure, vedere i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="0d42b-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="0d42b-171">Dimensioni delle macchine virtuali in Azure</span><span class="sxs-lookup"><span data-stu-id="0d42b-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="0d42b-172">[Dimensioni per le macchine virtuali Windows in Azure]</span><span class="sxs-lookup"><span data-stu-id="0d42b-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="0d42b-173">[Dimensioni delle macchine virtuali Linux in Azure]</span><span class="sxs-lookup"><span data-stu-id="0d42b-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="0d42b-174">Prezzi delle macchine virtuali in Azure</span><span class="sxs-lookup"><span data-stu-id="0d42b-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="0d42b-175">[Prezzi delle macchine virtuali in Windows]</span><span class="sxs-lookup"><span data-stu-id="0d42b-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="0d42b-176">[Prezzi delle macchine virtuali in Linux]</span><span class="sxs-lookup"><span data-stu-id="0d42b-176">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Dimensioni per le macchine virtuali Windows in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Dimensioni delle macchine virtuali Linux in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Prezzi delle macchine virtuali in Windows]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Windows virtual-machine pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Prezzi delle macchine virtuali in Linux]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/
[Linux virtual-machine pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

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
