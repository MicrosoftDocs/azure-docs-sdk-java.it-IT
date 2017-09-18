---
title: Installare il Toolkit di Azure per Eclipse.
description: Informazioni su come installare Azure Toolkit for Eclipse.
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 59a8bfb6ab4db8ea8c6c9025ca3ced8a13192628
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installare il Toolkit di Azure per Eclipse.

Il Toolkit di Azure per Eclipse offre modelli e funzionalità che permettono di creare, sviluppare, testare e distribuire con facilità applicazioni di Azure tramite l'ambiente di sviluppo Eclipse. Il Toolkit di Azure per Eclipse è un progetto open source. Il codice sorgente è disponibile in base alla licenza MIT da <https://github.com/microsoft/azure-tools-for-java>.

La procedura seguente mostra come installare il Toolkit di Azure per Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Per installare il Toolkit di Azure per Eclipse

1. Avviare Eclipse.

1. Fare clic sul menu **Help** (Guida), quindi su **Install New Software** (Installa nuovo software), come illustrato di seguito.
   
   ![Installare il Toolkit di Azure per Eclipse.][01]

1. Nella finestra di dialogo **Available Software** (Software disponibile), nella casella di testo **Work with** (Usa), digitare `http://dl.microsoft.com/eclipse/` e quindi premere il tasto **Invio**.

1. Nel riquadro **Nome**, controllare il **Toolkit di Azure per Eclipse**, e deselezionare **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto**. Verrà visualizzata una schermata simile alla seguente:
   
   ![Installare il Toolkit di Azure per Eclipse.][02]

1. Espandendo **Azure Toolkit for Eclipse**, verrà visualizzato un elenco di voci come nell'esempio seguente:
   
   * **Plug-in di Application Insights per Java**: questo componente consente di usare servizi di analisi e di registrazione dei dati di telemetria di Azure per le applicazioni e le istanze del server.
   * **Azure Access Control Services Filter**: questo componente supporta l'autenticazione degli utenti dell'applicazione ad ACS di Azure, offrendo scenari Single Sign-On ed esternalizzando la logica delle identità dall'applicazione.
   * **Plug-in Azure Common**: questo componente offre le funzionalità comuni necessarie per gli altri componenti del toolkit.
   * **Azure Explorer for Eclipse**: questo componente offre le funzionalità comuni necessarie per gli altri componenti del toolkit.
   * **Azure Plugin for Eclipse with Java**: questo componente supporta lo sviluppo di progetti per compilare, testare e distribuire le applicazioni Java per il cloud di Microsoft Azure in Eclipse e tramite riga di comando.
   * **Azure Web Apps Plugin for Java**: questo componente supporta la distribuzione di applicazioni Web Java in contenitori App Web di Microsoft Azure.
   * **Microsoft JDBC Driver 4.2 per SQL Server**: questo componente fornisce l'API JDBC per SQL Server e il database SQL di Microsoft Azure per Java Platform Enterprise Edition 8.
   * **Package for Apache Qpid Client Libraries for JMS**: questo componente fornisce il componente client JSMS dal progetto Apache Qpid per consentire all'applicazione di usare la messaggistica basata su AMQP in Microsoft Azure.
   * **Package for Microsoft Azure Libraries for Java**: questo componente fornisce le API per accedere ai servizi di Microsoft Azure, ad esempio archiviazione, bus di servizio, runtime del servizio e così via.

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

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
