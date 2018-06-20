---
title: Note sulla versione delle librerie di gestione di Azure per Java | Microsoft Docs
description: Informazioni sulle novità e sulle modifiche di rilievo nelle librerie di gestione di Azure per Java
keywords: Azure, Java, API, informazioni di riferimento, note, aggiornamenti, deprecare
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 924ccf9bdaad4bc635f133adbcfcc8f797d06644
ms.sourcegitcommit: acc83bb537d77568b2a5427479d6354d6ae30885
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2017
ms.locfileid: "23982163"
---
# <a name="release-notes"></a>Note sulla versione 

## <a name="october-5-2017---130"></a>5 ottobre 2017 - 1.3.0 

La versione 1.3.0 è compatibile con le versioni precedenti per l'uso di funzionalità e servizi per cui è stata raggiunta la fase di disponibilità generale (stabile) nelle versioni precedenti.

Eventuali modifiche significative rispetto alle versioni di anteprima per tali servizi sono contrassegnate con l'annotazione @Beta.

Se si esegue la migrazione del codice alla versione 1.3.0, è possibile usare [queste note](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) per preparare il codice esistente per la versione 1.3.

### <a name="generally-availabile-in-v13"></a>Disponibilità generale nella versione 1.3

Alcune API disponibili come Beta nelle versioni precedenti sono ora disponibili a livello generale, in particolare:

- Metodi asincroni
- Tutti i metodi della rete CDN disponibili in precedenza come versione Beta
- Tutti i metodi e tutte le interfacce nei gateway applicazione disponibili in precedenza come versione Beta

 Alcune parti della libreria sono ancora disponibili in anteprima. Per informazioni sullo stato attuale delle librerie, vedere la tabella seguente:

Servizio o funzionalità | Disponibile a livello generale | Disponibile come anteprima 
---------|---------|---------|-
Calcolo  | Macchine virtuali ed estensioni di VM, set di scalabilità di macchine virtuali, Managed Disks   | Servizio contenitore di Azure, Registro contenitori di Azure 
Archiviazione   |  Account di archiviazione       |    Crittografia     
Database SQL  | Database, firewall, pool elastici              
Rete    |  Reti virtuali, interfacce di rete, indirizzi IP, tabelle di routing, gruppi di sicurezza di rete, DNS, strumenti di gestione traffico gateway, applicazione  |    Servizi di bilanciamento del carico, peering reti, gateway di rete virtuale, watcher di rete 
Altri servizi    |  Resource Manager, Key Vault, Redis, rete CDN, Batch       |  App Web, app per le funzioni, bus di servizio, controllo degli accessi in base al ruolo per Graph, Cosmos DB, Ricerca  
Nozioni fondamentali     |   Autenticazione: di base, metodi asincroni, identità del servizio gestito      |      |

> Le funzionalità in anteprima sono contrassegnate con l'annotazione `@Beta` a livello di classe, di interfaccia o di metodo nelle librerie. Queste funzionalità sono soggette a modifiche. In futuro potrebbero essere modificate in qualsiasi modo o addirittura rimosse.

### <a name="import-with-maven"></a>Eseguire l'importazione con Maven

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

Per ottenere supporto per l'uso delle librerie nel codice, vedere la community di [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk). Se si rilevano bug o se si vogliono inviare suggerimenti per il miglioramento delle librerie, inviare commenti tramite [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).


