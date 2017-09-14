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
ms.openlocfilehash: ce0b003cc7c48c8690f4236547ddec36e6ea9d53
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2017
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="a05f6-103">Strumenti di Azure per gli sviluppatori Java</span><span class="sxs-lookup"><span data-stu-id="a05f6-103">Azure tools for Java developers</span></span>

## <a name="client-and-management-libraries"></a><span data-ttu-id="a05f6-104">Librerie client e di gestione</span><span class="sxs-lookup"><span data-stu-id="a05f6-104">Client and management libraries</span></span>

<span data-ttu-id="a05f6-105">Connettersi ai servizi e gestire le risorse di Azure dalle applicazioni con le librerie di Azure per Java.</span><span class="sxs-lookup"><span data-stu-id="a05f6-105">Connect to services and manage Azure resources from your applications with the Azure libraries for Java.</span></span> <span data-ttu-id="a05f6-106">Importare le librerie di gestione nei progetti Maven aggiungendo questa dipendenza al progetto *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="a05f6-106">Import the management libraries into your Maven projects by adding this dependency to your project *pom.xml*.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

<span data-ttu-id="a05f6-107">Visualizzare l'[elenco completo di librerie](java-sdk-azure-install.md) e [iniziare a usare](java-sdk-azure-get-started.md) le librerie di Azure per Java.</span><span class="sxs-lookup"><span data-stu-id="a05f6-107">View the [complete list of libraries](java-sdk-azure-install.md) and [get started](java-sdk-azure-get-started.md) with the Azure libraries for Java.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="a05f6-108">Plug-in di Eclipse e IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a05f6-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="a05f6-109">Gestire le risorse di Azure e distribuire app dall'IDE con i toolkit di Azure per [Eclipse](eclipse/azure-toolkit-for-eclipse.md) e [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span><span class="sxs-lookup"><span data-stu-id="a05f6-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![Toolkit per IntelliJ con Azure Explorer](media/intelliJ-azure-explorer.png)

<span data-ttu-id="a05f6-111">[Introduzione ad Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Introduzione ad Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="a05f6-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="azure-cli-20"></a><span data-ttu-id="a05f6-112">Interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="a05f6-112">Azure CLI 2.0</span></span>

<span data-ttu-id="a05f6-113">L'interfaccia della riga di comando di Azure 2.0 fornisce un'esperienza da riga di comando per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="a05f6-113">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="a05f6-114">È possibile usarla nel browser con [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) oppure [installarla](https://docs.microsoft.com/cli/azure/install-azure-cli) in macOS, Linux e Windows ed eseguirla dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="a05f6-114">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="a05f6-115">[Introduzione all'interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a05f6-115">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="a05f6-116">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="a05f6-116">Azure Storage Explorer</span></span> 

<span data-ttu-id="a05f6-117">Gestire gli account di archiviazione, i contenitori e i BLOB/file di Azure dal desktop.</span><span class="sxs-lookup"><span data-stu-id="a05f6-117">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="a05f6-118">Azure Storage Explorer è attualmente disponibile in anteprima per Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="a05f6-118">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="a05f6-119">Guida introduttiva ad Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="a05f6-119">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)