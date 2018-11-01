---
title: Librerie di Archiviazione di Azure per Java
description: ''
keywords: Azure, Java, SDK, API, Archiviazione
author: douge
ms.author: douge
manager: douge
ms.date: 10/19/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: fba48dfa04f223dce72a0ee54da967565ebd3687
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235262"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="9a319-103">Librerie di Archiviazione di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="9a319-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="9a319-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9a319-104">Overview</span></span>

<span data-ttu-id="9a319-105">Leggere e scrivere dati di BLOB (oggetto), file e messaggi dalle applicazioni Java con [Archiviazione di Azure](/azure/storage/storage-introduction).</span><span class="sxs-lookup"><span data-stu-id="9a319-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="9a319-106">Per iniziare a usare l'Archiviazione di Azure, vedere [Come usare l'archiviazione BLOB da Java](/azure/storage/blobs/storage-quickstart-blobs-java-v10).</span><span class="sxs-lookup"><span data-stu-id="9a319-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/blobs/storage-quickstart-blobs-java-v10).</span></span>

## <a name="client-library"></a><span data-ttu-id="9a319-107">Libreria client</span><span class="sxs-lookup"><span data-stu-id="9a319-107">Client library</span></span>

<span data-ttu-id="9a319-108">Usare una chiave condivisa, un token di firma di accesso condiviso o un token OAuth di Azure Active Directory per autorizzare i servizi di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a319-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="9a319-109">Usare quindi le classi e i metodi delle librerie client per interagire con BLOB, file o archiviazione code.</span><span class="sxs-lookup"><span data-stu-id="9a319-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="9a319-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9a319-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="9a319-111">**Dipendenza per il servizio BLOB**:</span><span class="sxs-lookup"><span data-stu-id="9a319-111">**Dependency for Blob service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

<span data-ttu-id="9a319-112">**Dipendenza per il servizio di accodamento**:</span><span class="sxs-lookup"><span data-stu-id="9a319-112">**Dependency for Queue service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a><span data-ttu-id="9a319-113">Esempio</span><span class="sxs-lookup"><span data-stu-id="9a319-113">Example</span></span>

<span data-ttu-id="9a319-114">Scrivere un file di immagine dal file system locale in un nuovo BLOB di un contenitore BLOB esistente di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a319-114">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
// Retrieve the credentials and initialize SharedKeyCredentials
String accountName = System.getenv("AZURE_STORAGE_ACCOUNT");
String accountKey = System.getenv("AZURE_STORAGE_ACCESS_KEY");

// Create a BlockBlobURL to run operations on Block Blobs. Alternatively create a ServiceURL, or ContainerURL for operations on Blob service, and Blob containers
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);

// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final BlockBlobURL blobURL = new BlockBlobURL(
    new URL("https://" + accountName + ".blob.core.windows.net/mycontainer/myimage.jpg"), 
        StorageURL.createPipeline(creds, new PipelineOptions())
);

AsynchronousFileChannel fileChannel = AsynchronousFileChannel.open(Paths.get("myimage.jpg"));
TransferManager.uploadFileToBlockBlob(fileChannel, blobURL,0, null).blockingGet();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9a319-115">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="9a319-115">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="9a319-116">API di gestione</span><span class="sxs-lookup"><span data-stu-id="9a319-116">Management API</span></span>

<span data-ttu-id="9a319-117">Creare e gestire account di archiviazione di Azure e chiavi di connessione con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="9a319-117">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="9a319-118">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9a319-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="9a319-119">Esempio</span><span class="sxs-lookup"><span data-stu-id="9a319-119">Example</span></span>

<span data-ttu-id="9a319-120">Creare un nuovo account di archiviazione di Azure nella sottoscrizione e recuperare le rispettive chiavi di accesso.</span><span class="sxs-lookup"><span data-stu-id="9a319-120">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="9a319-121">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="9a319-121">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="9a319-122">Esempi</span><span class="sxs-lookup"><span data-stu-id="9a319-122">Samples</span></span>

<span data-ttu-id="9a319-123">[Azure Storage SDK per Java](https://github.com/azure/azure-storage-java)
[Leggere e scrivere oggetti nell'archivio BLOB](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="9a319-123">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="9a319-124">Leggere e scrivere messaggi con le code</span><span class="sxs-lookup"><span data-stu-id="9a319-124">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
