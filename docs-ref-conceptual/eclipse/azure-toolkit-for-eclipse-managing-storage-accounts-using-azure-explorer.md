---
title: Gestire gli account di archiviazione con Azure Explorer per Eclipse
description: Informazioni su come gestire gli account di archiviazione di Azure con Azure Explorer per Eclipse.
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
ms.openlocfilehash: dc39298d88f2fb0b2f56d6fbc0071b68b8d27989
ms.sourcegitcommit: 24f037d133875f86761ec893dfa74e723de040b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636657"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>Gestire gli account di archiviazione con Azure Explorer per Eclipse

Incluso nel Toolkit di Azure per Eclipse, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione degli account di archiviazione con il proprio account Azure nell'IDE (ambiente di sviluppo integrato) di Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a>Creazione di un account di archiviazione in Eclipse

Per creare un account di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Accedere al proprio account Azure seguendo le [Istruzioni di accesso ad Azure per il Toolkit di Azure per Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).

1. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Account di archiviazione** e quindi fare clic su **Crea account di archiviazione**.

   ![Comando Crea account di archiviazione][CS01]

1. Nella finestra di dialogo **Crea account di archiviazione** specificare le opzioni seguenti:

   ![Finestra di dialogo Crea un nuovo account di archiviazione][CS02]

   * **Nome**: specifica il nome del nuovo account di archiviazione.

   * **Sottoscrizione**: specifica la sottoscrizione di Azure da usare per il nuovo account di archiviazione.

   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
      * **Create New** (Crea nuovo): specifica che si vuole creare un nuovo gruppo di risorse.
      * **Use Existing** (Usa esistente): specifica che verrà effettuata una selezione da un elenco di gruppi di risorse associati all'account Azure.

   * **Area**: specifica la località in cui verrà creato l'account di archiviazione, ad esempio "West US" (Stati Uniti occidentali).

   * **Account kind** (Tipologia account): specifica il tipo di account di archiviazione da creare, ad esempio "Blob storage" (Archiviazione BLOB). Per altre informazioni, vedere [Informazioni sugli account di archiviazione di Azure].

   * **Prestazioni**: specifica l'offerta di account di archiviazione dell'editore selezionato da usare, ad esempio "Premium". Per altre informazioni, vedere [Obiettivi di scalabilità e prestazioni per Archiviazione di Azure].

   * **Replica**: specifica la replica per l'account di archiviazione, ad esempio "Zone-Redundant" (Con ridondanza della zona). Per altre informazioni, vedere [Replica di Archiviazione di Azure].

1. Dopo avere specificato tutte le opzioni precedenti, fare clic su **Crea**.

## <a name="create-a-storage-container-in-eclipse"></a>Creazione di un contenitore di archiviazione in Eclipse

Per creare un contenitore di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sull'account di archiviazione in cui si desidera creare un contenitore e quindi fare clic su **Crea contenitore BLOB**.

   ![Comando Crea contenitore BLOB][CC01]

1. Nella finestra di dialogo **Crea contenitore BLOB** specificare il nome per il contenitore e quindi fare clic su **OK**. Per altre informazioni sulla denominazione dei contenitori di archiviazione, vedere l'articolo relativo alla [denominazione e riferimento a contenitori, BLOB e metadati].

   ![Finestra di dialogo Crea contenitore BLOB][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a>Eliminazione di un contenitore di archiviazione in Eclipse

Per eliminare un contenitore di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sul contenitore di archiviazione e quindi fare clic su **Elimina**.

   ![Comando Elimina contenitore di archiviazione][DC01]

1. Nella finestra di conferma fare clic su **OK**.

   ![Finestra di conferma dell'eliminazione del contenitore di archiviazione][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a>Eliminazione di un account di archiviazione in Eclipse

Per eliminare un account di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sull'account di archiviazione e quindi selezionare **Elimina**.

   ![Comando Elimina account di archiviazione][DS01]

1. Nella finestra di conferma fare clic su **OK**.

   ![Finestra di conferma dell'eliminazione dell'account di archiviazione][DS02]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sugli account di archiviazione, sulle dimensioni e sui prezzi di Azure, vedere le risorse seguenti:

* [Introduzione ad Archiviazione di Microsoft Azure]
* [Informazioni sugli account di archiviazione di Azure]
* Dimensioni degli account di archiviazione di Azure
  * [Sizes for Windows storage accounts in Azure] (Dimensioni degli account di archiviazione Windows in Azure)
  * [Sizes for Linux storage accounts in Azure] (Dimensioni degli account di archiviazione Linux in Azure)
* Prezzi per gli account di archiviazione di Azure
  * [Prezzi per gli account di archiviazione Windows]
  * [Prezzi per gli account di archiviazione Linux]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Introduzione ad Archiviazione di Microsoft Azure]: /azure/storage/storage-introduction
[Informazioni sugli account di archiviazione di Azure]: /azure/storage/storage-create-storage-account
[Replica di Archiviazione di Azure]: /azure/storage/storage-redundancy
[Obiettivi di scalabilità e prestazioni per Archiviazione di Azure]: /azure/storage/storage-scalability-targets
[Denominazione e riferimento a contenitori, BLOB e metadati]: http://go.microsoft.com/fwlink/?LinkId=255555

[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes (Dimensioni degli account di archiviazione Windows in Azure)
[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes (Dimensioni degli account di archiviazione Linux in Azure)
[Prezzi per gli account di archiviazione Windows]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Prezzi per gli account di archiviazione Linux]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
