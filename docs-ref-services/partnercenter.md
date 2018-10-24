---
title: Partner Center Java SDK
description: Documentazione di riferimento per Partner Center Java SDK
keywords: API, CSP, Provider di soluzioni cloud, Java, Centro per i partner, SDK
author: iswillia
ms.author: iswillia
manager: lesliep
ms.date: 10/09/2018
ms.topic: reference
ms.prod: partnercenter
ms.technology: partnercenter
ms.devlang: java
ms.openlocfilehash: 2adfbdbd37d6eb91e48ee37091f951b3f7d59eb9
ms.sourcegitcommit: 4da7d2f470331feb7d76a1658f5825f365cdba9a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49121644"
---
# <a name="partner-center-libraries-for-java"></a><span data-ttu-id="6b62a-104">Librerie di Centro per i partner per Java</span><span class="sxs-lookup"><span data-stu-id="6b62a-104">Partner Center libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="6b62a-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6b62a-105">Overview</span></span>

<span data-ttu-id="6b62a-106">Le librerie di Centro per i Partner per Java sono raccolte in un SDK e consentono di interagire con il servizio Centro per i Partner di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6b62a-106">The Partner Center libraries for Java is a SDK that provides the ability to interact with Microsoft's Partner Center service.</span></span> <span data-ttu-id="6b62a-107">I partner possono così eseguire le operazioni del Centro per i partner a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="6b62a-107">This enables partners to perform the Partner Center operations programmatically.</span></span> <span data-ttu-id="6b62a-108">Si tratta dell'aggiunta più recente alla raccolta esistente di API REST e di .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="6b62a-108">This is the latest addition to existing portfolio of REST APIs and the .NET SDK.</span></span>

## <a name="client-library"></a><span data-ttu-id="6b62a-109">Libreria client</span><span class="sxs-lookup"><span data-stu-id="6b62a-109">Client library</span></span>

<span data-ttu-id="6b62a-110">Gestire le risorse nel Centro per i Partner usando la libreria client.</span><span class="sxs-lookup"><span data-stu-id="6b62a-110">Manage resources in Partner Center using the client library.</span></span>

<span data-ttu-id="6b62a-111">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="6b62a-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```xml
<dependency>
    <groupId>com.microsoft.store</groupId>
    <artifactId>partnercenter</artifactId>
    <version>1.8.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="6b62a-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="6b62a-112">Example</span></span>

<span data-ttu-id="6b62a-113">Recuperare un elenco completo di clienti associati al rivenditore autenticato.</span><span class="sxs-lookup"><span data-stu-id="6b62a-113">Retrieves a complete list of customer associated with the authenticated reseller.</span></span>

```java
IPartnerCredentials appCredentials = PartnerCredentials.getInstance().generateByApplicationCredentials('YOUR_APP_ID', 'YOUR_APP_SECRET', 'YOUR_TENANT_ID');
IAggregatePartner partnerOperations = PartnerService.getInstance().createPartnerOperations(appCredentials);

// Query the customers
SeekBasedResourceCollection<Customer> customersPage = partnerOperations.getCustomers().query(QueryFactory.getInstance().buildIndexedQuery(100));
this.getContext().getConsoleHelper().stopProgress();

// Create a customer enumerator which will aid us in traversing the customer pages
IResourceCollectionEnumerator<SeekBasedResourceCollection<Customer>> customersEnumerator =
    partnerOperations.getEnumerators().getCustomers().create( customersPage );

int pageNumber = 1;

while ( customersEnumerator.hasValue() )
{
    /*
     * Perform some operation with the current page of customers using 
     * customersEnumerator.getCurrent()  
     */

    // Get the next page of customers
    customersEnumerator.next();
}
```

## <a name="samples"></a><span data-ttu-id="6b62a-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="6b62a-114">Samples</span></span>

<span data-ttu-id="6b62a-115">[Scenari supportati](https://docs.microsoft.com/partner-center/develop/scenarios)
[Applicazione console di esempio](https://github.com/Microsoft/Partner-Center-Java-Samples)</span><span class="sxs-lookup"><span data-stu-id="6b62a-115">[Supported scenarios](https://docs.microsoft.com/partner-center/develop/scenarios)
[Sample console application](https://github.com/Microsoft/Partner-Center-Java-Samples)</span></span>  