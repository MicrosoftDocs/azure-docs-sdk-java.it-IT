---
title: Strumenti per sviluppatori Java per Azure | Microsoft Docs
description: Integrazioni dell'IDE, emulatori, strumenti per l'esplorazione delle risorse e interfacce della riga di comando per gli sviluppatori Java in Azure.
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 01fe31f2c59810f972875331d49ce5130755c8f2
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-tools-for-java-developers"></a>Strumenti di Azure per gli sviluppatori Java

## <a name="client-and-management-libraries"></a>Librerie client e di gestione

Connettersi ai servizi e gestire le risorse di Azure dalle applicazioni con le librerie di Azure per Java. Importare le librerie di gestione nei progetti Maven aggiungendo questa dipendenza al progetto *pom.xml*.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

Visualizzare l'[elenco completo di librerie](java-sdk-azure-install.md) e [iniziare a usare](java-sdk-azure-get-started.md) le librerie di Azure per Java.

## <a name="eclipse-and-intellij-plugins"></a>Plug-in di Eclipse e IntelliJ

Gestire le risorse di Azure e distribuire app dall'IDE con i toolkit di Azure per [Eclipse](https://docs.microsoft.com/azure/azure-toolkit-for-eclipse) e [IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij).   

![Toolkit per IntelliJ con Azure Explorer](media/intelliJ-azure-explorer.png)

[Introduzione ad Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Introduzione ad Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app) 

## <a name="azure-cli-20"></a>Interfaccia della riga di comando di Azure 2.0

L'interfaccia della riga di comando di Azure 2.0 fornisce un'esperienza da riga di comando per la gestione delle risorse di Azure. È possibile usarla nel browser con [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) oppure [installarla](https://docs.microsoft.com/cli/azure/install-azure-cli) in macOS, Linux e Windows ed eseguirla dalla riga di comando.

[Introduzione all'interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

## <a name="azure-storage-explorer"></a>Azure Storage Explorer 

Gestire gli account di archiviazione, i contenitori e i BLOB/file di Azure dal desktop. Azure Storage Explorer è attualmente disponibile in anteprima per Windows, macOS e Linux.

[Guida introduttiva ad Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)