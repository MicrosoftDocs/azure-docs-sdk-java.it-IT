---
title: Esempi di librerie di gestione di Azure per app Web Java
description: Ottenere codice di esempio per la creazione e l'aggiornamento di app Web di Azure ospitate nel servizio app tramite le librerie di gestione di Azure per Java
keywords: Azure, Java, SDK, API, Maven, Gradle, app Web, servizio app
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.openlocfilehash: be4031059587ceb88f6824356f677c37a198de80
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592586"
---
# <a name="azure-management-libraries-for-java-samples-for-web-apps"></a><span data-ttu-id="d8d3a-104">Esempi di librerie di gestione di Azure per app Web Java</span><span class="sxs-lookup"><span data-stu-id="d8d3a-104">Azure management libraries for Java samples for web apps</span></span>

| <span data-ttu-id="d8d3a-105">**Creare un'app**</span><span class="sxs-lookup"><span data-stu-id="d8d3a-105">**Create an app**</span></span> ||
|---|---|
| <span data-ttu-id="d8d3a-106">[Creare un'app Web e distribuirla da FTP o GitHub][1]</span><span class="sxs-lookup"><span data-stu-id="d8d3a-106">[Create a web app and deploy from FTP or GitHub][1]</span></span> | <span data-ttu-id="d8d3a-107">Distribuire app Web da Git locale, FTP e tramite l'integrazione continua da GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8d3a-107">Deploy web apps from local Git, FTP, and continuous integration from GitHub.</span></span> |
| <span data-ttu-id="d8d3a-108">[Creare un'app Web e gestire gli slot di distribuzione][2]</span><span class="sxs-lookup"><span data-stu-id="d8d3a-108">[Create a web app and manage deployment slots][2]</span></span> | <span data-ttu-id="d8d3a-109">Creare un'app Web e distribuirla negli slot di staging, quindi scambiare le distribuzioni tra gli slot.</span><span class="sxs-lookup"><span data-stu-id="d8d3a-109">Create a web app and deploy to staging slots, and then swap deployments between slots.</span></span> |
| <span data-ttu-id="d8d3a-110">**Configurare l'applicazione**</span><span class="sxs-lookup"><span data-stu-id="d8d3a-110">**Configure app**</span></span> ||
| <span data-ttu-id="d8d3a-111">[Creare un'app Web e configurare un dominio personalizzato][3]</span><span class="sxs-lookup"><span data-stu-id="d8d3a-111">[Create a web app and configure a custom domain][3]</span></span> | <span data-ttu-id="d8d3a-112">Creare un'app Web con un dominio personalizzato e un certificato SSL autofirmato.</span><span class="sxs-lookup"><span data-stu-id="d8d3a-112">Create a web app with a custom domain and self-signed SSL certificate.</span></span> |
| <span data-ttu-id="d8d3a-113">**Ridimensionare le app**</span><span class="sxs-lookup"><span data-stu-id="d8d3a-113">**Scale apps**</span></span> ||
| <span data-ttu-id="d8d3a-114">[Ridimensionare un'app Web a disponibilità elevata in più aree][4]</span><span class="sxs-lookup"><span data-stu-id="d8d3a-114">[Scale a web app with high availability across multiple regions][4]</span></span> | <span data-ttu-id="d8d3a-115">Ridimensionare un'app Web in tre aree geografiche diverse e renderle disponibili tramite un singolo endpoint con Gestione traffico di Azure.</span><span class="sxs-lookup"><span data-stu-id="d8d3a-115">Scale a web app in three different geographical regions and make them available through a single endpoint using Azure Traffic Manager.</span></span> | 
| <span data-ttu-id="d8d3a-116">**Collegare l'app alle risorse**</span><span class="sxs-lookup"><span data-stu-id="d8d3a-116">**Connect app to resources**</span></span> ||
| <span data-ttu-id="d8d3a-117">[Connettere un'app Web a un account di archiviazione][5]</span><span class="sxs-lookup"><span data-stu-id="d8d3a-117">[Connect a web app to a storage account][5]</span></span> | <span data-ttu-id="d8d3a-118">Creare un account di archiviazione di Azure e aggiungere la stringa di connessione dell'account di archiviazione alle impostazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="d8d3a-118">Create an Azure storage account and add the storage account connection string to the app settings.</span></span> |
| <span data-ttu-id="d8d3a-119">[Connettere un'app Web a un database SQL][6]</span><span class="sxs-lookup"><span data-stu-id="d8d3a-119">[Connect a web app to a SQL database][6]</span></span> | <span data-ttu-id="d8d3a-120">Creare un'app Web e un database SQL e quindi aggiungere la stringa di connessione del database SQL alle impostazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="d8d3a-120">Create a web app and SQL database, and then add the SQL database connection string to the app settings.</span></span> |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/