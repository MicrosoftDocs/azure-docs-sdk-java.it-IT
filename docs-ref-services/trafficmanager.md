---
title: Librerie di Gestione traffico di Azure per Java
description: Documentazione di riferimento per le librerie di Gestione traffico per Java
keywords: Azure, Java, SDK, API, servizio di bilanciamento del carico, distribuzione del carico, rete, Gestione traffico
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: a38db9a0d92d7dedc5ff49b83ad2658aa61729dd
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="814d0-104">Librerie di Gestione traffico di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="814d0-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="814d0-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="814d0-105">Overview</span></span>

<span data-ttu-id="814d0-106">[Gestione traffico di Azure](/azure/traffic-manager/traffic-manager-overview) consente di controllare la distribuzione del traffico utente per gli endpoint servizio in diversi data center.</span><span class="sxs-lookup"><span data-stu-id="814d0-106">Control the distribution of user traffic for service endpoints in different datacenters with [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview).</span></span>

<span data-ttu-id="814d0-107">Per iniziare a usare Gestione traffico di Azure, vedere [Creazione di un profilo di Gestione traffico](/azure/traffic-manager/traffic-manager-create-profile).</span><span class="sxs-lookup"><span data-stu-id="814d0-107">To get started with Azure Traffic Manager, see [Create a Traffic Manager profile](/azure/traffic-manager/traffic-manager-create-profile).</span></span>

## <a name="management-api"></a><span data-ttu-id="814d0-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="814d0-108">Management API</span></span>

<span data-ttu-id="814d0-109">Creare profili di Gestione traffico, definire gli endpoint e modificare il metodo di routing con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="814d0-109">Create Traffic Manager profiles, define endpoints, and change the routing method with the management API.</span></span> 

<span data-ttu-id="814d0-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="814d0-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.2.1</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="814d0-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="814d0-111">Example</span></span>

<span data-ttu-id="814d0-112">Creare un profilo di Gestione traffico e assegnare un endpoint singolo.</span><span class="sxs-lookup"><span data-stu-id="814d0-112">Create a Traffic Manager profile and assign a single endpoint.</span></span>

```java
TrafficManagerProfile tmProfile = azure.trafficManagerProfiles().define("testTMProfile")
        .withNewResourceGroup(Region.US_EAST)
        .withLeafDomainLabel("testTMProfile")
        .withPriorityBasedRouting()
        .defineAzureTargetEndpoint("endpoint")
        .toResourceId(webAppId)
        .withRoutingPriority(1)
        .attach()
        .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="814d0-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="814d0-113">Explore the Management APIs</span></span>](/java/api/overview/azure/trafficmanager/managementapi)

## <a name="samples"></a><span data-ttu-id="814d0-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="814d0-114">Samples</span></span>

[<span data-ttu-id="814d0-115">Bilanciare il traffico di app Web tra più aree</span><span class="sxs-lookup"><span data-stu-id="814d0-115">Balance web app traffic across multiple regions</span></span>](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

<span data-ttu-id="814d0-116">Esplorare altri [esempi di codice Java per Gestione traffico di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="814d0-116">Explore more [sample Java code for Azure Traffic Manager](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) you can use in your apps.</span></span>
