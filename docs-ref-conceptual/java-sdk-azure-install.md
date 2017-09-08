---
title: Sviluppatori Azure per Java | Microsoft Docs
description: Java SDK e informazioni di riferimento sulle API per Azure
keywords: Azure Java, informazioni di riferimento sulle API Azure per Java, libreria di classi Azure per Java, Azure SDK
author: routlaw
manager: douge
ms.assetid: 7b92e776-959b-4632-8b1d-047ce1417616
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 10073a1b2250a37347128dd9c8faf1375b2ab6ae
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="3b78d-104">Librerie di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="3b78d-104">Azure libraries for Java</span></span>

<span data-ttu-id="3b78d-105">Le librerie di Azure aiutano a usare i servizi di Azure nelle app Java con interfacce native.</span><span class="sxs-lookup"><span data-stu-id="3b78d-105">Azure libraries help you consume Azure services in your Java apps using native interfaces.</span></span> <span data-ttu-id="3b78d-106">Ogni libreria è indipendente e può essere usata separatamente dalle altre.</span><span class="sxs-lookup"><span data-stu-id="3b78d-106">Each library is independent and can be used separately from the others another.</span></span>

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [<span data-ttu-id="3b78d-107">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="3b78d-107">Azure Storage</span></span>](#azure-storage) | [<span data-ttu-id="3b78d-108">Database SQL</span><span class="sxs-lookup"><span data-stu-id="3b78d-108">SQL Database</span></span>](#sql-database)  | [<span data-ttu-id="3b78d-109">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="3b78d-109">Redis Cache</span></span>](#redis-cache)   | [<span data-ttu-id="3b78d-110">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b78d-110">DocumentDB</span></span>](#documentdb) |
| [<span data-ttu-id="3b78d-111">Bus di servizio</span><span class="sxs-lookup"><span data-stu-id="3b78d-111">Service Bus</span></span>](#servicebus)  | [<span data-ttu-id="3b78d-112">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b78d-112">Azure Active Directory</span></span>](#azuread) | [<span data-ttu-id="3b78d-113">Insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="3b78d-113">Key Vault</span></span>](#keyvault)  | [<span data-ttu-id="3b78d-114">Hub eventi</span><span class="sxs-lookup"><span data-stu-id="3b78d-114">Event Hub</span></span>](#eventhub)
| [<span data-ttu-id="3b78d-115">Servizio IoT</span><span class="sxs-lookup"><span data-stu-id="3b78d-115">IoT Service</span></span>](#iotservice) | [<span data-ttu-id="3b78d-116">Dispositivo IoT</span><span class="sxs-lookup"><span data-stu-id="3b78d-116">IoT Device</span></span>](#iotdevice) | [<span data-ttu-id="3b78d-117">Data Lake</span><span class="sxs-lookup"><span data-stu-id="3b78d-117">Data Lake</span></span>](#datalake)  | [<span data-ttu-id="3b78d-118">AppInsights</span><span class="sxs-lookup"><span data-stu-id="3b78d-118">AppInsights</span></span>](#appinsights) | 
| [<span data-ttu-id="3b78d-119">Batch</span><span class="sxs-lookup"><span data-stu-id="3b78d-119">Batch</span></span>](#batch) | [<span data-ttu-id="3b78d-120">Manage Azure resources (Gestire risorse di Azure)</span><span class="sxs-lookup"><span data-stu-id="3b78d-120">Manage Azure resources</span></span>](#management) |

## <a name="install-with-maven"></a><span data-ttu-id="3b78d-121">Eseguire l'installazione con Maven</span><span class="sxs-lookup"><span data-stu-id="3b78d-121">Install with Maven</span></span>

<span data-ttu-id="3b78d-122">Aggiungere una voce di dipendenza in `pom.xml` per importare una libreria nel progetto [Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="3b78d-122">Add a dependency entry in your `pom.xml` to import a library into your [Maven](https://maven.apache.org) project.</span></span>

<span data-ttu-id="3b78d-123">Per includere, ad esempio, la versione più recente delle [librerie di gestione di Azure per Java](#management):</span><span class="sxs-lookup"><span data-stu-id="3b78d-123">For example, to include the latest version of the [Azure management libraries for Java](#management):</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

<span data-ttu-id="3b78d-124">Sono supportati anche altri strumenti di compilazione Java, come Gradle, ma i passaggi di installazione non vengono descritti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="3b78d-124">Other Java build tools like Gradle are supported but the install steps are not provided in this article.</span></span> <span data-ttu-id="3b78d-125">Vedere la documentazione per lo strumento di compilazione in uso per informazioni su come usare le importazioni di Maven.</span><span class="sxs-lookup"><span data-stu-id="3b78d-125">Review the documentation for your build tool on how to consume Maven imports.</span></span>

## <a name="azure-service-libraries"></a><span data-ttu-id="3b78d-126">Librerie di servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="3b78d-126">Azure service libraries</span></span>

<span data-ttu-id="3b78d-127">Integrare i servizi di Azure per aggiungere funzionalità alle app usando queste librerie.</span><span class="sxs-lookup"><span data-stu-id="3b78d-127">Integrate Azure services to add functionality to your apps using these libraries.</span></span> <span data-ttu-id="3b78d-128">Per altre informazioni sulla compilazione di app con i servizi di Azure, vedere il [Centro per sviluppatori Java](https://azure.microsoft.com/develop/java).</span><span class="sxs-lookup"><span data-stu-id="3b78d-128">Learn more about building apps with Azure services at the [Java developer center](https://azure.microsoft.com/develop/java).</span></span>

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[<span data-ttu-id="3b78d-129">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="3b78d-129">Azure Storage</span></span>](/azure/storage/storage-introduction)  

<span data-ttu-id="3b78d-130">Archiviazione dei dati e messaggistica per le applicazioni.</span><span class="sxs-lookup"><span data-stu-id="3b78d-130">Data storage and messaging for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

[<span data-ttu-id="3b78d-131">Esempi](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [Riferimenti](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-131">Samples](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [Reference](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[<span data-ttu-id="3b78d-132">Database SQL</span><span class="sxs-lookup"><span data-stu-id="3b78d-132">SQL Database</span></span>](/azure/sql-database/sql-database-technical-overview)

<span data-ttu-id="3b78d-133">Driver JDBC per il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="3b78d-133">JDBC driver for Azure SQL Database.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[<span data-ttu-id="3b78d-134">Esempi](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [Riferimenti](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-134">Samples](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [Reference](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [Release Notes</span></span>](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[<span data-ttu-id="3b78d-135">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="3b78d-135">Redis Cache</span></span>](https://azure.microsoft.com/services/cache/)

<span data-ttu-id="3b78d-136">Archivio di coppie chiave-valore a bassa latenza e ad alte prestazioni.</span><span class="sxs-lookup"><span data-stu-id="3b78d-136">Low-latency, high-performance key-value store.</span></span>

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

[<span data-ttu-id="3b78d-137">Esempi](/azure/redis-cache/cache-java-get-started) | [Riferimenti](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-137">Samples](/azure/redis-cache/cache-java-get-started) | [Reference](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [Release Notes</span></span>](https://github.com/xetorthio/jedis/releases)  

<a name="documentdb"></a>

### <a name="cosmos-dbazuredocumentdbdocumentdb-introduction"></a>[<span data-ttu-id="3b78d-138">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3b78d-138">Cosmos DB</span></span>](/azure/documentdb/documentdb-introduction)

<span data-ttu-id="3b78d-139">Database NoSQL scalabile con documenti JSON e una sintassi di query SQL o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b78d-139">Scalable NoSQL database with JSON documents and a SQL or JavaScript query syntax.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

[<span data-ttu-id="3b78d-140">Esempi](/azure/documentdb/documentdb-java-application) | [Riferimenti](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-140">Samples](/azure/documentdb/documentdb-java-application) | [Reference](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [Release Notes</span></span>](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="3b78d-141">Bus di servizio</span><span class="sxs-lookup"><span data-stu-id="3b78d-141">ServiceBus</span></span>](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 <span data-ttu-id="3b78d-142">Il bus di servizio è un servizio di piattaforma di messaggistica transazionale di livello aziendale.</span><span class="sxs-lookup"><span data-stu-id="3b78d-142">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 [<span data-ttu-id="3b78d-143">Esempi](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Riferimenti](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-143">Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-service-bus-java)   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[<span data-ttu-id="3b78d-144">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b78d-144">Azure Active Directory</span></span>](/azure/active-directory/active-directory-whatis)   

<span data-ttu-id="3b78d-145">Gestione delle identità e accesso sicuro per le applicazioni.</span><span class="sxs-lookup"><span data-stu-id="3b78d-145">Identity management and secure sign-in for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
[<span data-ttu-id="3b78d-146">Esempi](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [Riferimenti](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-146">Samples](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [Reference](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [Release Notes</span></span>](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[<span data-ttu-id="3b78d-147">Insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="3b78d-147">Key Vault</span></span>](/azure/key-vault) 

<span data-ttu-id="3b78d-148">Accesso sicuro a chiavi e segreti dalle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="3b78d-148">Safely access keys and secrets from your applications.</span></span> 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

[<span data-ttu-id="3b78d-149">Esempi](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [Riferimenti](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-149">Samples](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [Reference](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [Release Notes</span></span>](https://github.com/Azure/azure-keyvault-java) 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[<span data-ttu-id="3b78d-150">Hub eventi</span><span class="sxs-lookup"><span data-stu-id="3b78d-150">Event Hub</span></span>](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
<span data-ttu-id="3b78d-151">Gestione di telemetria ed eventi con velocità effettiva elevata per la strumentazione o gli scenari IoT.</span><span class="sxs-lookup"><span data-stu-id="3b78d-151">High-throughput event and telemetry handling for your instrumentation or IoT scenarios.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

[<span data-ttu-id="3b78d-152">Esempi](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [Riferimenti](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-152">Samples](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [Reference](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-event-hubs-java)

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[<span data-ttu-id="3b78d-153">Servizio IoT</span><span class="sxs-lookup"><span data-stu-id="3b78d-153">IoT Service</span></span>](/azure/iot-hub/)

<span data-ttu-id="3b78d-154">Gestione delle identità, invio di messaggi e ricezione di commenti e suggerimenti dai dispositivi registrati con l'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="3b78d-154">Manage identities, send messages, and get feedback from devices registered with your IoT hub.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
[<span data-ttu-id="3b78d-155">Esempi](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [Riferimenti](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-155">Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes</span></span>](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[<span data-ttu-id="3b78d-156">Dispositivo IoT</span><span class="sxs-lookup"><span data-stu-id="3b78d-156">IoT Device</span></span>](/azure/iot-hub/iot-hub-devguide)

<span data-ttu-id="3b78d-157">Invio di un messaggio a un hub IoT dal dispositivo.</span><span class="sxs-lookup"><span data-stu-id="3b78d-157">Send a message to an IoT hub from your device.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

[<span data-ttu-id="3b78d-158">Esempi](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [Riferimenti](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-158">Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes</span></span>](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[<span data-ttu-id="3b78d-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b78d-159">Data Lake Store</span></span>](/azure/data-lake-store/data-lake-store-overview)   
   
<span data-ttu-id="3b78d-160">Acquisizione di dati di qualsiasi dimensione e forma in un'unica posizione per l'esecuzione di analisi.</span><span class="sxs-lookup"><span data-stu-id="3b78d-160">Capture data of any size and shape into a single location for performing analytics.</span></span>    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

[<span data-ttu-id="3b78d-161">Esempi](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [Riferimenti](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-161">Samples](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [Reference](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [Release Notes</span></span>](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[<span data-ttu-id="3b78d-162">AppInsights</span><span class="sxs-lookup"><span data-stu-id="3b78d-162">AppInsights</span></span>](/azure/application-insights/app-insights-overview)

<span data-ttu-id="3b78d-163">Monitoraggio dell'utilizzo e delle app Web e aggiunta di telemetria.</span><span class="sxs-lookup"><span data-stu-id="3b78d-163">Track usage, add telemetry, and monitor your web apps.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

[<span data-ttu-id="3b78d-164">Esempi](/azure/application-insights/app-insights-java-get-started) | [Riferimenti](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-164">Samples](/azure/application-insights/app-insights-java-get-started) | [Reference](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [Release Notes</span></span>](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)

<a name="batch"></a>

### <a name="batchazurebatch"></a>[<span data-ttu-id="3b78d-165">Batch</span><span class="sxs-lookup"><span data-stu-id="3b78d-165">Batch</span></span>](/azure/batch)

<span data-ttu-id="3b78d-166">Esecuzione di applicazioni di calcolo parallele a prestazioni elevate su larga scala nel cloud in modo efficiente.</span><span class="sxs-lookup"><span data-stu-id="3b78d-166">Run large-scale parallel and high-performance computing applications efficiently in the cloud.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

[<span data-ttu-id="3b78d-167">Esempi](https://github.com/azure/azure-batch-samples) | [Riferimenti](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-167">Samples](https://github.com/azure/azure-batch-samples) | [Reference](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [Release Notes</span></span>](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)

<a name="management"></a> 

## <a name="manage-azure-resources"></a><span data-ttu-id="3b78d-168">Gestire le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="3b78d-168">Manage Azure resources</span></span>

<span data-ttu-id="3b78d-169">Creare, aggiornare ed eliminare le risorse di Azure dal codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3b78d-169">Create, update, and delete Azure resources from your application code.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

[<span data-ttu-id="3b78d-170">Esempi](https://github.com/Azure/azure-sdk-for-java#sample-code) | [Riferimenti](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-170">Samples](https://github.com/Azure/azure-sdk-for-java#sample-code) | [Reference](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [Release Notes</span></span>](java-sdk-azure-release-notes.md)

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomen-usazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="3b78d-171">Bus di servizio</span><span class="sxs-lookup"><span data-stu-id="3b78d-171">ServiceBus</span></span>](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) 
   
<span data-ttu-id="3b78d-172">Il bus di servizio è un servizio di piattaforma di messaggistica transazionale di livello aziendale.</span><span class="sxs-lookup"><span data-stu-id="3b78d-172">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

[<span data-ttu-id="3b78d-173">Esempi](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Riferimenti](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Note sulla versione</span><span class="sxs-lookup"><span data-stu-id="3b78d-173">Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-service-bus-java)

