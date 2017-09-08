---
title: Librerie di Azure Batch per Java
description: Documentazione di riferimento per le librerie Batch per Java
keywords: Azure, Java, SDK, API, Batch, elaborazione, pianificazione, a esecuzione prolungata
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: batch
ms.openlocfilehash: dfc14b08ee39a1422cb54a3b9a80fa579e704499
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-batch-libraries-for-java"></a><span data-ttu-id="d47da-104">Librerie di Azure Batch per Java</span><span class="sxs-lookup"><span data-stu-id="d47da-104">Azure Batch libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d47da-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d47da-105">Overview</span></span>

<span data-ttu-id="d47da-106">Eseguire in modo efficiente applicazioni di calcolo a prestazioni elevate su larga scala nel cloud con [Azure Batch](/azure/batch/batch-technical-overview).</span><span class="sxs-lookup"><span data-stu-id="d47da-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>   

<span data-ttu-id="d47da-107">Per iniziare a usare Azure Batch, vedere [Creare un account Batch nel portale di Azure](/azure/batch/batch-account-create-portal).</span><span class="sxs-lookup"><span data-stu-id="d47da-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="client-library"></a><span data-ttu-id="d47da-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="d47da-108">Client library</span></span>

<span data-ttu-id="d47da-109">Le librerie client di Azure Batch consentono di configurare i nodi e i pool di calcolo, definire le attività e configurarle per l'esecuzione nei processi e infine configurare un gestore di processi per controllare e monitorare l'esecuzione dei processi.</span><span class="sxs-lookup"><span data-stu-id="d47da-109">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="d47da-110">[Altre informazioni](/azure/batch/batch-api-basics) sull'uso di questi oggetti per l'esecuzione di soluzioni di calcolo parallele su larga scala.</span><span class="sxs-lookup"><span data-stu-id="d47da-110">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

<span data-ttu-id="d47da-111">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="d47da-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="d47da-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="d47da-112">Example</span></span>

<span data-ttu-id="d47da-113">Configurare un pool di nodi di calcolo di Linux in un account Batch:</span><span class="sxs-lookup"><span data-stu-id="d47da-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d47da-114">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="d47da-114">Explore the Client APIs</span></span>](/java/api/overview/azure/batch/clientlibrary)


## <a name="management-api"></a><span data-ttu-id="d47da-115">API di gestione</span><span class="sxs-lookup"><span data-stu-id="d47da-115">Management API</span></span>

<span data-ttu-id="d47da-116">Usare le librerie di gestione di Azure Batch per creare ed eliminare account Batch, leggere e rigenerare chiavi di account Batch e gestire l'archiviazione di account Batch.</span><span class="sxs-lookup"><span data-stu-id="d47da-116">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

<span data-ttu-id="d47da-117">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="d47da-117">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="d47da-118">Esempio</span><span class="sxs-lookup"><span data-stu-id="d47da-118">Example</span></span>

<span data-ttu-id="d47da-119">Creare un account Azure Batch e configurare una nuova applicazione e un account di archiviazione di Azure corrispondente.</span><span class="sxs-lookup"><span data-stu-id="d47da-119">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```java
BatchAccount batchAccount = azure.batchAccounts().define("newBatchAcct")
    .withRegion(Region.US_EAST)
    .withNewResourceGroup("myResourceGroup")
    .defineNewApplication("batchAppName")
        .defineNewApplicationPackage(applicationPackageName)
        .withAllowUpdates(true)
        .withDisplayName(applicationDisplayName)
        .attach()
    .withNewStorageAccount("batchStorageAcct")
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d47da-120">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="d47da-120">Explore the Management APIs</span></span>](/java/api/overview/azure/batch/managementapi)


## <a name="samples"></a><span data-ttu-id="d47da-121">Esempi</span><span class="sxs-lookup"><span data-stu-id="d47da-121">Samples</span></span>

<span data-ttu-id="d47da-122">[Gestire gli account Batch][1]</span><span class="sxs-lookup"><span data-stu-id="d47da-122">[Manage Batch accounts][1]</span></span>   

<span data-ttu-id="d47da-123">Esplorare altri [esempi di codice Java per Azure Batch](https://azure.microsoft.com/resources/samples/?platform=java&term=batch) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="d47da-123">Explore more [sample Java code for Azure Batch](https://azure.microsoft.com/resources/samples/?platform=java&term=batch) you can use in your apps.</span></span>

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts