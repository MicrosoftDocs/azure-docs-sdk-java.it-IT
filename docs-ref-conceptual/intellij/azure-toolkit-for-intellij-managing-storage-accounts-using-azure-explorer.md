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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: f37ae653286af1e36d730bda713527caa7504ac0
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Gestire gli account di archiviazione usando Azure Explorer per IntelliJ

Incluso in Azure Toolkit for IntelliJ, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione degli account di archiviazione con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>Creare un account di archiviazione in IntelliJ

Per creare un account di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Accedere al proprio account Azure seguendo le [Istruzioni di accesso per Azure Toolkit for IntelliJ]. 

2. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Account di archiviazione** e quindi fare clic su **Crea account di archiviazione**.

   ![Comando Crea account di archiviazione][CS01]

3. Nella finestra di dialogo **Crea account di archiviazione** specificare le opzioni seguenti:

   ![Finestra di dialogo Crea un nuovo account di archiviazione][CS02]

   * **Nome**: specifica il nome per il nuovo account di archiviazione.

   * **Tipologia account**: specifica il tipo di account di archiviazione da creare, ad esempio "archiviazione BLOB". Per altre informazioni, vedere [Informazioni sugli account di archiviazione di Azure]. 

   * **Prestazioni**: specifica l'account di archiviazione offerto da usare dall'autore selezionato, ad esempio "Premium". Per altre informazioni, vedere [Obiettivi di scalabilità e prestazioni per Archiviazione di Azure]. 

   * **Replica**: specifica la replica per l'account di archiviazione di account, ad esempio "Con ridondanza della zona". Per altre informazioni, vedere [Replica di Archiviazione di Azure]. 

   * **Sottoscrizione**: specifica la sottoscrizione di Azure da usare per il nuovo account di archiviazione.

   * **Località**: specifica la località in cui verrà creato l'account di archiviazione, ad esempio "Stati Uniti occidentali".

   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
      * **Crea nuovo**: specifica che si intende creare un nuovo gruppo di risorse.
      * **Usa esistente**: specifica che sarà possibile scegliere in un elenco i gruppi di risorse associati all'account di Azure.

4. Dopo avere specificato tutte le opzioni precedenti, fare clic su **OK**.

## <a name="create-a-storage-container-in-intellij"></a>Creare un contenitore di archiviazione in IntelliJ

Per creare un contenitore di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione Azure Explorer fare clic con il pulsante destro del mouse sull'account di archiviazione in cui si intende creare un contenitore e quindi fare clic su **Crea contenitore BLOB**.

   ![Comando Crea contenitore BLOB][CC01]

2. Nella finestra di dialogo **Crea contenitore BLOB** specificare il nome per il contenitore e quindi fare clic su **OK**. Per altre informazioni sulla denominazione dei contenitori di archiviazione, vedere [Denominazione e riferimento a contenitori, BLOB e metadati].

   ![Finestra di dialogo Crea contenitore di archiviazione][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>Eliminare un contenitore di archiviazione in IntelliJ

Per eliminare un contenitore di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione Azure Explorer fare clic con il pulsante destro del mouse sul contenitore di archiviazione e quindi fare clic su **Elimina**.

   ![Comando Elimina contenitore di archiviazione][DC01]

2. Nella finestra di conferma fare clic su **Sì**.

   ![Finestra di conferma dell'eliminazione del contenitore di archiviazione][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>Eliminare un account di archiviazione in IntelliJ

Per eliminare un account di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sull'account di archiviazione e quindi selezionare **Elimina**.

   ![Menu per l'eliminazione dell'account di archiviazione][DS01]

2. Nella finestra di conferma fare clic su **Sì**.

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

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

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
