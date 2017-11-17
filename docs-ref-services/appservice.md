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
ms.openlocfilehash: 7e1d7eed9d8fa8d2f872f2902e2ce3f2b3dab7b6
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2017
---
# <a name="azure-app-service-libraries-for-java"></a><span data-ttu-id="e342c-104">Librerie del servizio app di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="e342c-104">Azure App Service libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e342c-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e342c-105">Overview</span></span>

<span data-ttu-id="e342c-106">Distribuire e gestire siti Web, app web e API REST con il [servizio app di Azure](/azure/app-service).</span><span class="sxs-lookup"><span data-stu-id="e342c-106">Deploy and manage websites, web applications, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="e342c-107">Per iniziare a usare il servizio app di Azure, vedere [Creare la prima app Web Java in Azure](/azure/app-service-web/app-service-web-get-started-java).</span><span class="sxs-lookup"><span data-stu-id="e342c-107">To get started with Azure App Service, see [Create your first Java web app in Azure](/azure/app-service-web/app-service-web-get-started-java).</span></span>

## <a name="management-api"></a><span data-ttu-id="e342c-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="e342c-108">Management API</span></span>

<span data-ttu-id="e342c-109">Distribuire, ridimensionare e configurare applicazioni nel servizio app di Azure con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="e342c-109">Deploy, scale, and configure applications in Azure App Service with the management API.</span></span>

<span data-ttu-id="e342c-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e342c-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [<span data-ttu-id="e342c-111">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="e342c-111">Explore the Management APIs</span></span>](/java/api/overview/azure)

### <a name="example"></a><span data-ttu-id="e342c-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="e342c-112">Example</span></span>

<span data-ttu-id="e342c-113">Distribuire un'app Web da un'immagine Docker in un'app Web di Azure in esecuzione in Linux.</span><span class="sxs-lookup"><span data-stu-id="e342c-113">Deploy a webapp from a Docker image into an Azure Web App running on Linux.</span></span>

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a><span data-ttu-id="e342c-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="e342c-114">Samples</span></span>

<span data-ttu-id="e342c-115">[Distribuire un'app Web da FTP o GitHub][1]</span><span class="sxs-lookup"><span data-stu-id="e342c-115">[Deploy a web app from FTP or GitHub][1]</span></span>  
<span data-ttu-id="e342c-116">[Eseguire uno scambio tra le versioni dell'app e gli slot di distribuzione][2]</span><span class="sxs-lookup"><span data-stu-id="e342c-116">[Swap between app versions with deployment slots][2]</span></span>  
<span data-ttu-id="e342c-117">[Configurare un dominio personalizzato][3] </span><span class="sxs-lookup"><span data-stu-id="e342c-117">[Configure a custom domain][3] </span></span>  
<span data-ttu-id="e342c-118">[Ridimensionare un'app Web in più aree][4]</span><span class="sxs-lookup"><span data-stu-id="e342c-118">[Scale a web app across multiple regions][4]</span></span>   

<span data-ttu-id="e342c-119">Esplorare altri [esempi di codice Java per il servizio app di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="e342c-119">Explore more [sample Java code for Azure App Service](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice) you can use in your apps.</span></span>

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
