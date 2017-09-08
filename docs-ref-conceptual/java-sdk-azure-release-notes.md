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
# <a name="release-notes"></a><span data-ttu-id="1d10f-104">Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="1d10f-104">Release Notes</span></span> 

## <a name="june-30-2017---110"></a><span data-ttu-id="1d10f-105">30 giugno 2017 - 1.1.0</span><span class="sxs-lookup"><span data-stu-id="1d10f-105">June 30, 2017 - 1.1.0</span></span> 

<span data-ttu-id="1d10f-106">La versione 1.1 è compatibile con la versione 1.0 nelle API destinate all'uso pubblico che hanno raggiunto la fase di disponibilità generale (stabile) nella versione 1.0.</span><span class="sxs-lookup"><span data-stu-id="1d10f-106">V1.1 is backwards compatible with V1.0 in the APIs intended for public use that reached the general availability (stable) stage in V1.0.</span></span>

<span data-ttu-id="1d10f-107">Alcune modifiche di rilievo sono state introdotte nelle API contrassegnate con l'annotazione @Beta nella versione 1.0</span><span class="sxs-lookup"><span data-stu-id="1d10f-107">Some breaking changes were introduced in APIs marked with the @Beta annotation in V.0</span></span>

<span data-ttu-id="1d10f-108">Se si esegue la migrazione del codice alla versione 1.1.0, è possibile usare [queste note](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) per la preparazione del codice per la versione 1.1.0 dalla versione 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1d10f-108">If you are migrating your code to 1.1.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) for preparing your code for 1.1.0 from 1.0.0.</span></span>

### <a name="generally-availabile-in-v11"></a><span data-ttu-id="1d10f-109">Disponibile a livello generale nella versione 1.1</span><span class="sxs-lookup"><span data-stu-id="1d10f-109">Generally availabile in V1.1</span></span>

<span data-ttu-id="1d10f-110">Alcune delle API disponibili come Beta nella versione 1.0 sono ora disponibili a livello generale nella versione 1.1, in particolare:</span><span class="sxs-lookup"><span data-stu-id="1d10f-110">Some of the APIs that were still in Beta in V1.0 are now GA in V1.1, in particular:</span></span>

- <span data-ttu-id="1d10f-111">Metodi asincroni</span><span class="sxs-lookup"><span data-stu-id="1d10f-111">async methods</span></span>
- <span data-ttu-id="1d10f-112">Tutti i metodi della rete CDN disponibili in precedenza come versione Beta</span><span class="sxs-lookup"><span data-stu-id="1d10f-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="1d10f-113">Tutti i metodi e tutte le interfacce nei gateway applicazione disponibili in precedenza come versione Beta</span><span class="sxs-lookup"><span data-stu-id="1d10f-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

 <span data-ttu-id="1d10f-114">Alcune parti della libreria sono ancora disponibili in anteprima.</span><span class="sxs-lookup"><span data-stu-id="1d10f-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="1d10f-115">Per informazioni sullo stato attuale delle librerie, vedere la tabella seguente:</span><span class="sxs-lookup"><span data-stu-id="1d10f-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="1d10f-116">Servizio o funzionalità</span><span class="sxs-lookup"><span data-stu-id="1d10f-116">Service or feature</span></span> | <span data-ttu-id="1d10f-117">Disponibile a livello generale</span><span class="sxs-lookup"><span data-stu-id="1d10f-117">Available as GA</span></span> | <span data-ttu-id="1d10f-118">Disponibile come anteprima</span><span class="sxs-lookup"><span data-stu-id="1d10f-118">Available as Preview</span></span>  | <span data-ttu-id="1d10f-119">Presto disponibile</span><span class="sxs-lookup"><span data-stu-id="1d10f-119">Coming soon</span></span> |
