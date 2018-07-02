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
ms.sourcegitcommit: 33c70f921f1524c8bdb69ad5a1a3c1b4b1de97ba
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026793"
---
# <a name="azure-cosmos-db-libraries-for-java"></a><span data-ttu-id="b2d2b-104">Librerie di Azure Cosmos DB per Java</span><span class="sxs-lookup"><span data-stu-id="b2d2b-104">Azure Cosmos DB libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="b2d2b-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b2d2b-105">Overview</span></span>

<span data-ttu-id="b2d2b-106">Archiviare ed eseguire query su coppie chiave-valore, documenti JSON, grafi e dati di colonne in un database distribuito a livello globale con [Azure Cosmos DB](/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="b2d2b-106">Store and query key-value, JSON document, graph, and columnar data in a globally distributed database with [Azure Cosmos DB](/azure/cosmos-db/introduction).</span></span>

<span data-ttu-id="b2d2b-107">Per iniziare a usare Azure Cosmos DB, vedere [Azure Cosmos DB: Creare un'app per le API con Java e il portale di Azure](/azure/cosmos-db/create-sql-api-java).</span><span class="sxs-lookup"><span data-stu-id="b2d2b-107">To get started with Azure Cosmos DB, see [Azure Cosmos DB: Build an API app with Java and the Azure portal](/azure/cosmos-db/create-sql-api-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="b2d2b-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="b2d2b-108">Client library</span></span>

<span data-ttu-id="b2d2b-109">Connettersi ad Azure Cosmos DB usando la libreria client dell'[API SQL](/azure/cosmos-db/sql-api-introduction) per usare i dati JSON con la [sintassi di query SQL](/azure/cosmos-db/sql-api-sql-query).</span><span class="sxs-lookup"><span data-stu-id="b2d2b-109">Connect to Azure Cosmos DB using the [SQL API](/azure/cosmos-db/sql-api-introduction) client library to work with JSON data with [SQL query syntax](/azure/cosmos-db/sql-api-sql-query).</span></span>

<span data-ttu-id="b2d2b-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client di Cosmos DB nel progetto.</span><span class="sxs-lookup"><span data-stu-id="b2d2b-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the Cosmos DB client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="b2d2b-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="b2d2b-111">Example</span></span>

<span data-ttu-id="b2d2b-112">Selezionare i documenti JSON corrispondenti in Cosmos DB usando la sintassi di query SQL.</span><span class="sxs-lookup"><span data-stu-id="b2d2b-112">Select matching JSON documents in Cosmos DB using SQL query syntax.</span></span>

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
> [<span data-ttu-id="b2d2b-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="b2d2b-113">Explore the Client APIs</span></span>](/java/api/overview/azure/cosmosdb/client)


## <a name="samples"></a><span data-ttu-id="b2d2b-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="b2d2b-114">Samples</span></span>

<span data-ttu-id="b2d2b-115">[Develop a Java app using Azure Cosmos DB MongoDB API][2]  (Sviluppare un'app Java con l'API MongoDB di Azure Cosmos DB)</span><span class="sxs-lookup"><span data-stu-id="b2d2b-115">[Develop a Java app using Azure Cosmos DB MongoDB API][2] </span></span>  
<span data-ttu-id="b2d2b-116">[Develop a Java app using Azure Cosmos DB MongoDB API][3]  (Sviluppare un'app Java con l'API Graph di Azure Cosmos DB)</span><span class="sxs-lookup"><span data-stu-id="b2d2b-116">[Develop a Java app using Azure Cosmos DB Graph API][3] </span></span>  
<span data-ttu-id="b2d2b-117">[Develop a Java app using Azure Cosmos DB SQL API][4] (Sviluppare un'app Java con l'API SQL di Azure Cosmos DB)</span><span class="sxs-lookup"><span data-stu-id="b2d2b-117">[Develop a Java app using Azure Cosmos DB SQL API][4]</span></span>        

<span data-ttu-id="b2d2b-118">Esplorare altri [esempi di codice Java per Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="b2d2b-118">Explore more [sample Java code for Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) you can use in your apps.</span></span>

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
