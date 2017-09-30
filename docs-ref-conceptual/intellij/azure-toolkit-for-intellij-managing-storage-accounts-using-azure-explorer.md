---
title: Gestire gli account di archiviazione usando Azure Explorer per IntelliJ
description: Informazioni su come gestire gli account di archiviazione di Azure con Azure Explorer per IntelliJ.
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
ms.openlocfilehash: 44d2ade8a0900b60222f06dfb9f93c1df17228c8
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="47ced-103">Gestire gli account di archiviazione usando Azure Explorer per IntelliJ</span><span class="sxs-lookup"><span data-stu-id="47ced-103">Manage storage accounts by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="47ced-104">Incluso in Azure Toolkit for IntelliJ, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione degli account di archiviazione con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="47ced-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="47ced-105">Creare un account di archiviazione in IntelliJ</span><span class="sxs-lookup"><span data-stu-id="47ced-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="47ced-106">Per creare un account di archiviazione con Azure Explorer, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="47ced-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="47ced-107">Accedere al proprio account Azure seguendo le [Istruzioni di accesso per Azure Toolkit for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="47ced-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for IntelliJ].</span></span> 

2. <span data-ttu-id="47ced-108">Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Account di archiviazione** e quindi fare clic su **Crea account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="47ced-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![Comando Crea account di archiviazione][CS01]

3. <span data-ttu-id="47ced-110">Nella finestra di dialogo **Crea account di archiviazione** specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="47ced-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![Finestra di dialogo Crea un nuovo account di archiviazione][CS02]

   * <span data-ttu-id="47ced-112">**Nome**: specifica il nome per il nuovo account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="47ced-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="47ced-113">**Tipologia account**: specifica il tipo di account di archiviazione da creare, ad esempio "archiviazione BLOB".</span><span class="sxs-lookup"><span data-stu-id="47ced-113">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="47ced-114">Per altre informazioni, vedere [Informazioni sugli account di archiviazione di Azure].</span><span class="sxs-lookup"><span data-stu-id="47ced-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="47ced-115">**Prestazioni**: specifica l'account di archiviazione offerto da usare dall'autore selezionato, ad esempio "Premium".</span><span class="sxs-lookup"><span data-stu-id="47ced-115">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="47ced-116">Per altre informazioni, vedere [Obiettivi di scalabilità e prestazioni per Archiviazione di Azure].</span><span class="sxs-lookup"><span data-stu-id="47ced-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="47ced-117">**Replica**: specifica la replica per l'account di archiviazione, ad esempio "Con ridondanza della zona".</span><span class="sxs-lookup"><span data-stu-id="47ced-117">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="47ced-118">Per altre informazioni, vedere [Replica di Archiviazione di Azure].</span><span class="sxs-lookup"><span data-stu-id="47ced-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="47ced-119">**Sottoscrizione**: specifica la sottoscrizione di Azure da usare per il nuovo account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="47ced-119">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="47ced-120">**Località**: specifica la località in cui verrà creato l'account di archiviazione, ad esempio "Stati Uniti occidentali".</span><span class="sxs-lookup"><span data-stu-id="47ced-120">**Location**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="47ced-121">**Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="47ced-121">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="47ced-122">Selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="47ced-122">Select one of the following options:</span></span>
      * <span data-ttu-id="47ced-123">**Crea nuovo**: specifica che si intende creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="47ced-123">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="47ced-124">**Usa esistente**: specifica che sarà possibile scegliere in un elenco i gruppi di risorse associati all'account di Azure.</span><span class="sxs-lookup"><span data-stu-id="47ced-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="47ced-125">Dopo avere specificato tutte le opzioni precedenti, fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="47ced-125">When you have specified all of the preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="47ced-126">Creare un contenitore di archiviazione in IntelliJ</span><span class="sxs-lookup"><span data-stu-id="47ced-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="47ced-127">Per creare un contenitore di archiviazione con Azure Explorer, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="47ced-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="47ced-128">Nella visualizzazione Azure Explorer fare clic con il pulsante destro del mouse sull'account di archiviazione in cui si intende creare un contenitore e quindi fare clic su **Crea contenitore BLOB**.</span><span class="sxs-lookup"><span data-stu-id="47ced-128">In the Azure Explorer view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![Comando Crea contenitore BLOB][CC01]

