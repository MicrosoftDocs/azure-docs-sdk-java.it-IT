---
title: Gestire gli account di archiviazione di Azure con Java | Microsoft Docs
description: Codice di esempio per la gestione di account di archiviazione di Azure con Azure SDK per Java
author: rloutlaw
manager: douge
ms.assetid: 49be8b66-3b56-4c10-8f14-9d326d815cb4
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 620ee28691f70ed6cf29c4f7c169cd43a6e71351
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592576"
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a>Gestire gli account di archiviazione di Azure dalle applicazioni Java

[Questo esempio](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) crea un account di [archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e interagisce con le chiavi di accesso dell'account che usano le [librerie di gestione Java](https://github.com/Azure/azure-sdk-for-java). 

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Quindi eseguire:

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts).

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a>Creare un account di archiviazione

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

Il nome della risorsa di archiviazione fornito deve essere univoco in tutti i nomi di Azure e deve contenere solo lettere minuscole e numeri. Il profilo di prestazioni e replica predefinito usato per questo account è [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage).

## <a name="list-keys-in-a-storage-account"></a>Elencare le chiavi in un account di archiviazione
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

In ogni account di archiviazione di Azure vengono fornite due chiavi. È quindi possibile rigenerare una chiave consentendo al tempo stesso l'accesso alla risorsa di archiviazione usando l'altra chiave.

## <a name="regenerate-a-key-in-a-storage-account"></a>Rigenerare una chiave in un account di archiviazione

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

È necessario aggiornare tutte le risorse e le applicazioni di Azure con la nuova chiave dopo averne generata una nuova.

## <a name="list-all-storage-accounts-in-a-resource-group"></a>Elencare tutti gli account di archiviazione in un gruppo di risorse
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) fornisce un set di metodi utili per ispezionare la configurazione di un account di archiviazione.

## <a name="delete-a-storage-account"></a>Eliminare un account di archiviazione
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> È possibile che gli account di archiviazione con immagini del disco in uso connesse alle macchine virtuali o ai dischi in uso da altri elementi non vengano rimossi con questi metodi. Scollegare la risorsa di archiviazione da tali risorse prima della rimozione dell'account.

## <a name="sample-explanation"></a>Spiegazione dell'esempio

Il [codice di esempio su GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):

- Crea un account di archiviazione
- Legge e rigenera le chiavi di accesso
- Elenca tutti gli account di archiviazione in un gruppo di risorse
- Elimina l'account di archiviazione 

| Classe usata nell'esempio | Note
|-------|-------|
| [StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | Rappresentazione di un account di archiviazione di Azure. Usare i metodi nella classe per ottenere informazioni sull'account di archiviazione.
| [StorageAccountKey](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | `StorageAccount.getKeys()` restituisce le chiavi dell'account di archiviazione. Usare i metodi `regenerateKey` in `StorageAccount` per aggiornare le chiavi.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]