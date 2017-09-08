---
title: Note sulla versione delle librerie di gestione di Azure per Java | Microsoft Docs
description: "Informazioni sulle novità e sulle modifiche di rilievo nelle librerie di gestione di Azure per Java"
keywords: Azure, Java, API, informazioni di riferimento, note, aggiornamenti, deprecare
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: fc246d499b2f5f20a7efbaed16c4b4193d8d8e6c
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="release-notes"></a>Note sulla versione 

## <a name="june-30-2017---110"></a>30 giugno 2017 - 1.1.0 

La versione 1.1 è compatibile con la versione 1.0 nelle API destinate all'uso pubblico che hanno raggiunto la fase di disponibilità generale (stabile) nella versione 1.0.

Alcune modifiche di rilievo sono state introdotte nelle API contrassegnate con l'annotazione @Beta nella versione 1.0

Se si esegue la migrazione del codice alla versione 1.1.0, è possibile usare [queste note](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) per la preparazione del codice per la versione 1.1.0 dalla versione 1.0.0.

### <a name="generally-availabile-in-v11"></a>Disponibile a livello generale nella versione 1.1

Alcune delle API disponibili come Beta nella versione 1.0 sono ora disponibili a livello generale nella versione 1.1, in particolare:

- Metodi asincroni
- Tutti i metodi della rete CDN disponibili in precedenza come versione Beta
- Tutti i metodi e tutte le interfacce nei gateway applicazione disponibili in precedenza come versione Beta

 Alcune parti della libreria sono ancora disponibili in anteprima. Per informazioni sullo stato attuale delle librerie, vedere la tabella seguente:

Servizio o funzionalità | Disponibile a livello generale | Disponibile come anteprima  | Presto disponibile |
---------|---------|---------|---------|
Calcolo  | Macchine virtuali ed estensioni di VM, set di scalabilità di macchine virtuali, Managed Disks   | Servizio contenitore di Azure, Registro contenitori di Azure |    |
Archiviazione   |  Account di archiviazione       |         |   Crittografia      |
Database SQL  | Database, firewall, pool elastici        |         |   Altre funzionalità      |
Rete    |  Reti virtuali, interfacce di rete, indirizzi IP, tabelle di routing, gruppi di sicurezza di rete, DNS, strumenti di gestione traffico gateway, applicazione  |    Servizi di bilanciamento del carico     |   VPN, watcher di rete   |
Altri servizi    |  Resource Manager, Key Vault, Redis, rete CDN, Batch       |  App Web, app per le funzioni, bus di servizio, Controllo degli accessi in base al ruolo per Graph, DocumentDB   | Monitoraggio, Utilità di pianificazione, gestione delle funzioni, Ricerca, altre funzionalità di Controllo degli accessi in base al ruolo per Graph        |
Concetti fondamentali     |   Autenticazione, core, metodi asincroni       |      |         |

### <a name="import-with-maven"></a>Eseguire l'importazione con Maven

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

Per ottenere supporto per l'uso delle librerie nel codice, vedere la community di [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk). Se si rilevano bug o se si vogliono inviare suggerimenti per il miglioramento delle librerie, inviare commenti tramite [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).

### <a name="migrate-from-previous-releases"></a>Eseguire la migrazione da versioni precedenti

[Migrate from 1.0.0-beta5](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md) (Eseguire la migrazione da 1.0.0-beta5) [Migrate from 1.1.0](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) (Eseguire la migrazione da 1.1.0)


