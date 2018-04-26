---
title: Librerie di DNS di Azure per Java
description: Documentazione di riferimento per le librerie di gestione di DNS di Azure per Java
keywords: Azure, Java, SDK, API, dominio, DNS, nome, servizio, Domain Name Service
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: dns
ms.openlocfilehash: 2cd8fe7ee4d6a87da32a349fe8f1d2815d3fd36d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="azure-traffic-manager-libraries-for-java"></a>Librerie di Gestione traffico di Azure per Java

## <a name="overview"></a>Panoramica

[DNS di Azure](/azure/dns/dns-overview) consente di offrire la risoluzione dei nomi di dominio e gestire i record DNS con le credenziali, le API, gli strumenti e la fatturazione usati per gli altri servizi di Azure.

Per iniziare a usare DNS di Azure, vedere [Introduzione a DNS di Azure con l'interfaccia della riga di comando di Azure 2.0](/azure/dns/dns-getstarted-cli).

## <a name="management-api"></a>API di gestione

Creare zone DNS e aggiungere record alle zone con l'API di gestione.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>Esempio

Creare una zona DNS radice e aggiungere un record CNAME `www` in un gruppo di risorse esistente.

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/dns/management)

## <a name="samples"></a>Esempi

[Ospitare e gestire i domini con DNS di Azure](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

Esplorare altri [esempi di codice Java per DNS di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=dns) disponibili per l'uso nelle app.

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