2. <span data-ttu-id="47ced-130">Nella finestra di dialogo **Crea contenitore BLOB** specificare il nome per il contenitore e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="47ced-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="47ced-131">Per altre informazioni sulla denominazione dei contenitori di archiviazione, vedere [Denominazione e riferimento a contenitori, BLOB e metadati].</span><span class="sxs-lookup"><span data-stu-id="47ced-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Finestra di dialogo Crea contenitore di archiviazione][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="47ced-133">Eliminare un contenitore di archiviazione in IntelliJ</span><span class="sxs-lookup"><span data-stu-id="47ced-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="47ced-134">Per eliminare un contenitore di archiviazione con Azure Explorer, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="47ced-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="47ced-135">Nella visualizzazione Azure Explorer fare clic con il pulsante destro del mouse sul contenitore di archiviazione e quindi fare clic su **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="47ced-135">In the Azure Explorer view, right-click the storage container, and then click **Delete**.</span></span>

   ![Comando Elimina contenitore di archiviazione][DC01]

2. <span data-ttu-id="47ced-137">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="47ced-137">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma dell'eliminazione del contenitore di archiviazione][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="47ced-139">Eliminare un account di archiviazione in IntelliJ</span><span class="sxs-lookup"><span data-stu-id="47ced-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="47ced-140">Per eliminare un account di archiviazione con Azure Explorer, eseguire queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="47ced-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="47ced-141">Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sull'account di archiviazione e quindi selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="47ced-141">In the **Azure Explorer** view, right-click the storage account, and then select **Delete**.</span></span>

   ![Menu per l'eliminazione dell'account di archiviazione][DS01]

2. <span data-ttu-id="47ced-143">Nella finestra di conferma fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="47ced-143">In the confirmation window, click **Yes**.</span></span>

   ![Finestra di conferma dell'eliminazione dell'account di archiviazione][DS02]

## <a name="next-steps"></a><span data-ttu-id="47ced-145">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="47ced-145">Next steps</span></span>

<span data-ttu-id="47ced-146">Per altre informazioni sugli account di archiviazione, sulle dimensioni e sui prezzi di Azure, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="47ced-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="47ced-147">[Introduzione ad Archiviazione di Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="47ced-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="47ced-148">[Informazioni sugli account di archiviazione di Azure]</span><span class="sxs-lookup"><span data-stu-id="47ced-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="47ced-149">Dimensioni degli account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="47ced-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="47ced-150">[Sizes for Windows storage accounts in Azure] (Dimensioni degli account di archiviazione Windows in Azure)</span><span class="sxs-lookup"><span data-stu-id="47ced-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="47ced-151">[Sizes for Linux storage accounts in Azure] (Dimensioni degli account di archiviazione Linux in Azure)</span><span class="sxs-lookup"><span data-stu-id="47ced-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="47ced-152">Prezzi per gli account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="47ced-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="47ced-153">[Prezzi per gli account di archiviazione Windows]</span><span class="sxs-lookup"><span data-stu-id="47ced-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="47ced-154">[Prezzi per gli account di archiviazione Linux]</span><span class="sxs-lookup"><span data-stu-id="47ced-154">[Linux storage-account pricing]</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Istruzioni di accesso per Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Introduzione ad Archiviazione di Microsoft Azure]: /azure/storage/storage-introduction
[Informazioni sugli account di archiviazione di Azure]: /azure/storage/storage-create-storage-account
[Replica di Archiviazione di Azure]: /azure/storage/storage-redundancy
[Obiettivi di scalabilità e prestazioni per Archiviazione di Azure]: /azure/storage/storage-scalability-targets
[Denominazione e riferimento a contenitori, BLOB e metadati]: http://go.microsoft.com/fwlink/?LinkId=255555

[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes (Dimensioni degli account di archiviazione Windows in Azure)
[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes (Dimensioni degli account di archiviazione Linux in Azure)
[Prezzi per gli account di archiviazione Windows]: /pricing/details/virtual-machines/windows/
[Prezzi per gli account di archiviazione Linux]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
