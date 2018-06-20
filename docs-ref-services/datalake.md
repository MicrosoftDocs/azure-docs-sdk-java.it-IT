---
title: Librerie di Azure Data Lake Store per Java
description: Documentazione di riferimento per le librerie di Data Lake Store per Java
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
ms.openlocfilehash: bcd1fd17759f7d171006d7b2126019d00d06d1db
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823724"
---
# <a name="azure-data-lake-store-libraries-for-java"></a><span data-ttu-id="493ba-104">Librerie di Azure Data Lake Store per Java</span><span class="sxs-lookup"><span data-stu-id="493ba-104">Azure Data Lake Store libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="493ba-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="493ba-105">Overview</span></span>

<span data-ttu-id="493ba-106">Acquisire dati di qualsiasi dimensione, tipo e velocità di inserimento in un'unica posizione per le analisi con [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview).</span><span class="sxs-lookup"><span data-stu-id="493ba-106">Capture data of any size, type, and ingestion speed in a single place for analytics with [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview).</span></span>

<span data-ttu-id="493ba-107">Per iniziare a usare Data Lake Store, vedere [Introduzione ad Azure Data Lake Store con Java](/azure/data-lake-store/data-lake-store-get-started-java-sdk).</span><span class="sxs-lookup"><span data-stu-id="493ba-107">To get started with Data Lake Store, see [Get started with Azure Data Lake Store using Java](/azure/data-lake-store/data-lake-store-get-started-java-sdk).</span></span>


## <a name="client-library"></a><span data-ttu-id="493ba-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="493ba-108">Client library</span></span>

<span data-ttu-id="493ba-109">Leggere e scrivere file, configurare autorizzazioni e metadati e gestire file e directory in Data Lake Store con la libreria client.</span><span class="sxs-lookup"><span data-stu-id="493ba-109">Read and write files, set permissions and metadata, and manage files and directories in Data Lake Store with the client library.</span></span>

<span data-ttu-id="493ba-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="493ba-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="493ba-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="493ba-111">Example</span></span>

<span data-ttu-id="493ba-112">Creare un client Data Lake da un nome di dominio completo e un token di accesso OAuth2, quindi creare un file in Data Lake e scrivere nel file.</span><span class="sxs-lookup"><span data-stu-id="493ba-112">Create a Data Lake client from a fully qualified domain name and OAuth2 access token, then create a file in Data Lake and write to it.</span></span>

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="493ba-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="493ba-113">Explore the Client APIs</span></span>](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a><span data-ttu-id="493ba-114">API di gestione</span><span class="sxs-lookup"><span data-stu-id="493ba-114">Management API</span></span>

<span data-ttu-id="493ba-115">Usare l'API di gestione per gestire gli account, le regole del firewall e i provider di identità attendibili di Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="493ba-115">Use the management API to manage Data Lake Store accounts, firewall rules, and trusted identity providers.</span></span>

<span data-ttu-id="493ba-116">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="493ba-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="493ba-117">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="493ba-117">Explore the Management APIs</span></span>](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a><span data-ttu-id="493ba-118">Esempi</span><span class="sxs-lookup"><span data-stu-id="493ba-118">Samples</span></span>

<span data-ttu-id="493ba-119">[Azure Data Lake Get Started][1] (Introduzione ad Azure Data Lake)</span><span class="sxs-lookup"><span data-stu-id="493ba-119">[Azure Data Lake Get Started][1]</span></span> 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

<span data-ttu-id="493ba-120">Esplorare altri [esempi di codice Java per Azure Data Lake Store](https://azure.microsoft.com/resources/samples/?platform=java&term=lake) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="493ba-120">Explore more [sample Java code for Azure Data Lake Store](https://azure.microsoft.com/resources/samples/?platform=java&term=lake) you can use in your apps.</span></span>
