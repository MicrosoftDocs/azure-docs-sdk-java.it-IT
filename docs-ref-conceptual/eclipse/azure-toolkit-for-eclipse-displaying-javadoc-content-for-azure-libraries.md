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
ms.openlocfilehash: 6f1edcc1411e8ec716dbfe41271d21d2397515c5
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954502"
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Visualizzare il contenuto Javadoc in Eclipse per il pacchetto di librerie di Azure per Java

Il contenuto Javadoc per le librerie di Azure per Java può essere visualizzato nell'ambiente Eclipse associando il contenuto Javadoc per le librerie di Azure per Java. La procedura seguente mostra come usare questa funzionalità in Eclipse.

Questa procedura presuppone che la libreria di Azure per Java sia già stata aggiunta al percorso di compilazione.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Visualizzare il contenuto Javadoc in Eclipse per le librerie di Azure per Java

1. In Project Explorer di Eclipse, nella sezione del progetto **Librerie a cui si fa riferimento** , aprire il menu di scelta rapida per la libreria di Azure per Java JAR. Ad esempio, **microsoft-windowsazure-api-0.1.0.jar** (il numero della versione può essere diverso, a seconda della versione installata).

1. Fare clic su **Proprietà**.

1. Nella finestra di dialogo **Properties** (Proprietà) nel riquadro sinistro, fare clic su **Javadoc Location** (Posizione Javadoc). Viene visualizzata la finestra di dialogo **Posizione Javadoc** .

1. È possibile specificare un **URL Javadoc** o un **Javadoc nell'archivio**.

   * Se si sceglie di specificare un **URL Javadoc**, usare URL simili a **http://dl.windowsazure.com/javadoc** o **http://dl.windowsazure.com/storage/javadoc**.

   * Se si sceglie di utilizzare **Javadoc nell'archivio**, è possibile specificare un file esterno o un file dell’area di lavoro.

   Effettuare la selezione e sfogliare/convalidare in base alle esigenze. L'esempio seguente associa le librerie di Azure per Java con il corrispondente file JAR di Javadoc scaricato localmente in una cartella denominata **c:\MyAzureJARs**.

   ![Visualizzare il contenuto Javadoc.][ic553487]

1. *Passaggio facoltativo*: fare clic su **Convalida**. Potrebbero essere visualizzati dei potenziali problemi con il file JAR di Javadoc.

1. Fare clic su **OK**.

Una volta associato alla libreria, il contenuto Javadoc deve essere visualizzato nell'IDE di Eclipse. Ad esempio, se `blob` è definito come tipo `CloudBlockBlob` nel codice, quello che segue è un esempio del contenuto Javadoc che viene visualizzato quando si digita `blob.acquireLease` nel codice:

![Visualizzare il contenuto Javadoc.][ic553488]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
