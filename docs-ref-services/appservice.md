---
title: Librerie del servizio app di Azure per Java
description: Distribuzione automatica di app Web nel servizio app di Azure con le API di gestione di Azure.
keywords: Azure, Java, SDK, API, app Web, dispositivi mobili, servizio app
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/09/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appservice
ms.openlocfilehash: adbb527666553ecc3039ce35c035d017f502c801
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="azure-app-service-libraries-for-java"></a>Librerie del servizio app di Azure per Java

## <a name="overview"></a>Panoramica

Distribuire e gestire siti Web, app web e API REST con il [servizio app di Azure](/azure/app-service).

Per iniziare a usare il servizio app di Azure, vedere [Creare la prima app Web Java in Azure](/azure/app-service-web/app-service-web-get-started-java).

## <a name="management-api"></a>API di gestione

Distribuire, ridimensionare e configurare applicazioni nel servizio app di Azure con l'API di gestione.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/appservice/management)

### <a name="example"></a>Esempio

Distribuire un'app Web da un'immagine Docker in un'app Web di Azure in esecuzione in Linux.

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a>Esempi

[Distribuire un'app Web da FTP o GitHub][1]  
[Eseguire uno scambio tra le versioni dell'app e gli slot di distribuzione][2]  
[Configurare un dominio personalizzato][3]   
[Ridimensionare un'app Web in più aree][4]   

Esplorare altri [esempi di codice Java per il servizio app di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice) disponibili per l'uso nelle app.

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
