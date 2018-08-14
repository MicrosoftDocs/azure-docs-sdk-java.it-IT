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
ms.openlocfilehash: 5c8bb4b81080461285551573eefc0d76b47b2d3d
ms.sourcegitcommit: 61030d025614b084e897809e603b2ec79900ec8d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2018
ms.locfileid: "30302549"
---
# <a name="azure-libraries-for-java"></a>Librerie di Azure per Java

Le librerie di Azure aiutano a usare i servizi di Azure nelle app Java con interfacce native. Ogni libreria è indipendente e può essere usata separatamente dalle altre.

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [Archiviazione di Azure](#azure-storage) | [Database SQL](#sql-database)  | [Cache Redis](#redis-cache)   | [Azure Cosmos DB](#cosmos-db) |
| [Bus di servizio](#servicebus)  | [Azure Active Directory](#azuread) | [Insieme di credenziali di chiave](#keyvault)  | [Hub eventi](#eventhub)
| [Servizio IoT](#iotservice) | [Dispositivo IoT](#iotdevice) | [Data Lake](#datalake)  | [AppInsights](#appinsights) | 
| [Batch](#batch) | [Manage Azure resources (Gestire risorse di Azure)](#management) |

## <a name="install-with-maven"></a>Eseguire l'installazione con Maven

Aggiungere una voce di dipendenza in `pom.xml` per importare una libreria nel progetto [Maven](https://maven.apache.org).

Per includere, ad esempio, la versione più recente delle [librerie di gestione di Azure per Java](#management):

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

Sono supportati anche altri strumenti di compilazione Java, come Gradle, ma i passaggi di installazione non vengono descritti in questo articolo. Vedere la documentazione per lo strumento di compilazione in uso per informazioni su come usare le importazioni di Maven.

## <a name="azure-service-libraries"></a>Librerie di servizi di Azure

Integrare i servizi di Azure per aggiungere funzionalità alle app usando queste librerie. Per altre informazioni sulla compilazione di app con i servizi di Azure, vedere il [Centro per sviluppatori Java](https://azure.microsoft.com/develop/java).

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[Archiviazione di Azure](/azure/storage/storage-introduction)  

Archiviazione dei dati e messaggistica per le applicazioni.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

[Esempi](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [Riferimenti](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [Note sulla versione](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[Database SQL](/azure/sql-database/sql-database-technical-overview)

Driver JDBC per il database SQL di Azure.

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[Esempi](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [Riferimenti](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [Note sulla versione](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[Cache Redis](https://azure.microsoft.com/services/cache/)

Archivio di coppie chiave-valore a bassa latenza e ad alte prestazioni.

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

[Esempi](/azure/redis-cache/cache-java-get-started) | [Riferimenti](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [Note sulla versione](https://github.com/xetorthio/jedis/releases)  

<a name="cosmos-db"></a>

### <a name="azure-cosmos-dbazurecosmos-dbintroduction"></a>[Azure Cosmos DB](/azure/cosmos-db/introduction)

Database NoSQL scalabile con documenti JSON e una sintassi di query SQL o JavaScript.   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

[Esempi](/azure/cosmos-db/sql-api-java-application) | [Riferimenti](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [Note sulla versione](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[Bus di servizio](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 Il bus di servizio è un servizio di piattaforma di messaggistica transazionale di livello aziendale.
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 [Esempi](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Riferimenti](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Note sulla versione](https://github.com/Azure/azure-service-bus-java)   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[Azure Active Directory](/azure/active-directory/active-directory-whatis)   

Gestione delle identità e accesso sicuro per le applicazioni.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
[Esempi](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [Riferimenti](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [Note sulla versione](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[Insieme di credenziali di chiave](/azure/key-vault) 

Accesso sicuro a chiavi e segreti dalle applicazioni. 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

[Esempi](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [Riferimenti](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [Note sulla versione](https://github.com/Azure/azure-keyvault-java) 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[Hub eventi](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
Gestione di telemetria ed eventi con velocità effettiva elevata per la strumentazione o gli scenari IoT.

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

[Esempi](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [Riferimenti](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [Note sulla versione](https://github.com/Azure/azure-event-hubs-java)

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[Servizio IoT](/azure/iot-hub/)

Gestione delle identità, invio di messaggi e ricezione di commenti e suggerimenti dai dispositivi registrati con l'hub IoT.

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
[Esempi](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [Riferimenti](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Note sulla versione](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[Dispositivo IoT](/azure/iot-hub/iot-hub-devguide)

Invio di un messaggio a un hub IoT dal dispositivo.  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

[Esempi](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [Riferimenti](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Note sulla versione](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[Data Lake Store](/azure/data-lake-store/data-lake-store-overview)   
   
Acquisizione di dati di qualsiasi dimensione e forma in un'unica posizione per l'esecuzione di analisi.    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

[Esempi](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [Riferimenti](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [Note sulla versione](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[AppInsights](/azure/application-insights/app-insights-overview)

Monitoraggio dell'utilizzo e delle app Web e aggiunta di telemetria.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

[Esempi](/azure/application-insights/app-insights-java-get-started) | [Riferimenti](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [Note sulla versione](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)

<a name="batch"></a>

### <a name="batchazurebatch"></a>[Batch](/azure/batch)

Esecuzione di applicazioni di calcolo parallele a prestazioni elevate su larga scala nel cloud in modo efficiente.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

[Esempi](https://github.com/azure/azure-batch-samples) | [Riferimenti](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [Note sulla versione](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)

<a name="management"></a> 

## <a name="manage-azure-resources"></a>Gestire le risorse di Azure

Creare, aggiornare ed eliminare le risorse di Azure dal codice dell'applicazione.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

[Esempi](https://github.com/Azure/azure-sdk-for-java#sample-code) | [Riferimenti](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [Note sulla versione](java-sdk-azure-release-notes.md)

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomazureservice-bus-messagingservice-bus-messaging-overview"></a>[Bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) 
   
Il bus di servizio è un servizio di piattaforma di messaggistica transazionale di livello aziendale.

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

[Esempi](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Riferimenti](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Note sulla versione](https://github.com/Azure/azure-service-bus-java)

