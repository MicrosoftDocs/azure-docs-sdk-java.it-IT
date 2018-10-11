---
title: Librerie di Azure per Java
description: Panoramica delle librerie di gestione e di servizi di Azure per Java
keywords: Azure, Java, SDK, API
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 9aaf22a2-382a-4b13-a8e3-0e467d607207
ms.openlocfilehash: 22a337e928085475e905a271f991147729d97671
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892732"
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="59383-104">Librerie di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="59383-104">Azure libraries for Java</span></span>

<span data-ttu-id="59383-105">Le librerie di Azure per Java consentono di gestire le risorse di Azure e di connettersi ai servizi dal codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="59383-105">The Azure libraries for Java help you manage Azure resources and connect to services from your application code.</span></span> <span data-ttu-id="59383-106">Le librerie sono disponibili come [importazioni da Maven](java-sdk-azure-install.md) per l'uso nei progetti Java.</span><span class="sxs-lookup"><span data-stu-id="59383-106">The libraries are available as [Maven imports](java-sdk-azure-install.md) for use in your Java projects.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="59383-107">Gestire le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="59383-107">Manage Azure resources</span></span>

<span data-ttu-id="59383-108">Creare e gestire le risorse di Azure dalle applicazioni Java usando le [librerie di gestione di Azure per Java](java-sdk-azure-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="59383-108">Create and manage Azure resources from your Java applications using the [Azure management libraries for Java](java-sdk-azure-get-started.md).</span></span> <span data-ttu-id="59383-109">Usare queste librerie per creare strumenti e servizi di automazione di Azure personalizzati.</span><span class="sxs-lookup"><span data-stu-id="59383-109">Use these libraries to build your own Azure automation tools and services.</span></span> 

<span data-ttu-id="59383-110">Ad esempio, per creare una VM Linux, scrivere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="59383-110">For example, to create a Linux VM you would write the following code:</span></span>

```java
VirtualMachine linuxVM = azure.virtualMachines().define("myAzureVM")
           .withRegion(region)
           .withExistingResourceGroup("sampleResourceGroup")
           .withNewPrimaryNetwork("10.0.0.0/28")
           .withPrimaryPrivateIpAddressDynamic()
           .withoutPrimaryPublicIpAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withSsh(key)
           .withUnmanagedStorage()
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
 ```

<span data-ttu-id="59383-111">Vedere le [istruzioni di installazione](java-sdk-azure-install.md) per ottenere un elenco completo delle librerie e informazioni sull'importazione delle librerie nei progetti, quindi leggere l'[articolo introduttivo](java-sdk-azure-get-started.md) per configurare l'autenticazione ed eseguire il codice di esempio nella propria sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="59383-111">Review the [install instructions](java-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](java-sdk-azure-get-started.md) to set up your authentication and run sample code against your own Azure subscription.</span></span> 

## <a name="connect-to-azure-services"></a><span data-ttu-id="59383-112">Connettersi ai servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="59383-112">Connect to Azure services</span></span>

<span data-ttu-id="59383-113">Oltre a usare le librerie Java per creare e gestire risorse in Azure, è possibile usarle per connettersi a tali risorse e usarle nelle app.</span><span class="sxs-lookup"><span data-stu-id="59383-113">In addition to using Java libraries to create and manage resources within Azure, you can also use Java libraries to connect  and use those resources in your apps.</span></span> <span data-ttu-id="59383-114">È ad esempio possibile aggiornare una tabella di un database SQL o archiviare file in Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="59383-114">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="59383-115">Selezionare la libreria necessaria per un servizio specifico dall'[elenco completo di librerie](java-sdk-azure-install.md) e passare al [centro per sviluppatori Java](https://azure.microsoft.com/develop/java/) per ottenere esercitazioni e codice di esempio utili per scoprire come usare le librerie nelle app.</span><span class="sxs-lookup"><span data-stu-id="59383-115">Select the library you need for a particular service from the [complete list of libraries](java-sdk-azure-install.md) and visit the [Java developer center](https://azure.microsoft.com/develop/java/) for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="59383-116">Per stampare, ad esempio, i contenuti di ogni BLOB in un contenitore di archiviazione di Azure:</span><span class="sxs-lookup"><span data-stu-id="59383-116">For example, to print out the contents of every blob in an Azure storage container:</span></span>

```java
// get the container from the blob client
CloudBlobContainer container = blobClient.getContainerReference("blobcontainer");

// For each item in the container
for (ListBlobItem blobItem : container.listBlobs()) {
    // If the item is a blob, not a virtual directory
    if (blobItem instanceof CloudBlockBlob) {
        // Download the text
        CloudBlockBlob retrievedBlob = (CloudBlockBlob) blobItem;
        System.out.println(retrievedBlob.downloadText());
    }
}
```

## <a name="sample-code-and-reference"></a><span data-ttu-id="59383-117">Codice di esempio e informazioni di riferimento</span><span class="sxs-lookup"><span data-stu-id="59383-117">Sample code and reference</span></span>

<span data-ttu-id="59383-118">Gli esempi seguenti sono relativi ad attività di automazione comuni con le librerie di gestione di Azure per Java e includono codice pronto per l'uso nelle app:</span><span class="sxs-lookup"><span data-stu-id="59383-118">The following samples cover common automation tasks with the Azure management libraries for Java and have code ready to use in your own apps:</span></span>

- [<span data-ttu-id="59383-119">Macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="59383-119">Virtual machines</span></span>](java-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="59383-120">App Web</span><span class="sxs-lookup"><span data-stu-id="59383-120">Web apps</span></span>](java-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="59383-121">Database SQL</span><span class="sxs-lookup"><span data-stu-id="59383-121">SQL Database</span></span>](java-sdk-azure-sql-database-samples.md)
   
<span data-ttu-id="59383-122">Le [informazioni di riferimento](https://docs.microsoft.com/java/api) sono disponibili per tutti i pacchetti nelle librerie di gestione e di servizi.</span><span class="sxs-lookup"><span data-stu-id="59383-122">A [reference](https://docs.microsoft.com/java/api) is available for all packages in both the service and management libraries.</span></span> <span data-ttu-id="59383-123">Le nuove funzionalità, le modifiche di rilievo e le istruzioni per la migrazione dalle versioni precedenti sono disponibili nelle [note sulla versione](java-sdk-azure-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="59383-123">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](java-sdk-azure-release-notes.md).</span></span>