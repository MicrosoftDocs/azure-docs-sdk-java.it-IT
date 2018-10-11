---
title: Librerie di Archiviazione di Azure per Java
description: ''
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
ms.openlocfilehash: ec06e79374176b5a4795d27c5fbbb2260e65cd8c
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893592"
---
# <a name="azure-storage-libraries-for-java"></a>Librerie di Archiviazione di Azure per Java

## <a name="overview"></a>Panoramica

Leggere e scrivere file, dati di BLOB (oggetto), coppie chiave-valore e messaggi dalle applicazioni Java con l'[Archiviazione di Azure](/azure/storage/storage-introduction).

Per iniziare a usare l'Archiviazione di Azure, vedere [Come usare l'archiviazione BLOB da Java](/azure/storage/storage-java-how-to-use-blob-storage).

## <a name="client-library"></a>Libreria client

Usare le [stringhe di connessione](/azure/storage/storage-create-storage-account#manage-your-storage-account) per connettersi a un account di archiviazione di Azure e quindi usare le classi e i metodi delle librerie client per usare l'archiviazione BLOB, tabelle, file o code. 

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a>Esempio

Scrivere un file di immagine dal file system locale a un nuovo in un contenitore esistente del BLOB del servizio di archiviazione di Azure.


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
> [Esplorare le API client](/java/api/overview/azure/storage/client)

## <a name="management-api"></a>API di gestione

Creare e gestire account di archiviazione di Azure e chiavi di connessione con l'API di gestione.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a>Esempio

Creare un nuovo account di archiviazione di Azure nella sottoscrizione e recuperare le rispettive chiavi di accesso.

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
> [Esplorare le API di gestione](/java/api/overview/azure/storage/management)


## <a name="samples"></a>Esempi

[Gestire gli account di archiviazione di Azure](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)    
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blob-java-getting-started)  (Leggere e scrivere oggetti nell'archiviazione BLOB)  
[Read and write messages with queues](https://github.com/Azure-Samples/storage-queue-java-getting-started)  (Leggere e scrivere messaggi con le code)  
[Read files from blob storage in a web app](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux) (Leggere file dall'archiviazione BLOB in un'app Web)

Esplorare altri [esempi di codice Java per Archiviazione di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=storage) disponibili per l'uso nelle app.
