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
ms.openlocfilehash: 5945164b2b04e1fa9169590a71f6c5f9f45842d6
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931057"
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a><span data-ttu-id="ed572-103">Gestire gli account di archiviazione di Azure dalle applicazioni Java</span><span class="sxs-lookup"><span data-stu-id="ed572-103">Manage Azure storage accounts from your Java applications</span></span>

<span data-ttu-id="ed572-104">[Questo esempio](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) crea un account di [archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e interagisce con le chiavi di accesso dell'account che usano le [librerie di gestione Java](https://github.com/Azure/azure-sdk-for-java).</span><span class="sxs-lookup"><span data-stu-id="ed572-104">[This sample](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) creates an [Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction) account and works with the account access keys using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="ed572-105">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="ed572-105">Run the sample</span></span>

<span data-ttu-id="ed572-106">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="ed572-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="ed572-107">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="ed572-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

<span data-ttu-id="ed572-108">Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts).</span><span class="sxs-lookup"><span data-stu-id="ed572-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="ed572-109">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="ed572-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a><span data-ttu-id="ed572-110">Creare un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ed572-110">Create a storage account</span></span>

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

<span data-ttu-id="ed572-111">Il nome della risorsa di archiviazione fornito deve essere univoco in tutti i nomi di Azure e deve contenere solo lettere minuscole e numeri.</span><span class="sxs-lookup"><span data-stu-id="ed572-111">The storage name provided must be unique across all names in Azure and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="ed572-112">Il profilo di prestazioni e replica predefinito usato per questo account è [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="ed572-112">The default performance and replication profile used for this account is [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage).</span></span>

## <a name="list-keys-in-a-storage-account"></a><span data-ttu-id="ed572-113">Elencare le chiavi in un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ed572-113">List keys in a storage account</span></span>
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

<span data-ttu-id="ed572-114">In ogni account di archiviazione di Azure vengono fornite due chiavi. È quindi possibile rigenerare una chiave consentendo al tempo stesso l'accesso alla risorsa di archiviazione usando l'altra chiave.</span><span class="sxs-lookup"><span data-stu-id="ed572-114">Two keys are provided in each Azure storage account so that you can regenerate one key while still allowing access to storage using the other key.</span></span>

## <a name="regenerate-a-key-in-a-storage-account"></a><span data-ttu-id="ed572-115">Rigenerare una chiave in un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ed572-115">Regenerate a key in a storage account</span></span>

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

<span data-ttu-id="ed572-116">È necessario aggiornare tutte le risorse e le applicazioni di Azure con la nuova chiave dopo averne generata una nuova.</span><span class="sxs-lookup"><span data-stu-id="ed572-116">You must update all Azure resources and applications with the new key after generating a new one.</span></span>

## <a name="list-all-storage-accounts-in-a-resource-group"></a><span data-ttu-id="ed572-117">Elencare tutti gli account di archiviazione in un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="ed572-117">List all storage accounts in a resource group</span></span>
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

<span data-ttu-id="ed572-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) fornisce un set di metodi utili per ispezionare la configurazione di un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ed572-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) provides a set of useful methods to inspect the configuration of a storage account.</span></span>

## <a name="delete-a-storage-account"></a><span data-ttu-id="ed572-119">Eliminare un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ed572-119">Delete a storage account</span></span>
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> <span data-ttu-id="ed572-120">È possibile che gli account di archiviazione con immagini del disco in uso connesse alle macchine virtuali o ai dischi in uso da altri elementi non vengano rimossi con questi metodi.</span><span class="sxs-lookup"><span data-stu-id="ed572-120">Storage accounts with in-use disk images connected to virtual machines or disks in use by other artifacts may not remove with these methods.</span></span> <span data-ttu-id="ed572-121">Scollegare la risorsa di archiviazione da tali risorse prima della rimozione dell'account.</span><span class="sxs-lookup"><span data-stu-id="ed572-121">Detach the storage from these resources before removing the account.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="ed572-122">Spiegazione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="ed572-122">Sample explanation</span></span>

<span data-ttu-id="ed572-123">Il [codice di esempio su GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):</span><span class="sxs-lookup"><span data-stu-id="ed572-123">[The sample code on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):</span></span>

- <span data-ttu-id="ed572-124">Crea un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ed572-124">creates a storage account</span></span>
- <span data-ttu-id="ed572-125">Legge e rigenera le chiavi di accesso</span><span class="sxs-lookup"><span data-stu-id="ed572-125">reads and regenerates access keys</span></span>
- <span data-ttu-id="ed572-126">Elenca tutti gli account di archiviazione in un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="ed572-126">lists all storage accounts in a resource group</span></span>
- <span data-ttu-id="ed572-127">Elimina l'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ed572-127">deletes the storage account</span></span> 

| <span data-ttu-id="ed572-128">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="ed572-128">Class used in sample</span></span> | <span data-ttu-id="ed572-129">Note</span><span class="sxs-lookup"><span data-stu-id="ed572-129">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="ed572-130">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="ed572-130">StorageAccount</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | <span data-ttu-id="ed572-131">Rappresentazione di un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ed572-131">Representation of an Azure storage account.</span></span> <span data-ttu-id="ed572-132">Usare i metodi nella classe per ottenere informazioni sull'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ed572-132">Use the methods in the class to get information about the storage account.</span></span>
| [<span data-ttu-id="ed572-133">StorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="ed572-133">StorageAccountKey</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | <span data-ttu-id="ed572-134">`StorageAccount.getKeys()` restituisce le chiavi dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ed572-134">`StorageAccount.getKeys()` returns the storage account keys.</span></span> <span data-ttu-id="ed572-135">Usare i metodi `regenerateKey` in `StorageAccount` per aggiornare le chiavi.</span><span class="sxs-lookup"><span data-stu-id="ed572-135">Use the `regenerateKey` methods in `StorageAccount` to update the keys.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed572-136">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="ed572-136">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]