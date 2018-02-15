---
title: Librerie di Archiviazione di Azure per Java
description: 
keywords: Azure, Java, SDK, API, Archiviazione
author: douge
ms.author: douge
manager: douge
ms.date: 02/07/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 3c7a3a1fcf2e97202e7f38f8df5acb6637fb4b47
ms.sourcegitcommit: 2ae0d99c02f4ad7efa9e3d3fbd1db7e9de20c6e7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2018
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="85004-103">Librerie di Archiviazione di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="85004-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="85004-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="85004-104">Overview</span></span>

<span data-ttu-id="85004-105">Leggere e scrivere file, dati di BLOB (oggetto), coppie chiave-valore e messaggi dalle applicazioni Java con l'[Archiviazione di Azure](/azure/storage/storage-introduction).</span><span class="sxs-lookup"><span data-stu-id="85004-105">Read and write files, blob (object) data, key-value pairs, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="85004-106">Per iniziare a usare l'Archiviazione di Azure, vedere [Come usare l'archiviazione BLOB da Java](/azure/storage/storage-java-how-to-use-blob-storage).</span><span class="sxs-lookup"><span data-stu-id="85004-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/storage-java-how-to-use-blob-storage).</span></span>

## <a name="client-library"></a><span data-ttu-id="85004-107">Libreria client</span><span class="sxs-lookup"><span data-stu-id="85004-107">Client library</span></span>

<span data-ttu-id="85004-108">Usare le [stringhe di connessione](/azure/storage/storage-create-storage-account#manage-your-storage-account) per connettersi a un account di archiviazione di Azure e quindi usare le classi e i metodi delle librerie client per usare l'archiviazione BLOB, tabelle, file o code.</span><span class="sxs-lookup"><span data-stu-id="85004-108">Use [connection strings](/azure/storage/storage-create-storage-account#manage-your-storage-account) to connect to an Azure Storage account, then use the client libraries' classes and methods to work with blob, table, file, or queue storage.</span></span> 

<span data-ttu-id="85004-109">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="85004-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="85004-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="85004-110">Example</span></span>

<span data-ttu-id="85004-111">Scrivere un file di immagine dal file system locale a un nuovo in un contenitore esistente del BLOB del servizio di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="85004-111">Write a image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
String storageConnectionString = "DefaultEndpointsProtocol=https;" + 
"AccountName=fabrikamblobstorage;" + 
"AccountKey=keyvalue;EndpointSuffix=core.windows.net";

CloudStorageAccount account = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient serviceClient = account.createCloudBlobClient();
CloudBlobContainer container = serviceClient.getContainerReference(blobContainer);

// write a blob from a local filesystem path to the container as logo.png
CloudBlockBlob blob = container.getBlockBlobReference("logo.png");
blob.uploadFromFile("/Users/raisa/fabrikam.png");
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="85004-112">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="85004-112">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/clientlibrary)

## <a name="management-api"></a><span data-ttu-id="85004-113">API di gestione</span><span class="sxs-lookup"><span data-stu-id="85004-113">Management API</span></span>

<span data-ttu-id="85004-114">Creare e gestire account di archiviazione di Azure e chiavi di connessione con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="85004-114">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="85004-115">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="85004-115">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="85004-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="85004-116">Example</span></span>

<span data-ttu-id="85004-117">Creare un nuovo account di archiviazione di Azure nella sottoscrizione e recuperare le rispettive chiavi di accesso.</span><span class="sxs-lookup"><span data-stu-id="85004-117">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

```java
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
        .withRegion(Region.US_EAST)
        .withNewResourceGroup(rgName)
        .create();

// get a list of storage account keys related to the account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="85004-118">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="85004-118">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/managementapi)


## <a name="samples"></a><span data-ttu-id="85004-119">Esempi</span><span class="sxs-lookup"><span data-stu-id="85004-119">Samples</span></span>

<span data-ttu-id="85004-120">[Gestire gli account di archiviazione di Azure](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span><span class="sxs-lookup"><span data-stu-id="85004-120">[Manage Azure Storage accounts](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span></span>  
<span data-ttu-id="85004-121">[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blob-java-getting-started)  (Leggere e scrivere oggetti nell'archiviazione BLOB)</span><span class="sxs-lookup"><span data-stu-id="85004-121">[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span></span>  
<span data-ttu-id="85004-122">[Read and write messages with queues](https://github.com/Azure-Samples/storage-queue-java-getting-started)  (Leggere e scrivere messaggi con le code)</span><span class="sxs-lookup"><span data-stu-id="85004-122">[Read and write messages with queues](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span></span>  
<span data-ttu-id="85004-123">[Read files from blob storage in a web app](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux) (Leggere file dall'archiviazione BLOB in un'app Web)</span><span class="sxs-lookup"><span data-stu-id="85004-123">[Read files from blob storage in a web app](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)</span></span>

<span data-ttu-id="85004-124">Esplorare altri [esempi di codice Java per Archiviazione di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=storage) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="85004-124">Explore more [sample Java code for Azure Storage](https://azure.microsoft.com/resources/samples/?platform=java&term=storage) you can use in your apps.</span></span>
