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
# <a name="azure-data-lake-store-libraries-for-java"></a>Librerie di Azure Data Lake Store per Java

## <a name="overview"></a>Panoramica

Acquisire dati di qualsiasi dimensione, tipo e velocità di inserimento in un'unica posizione per le analisi con [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview).

Per iniziare a usare Data Lake Store, vedere [Introduzione ad Azure Data Lake Store con Java](/azure/data-lake-store/data-lake-store-get-started-java-sdk).


## <a name="client-library"></a>Libreria client

Leggere e scrivere file, configurare autorizzazioni e metadati e gestire file e directory in Data Lake Store con la libreria client.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a>Esempio

Creare un client Data Lake da un nome di dominio completo e un token di accesso OAuth2, quindi creare un file in Data Lake e scrivere nel file.

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
> [Esplorare le API client](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a>API di gestione

Usare l'API di gestione per gestire gli account, le regole del firewall e i provider di identità attendibili di Data Lake Store.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a>Esempi

[Azure Data Lake Get Started][1] (Introduzione ad Azure Data Lake) 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

Esplorare altri [esempi di codice Java per Azure Data Lake Store](https://azure.microsoft.com/resources/samples/?platform=java&term=lake) disponibili per l'uso nelle app.
