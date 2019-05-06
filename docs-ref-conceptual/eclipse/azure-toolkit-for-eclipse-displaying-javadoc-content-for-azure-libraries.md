---
title: Visualizzare il contenuto Javadoc in Eclipse per il pacchetto di librerie di Azure per Java
description: Come visualizzare il contenuto Javadoc per le librerie di Azure in Eclipse.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: bfed1a879bacf82436b71654c0ceca2826381eed
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61590611"
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="143b8-103">Visualizzare il contenuto Javadoc in Eclipse per il pacchetto di librerie di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="143b8-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>

<span data-ttu-id="143b8-104">Il contenuto Javadoc per le librerie di Azure per Java può essere visualizzato nell'ambiente Eclipse associando il contenuto Javadoc per le librerie di Azure per Java.</span><span class="sxs-lookup"><span data-stu-id="143b8-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="143b8-105">La procedura seguente mostra come usare questa funzionalità in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="143b8-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="143b8-106">Questa procedura presuppone che la libreria di Azure per Java sia già stata aggiunta al percorso di compilazione.</span><span class="sxs-lookup"><span data-stu-id="143b8-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="143b8-107">Visualizzare il contenuto Javadoc in Eclipse per le librerie di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="143b8-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>

1. <span data-ttu-id="143b8-108">In Project Explorer di Eclipse, nella sezione del progetto **Librerie a cui si fa riferimento** , aprire il menu di scelta rapida per la libreria di Azure per Java JAR.</span><span class="sxs-lookup"><span data-stu-id="143b8-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="143b8-109">Ad esempio, **microsoft-windowsazure-api-0.1.0.jar** (il numero della versione può essere diverso, a seconda della versione installata).</span><span class="sxs-lookup"><span data-stu-id="143b8-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

1. <span data-ttu-id="143b8-110">Fare clic su **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="143b8-110">Click **Properties**.</span></span>

1. <span data-ttu-id="143b8-111">Nella finestra di dialogo **Properties** (Proprietà) nel riquadro sinistro, fare clic su **Javadoc Location** (Posizione Javadoc).</span><span class="sxs-lookup"><span data-stu-id="143b8-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="143b8-112">Viene visualizzata la finestra di dialogo **Posizione Javadoc** .</span><span class="sxs-lookup"><span data-stu-id="143b8-112">The **Javadoc Location** dialog is displayed.</span></span>

1. <span data-ttu-id="143b8-113">È possibile specificare un **URL Javadoc** o un **Javadoc nell'archivio**.</span><span class="sxs-lookup"><span data-stu-id="143b8-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="143b8-114">Se si sceglie di specificare un **URL Javadoc**, usare URL come **http://dl.windowsazure.com/javadoc** o **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="143b8-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="143b8-115">Se si sceglie di utilizzare **Javadoc nell'archivio**, è possibile specificare un file esterno o un file dell’area di lavoro.</span><span class="sxs-lookup"><span data-stu-id="143b8-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="143b8-116">Effettuare la selezione e sfogliare/convalidare in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="143b8-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="143b8-117">L'esempio seguente associa le librerie di Azure per Java con il corrispondente file JAR di Javadoc scaricato localmente in una cartella denominata **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="143b8-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![Visualizzare il contenuto Javadoc.][ic553487]

1. <span data-ttu-id="143b8-119">*Passaggio facoltativo*: Fare clic su **Convalida**.</span><span class="sxs-lookup"><span data-stu-id="143b8-119">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="143b8-120">Potrebbero essere visualizzati dei potenziali problemi con il file JAR di Javadoc.</span><span class="sxs-lookup"><span data-stu-id="143b8-120">Potential issues with the Javadoc JAR could be displayed here.</span></span>

1. <span data-ttu-id="143b8-121">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="143b8-121">Click **OK**.</span></span>

<span data-ttu-id="143b8-122">Una volta associato alla libreria, il contenuto Javadoc deve essere visualizzato nell'IDE di Eclipse.</span><span class="sxs-lookup"><span data-stu-id="143b8-122">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="143b8-123">Ad esempio, se `blob` è definito come tipo `CloudBlockBlob` nel codice, quello che segue è un esempio del contenuto Javadoc che viene visualizzato quando si digita `blob.acquireLease` nel codice:</span><span class="sxs-lookup"><span data-stu-id="143b8-123">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![Visualizzare il contenuto Javadoc.][ic553488]

## <a name="next-steps"></a><span data-ttu-id="143b8-125">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="143b8-125">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
