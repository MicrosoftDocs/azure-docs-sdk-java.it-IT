---
title: Librerie di Azure Resource Manager per Java
description: Documentazione di riferimento per le librerie di Resource Manager per Java
keywords: Azure, Java, SDK, API, gruppi di risorse, Azure Resource Manager, Resource Manager
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 4b73f6b03258e7d99bcca235cc2728d5e085b32a
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2017
---
# <a name="azure-resource-manager-libraries-for-java"></a>Librerie di Azure Resource Manager per Java

## <a name="overview"></a>Panoramica

[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) consente di distribuire, monitorare e gestire le risorse nei gruppi.

## <a name="management-api"></a>API di gestione

Usare l'API di gestione per creare gruppi di risorse e distribuire le risorse dai modelli.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a>Esempio

Creare un nuovo gruppo di risorse nell'area di Azure Stati Uniti orientali.

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a>Esempi

[Gestire gruppi di risorse di Azure con Java][1] 
[Distribuire le risorse con un modello di Azure Resource Manager][2]

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=java&term=resource) degli esempi di Azure Resource Manager.
