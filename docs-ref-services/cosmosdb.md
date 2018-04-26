---
title: Librerie di Azure Cosmos DB per Java
description: Documentazione di riferimento per le librerie client Java per Azure Cosmos DB
keywords: Azure, Java, SDK, API, SQL, database, MongoDB, Cosmos DB, NoSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/10/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cosmosdb
ms.openlocfilehash: 6fc9f90cb3c8130aa82b20554a94a8b5ab78c083
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="azure-cosmos-db-libraries-for-java"></a>Librerie di Azure Cosmos DB per Java

## <a name="overview"></a>Panoramica

Archiviare ed eseguire query su coppie chiave-valore, documenti JSON, grafi e dati di colonne in un database distribuito a livello globale con [Azure Cosmos DB](/azure/cosmos-db/introduction).

Per iniziare a usare Azure Cosmos DB, vedere [Azure Cosmos DB: Creare un'app per le API con Java e il portale di Azure](/azure/cosmos-db/create-sql-api-java).

## <a name="client-library"></a>Libreria client

Connettersi ad Azure Cosmos DB usando la libreria client dell'[API SQL](/azure/cosmos-db/sql-api-introduction) per usare i dati JSON con la [sintassi di query SQL](/azure/cosmos-db/sql-api-sql-query).

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client di Cosmos DB nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a>Esempio

Selezionare i documenti JSON corrispondenti in Cosmos DB usando la sintassi di query SQL.

```java
DocumentClient client = new DocumentClient("https://contoso.documents.azure.com:443",
                "contosoCosmosDBKey", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);

List<Document> results = client.queryDocuments("dbs/" + DATABASE_ID + "/colls/" + COLLECTION_ID,
        "SELECT * FROM myCollection WHERE myCollection.email = 'allen [at] contoso.com'",
        null)
    .getQueryIterable()
    .toList();

```

> [!div class="nextstepaction"]
> [Esplorare le API client](/java/api/overview/azure/cosmosdb/client)


## <a name="samples"></a>Esempi

[Develop a Java app using Azure Cosmos DB MongoDB API][2]  (Sviluppare un'app Java con l'API MongoDB di Azure Cosmos DB)  
[Develop a Java app using Azure Cosmos DB MongoDB API][3]  (Sviluppare un'app Java con l'API Graph di Azure Cosmos DB)  
[Develop a Java app using Azure Cosmos DB SQL API][4] (Sviluppare un'app Java con l'API SQL di Azure Cosmos DB)        

Esplorare altri [esempi di codice Java per Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) disponibili per l'uso nelle app.

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
