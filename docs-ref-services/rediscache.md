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
ms.openlocfilehash: 25c91e02d1ce52afab68286c89c859ab61da56fe
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="redis-cache-libraries-for-java"></a>Librerie di Cache Redis per Java

## <a name="overview"></a>Panoramica

Cache Redis di Azure è un archivio chiave-valore distribuito basato sulla popolare cache open source [Redis](https://redis.io/). 

Per iniziare a usare Cache Redis di Azure, vedere [Come usare Cache Redis di Azure con Java](/azure/redis-cache/cache-java-get-started).

## <a name="client-library"></a>Libreria client

Connettere Cache Redis di Azure e l'archivio per recuperare i valori dalla cache con il client open source [Jedis](https://github.com/xetorthio/jedis).  

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a>Esempio

Connettersi a Cache Redis di Azure e inserire una stringa nella cache.

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a>API di gestione

Creare e scalare le risorse di Cache Redis di Azure e gestire le chiavi di accesso con l'API di gestione.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.1.2</version>
</dependency>
```

## <a name="example"></a>Esempio

Creare una nuova istanza di Cache Redis di Azure nel [livello standard a due nodi](https://azure.microsoft.com/services/cache/). 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/rediscache/managementapi)

## <a name="samples"></a>Esempi

[Gestire Cache Redis di Azure](https://github.com/Azure-Samples/redis-java-manage-cache)   

Esplorare altri [esempi di codice Java per Cache Redis di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=redis) disponibili per l'uso nelle app.
