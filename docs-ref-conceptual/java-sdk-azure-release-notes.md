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
ms.openlocfilehash: 4681e07993e18344655f91e35148bbe956974e95
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592596"
---
# <a name="release-notes"></a><span data-ttu-id="e93ac-104">Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="e93ac-104">Release Notes</span></span> 

## <a name="october-5-2017---130"></a><span data-ttu-id="e93ac-105">5 ottobre 2017 - 1.3.0</span><span class="sxs-lookup"><span data-stu-id="e93ac-105">October 5, 2017 - 1.3.0</span></span> 

<span data-ttu-id="e93ac-106">La versione 1.3.0 è compatibile con le versioni precedenti per l'uso di funzionalità e servizi per cui è stata raggiunta la fase di disponibilità generale (stabile) nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="e93ac-106">Version 1.3.0 is backwards compatible with previous versions for services and features use that reached the general availability (stable) stage in previous releases.</span></span>

<span data-ttu-id="e93ac-107">Eventuali modifiche significative rispetto alle versioni di anteprima per tali servizi sono contrassegnate con l'annotazione @Beta.</span><span class="sxs-lookup"><span data-stu-id="e93ac-107">Any breaking changes from Preview versions for those services are marked with the @Beta annotation.</span></span>

<span data-ttu-id="e93ac-108">Se si esegue la migrazione del codice alla versione 1.3.0, è possibile usare [queste note](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) per preparare il codice esistente per la versione 1.3.</span><span class="sxs-lookup"><span data-stu-id="e93ac-108">If you are migrating your code to 1.3.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) to prepare your existing code for the 1.3 version.</span></span>

### <a name="generally-availabile-in-v13"></a><span data-ttu-id="e93ac-109">Disponibilità generale nella versione 1.3</span><span class="sxs-lookup"><span data-stu-id="e93ac-109">Generally availabile in V1.3</span></span>

<span data-ttu-id="e93ac-110">Alcune API disponibili come Beta nelle versioni precedenti sono ora disponibili a livello generale, in particolare:</span><span class="sxs-lookup"><span data-stu-id="e93ac-110">Some of the APIs that were still in Beta in previous releases are now GA, in particular:</span></span>

- <span data-ttu-id="e93ac-111">Metodi asincroni</span><span class="sxs-lookup"><span data-stu-id="e93ac-111">async methods</span></span>
- <span data-ttu-id="e93ac-112">Tutti i metodi della rete CDN disponibili in precedenza come versione Beta</span><span class="sxs-lookup"><span data-stu-id="e93ac-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="e93ac-113">Tutti i metodi e tutte le interfacce nei gateway applicazione disponibili in precedenza come versione Beta</span><span class="sxs-lookup"><span data-stu-id="e93ac-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

  <span data-ttu-id="e93ac-114">Alcune parti della libreria sono ancora disponibili in anteprima.</span><span class="sxs-lookup"><span data-stu-id="e93ac-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="e93ac-115">Per informazioni sullo stato attuale delle librerie, vedere la tabella seguente:</span><span class="sxs-lookup"><span data-stu-id="e93ac-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="e93ac-116">Servizio o funzionalità</span><span class="sxs-lookup"><span data-stu-id="e93ac-116">Service or feature</span></span> | <span data-ttu-id="e93ac-117">Disponibile a livello generale</span><span class="sxs-lookup"><span data-stu-id="e93ac-117">Available as GA</span></span> | <span data-ttu-id="e93ac-118">Disponibile come anteprima</span><span class="sxs-lookup"><span data-stu-id="e93ac-118">Available as Preview</span></span> 
