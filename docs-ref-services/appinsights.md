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
ms.locfileid: "31823764"
---
# <a name="azure-application-insights-libraries-for-java"></a><span data-ttu-id="17c88-104">Librerie di Azure Application Insights per Java</span><span class="sxs-lookup"><span data-stu-id="17c88-104">Azure Application Insights libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="17c88-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="17c88-105">Overview</span></span>

<span data-ttu-id="17c88-106">Rilevare, valutare e diagnosticare i problemi nelle app e nei servizi Web con [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="17c88-106">Detect, triage, and diagnose issues in your web apps and services with [Application Insights](/azure/application-insights/app-insights-overview).</span></span>

<span data-ttu-id="17c88-107">Per iniziare a usare Application Insights, vedere [Introduzione ad Application Insights in un progetto Web Java](/azure/application-insights/app-insights-java-get-started).</span><span class="sxs-lookup"><span data-stu-id="17c88-107">To get started with Application Insights, see [Get started with Application Insights in a Java web project](/azure/application-insights/app-insights-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="17c88-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="17c88-108">Client library</span></span>

<span data-ttu-id="17c88-109">Aggiungere la telemetria per tenere traccia di eventi, eccezioni e metriche utente nelle app con la libreria client di Application Insights.</span><span class="sxs-lookup"><span data-stu-id="17c88-109">Add telemetry to track events, exceptions, and user metrics in your apps with the Application Insights client library.</span></span>

<span data-ttu-id="17c88-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="17c88-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="17c88-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="17c88-111">Example</span></span>

<span data-ttu-id="17c88-112">Creare una nuova voce di metrica e registrare il rispettivo valore.</span><span class="sxs-lookup"><span data-stu-id="17c88-112">Create a new metric entry and record a value for it.</span></span>

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="17c88-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="17c88-113">Explore the Client APIs</span></span>](/java/api/overview/azure/appinsights/client)

## <a name="samples"></a><span data-ttu-id="17c88-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="17c88-114">Samples</span></span>

<span data-ttu-id="17c88-115">Esplorare altri [esempi di codice Java per Application Insights](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="17c88-115">Explore more [sample Java code for Application Insights](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java) you can use in your apps.</span></span>
