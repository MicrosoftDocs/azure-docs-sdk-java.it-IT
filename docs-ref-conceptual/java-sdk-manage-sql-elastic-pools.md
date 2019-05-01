---
title: Gestire un pool elastico di database SQL con Java | Microsoft Docs
description: Codice di esempio per creare e configurare database SQL di Azure con Azure SDK per Java
author: rloutlaw
manager: douge
ms.assetid: 9b461de8-46bc-4650-8e9e-59531f4e2a53
ms.topic: article
ms.service: Azure
ms.devlang: java
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 59d2601ef21b91a364887fe0548cb95023c79b5e
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592686"
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a><span data-ttu-id="b3745-103">Gestire database SQL di Azure nei pool elastici dalle applicazioni Java</span><span class="sxs-lookup"><span data-stu-id="b3745-103">Manage Azure SQL databases in elastic pools from your Java applications</span></span>

<span data-ttu-id="b3745-104">[Questo esempio](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool) crea un server di database SQL con un [pool elastico](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) per gestire e ridimensionare le risorse per più database in un singolo piano.</span><span class="sxs-lookup"><span data-stu-id="b3745-104">[This sample](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool) creates a SQL database server with an [elastic pool](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to manage and scale resources for mulitple databases in a single plan.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="b3745-105">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="b3745-105">Run the sample</span></span>

<span data-ttu-id="b3745-106">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="b3745-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="b3745-107">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="b3745-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

[<span data-ttu-id="b3745-108">Visualizzare l'esempio di codice completo in GitHub</span><span class="sxs-lookup"><span data-stu-id="b3745-108">View the complete code sample on GitHub</span></span>](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a><span data-ttu-id="b3745-109">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="b3745-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a><span data-ttu-id="b3745-110">Creare un server di database SQL con un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-110">Create a SQL database server with an elastic pool</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    // Use ElasticPoolEditions.STANDARD as the edition
                    // and create two databases
                    .withNewElasticPool(elasticPoolName, ElasticPoolEditions.STANDARD, 
                        database1Name, database2Name)
                    .create();
```

<span data-ttu-id="b3745-111">Vedere le [informazioni di riferimento sulla classe ElasticPoolEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) per i valori dell'edizione corrente.</span><span class="sxs-lookup"><span data-stu-id="b3745-111">See the [ElasticPoolEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) for current edition values.</span></span> <span data-ttu-id="b3745-112">Vedere la [documentazione dei pool elastici di database SQL](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) per confrontare le caratteristiche delle risorse delle edizioni.</span><span class="sxs-lookup"><span data-stu-id="b3745-112">Review the [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to compare edition resource characteristics.</span></span> 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a><span data-ttu-id="b3745-113">Modificare le impostazioni dell'unità di transazione di database (DTU) in un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-113">Change Database Transaction Unit (DTU) settings in an elastic pool</span></span>

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

<span data-ttu-id="b3745-114">Vedere la [documentazione relativa a DTU ed eDTU](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu) per altre informazioni sull'allocazione delle risorse ai database SQL.</span><span class="sxs-lookup"><span data-stu-id="b3745-114">Review the [DTUs and eDTUs documentation](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu) to learn more about allocating resources to SQL databases.</span></span>

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a><span data-ttu-id="b3745-115">Creare un nuovo database e aggiungerlo a un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-115">Create a new database and add it to an elastic pool</span></span>

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

<span data-ttu-id="b3745-116">L'API crea `anotherDatabase` al [livello S0](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) nella prima istruzione.</span><span class="sxs-lookup"><span data-stu-id="b3745-116">The API creates `anotherDatabase` at [S0 tier](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) in the first statement.</span></span> <span data-ttu-id="b3745-117">Spostando `anotherDatabase` nel pool elastico, le risorse di database vengono assegnate in base alle impostazioni del pool.</span><span class="sxs-lookup"><span data-stu-id="b3745-117">Moving `anotherDatabase` to the elastic pool assigns the database resources based on the pool settings.</span></span>

## <a name="remove-a-database-from-an-elastic-pool"></a><span data-ttu-id="b3745-118">Rimuovere un database da un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-118">Remove a database from an elastic pool</span></span>
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

<span data-ttu-id="b3745-119">Vedere le [informazioni di riferimento sulla classe DatabaseEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) per i valori da passare a `withEdition()`.</span><span class="sxs-lookup"><span data-stu-id="b3745-119">See the [DatabaseEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) for values to pass to `withEdition()`.</span></span>

## <a name="list-current-database-activities-in-an-elastic-pool"></a><span data-ttu-id="b3745-120">Elencare le attività di database correnti in un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-120">List current database activities in an elastic pool</span></span>
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

<span data-ttu-id="b3745-121">Le attività di database includono lo spostamento di un database all'interno o all'esterno di un pool elastico e l'aggiornamento dei database in un pool elastico.</span><span class="sxs-lookup"><span data-stu-id="b3745-121">Database activities include moving a database in or out of an elastic pool and updating databases in an elastic pool.</span></span>


## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="b3745-122">Elencare i database in un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-122">List databases in an elastic pool</span></span>
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

<span data-ttu-id="b3745-123">Esaminare i metodi in [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) per eseguire query sui database in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="b3745-123">Review the methods in [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) to query the databases in more detail.</span></span>

## <a name="delete-an-elastic-pool"></a><span data-ttu-id="b3745-124">Eliminare un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-124">Delete an elastic pool</span></span>
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

<span data-ttu-id="b3745-125">Il pool elastico deve essere vuoto prima di eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="b3745-125">The elastic pool must be empty before deleting it.</span></span>

## <a name="sample-code-summary"></a><span data-ttu-id="b3745-126">Riepilogo del codice di esempio</span><span class="sxs-lookup"><span data-stu-id="b3745-126">Sample code summary.</span></span>

<span data-ttu-id="b3745-127">L'esempio crea un server SQL con due database gestiti in un singolo pool elastico.</span><span class="sxs-lookup"><span data-stu-id="b3745-127">The sample creates a SQL server with two databases managed in a single elasic pool.</span></span> <span data-ttu-id="b3745-128">I limiti delle risorse del pool elastico vengono aggiornati e quindi vengono aggiunti altri database al pool.</span><span class="sxs-lookup"><span data-stu-id="b3745-128">The elastic pool resource limits are updated, then additional databases are added to the pool.</span></span> <span data-ttu-id="b3745-129">Il codice legge quindi la configurazione e lo stato del pool elastico.</span><span class="sxs-lookup"><span data-stu-id="b3745-129">The code then reads the elastic pool's configuration and state.</span></span> 

<span data-ttu-id="b3745-130">L'esempio elimina tutte le risorse create prima di uscire.</span><span class="sxs-lookup"><span data-stu-id="b3745-130">The sample deletes all resources it created before exiting.</span></span>

| <span data-ttu-id="b3745-131">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="b3745-131">Class used in sample</span></span> | <span data-ttu-id="b3745-132">Note</span><span class="sxs-lookup"><span data-stu-id="b3745-132">Notes</span></span> |
|-------|-------|
| [<span data-ttu-id="b3745-133">SqlServer</span><span class="sxs-lookup"><span data-stu-id="b3745-133">SqlServer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | <span data-ttu-id="b3745-134">Server di database SQL in Azure creato dalla catena Fluent `azure.sqlServers().define()...create()`.</span><span class="sxs-lookup"><span data-stu-id="b3745-134">SQL DB server in Azure created by `azure.sqlServers().define()...create()` fluent chain.</span></span> <span data-ttu-id="b3745-135">Fornisce metodi per creare e usare pool elastici e database.</span><span class="sxs-lookup"><span data-stu-id="b3745-135">Provides methods to create and work with elastic pools and databases.</span></span> 
| [<span data-ttu-id="b3745-136">SqlDatabase</span><span class="sxs-lookup"><span data-stu-id="b3745-136">SqlDatabase</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | <span data-ttu-id="b3745-137">Oggetto lato client che rappresenta un database SQL.</span><span class="sxs-lookup"><span data-stu-id="b3745-137">Client side object representing a SQL database.</span></span> <span data-ttu-id="b3745-138">Viene creato tramite `sqlServer().define()...create()`.</span><span class="sxs-lookup"><span data-stu-id="b3745-138">Created through `sqlServer().define()...create()`.</span></span> 
| [<span data-ttu-id="b3745-139">DatabaseEditions</span><span class="sxs-lookup"><span data-stu-id="b3745-139">DatabaseEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | <span data-ttu-id="b3745-140">Campi statici costanti usati per impostare le risorse di database quando si crea un database fuori da un pool elastico o quando si sposta un database all'esterno di un pool elastico</span><span class="sxs-lookup"><span data-stu-id="b3745-140">Constant static fields used to set database resources when creating a database outside of an elastic pool or when moving a database out of an elastic pool</span></span>  
| [<span data-ttu-id="b3745-141">SqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="b3745-141">SqlElasticPool</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | <span data-ttu-id="b3745-142">Creato dalla sezione `withNewElasticPool()` della catena Fluent che ha creato l'oggetto SqlServer in Azure.</span><span class="sxs-lookup"><span data-stu-id="b3745-142">Created from the `withNewElasticPool()` section of the fluent chain that created the SqlServer in Azure.</span></span> <span data-ttu-id="b3745-143">Fornisce metodi per impostare i limiti delle risorse per i database in esecuzione nel pool elastico e per il pool elastico stesso.</span><span class="sxs-lookup"><span data-stu-id="b3745-143">Provides methods to set resource limits for databases running in the elastic pool and for the elastic pool itself.</span></span> 
| [<span data-ttu-id="b3745-144">ElasticPoolEditions</span><span class="sxs-lookup"><span data-stu-id="b3745-144">ElasticPoolEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | <span data-ttu-id="b3745-145">Classe di campi costanti che definisce le risorse disponibili per un pool elastico.</span><span class="sxs-lookup"><span data-stu-id="b3745-145">Class of constant fields defining the resources available to an elastic pool.</span></span> <span data-ttu-id="b3745-146">Vedere la [documentazione dei pool elastici di database SQL](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) per i dettagli sui livelli.</span><span class="sxs-lookup"><span data-stu-id="b3745-146">See [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) for tier details.</span></span> 
| [<span data-ttu-id="b3745-147">ElasticPoolDatabaseActivity</span><span class="sxs-lookup"><span data-stu-id="b3745-147">ElasticPoolDatabaseActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | <span data-ttu-id="b3745-148">Recuperato da `SqlElasticPool.listDatabaseActivities()`.</span><span class="sxs-lookup"><span data-stu-id="b3745-148">Retreived from `SqlElasticPool.listDatabaseActivities()`.</span></span> <span data-ttu-id="b3745-149">Ogni oggetto di questo tipo rappresenta un'attività eseguita su un database nel pool elastico.</span><span class="sxs-lookup"><span data-stu-id="b3745-149">Each object of this type represents an activity performed on a database in the elastic pool.</span></span>
| [<span data-ttu-id="b3745-150">ElasticPoolActivity</span><span class="sxs-lookup"><span data-stu-id="b3745-150">ElasticPoolActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | <span data-ttu-id="b3745-151">Recuperato in un elenco da `SqlElasticPool.listActivities()`.</span><span class="sxs-lookup"><span data-stu-id="b3745-151">Retrieved in a List from `SqlElasticPool.listActivities()`.</span></span> <span data-ttu-id="b3745-152">Ogni oggetto nell'elenco rappresenta un'attività eseguita sul pool elastico, non sui database del pool elastico.</span><span class="sxs-lookup"><span data-stu-id="b3745-152">Each of object in the list represents an activity performed on the elastic pool (not the databases in the elastic pool).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3745-153">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b3745-153">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]