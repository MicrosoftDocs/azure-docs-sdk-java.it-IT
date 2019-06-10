---
title: Istruzioni di accesso per Azure Toolkit for Eclipse
description: Informazioni su come accedere a Microsoft Azure con il Toolkit di Azure per Eclipse.
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
ms.openlocfilehash: b4b13de38913ae6e7ae2bb09210ac742efc9d0ad
ms.sourcegitcommit: d18d9dce22b7f7af178f756bd341433d24e3c3b5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66575314"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Istruzioni di accesso per Azure Toolkit for Eclipse

Il Toolkit di Azure per Eclipse consente di accedere all'account Azure in due metodi diversi:

  - [Accedere all'account Azure tramite accesso dispositivo](#sign-in-to-your-azure-account-by-device-login)
  - [Accedere all'account Azure tramite entità servizio](#sign-in-to-your-azure-account-by-service-principal)

Sono anche disponibili metodi di [**disconnessione**](#sign-out-of-your-azure-account).

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>Accedere all'account Azure tramite accesso dispositivo

Per accedere ad Azure tramite accesso dispositivo, procedere come segue:

1. Aprire il progetto con Eclipse.

2. Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Sign In** (Accedi).
   ![Menu di Eclipse per l'accesso ad Azure][I01]

3. Nella finestra **Azure Sign In** (Accesso ad Azure), selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign In** (Accedi).

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

4. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

   ![Finestra di dialogo di accesso ad Azure][I03]

> [!NOTE]
>
> Se il browser non si apre, configurare Eclipse per l'uso di un browser esterno, come Internet Explorer, Firefox o Chrome:
>
> 1. Aprire Preferences -> General -> Web Browser -> Use external web browser in Eclipse (Preferenze -> Generale -> Web Browser -> Usa Web browser esterno in Eclipse)
>
> 2. Selezionare il browser che si preferisce usare
>

5. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

   ![Accesso al dispositivo nel browser][I04]

6. Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>Accedere all'account Azure tramite entità servizio

La sezione seguente illustra come creare un file di credenziali che contiene i dati dell'entità servizio. Dopo avere completato questo processo, Eclipse usa il file di credenziali per l'accesso automatico ad Azure quando si apre il progetto.

1. Aprire il progetto con Eclipse.

2. Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Sign In** (Accedi).
   ![Comando di accesso ad Azure in Eclipse][A01]

3. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Service Principal** (Entità servizio). Se il file di autenticazione dell'entità servizio non è già disponibile, fare clic su **New** (Nuovo) per crearne uno. Altrimenti è possibile fare clic su **Browse** (Sfoglia) per aprirlo e procedere con il passaggio 8.

   ![Finestra di accesso ad Azure con l'entità servizio selezionata][A02]

4. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

   ![Finestra di dialogo di accesso ad Azure][A08]

> [!NOTE]
>
> Se il browser non si apre, configurare Eclipse per l'uso di un browser esterno, come IE o Chrome:
>
> 1. Aprire Preferences -> General -> Web Browser -> Use external web browser in Eclipse (Preferenze -> Generale -> Web Browser -> Usa Web browser esterno in Eclipse)
>
> 2. Selezionare il browser che si preferisce usare
>

5. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

   ![Accesso al dispositivo nel browser][A03]

6. Nella finestra **Create authentication files** (Crea file di autenticazione) selezionare le sottoscrizioni da usare, scegliere la directory di destinazione e quindi fare cli su **Start** (Avvio).

   ![Finestra di creazione dei file di autenticazione][A04]

7. Nella finestra di dialogo **Service Principal Creation Status** (Stato creazione entità servizio) fare clic su **OK** al termine della creazione dei file.

   ![Finestra di dialogo sullo stato di creazione dell'entità servizio][A05]

8. L'indirizzo del file creato verrà inserito automaticamente nella finestra **Azure Sign In** (Accesso ad Azure). Fare clic su **Sign in** (Accedi).

   ![Finestra di dialogo di accesso ad Azure][A06]

9. Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   ![Finestra di dialogo Seleziona sottoscrizioni][A07]

## <a name="sign-out-of-your-azure-account"></a>Disconnettersi dall'account Azure

Dopo avere configurato l'account usando i passaggi precedenti, l'accesso verrà eseguito automaticamente ogni volta che si avvia Eclipse. Se tuttavia ci si vuole disconnettere dall'account Azure, eseguire queste operazioni.

1. In Eclipse fare clic su **Tools** (Strumenti), su **Azure** e quindi su **Sign Out** (Disconnetti).

   ![Menu di Eclipse per la disconnessione da Azure][L01]

2. Quando viene visualizzata la finestra di dialogo **Azure Sign Out** (Disconnessione da Azure), fare clic su **Yes** (Sì).

   ![Finestra di dialogo di disconnessione][L02]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

[A01]: media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