---------|---------|---------|---------|
<span data-ttu-id="1d10f-120">Calcolo</span><span class="sxs-lookup"><span data-stu-id="1d10f-120">Compute</span></span>  | <span data-ttu-id="1d10f-121">Macchine virtuali ed estensioni di VM, set di scalabilità di macchine virtuali, Managed Disks</span><span class="sxs-lookup"><span data-stu-id="1d10f-121">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="1d10f-122">Servizio contenitore di Azure, Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="1d10f-122">Azure container service, Azure container registry</span></span> |    |
<span data-ttu-id="1d10f-123">Archiviazione</span><span class="sxs-lookup"><span data-stu-id="1d10f-123">Storage</span></span>   |  <span data-ttu-id="1d10f-124">Account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="1d10f-124">Storage accounts</span></span>       |         |   <span data-ttu-id="1d10f-125">Crittografia</span><span class="sxs-lookup"><span data-stu-id="1d10f-125">Encryption</span></span>      |
<span data-ttu-id="1d10f-126">Database SQL</span><span class="sxs-lookup"><span data-stu-id="1d10f-126">SQL Database</span></span>  | <span data-ttu-id="1d10f-127">Database, firewall, pool elastici</span><span class="sxs-lookup"><span data-stu-id="1d10f-127">Databases, firewalls, elastic pools</span></span>        |         |   <span data-ttu-id="1d10f-128">Altre funzionalità</span><span class="sxs-lookup"><span data-stu-id="1d10f-128">More features</span></span>      |
<span data-ttu-id="1d10f-129">Rete</span><span class="sxs-lookup"><span data-stu-id="1d10f-129">Networking</span></span>    |  <span data-ttu-id="1d10f-130">Reti virtuali, interfacce di rete, indirizzi IP, tabelle di routing, gruppi di sicurezza di rete, DNS, strumenti di gestione traffico gateway, applicazione</span><span class="sxs-lookup"><span data-stu-id="1d10f-130">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="1d10f-131">Servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="1d10f-131">Load balancers</span></span>     |   <span data-ttu-id="1d10f-132">VPN, watcher di rete</span><span class="sxs-lookup"><span data-stu-id="1d10f-132">VPN, Network watchers</span></span>   |
<span data-ttu-id="1d10f-133">Altri servizi</span><span class="sxs-lookup"><span data-stu-id="1d10f-133">More services</span></span>    |  <span data-ttu-id="1d10f-134">Resource Manager, Key Vault, Redis, rete CDN, Batch</span><span class="sxs-lookup"><span data-stu-id="1d10f-134">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="1d10f-135">App Web, app per le funzioni, bus di servizio, Controllo degli accessi in base al ruolo per Graph, DocumentDB</span><span class="sxs-lookup"><span data-stu-id="1d10f-135">Web apps, Function apps, Service Bus, Graph RBAC, DocumentDB</span></span>   | <span data-ttu-id="1d10f-136">Monitoraggio, Utilità di pianificazione, gestione delle funzioni, Ricerca, altre funzionalità di Controllo degli accessi in base al ruolo per Graph</span><span class="sxs-lookup"><span data-stu-id="1d10f-136">Monitor ,Scheduler, Functions management, Search, more Graph RBAC features</span></span>        |
<span data-ttu-id="1d10f-137">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="1d10f-137">Fundamentals</span></span>     |   <span data-ttu-id="1d10f-138">Autenticazione, core, metodi asincroni</span><span class="sxs-lookup"><span data-stu-id="1d10f-138">Authentication - core , Async methods</span></span>       |      |         |

### <a name="import-with-maven"></a><span data-ttu-id="1d10f-139">Eseguire l'importazione con Maven</span><span class="sxs-lookup"><span data-stu-id="1d10f-139">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="1d10f-140">Ottenere supporto e inviare commenti</span><span class="sxs-lookup"><span data-stu-id="1d10f-140">Get help and give feedback</span></span>

<span data-ttu-id="1d10f-141">Per ottenere supporto per l'uso delle librerie nel codice, vedere la community di [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk).</span><span class="sxs-lookup"><span data-stu-id="1d10f-141">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="1d10f-142">Se si rilevano bug o se si vogliono inviare suggerimenti per il miglioramento delle librerie, inviare commenti tramite [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span><span class="sxs-lookup"><span data-stu-id="1d10f-142">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>

### <a name="migrate-from-previous-releases"></a><span data-ttu-id="1d10f-143">Eseguire la migrazione da versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="1d10f-143">Migrate from previous releases</span></span>

[<span data-ttu-id="1d10f-144">Migrate from 1.0.0-beta5](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md) (Eseguire la migrazione da 1.0.0-beta5) [Migrate from 1.1.0</span><span class="sxs-lookup"><span data-stu-id="1d10f-144">Migrate from 1.0.0-beta5](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [Migrate from 1.1.0</span></span>](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) (Eseguire la migrazione da 1.1.0)


