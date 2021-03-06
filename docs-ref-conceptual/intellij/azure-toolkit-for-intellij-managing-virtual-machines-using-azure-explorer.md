---
title: Gestire macchine virtuali con Azure Explorer per IntelliJ
description: Informazioni su come gestire macchine virtuali di Azure con Azure Explorer per IntelliJ.
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
ms.openlocfilehash: a3aff77bc2fd2dac0396187d9e6b27910bc60e58
ms.sourcegitcommit: 24f037d133875f86761ec893dfa74e723de040b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636416"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a>Gestire macchine virtuali con Azure Explorer per IntelliJ

Azure Explorer, che fa parte di Azure Toolkit for IntelliJ, offre agli sviluppatori Java una soluzione di facile uso per la gestione delle macchine virtuali con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a>Creare una macchina virtuale in IntelliJ

Per creare una macchina virtuale con Azure Explorer, eseguire queste operazioni: 

1. Accedere al proprio account Azure tramite la procedura descritta nell'articolo [Istruzioni di accesso per Azure Toolkit for IntelliJ].

2. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Macchine virtuali** e quindi fare clic su **Crea macchina virtuale**. 

   ![Comando Crea macchina virtuale][CR01]  
    Viene visualizzata la procedura guidata **Creare una nuova macchina virtuale**.

3. Nella finestra **Scegliere una sottoscrizione** selezionare la sottoscrizione e fare clic su **Avanti**. 

   ![Finestra di dialogo Scegliere una sottoscrizione][CR02]

4. Nella finestra **Selezionare un'immagine di macchina virtuale** immettere le informazioni seguenti:

   * **Località**: specifica dove verrà creata la macchina virtuale, ad esempio *West US* (Stati Uniti occidentali). 

   * **Recommended image** (Immagine consigliata): specifica che si sceglierà un'immagine in un elenco abbreviato delle immagini comunemente usate.

   * **Custom image** (Immagine personalizzata): specifica che si sceglierà un'immagine personalizzata specificando le informazioni seguenti.

      * **Publisher** (Editore): specifica l'editore che ha creato l'immagine che verrà usata per la macchina virtuale, ad esempio *Microsoft*.

      * **Offer** (Offerta): specifica l'offerta di macchina virtuale dell'editore selezionato da usare, ad esempio *JDK*.

      * **Sku**: specifica lo SKU dell'offerta selezionata da usare, ad esempio *JDK_8*.

      * **Version #** (N. versione): specifica la versione dello SKU selezionato da usare.

   ![Finestra Selezionare un'immagine di macchina virtuale][CR03]

5. Fare clic su **Avanti**. 

6. Nella finestra **Impostazioni di base della macchina virtuale** immettere le informazioni seguenti:

   * **Nome macchina virtuale**: specifica il nome della nuova macchina virtuale, che deve iniziare con una lettera e contenere solo lettere, numeri e trattini.

   * **Dimensione**: specifica il numero di core e la quantità di memoria da allocare per la macchina virtuale.

   * **User name** (Nome utente): specifica l'account amministratore da creare per la gestione della macchina virtuale.

   * **Password** e **Confirm** (Conferma): specificano la password per l'account amministratore.

   ![Finestra Impostazioni di base della macchina virtuale][CR04]

7. Fare clic su **Avanti**. 

8. Nella finestra **Risorse associate** immettere le informazioni seguenti:

   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
      * **Crea nuovo**: specifica che si vuole creare un nuovo gruppo di risorse.
      * **Use existing** (Usa esistente): specifica che si vuole effettuare una selezione da un elenco di gruppi di risorse associati all'account Azure.

       ![Finestra Risorse associate][CR07]

   * **Account di archiviazione**: specifica l'account di archiviazione da usare per archiviare la macchina virtuale. È possibile usare un account di archiviazione esistente o crearne uno nuovo. Se si sceglie **Crea nuovo**, verrà visualizzata la finestra di dialogo seguente:

      ![Finestra di dialogo Crea account di archiviazione][CR05]

   * **Virtual Network** (Rete virtuale) e **Subnet**: specificano la rete virtuale e la subnet a cui si connetterà la macchina virtuale. È possibile usare una subnet e una rete esistente oppure creare una rete e una subnet nuove. Se si seleziona **Crea nuovo**, verrà visualizzata la finestra di dialogo seguente:

      ![Finestra di dialogo Crea rete virtuale][CR06]

   * **Indirizzo IP pubblico**: specifica un indirizzo IP con accesso all'esterno per la macchina virtuale. È possibile scegliere di creare un nuovo indirizzo IP o, se la macchina virtuale non avrà un indirizzo IP pubblico, è possibile selezionare **(Nessuno)**. 

   * **Network security group** (Gruppo di sicurezza di rete): specifica un firewall di rete facoltativo per la macchina virtuale. È possibile selezionare un firewall esistente oppure, se la macchina virtuale non usa un firewall di rete, è possibile selezionare **(Nessuno)**. 

   * **Set di disponibilità**: specifica un set di disponibilità facoltativo a cui può appartenere la macchina virtuale. È possibile selezionare un set di disponibilità esistente, creare un nuovo set di disponibilità o, se la macchina virtuale non apparterrà a un set di disponibilità, selezionare **(Nessuno)**.

9. Fare clic su **Fine**.  
    La nuova macchina virtuale viene visualizzata nella finestra dello strumento Azure Explorer. 

   ![Nuova macchina virtuale nella visualizzazione Azure Explorer][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a>Riavviare una macchina virtuale in IntelliJ

Per riavviare una macchina virtuale con Azure Explorer in IntelliJ, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Riavvia**.

   ![Comando di riavvio della macchina virtuale][RE01]

2. Nella finestra di conferma fare clic su **Sì**. 

   ![Finestra di conferma del riavvio della macchina virtuale][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a>Arrestare una macchina virtuale in IntelliJ

Per arrestare una macchina virtuale in esecuzione con Azure Explorer in IntelliJ, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Chiudi**.

   ![Comando di arresto della macchina virtuale][SH01]

2. Nella finestra di conferma fare clic su **Sì**. 

   ![Finestra di conferma dell'arresto della macchina virtuale][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a>Eliminare una macchina virtuale in IntelliJ

Per eliminare una macchina virtuale con Azure Explorer in IntelliJ, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Elimina**.

   ![Comando di eliminazione della macchina virtuale][DE01]

2. Nella finestra di conferma fare clic su **Sì**. 

   ![Finestra di conferma dell'eliminazione della macchina virtuale][DE02]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle dimensioni e sui prezzi delle macchine virtuali in Azure, vedere i collegamenti seguenti:

* Dimensioni delle macchine virtuali in Azure
  * [Dimensioni per le macchine virtuali Windows in Azure]
  * [Dimensioni delle macchine virtuali Linux in Azure]
* Prezzi delle macchine virtuali in Azure
  * [Prezzi delle macchine virtuali in Windows]
  * [Prezzi delle macchine virtuali in Linux]

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Istruzioni di accesso per Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Dimensioni per le macchine virtuali Windows in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Dimensioni delle macchine virtuali Linux in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Prezzi delle macchine virtuali in Windows]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Prezzi delle macchine virtuali in Linux]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
