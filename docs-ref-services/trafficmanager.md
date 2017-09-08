---
title: Librerie di Gestione traffico di Azure per Java
description: Documentazione di riferimento per le librerie di Gestione traffico per Java
keywords: Azure, Java, SDK, API, servizio di bilanciamento del carico, distribuzione del carico, rete, Gestione traffico
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: 3b9c8f0e3bd5e8feb5a819a7c2583ffbbcf53e38
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a>Librerie di Gestione traffico di Azure per Java

## <a name="overview"></a>Panoramica

[Gestione traffico di Azure](/azure/traffic-manager/traffic-manager-overview) consente di controllare la distribuzione del traffico utente per gli endpoint servizio in diversi data center.

Per iniziare a usare Gestione traffico di Azure, vedere [Creazione di un profilo di Gestione traffico](/azure/traffic-manager/traffic-manager-create-profile).

## <a name="management-api"></a>API di gestione

Creare profili di Gestione traffico, definire gli endpoint e modificare il metodo di routing con l'API di gestione. 

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.1.2</version>
</dependency>
```   

## <a name="example"></a>Esempio

Creare un profilo di Gestione traffico e assegnare un endpoint singolo.

```java
TrafficManagerProfile tmProfile = azure.trafficManagerProfiles().define("testTMProfile")
        .withNewResourceGroup(Region.US_EAST)
        .withLeafDomainLabel("testTMProfile")
        .withPriorityBasedRouting()
        .defineAzureTargetEndpoint("endpoint")
        .toResourceId(webAppId)
        .withRoutingPriority(1)
        .attach()
        .create();
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/trafficmanager/managementapi)

## <a name="samples"></a>Esempi

[Bilanciare il traffico di app Web tra più aree](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

Esplorare altri [esempi di codice Java per Gestione traffico di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) disponibili per l'uso nelle app.
