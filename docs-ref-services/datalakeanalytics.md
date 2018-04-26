---
title: Librerie di Azure Data Lake Analytics per Java
description: Documentazione di riferimento per le librerie di Data Lake Analytics per Java
keywords: Azure, Java, SDK, API, Big Data, Data Lake
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: c14c89f961951d114362adee4fec6239e78cffb3
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a>Librerie di Azure Data Lake Analytics per Java

## <a name="overview"></a>Panoramica

[Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) consente di eseguire processi di analisi di Big Data scalabili fino a set di dati di dimensioni molto grandi.

Per iniziare a usare Azure Data Lake Analytics, vedere [Introduzione ad Azure Data Lake Analytics con Java SDK](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk).

## <a name="management-api"></a>API di gestione

Usare l'API di gestione per gestire account, processi, criteri e cataloghi di Data Lake Analytics.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a>Esempio

Inviare un nuovo processo U-SQL a Data Lake Analytics.

```java
// authenticate with service principal credentials
ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
DataLakeAnalyticsJobManagementClient adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);

// set up job parameters
UUID jobId = java.util.UUID.randomUUID();
USqlJobProperties properties = new USqlJobProperties();
properties.setScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();");
JobInformation parameters = new JobInformation();
parameters.setName("testJob");
parameters.setJobId(jobId);
parameters.setType(JobType.USQL);
parameters.setProperties(properties);

// create the job
JobInformation jobInfo = adlaJobClient.getJobOperations().create(accountName, jobId, parameters).getBody();

```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a>Esempi

[Azure Data Lake Analytics con Java SDK][1] 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

Vedere l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics) degli esempi per Azure Data Lake Analytics.
