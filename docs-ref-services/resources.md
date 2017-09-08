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
ms.openlocfilehash: b3aa5830b91f4a00da64db2656d024d3c4fc51f9
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-resource-manager-libraries-for-java"></a><span data-ttu-id="bc5ab-104">Librerie di Azure Resource Manager per Java</span><span class="sxs-lookup"><span data-stu-id="bc5ab-104">Azure Resource Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="bc5ab-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="bc5ab-105">Overview</span></span>

<span data-ttu-id="bc5ab-106">[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) consente di distribuire, monitorare e gestire le risorse nei gruppi.</span><span class="sxs-lookup"><span data-stu-id="bc5ab-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="bc5ab-107">API di gestione</span><span class="sxs-lookup"><span data-stu-id="bc5ab-107">Management API</span></span>

<span data-ttu-id="bc5ab-108">Usare l'API di gestione per creare gruppi di risorse e distribuire le risorse dai modelli.</span><span class="sxs-lookup"><span data-stu-id="bc5ab-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

<span data-ttu-id="bc5ab-109">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="bc5ab-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.1.2</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="bc5ab-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="bc5ab-110">Example</span></span>

<span data-ttu-id="bc5ab-111">Creare un nuovo gruppo di risorse nell'area di Azure Stati Uniti orientali.</span><span class="sxs-lookup"><span data-stu-id="bc5ab-111">Create a new resource group in the Azure Eastern US region.</span></span>

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="bc5ab-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="bc5ab-112">Explore the Management APIs</span></span>](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a><span data-ttu-id="bc5ab-113">Esempi</span><span class="sxs-lookup"><span data-stu-id="bc5ab-113">Samples</span></span>

<span data-ttu-id="bc5ab-114">[Gestire gruppi di risorse di Azure con Java][1] 
[Distribuire le risorse con un modello di Azure Resource Manager][2]</span><span class="sxs-lookup"><span data-stu-id="bc5ab-114">[Manage Azure Resource Groups with Java][1] 
[Deploy resources using an ARM template][2]</span></span>

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

<span data-ttu-id="bc5ab-115">Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=java&term=resource) degli esempi di Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bc5ab-115">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=resource) of Azure Resource Manager samples.</span></span>
