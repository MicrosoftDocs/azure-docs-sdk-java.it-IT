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
# <a name="partner-center-libraries-for-java"></a>Librerie di Centro per i partner per Java

## <a name="overview"></a>Panoramica

Le librerie di Centro per i Partner per Java sono raccolte in un SDK e consentono di interagire con il servizio Centro per i Partner di Microsoft. I partner possono così eseguire le operazioni del Centro per i partner a livello di codice. Si tratta dell'aggiunta più recente alla raccolta esistente di API REST e di .NET SDK.

## <a name="client-library"></a>Libreria client

Gestire le risorse nel Centro per i Partner usando la libreria client.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```xml
<dependency>
    <groupId>com.microsoft.store</groupId>
    <artifactId>partnercenter</artifactId>
    <version>1.8.0</version>
</dependency>
```   

### <a name="example"></a>Esempio

Recuperare un elenco completo di clienti associati al rivenditore autenticato.

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

## <a name="samples"></a>Esempi

[Scenari supportati](https://docs.microsoft.com/partner-center/develop/scenarios)
[Applicazione console di esempio](https://github.com/Microsoft/Partner-Center-Java-Samples)  