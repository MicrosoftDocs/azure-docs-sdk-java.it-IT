---
title: Gestione delle Cache Redis con Azure Explorer per IntelliJ
description: Informazioni su come gestire le Cache Redis di Azure con Azure Explorer per IntelliJ.
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
ms.openlocfilehash: 046ae0428d50a7f173f5ad15be53ffd8e66c11c5
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892522"
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="4f9c2-103">Gestione delle Cache Redis con Azure Explorer per IntelliJ</span><span class="sxs-lookup"><span data-stu-id="4f9c2-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="4f9c2-104">Azure Explorer, che fa parte del Toolkit di Azure per IntelliJ, offre agli sviluppatori Java una soluzione di facile uso per la gestione delle Cache Redis con il proprio account Azure nell'ambiente IDE di IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="4f9c2-105">Creare una Cache Redis tramite IntelliJ</span><span class="sxs-lookup"><span data-stu-id="4f9c2-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="4f9c2-106">I passaggi seguenti illustrano come creare una Cache Redis con Azure Explorer.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="4f9c2-107">Accedere al proprio account Azure tramite la procedura descritta nell'articolo [Istruzioni di accesso ad Azure per il Toolkit di Azure per IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="4f9c2-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="4f9c2-108">Nella finestra degli strumenti **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Cache Redis** e quindi fare clic su **Crea Cache Redis**.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Creare un menu di Cache Redis][CR01]

1. <span data-ttu-id="4f9c2-110">Quando viene visualizzata la finestra di dialogo **Nuova Cache Redis**, specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f9c2-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![Finestra di dialogo Crea nuova Cache Redis][CR02]

   <span data-ttu-id="4f9c2-112">a.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-112">a.</span></span> <span data-ttu-id="4f9c2-113">**Nome DNS**: specifica il sottodominio DNS per la nuova Cache Redis che verrà anteposto a ".redis.cache.windows.net"; ad esempio: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="4f9c2-114">b.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-114">b.</span></span> <span data-ttu-id="4f9c2-115">**Sottoscrizione**: specifica la sottoscrizione di Azure da usare per la nuova Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="4f9c2-116">c.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-116">c.</span></span> <span data-ttu-id="4f9c2-117">**Gruppo di risorse**: specifica il gruppo di risorse per la Cache Redis. Scegliere una delle opzioni indicate di seguito:</span><span class="sxs-lookup"><span data-stu-id="4f9c2-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span> 
      * <span data-ttu-id="4f9c2-118">**Crea nuovo**: specifica che si intende creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-118">**Create New**: Specifies that you want to create a new resource group.</span></span> 
      * <span data-ttu-id="4f9c2-119">**Usa esistente**: specifica che sarà possibile scegliere in un elenco i gruppi di risorse associati all'account di Azure.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span> 

   <span data-ttu-id="4f9c2-120">d.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-120">d.</span></span> <span data-ttu-id="4f9c2-121">**Posizione**: specifica la località in cui verrà creata la Cache Redis, ad esempio *Stati Uniti occidentali*.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="4f9c2-122">e.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-122">e.</span></span> <span data-ttu-id="4f9c2-123">**Piano tariffario**: specifica il piano tariffario usato dalla Cache Redis. Questa impostazione determina il numero di connessioni client</span><span class="sxs-lookup"><span data-stu-id="4f9c2-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="4f9c2-124">(Per altre informazioni, vedere [Prezzi di Cache Redis].)</span><span class="sxs-lookup"><span data-stu-id="4f9c2-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="4f9c2-125">f.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-125">f.</span></span> <span data-ttu-id="4f9c2-126">**Porta non SSL**: specifica se la Cache Redis consente le connessioni non SSL. Per impostazione predefinita, sono consentite solo le connessioni SSL.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="4f9c2-127">Dopo aver specificato tutte le impostazioni della Cache Redis, fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="4f9c2-128">Dopo aver creato la Cache Redis, quest'ultima verrà visualizzata in Azure Explorer.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Cache Redis in Azure Explorer][CR03]

> [!NOTE]
>
> <span data-ttu-id="4f9c2-130">Per altre informazioni sulla configurazione delle impostazioni di Cache Redis di Azure, vedere [Come configurare Cache Redis di Azure].</span><span class="sxs-lookup"><span data-stu-id="4f9c2-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="4f9c2-131">Visualizzare le proprietà della Cache Redis in IntelliJ</span><span class="sxs-lookup"><span data-stu-id="4f9c2-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="4f9c2-132">In Azure Explorer, fare clic con il pulsante destro sulla Cache Redis e quindi su **Mostra proprietà**.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Menu di scelta rapida Azure Explorer per mostrare le proprietà della Cache Redis][SP01]

1. <span data-ttu-id="4f9c2-134">Azure Explorer consente di visualizzare le proprietà della Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Proprietà della Cache Redis][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="4f9c2-136">Eliminare la Cache Redis tramite IntelliJ</span><span class="sxs-lookup"><span data-stu-id="4f9c2-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="4f9c2-137">In Azure Explorer, fare clic con il pulsante destro sulla Cache Redis e quindi su **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Menu di scelta rapida Azure Explorer per eliminare una Cache Redis][DE01]

1. <span data-ttu-id="4f9c2-139">Quando viene richiesto di eliminare la Cache Redis, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="4f9c2-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![Eliminare il prompt della Cache Redis][DE02]

## <a name="next-steps"></a><span data-ttu-id="4f9c2-141">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4f9c2-141">Next steps</span></span>

<span data-ttu-id="4f9c2-142">Per altre informazioni sulle Cache Redis di Azure, le impostazioni di configurazione e i prezzi, vedere i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f9c2-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="4f9c2-143">[Cache Redis di Azure]</span><span class="sxs-lookup"><span data-stu-id="4f9c2-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="4f9c2-144">[Documentazione di Cache Redis]</span><span class="sxs-lookup"><span data-stu-id="4f9c2-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="4f9c2-145">[Prezzi di Cache Redis]</span><span class="sxs-lookup"><span data-stu-id="4f9c2-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="4f9c2-146">[Come configurare Cache Redis di Azure]</span><span class="sxs-lookup"><span data-stu-id="4f9c2-146">[How to configure Azure Redis Cache]</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Prezzi di Cache Redis]: https://azure.microsoft.com/pricing/details/cache/
[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/
[Cache Redis di Azure]: https://azure.microsoft.com/services/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Documentazione di Cache Redis]: /azure/redis-cache
[Redis Cache Documentation]: /azure/redis-cache
[Come configurare Cache Redis di Azure]: /azure/redis-cache/cache-configure
[How to configure Azure Redis Cache]: /azure/redis-cache/cache-configure
[Istruzioni di accesso ad Azure per il Toolkit di Azure per IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
