---
title: Librerie della rete CDN di Azure per Java
description: Documentazione di riferimento per le librerie di gestione della rete CDN per Java
keywords: Azure, Java, SDK, API, contenuti, distribuzione, rete, CDN
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cdn
ms.openlocfilehash: 91df958d2d78fb4fd959c228b28c6ae003716be6
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2017
---
# <a name="azure-cdn-libraries-for-java"></a><span data-ttu-id="06bc9-104">Librerie della rete CDN di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="06bc9-104">Azure CDN libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="06bc9-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="06bc9-105">Overview</span></span>

<span data-ttu-id="06bc9-106">La [rete per la distribuzione di contenuti (CDN) di Azure](/azure/cdn/cdn-overview) consente di memorizzare nella cache il contenuto Web statico in località strategiche per offrire la massima velocità effettiva agli utenti.</span><span class="sxs-lookup"><span data-stu-id="06bc9-106">Cache static web content at strategically placed locations to provide maximum throughput for users with [Azure Content Delivery Network](/azure/cdn/cdn-overview) (CDN).</span></span>

<span data-ttu-id="06bc9-107">Per iniziare a usare la rete CDN di Azure, vedere [Introduzione alla rete CDN di Azure](/azure/cdn/cdn-create-new-endpoint).</span><span class="sxs-lookup"><span data-stu-id="06bc9-107">To get started with Azure CDN, see [Getting started with Azure CDN](/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-api"></a><span data-ttu-id="06bc9-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="06bc9-108">Management API</span></span>

<span data-ttu-id="06bc9-109">Creare profili CDN, definire gli endpoint e aggiungere contenuto alla rete CDN usando l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="06bc9-109">Create CDN profiles, define endpoints, and add content to the CDN using the management API.</span></span>

<span data-ttu-id="06bc9-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="06bc9-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="06bc9-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="06bc9-111">Example</span></span>

<span data-ttu-id="06bc9-112">Creare un profilo CDN, assegnare gli endpoint e caricare contenuto nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="06bc9-112">Create a CDN profile, assign endpoints, and load content into the CDN.</span></span>

```java
CdnProfile profile = azure.cdnProfiles().define("testCDN")
        .withRegion(Region.US_EAST)
        .withNewResourceGroup()
        .withStandardVerizonSku()
        .withNewEndpoint("webAppHostname1")
        .withNewEndpoint("webAppHostname2")
        .create();

List<String> contentList = new ArrayList<String>();
contentList.add("/client.js");
contentList.add("/media/fabrikam_logo.png");

for (CdnEndpoint endpoint : profile.endpoints().values()) {
    endpoint.loadContent(contentList);
}
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="06bc9-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="06bc9-113">Explore the Management APIs</span></span>](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a><span data-ttu-id="06bc9-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="06bc9-114">Samples</span></span>

[<span data-ttu-id="06bc9-115">Gestire reti CDN con Java</span><span class="sxs-lookup"><span data-stu-id="06bc9-115">Manage CDNs with Java</span></span>](https://github.com/Azure-Samples/cdn-java-manage-cdn)

<span data-ttu-id="06bc9-116">Esplorare altri [esempi di codice Java per la rete CDN di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="06bc9-116">Explore more [sample Java code for Azure CDN](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn) you can use in your apps.</span></span>