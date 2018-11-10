---
title: Librerie di Archiviazione di Azure per Java
description: ''
keywords: Azure, Java, SDK, API, Archiviazione
author: douge
ms.author: seguler
manager: dineshm
ms.date: 10/29/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 68a5fdbf8e2fbfe83f9ea09112d64488e0c11d08
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235252"
---
# <a name="azure-storage-libraries-for-java"></a>Librerie di Archiviazione di Azure per Java

## <a name="overview"></a>Panoramica

Leggere e scrivere dati di BLOB (oggetto), file e messaggi dalle applicazioni Java con [Archiviazione di Azure](/azure/storage/storage-introduction).

Per iniziare a usare Archiviazione di Azure, vedere [Come usare l'archiviazione BLOB da Java SDK v7](/azure/storage/blobs/storage-quickstart-blobs-java).

## <a name="client-library"></a>Libreria client

Usare una chiave condivisa, un token di firma di accesso condiviso o un token OAuth di Azure Active Directory per autorizzare i servizi di Archiviazione di Azure. Usare quindi le classi e i metodi delle librerie client per interagire con BLOB, file o archiviazione code. 

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.   

**Dipendenza per Azure Storage SDK v7 legacy**:
```XML
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>8.0.0</version>
</dependency>
```

### <a name="example"></a>Esempio

Scrivere un file di immagine dal file system locale in un nuovo BLOB di un contenitore BLOB esistente di Archiviazione di Azure.


```java
// Parse the connection string and create a blob client to interact with Blob storage
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();
CloudBlobContainer container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());         
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

[Azure Storage SDK per Java](https://github.com/azure/azure-storage-java)
[Leggere e scrivere oggetti nell'archivio BLOB](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart)   
[Leggere e scrivere messaggi con le code](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
