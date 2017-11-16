---
title: Librerie di Azure Cosmos DB per Java
description: Documentazione di riferimento per le librerie client Java per Azure Cosmos DB
keywords: Azure, Java, SDK, API, SQL, database, MongoDB, Cosmos DB, NoSQL, DocumentDB
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/10/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cosmosdb
ms.openlocfilehash: 393f57df0ea2076c6ee7045b56883ee088716fad
ms.sourcegitcommit: 93107ca9ed76a29573a5faf8f39737c85e6bbaff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2017
---
# <a name="azure-cosmos-db-libraries-for-java"></a>Librerie di Azure Cosmos DB per Java

## <a name="overview"></a>Panoramica

Archiviare ed eseguire query su coppie chiave-valore, documenti JSON, grafi e dati di colonne in un database distribuito a livello globale con [Cosmos DB](/azure/cosmos-db/introduction).

Per iniziare a usare Cosmos DB, vedere [Azure Cosmos DB: Creare un'app per le API con Java e il portale di Azure](/azure/cosmos-db/create-documentdb-java).

## <a name="client-library"></a>Libreria client

Connettersi a Cosmos DB usando la libreria client di [DocumentDB](/azure/cosmos-db/documentdb-introduction) per usare i dati JSON con la [sintassi di query SQL](/azure/cosmos-db/documentdb-sql-query).

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
> [Esplorare le API client](/java/api/overview/azure/cosmosdb/clientlibrary)


## <a name="samples"></a>Esempi

[Develop a Java app using Azure Cosmos DB MongoDB API][2]  (Sviluppare un'app Java con l'API MongoDB di Azure Cosmos DB)  
[Develop a Java app using Azure Cosmos DB MongoDB API][3]  (Sviluppare un'app Java con l'API Graph di Azure Cosmos DB)  
[Develop a Java app using Azure Cosmos DB DocumentDB API][4] (Sviluppare un'app Java usando l'API DocumentDB di Azure Cosmos DB)        

Esplorare altri [esempi di codice Java per Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) disponibili per l'uso nelle app.

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started