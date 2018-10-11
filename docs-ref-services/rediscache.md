---
title: Librerie di Cache Redis per Java
description: Documentazione di riferimento per le librerie di gestione e client Java per Cache Redis
keywords: Azure, Java, SDK, API, cache, Redis, cache Web, chiave-valore, in memoria
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: redis-cache
ms.openlocfilehash: dd03825d9ae7cba32087f92262d5ef213cf3af0b
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892772"
---
# <a name="redis-cache-libraries-for-java"></a><span data-ttu-id="d4842-104">Librerie di Cache Redis per Java</span><span class="sxs-lookup"><span data-stu-id="d4842-104">Redis Cache libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d4842-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d4842-105">Overview</span></span>

<span data-ttu-id="d4842-106">Cache Redis di Azure è un archivio chiave-valore distribuito basato sulla popolare cache open source [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="d4842-106">Azure Redis Cache is a secure, distributed key-value store based on the popular open source [Redis](https://redis.io/) cache.</span></span> 

<span data-ttu-id="d4842-107">Per iniziare a usare Cache Redis di Azure, vedere [Come usare Cache Redis di Azure con Java](/azure/redis-cache/cache-java-get-started).</span><span class="sxs-lookup"><span data-stu-id="d4842-107">To get started with Azure Redis Cache, see [How to use Azure Redis Cache with Java](/azure/redis-cache/cache-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="d4842-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="d4842-108">Client library</span></span>

<span data-ttu-id="d4842-109">Connettere Cache Redis di Azure e l'archivio per recuperare i valori dalla cache con il client open source [Jedis](https://github.com/xetorthio/jedis).</span><span class="sxs-lookup"><span data-stu-id="d4842-109">Connect to Azure Redis Cache and store and retrieve values from the cache using the open-source [Jedis](https://github.com/xetorthio/jedis) client.</span></span>  

<span data-ttu-id="d4842-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="d4842-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a><span data-ttu-id="d4842-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="d4842-111">Example</span></span>

<span data-ttu-id="d4842-112">Connettersi a Cache Redis di Azure e inserire una stringa nella cache.</span><span class="sxs-lookup"><span data-stu-id="d4842-112">Connect to Azure Redis and insert a string into the cache.</span></span>

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a><span data-ttu-id="d4842-113">API di gestione</span><span class="sxs-lookup"><span data-stu-id="d4842-113">Management API</span></span>

<span data-ttu-id="d4842-114">Creare e scalare le risorse di Cache Redis di Azure e gestire le chiavi di accesso con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="d4842-114">Create and scale Azure Redis resources and manage access keys to with the management API.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="d4842-115">Esempio</span><span class="sxs-lookup"><span data-stu-id="d4842-115">Example</span></span>

<span data-ttu-id="d4842-116">Creare una nuova istanza di Cache Redis di Azure nel [livello standard a due nodi](https://azure.microsoft.com/services/cache/).</span><span class="sxs-lookup"><span data-stu-id="d4842-116">Create a new Azure Redis Cache in the [two-node standard tier](https://azure.microsoft.com/services/cache/).</span></span> 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d4842-117">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="d4842-117">Explore the Management APIs</span></span>](/java/api/overview/azure/rediscache/management)

## <a name="samples"></a><span data-ttu-id="d4842-118">Esempi</span><span class="sxs-lookup"><span data-stu-id="d4842-118">Samples</span></span>

[<span data-ttu-id="d4842-119">Gestire Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="d4842-119">Manage Azure Redis Cache</span></span>](https://github.com/Azure-Samples/redis-java-manage-cache)   

<span data-ttu-id="d4842-120">Esplorare altri [esempi di codice Java per Cache Redis di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=redis) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="d4842-120">Explore more [sample Java code for Azure Redis Cache](https://azure.microsoft.com/resources/samples/?platform=java&term=redis) you can use in your apps.</span></span>
