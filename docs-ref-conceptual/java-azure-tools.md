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
ms.date: 11/13/2018
ms.author: routlaw
ms.openlocfilehash: 9e9828540ddc678f69eab11f5a61eef8bf8e916c
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339015"
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="055f6-103">Strumenti di Azure per gli sviluppatori Java</span><span class="sxs-lookup"><span data-stu-id="055f6-103">Azure tools for Java developers</span></span>

## <a name="supported-jdk-runtimes"></a><span data-ttu-id="055f6-104">Runtime JDK supportati</span><span class="sxs-lookup"><span data-stu-id="055f6-104">Supported JDK runtimes</span></span>

<span data-ttu-id="055f6-105">Gli sviluppatori Java in Azure e Azure Stack possono compilare ed eseguire applicazioni Java 7, 8 e 11 di produzione usando le [build Zulu Enterprise di OpenJDK di Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) senza incorrere in costi di supporto aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="055f6-105">Java developers on Azure and Azure Stack can build and run production Java 7,8, and 11 applications using [Azul Systems Zulu Enterprise builds of OpenJDK](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="055f6-106">Se si eseguono attualmente app Java con altri JDK, considerare la possibilità di eseguire la migrazione a Zulu in Azure per ottenere supporto e manutenzione gratuiti.</span><span class="sxs-lookup"><span data-stu-id="055f6-106">If you’re currently running Java apps with other JDKs, consider migrating to Zulu on Azure for free support and maintenance.</span></span> 

<span data-ttu-id="055f6-107">Per altre informazioni sulle versioni di JDK supportate e utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="055f6-107">For more information about the supported JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="055f6-108">Plug-in di Eclipse e IntelliJ</span><span class="sxs-lookup"><span data-stu-id="055f6-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="055f6-109">Gestire le risorse di Azure e distribuire app dall'IDE con i toolkit di Azure per [Eclipse](eclipse/azure-toolkit-for-eclipse.md) e [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span><span class="sxs-lookup"><span data-stu-id="055f6-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![Toolkit per IntelliJ con Azure Explorer](media/intelliJ-azure-explorer.png)

<span data-ttu-id="055f6-111">[Introduzione ad Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Introduzione ad Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="055f6-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="visual-studio-code"></a><span data-ttu-id="055f6-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="055f6-112">Visual Studio Code</span></span>

<span data-ttu-id="055f6-113">[VS Code](https://code.visualstudio.com/) è un editor di codice leggero ma potente disponibile per MacOS, Windows e Linux.</span><span class="sxs-lookup"><span data-stu-id="055f6-113">[VS Code](https://code.visualstudio.com/) is a lightweight but powerful code editor available for MacOS, Windows, and Linux.</span></span> <span data-ttu-id="055f6-114">VS Code supporta un flusso di lavoro di sviluppo Java semplice e moderno grazie a un set di estensioni che forniscono supporto per i progetti, completamento del codice, debug, analisi ed esplorazione.</span><span class="sxs-lookup"><span data-stu-id="055f6-114">VS Code supports a simple, modern Java development workflow through a set of extensions that provide project support, code completion, debugging, linting, and navigation.</span></span>

<span data-ttu-id="055f6-115">[Get Started with VS Code and Java](https://code.visualstudio.com/docs/java) (Introduzione a VS Code e Java)
[Java extension pack for VS Code](https://code.visualstudio.com/docs/java/extensions) (Java Extension Pack per VS Code)</span><span class="sxs-lookup"><span data-stu-id="055f6-115">[Get Started with VS Code and Java](https://code.visualstudio.com/docs/java)
[Java extension pack for VS Code](https://code.visualstudio.com/docs/java/extensions)</span></span>  

## <a name="azure-cli-20"></a><span data-ttu-id="055f6-116">Interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="055f6-116">Azure CLI 2.0</span></span>

<span data-ttu-id="055f6-117">L'interfaccia della riga di comando di Azure 2.0 fornisce un'esperienza da riga di comando per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="055f6-117">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="055f6-118">È possibile usarla nel browser con [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) oppure [installarla](https://docs.microsoft.com/cli/azure/install-azure-cli) in macOS, Linux e Windows ed eseguirla dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="055f6-118">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="055f6-119">[Introduzione all'interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="055f6-119">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="055f6-120">Esplora archivi Azure</span><span class="sxs-lookup"><span data-stu-id="055f6-120">Azure Storage Explorer</span></span> 

<span data-ttu-id="055f6-121">Gestire gli account di archiviazione, i contenitori e i BLOB/file di Azure dal desktop.</span><span class="sxs-lookup"><span data-stu-id="055f6-121">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="055f6-122">Azure Storage Explorer è attualmente disponibile in anteprima per Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="055f6-122">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="055f6-123">Guida introduttiva ad Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="055f6-123">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