---------|---------|---------|-
<span data-ttu-id="e93ac-119">Calcolo</span><span class="sxs-lookup"><span data-stu-id="e93ac-119">Compute</span></span>  | <span data-ttu-id="e93ac-120">Macchine virtuali ed estensioni di VM, set di scalabilità di macchine virtuali, Managed Disks</span><span class="sxs-lookup"><span data-stu-id="e93ac-120">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="e93ac-121">servizio Azure Container, Registro Azure Container</span><span class="sxs-lookup"><span data-stu-id="e93ac-121">Azure container service, Azure container registry</span></span> 
<span data-ttu-id="e93ac-122">Archiviazione</span><span class="sxs-lookup"><span data-stu-id="e93ac-122">Storage</span></span>   |  <span data-ttu-id="e93ac-123">Account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="e93ac-123">Storage accounts</span></span>       |    <span data-ttu-id="e93ac-124">Crittografia</span><span class="sxs-lookup"><span data-stu-id="e93ac-124">Encryption</span></span>     
<span data-ttu-id="e93ac-125">Database SQL</span><span class="sxs-lookup"><span data-stu-id="e93ac-125">SQL Database</span></span>  | <span data-ttu-id="e93ac-126">Database, firewall, pool elastici</span><span class="sxs-lookup"><span data-stu-id="e93ac-126">Databases, firewalls, elastic pools</span></span>              
<span data-ttu-id="e93ac-127">Rete</span><span class="sxs-lookup"><span data-stu-id="e93ac-127">Networking</span></span>    |  <span data-ttu-id="e93ac-128">Reti virtuali, interfacce di rete, indirizzi IP, tabelle di routing, gruppi di sicurezza di rete, DNS, strumenti di gestione traffico gateway, applicazione</span><span class="sxs-lookup"><span data-stu-id="e93ac-128">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="e93ac-129">Servizi di bilanciamento del carico, peering reti, gateway di rete virtuale, watcher di rete</span><span class="sxs-lookup"><span data-stu-id="e93ac-129">Load balancers, Network peering, Virtual Network Gateway, Network watchers</span></span> 
<span data-ttu-id="e93ac-130">Altri servizi</span><span class="sxs-lookup"><span data-stu-id="e93ac-130">More services</span></span>    |  <span data-ttu-id="e93ac-131">Resource Manager, Key Vault, Redis, rete CDN, Batch</span><span class="sxs-lookup"><span data-stu-id="e93ac-131">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="e93ac-132">App Web, app per le funzioni, bus di servizio, controllo degli accessi in base al ruolo per Graph, Cosmos DB, Ricerca</span><span class="sxs-lookup"><span data-stu-id="e93ac-132">Web apps, Function apps, Service Bus, Graph RBAC, Cosmos DB, Search</span></span>  
<span data-ttu-id="e93ac-133">Nozioni fondamentali</span><span class="sxs-lookup"><span data-stu-id="e93ac-133">Fundamentals</span></span>     |   <span data-ttu-id="e93ac-134">Autenticazione: di base, metodi asincroni, identità del servizio gestito</span><span class="sxs-lookup"><span data-stu-id="e93ac-134">Authentication - core , Async methods , Managed Service Identity</span></span>      |      |

> <span data-ttu-id="e93ac-135">Le funzionalità in anteprima sono contrassegnate con l'annotazione `@Beta` a livello di classe, di interfaccia o di metodo nelle librerie.</span><span class="sxs-lookup"><span data-stu-id="e93ac-135">Preview features are marked with a `@Beta` annotation at the class or interface or method level in libraries.</span></span> <span data-ttu-id="e93ac-136">Queste funzionalità sono soggette a modifiche.</span><span class="sxs-lookup"><span data-stu-id="e93ac-136">These features are subject to change.</span></span> <span data-ttu-id="e93ac-137">In futuro potrebbero essere modificate in qualsiasi modo o addirittura rimosse.</span><span class="sxs-lookup"><span data-stu-id="e93ac-137">They can be modified in any way, or even removed, in the future.</span></span>

### <a name="import-with-maven"></a><span data-ttu-id="e93ac-138">Eseguire l'importazione con Maven</span><span class="sxs-lookup"><span data-stu-id="e93ac-138">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="e93ac-139">Ottenere supporto e inviare commenti</span><span class="sxs-lookup"><span data-stu-id="e93ac-139">Get help and give feedback</span></span>

<span data-ttu-id="e93ac-140">Per ottenere supporto per l'uso delle librerie nel codice, vedere la community di [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk).</span><span class="sxs-lookup"><span data-stu-id="e93ac-140">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="e93ac-141">Se si rilevano bug o se si vogliono inviare suggerimenti per il miglioramento delle librerie, inviare commenti tramite [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span><span class="sxs-lookup"><span data-stu-id="e93ac-141">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>


