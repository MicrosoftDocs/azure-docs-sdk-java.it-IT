---
title: Installare il Toolkit di Azure per Eclipse.
description: Informazioni su come installare il plug-in Azure Toolkit for Eclipse per creare e distribuire applicazioni cloud in Azure.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 8e6630f7e019d950249e7e84024ac800a0f2f136
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270840"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installare il Toolkit di Azure per Eclipse.

È possibile installare Azure Toolkit for Eclipse in due modi:

  - [Eclipse Marketplace](#eclipse-marketplace)
  - [Installazione di nuovo software](#install-new-software)

> [!NOTE] 
> 
> Azure Toolkit for Eclipse è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente: 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [azure-toolkit-for-eclipse-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="eclipse-marketplace"></a>Eclipse Marketplace

1. Trascinare il pulsante seguente nell'area di lavoro di Eclipse in esecuzione.

    [![Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Trascinare nell'area di lavoro di Eclipse* in esecuzione. *Richiede il client Eclipse Marketplace")

2. Altrimenti, è anche possibile cercare e installare il **plug-in Azure Toolkit for Eclipse** tramite la **Guida di Eclipse Marketplace**.

    ![Marketplace](./media/azure-toolkit-for-eclipse-installation/marketplace.png)

## <a name="install-new-software"></a>Installazione di nuovo software

1. Avviare Eclipse.

1. Fare clic sul menu **Help** (Guida), quindi su **Install New Software** (Installa nuovo software), come illustrato di seguito.

   ![Installare il Toolkit di Azure per Eclipse.][01]

1. Nella finestra di dialogo **Available Software** (Software disponibile), nella casella di testo **Work with** (Usa), digitare `http://dl.microsoft.com/eclipse/` e quindi premere il tasto **Invio**.

1. Nel riquadro **Nome**, controllare il **Toolkit di Azure per Java** e deselezionare **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto**. Verrà visualizzata una schermata simile alla seguente:

   ![Installare il Toolkit di Azure per Eclipse.][02]

1. Espandendo **Azure Toolkit for Eclipse**, viene visualizzato un elenco di componenti che verranno installati, ad esempio:

   | Funzionalità | DESCRIZIONE | 
   |---|---| 
   | **Plug-in di Application Insights per Java** | Consente di usare servizi di analisi e di registrazione dei dati di telemetria di Azure per le applicazioni e le istanze del server. | 
   | **Plug-in Azure Common** | Offre le funzionalità comuni necessarie per gli altri componenti del toolkit. | 
   | **Azure Container Tools for Eclipse** | Consente di compilare e distribuire un file WAR come contenitore Docker in un computer Docker. | 
   | **Azure Containers for Eclipse** | Consente di distribuire un file WAR o un elemento JAR come contenitore Docker in una macchina virtuale di Azure. | 
   | **Azure Explorer per Eclipse** | Offre un'interfaccia di tipo Esplora risorse per la gestione delle risorse di Azure. | 
   | **Microsoft JDBC Driver 6.1 per SQL Server** | Fornisce l'API JDBC per SQL Server e il database SQL di Microsoft Azure per Java Platform Enterprise Edition 8. | 
   | **Pacchetto per Librerie di Microsoft Azure per Java** | Fornisce le API per accedere ai servizi di Microsoft Azure, ad esempio l'archiviazione, il bus di servizio, il runtime del servizio e così via. | 

1. Fare clic su **Avanti**. (Se si verificano ritardi insoliti durante l'installazione del toolkit, assicurarsi che **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto** sia deselezionato.)

1. Nella finestra di dialogo **Install Details** fare clic su **Next** (Avanti).

   ![Verificare i dettagli di installazione][03]

1. Nella finestra di dialogo **Esaminare licenze** , rivedere le condizioni dei contratti di licenza. Se si accettano le condizioni dei contratti di licenza, fare clic su **I accept the terms of the license agreements** (Accetto le condizioni dei contratti di licenza) e quindi fare clic su **Finish** (Fine). (I passaggi rimanenti suppongono che le condizioni dei contratti di licenza siano state accettate. Se non si accettano le condizioni dei contratti di licenza, uscire dal processo di installazione.)

   ![Esaminare licenze][04]

   Eclipse scarica e installa i pacchetti necessari.

   ![Stato dell'installazione][05]

1. Se viene richiesto di riavviare Eclipse per completare l'installazione, fare clic su **Sì**.

   ![Riavviare il prompt dei comandi][06]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->
[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
