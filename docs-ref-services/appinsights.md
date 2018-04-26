---
title: Librerie di Azure Application Insights per Java
description: Documentazione di riferimento per l'API di gestione Java per Azure Appplication Insights
keywords: Azure, Java, SDK, API, AppInsights, telemetria, diagnostica, traccia, log, prestazioni
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/21/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appinsights
ms.openlocfilehash: d881ff66ad806e13f7d2cbafff6ce85c23240304
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="azure-application-insights-libraries-for-java"></a>Librerie di Azure Application Insights per Java

## <a name="overview"></a>Panoramica

Rilevare, valutare e diagnosticare i problemi nelle app e nei servizi Web con [Application Insights](/azure/application-insights/app-insights-overview).

Per iniziare a usare Application Insights, vedere [Introduzione ad Application Insights in un progetto Web Java](/azure/application-insights/app-insights-java-get-started).

## <a name="client-library"></a>Libreria client

Aggiungere la telemetria per tenere traccia di eventi, eccezioni e metriche utente nelle app con la libreria client di Application Insights.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a>Esempio

Creare una nuova voce di metrica e registrare il rispettivo valore.

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [Esplorare le API client](/java/api/overview/azure/appinsights/client)

## <a name="samples"></a>Esempi

Esplorare altri [esempi di codice Java per Application Insights](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java) disponibili per l'uso nelle app.
