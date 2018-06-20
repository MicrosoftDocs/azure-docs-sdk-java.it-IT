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
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
ms.locfileid: "21930877"
---
# <a name="azure-libraries-for-java"></a>Librerie di Azure per Java

Le librerie di Azure per Java consentono di gestire le risorse di Azure e di connettersi ai servizi dal codice dell'applicazione. Le librerie sono disponibili come [importazioni da Maven](java-sdk-azure-install.md) per l'uso nei progetti Java. 

## <a name="manage-azure-resources"></a>Gestire le risorse di Azure

Creare e gestire le risorse di Azure dalle applicazioni Java usando le [librerie di gestione di Azure per Java](java-sdk-azure-get-started.md). Usare queste librerie per creare strumenti e servizi di automazione di Azure personalizzati. 

Ad esempio, per creare una VM Linux, scrivere il codice seguente:

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

Vedere le [istruzioni di installazione](java-sdk-azure-install.md) per ottenere un elenco completo delle librerie e informazioni sull'importazione delle librerie nei progetti, quindi leggere l'[articolo introduttivo](java-sdk-azure-get-started.md) per configurare l'autenticazione ed eseguire il codice di esempio nella propria sottoscrizione di Azure. 

## <a name="connect-to-azure-services"></a>Connettersi ai servizi di Azure

Oltre a usare le librerie Java per creare e gestire risorse in Azure, è possibile usarle per connettersi a tali risorse e usarle nelle app. È ad esempio possibile aggiornare una tabella di un database SQL o archiviare file in Archiviazione di Azure. Selezionare la libreria necessaria per un servizio specifico dall'[elenco completo di librerie](java-sdk-azure-install.md) e passare al [centro per sviluppatori Java](https://azure.microsoft.com/develop/java/) per ottenere esercitazioni e codice di esempio utili per scoprire come usare le librerie nelle app.

Per stampare, ad esempio, i contenuti di ogni BLOB in un contenitore di archiviazione di Azure:

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

## <a name="sample-code-and-reference"></a>Codice di esempio e informazioni di riferimento

Gli esempi seguenti sono relativi ad attività di automazione comuni con le librerie di gestione di Azure per Java e includono codice pronto per l'uso nelle app:

- [Macchine virtuali](java-sdk-azure-virtual-machine-samples.md)
- [App Web](java-sdk-azure-web-apps-samples.md)
- [Database SQL](java-sdk-azure-sql-database-samples.md)
   
Le [informazioni di riferimento](https://docs.microsoft.com/java/api) sono disponibili per tutti i pacchetti nelle librerie di gestione e di servizi. Le nuove funzionalità, le modifiche di rilievo e le istruzioni per la migrazione dalle versioni precedenti sono disponibili nelle [note sulla versione](java-sdk-azure-release-notes.md).