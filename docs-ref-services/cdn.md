---
title: Librerie della rete CDN di Azure per Java
description: Documentazione di riferimento per le librerie di gestione della rete CDN per Java
keywords: Azure, Java, SDK, API, contenuti, distribuzione, rete, CDN
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cdn
ms.openlocfilehash: 91df958d2d78fb4fd959c228b28c6ae003716be6
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2017
---
# <a name="azure-cdn-libraries-for-java"></a>Librerie della rete CDN di Azure per Java

## <a name="overview"></a>Panoramica

La [rete per la distribuzione di contenuti (CDN) di Azure](/azure/cdn/cdn-overview) consente di memorizzare nella cache il contenuto Web statico in località strategiche per offrire la massima velocità effettiva agli utenti.

Per iniziare a usare la rete CDN di Azure, vedere [Introduzione alla rete CDN di Azure](/azure/cdn/cdn-create-new-endpoint).

## <a name="management-api"></a>API di gestione

Creare profili CDN, definire gli endpoint e aggiungere contenuto alla rete CDN usando l'API di gestione.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>Esempio

Creare un profilo CDN, assegnare gli endpoint e caricare contenuto nella rete CDN.

```java
CdnProfile profile = azure.cdnProfiles().define("testCDN")
        .withRegion(Region.US_EAST)
        .withNewResourceGroup()
        .withStandardVerizonSku()
        .withNewEndpoint("webAppHostname1")
        .withNewEndpoint("webAppHostname2")
        .create();

List<String> contentList = new ArrayList<String>();
contentList.add("/client.js");
contentList.add("/media/fabrikam_logo.png");

for (CdnEndpoint endpoint : profile.endpoints().values()) {
    endpoint.loadContent(contentList);
}
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a>Esempi

[Gestire reti CDN con Java](https://github.com/Azure-Samples/cdn-java-manage-cdn)

Esplorare altri [esempi di codice Java per la rete CDN di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn) disponibili per l'uso nelle app.