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
ms.sourcegitcommit: 67b3542b174e8448f9ca3e7c9506f1216ea6a8fe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285667"
---
# <a name="azure-storage-libraries-for-java"></a>Librerie di Archiviazione di Azure per Java

## <a name="overview"></a>Panoramica

Leggere e scrivere dati di BLOB (oggetto), file e messaggi dalle applicazioni Java con [Archiviazione di Azure](/azure/storage/storage-introduction).

Per iniziare a usare l'Archiviazione di Azure, vedere [Come usare l'archiviazione BLOB da Java](/azure/storage/blobs/storage-quickstart-blobs-java-v10).

## <a name="client-library"></a>Libreria client

Usare una chiave condivisa, un token di firma di accesso condiviso o un token OAuth di Azure Active Directory per autorizzare i servizi di Archiviazione di Azure. Usare quindi le classi e i metodi delle librerie client per interagire con BLOB, file o archiviazione code. 

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.   

**Dipendenza per il servizio BLOB**:
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

**Dipendenza per il servizio di accodamento**:
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a>Esempio

Scrivere un file di immagine dal file system locale in un nuovo BLOB di un contenitore BLOB esistente di Archiviazione di Azure.


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
