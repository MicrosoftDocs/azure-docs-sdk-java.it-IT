---
title: Librerie della rete di Azure per Java
description: Documentazione di riferimento per le librerie di gestione della rete di Azure per Java
keywords: Azure, Java, SDK, API, rete, bilanciamento del carico, rete virtuale, subnet
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: 4eb4f583da686508db9fb0c4346f2525502f669d
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593326"
---
# <a name="azure-network-libraries-for-java"></a>Librerie della rete di Azure per Java

## <a name="overview"></a>Panoramica

Connettere le risorse di Azure, filtrare e bilanciare il traffico e gestire il routing con la [rete di Azure](/azure/networking/networking-overview).

Per iniziare a usare la rete di Azure, vedere [Creare la prima rete virtuale](/azure/virtual-network/virtual-network-get-started-vnet-subnet).

## <a name="management-api"></a>API di gestione

Creare e gestire [reti virtuali](/azure/virtual-network/virtual-networks-overview) di Azure, route [ExpressRoute](/azure/expressroute/) e [gateway applicazione](/azure/application-gateway/) con l'API di gestione.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>Esempio

Creare una nuova rete virtuale con una sola subnet.

```java
Network virtualNetwork1 = azure.networks().define(vnetName1)
        .withRegion(Region.US_EAST)
        .withExistingResourceGroup("myResourceGroup")
        .withAddressSpace("192.168.0.0/16")
        .defineSubnet("myVirtualNetwork")
            .withAddressPrefix("192.168.2.0/24")
            .attach()
        .create();
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/networking/management)

## <a name="samples"></a>Esempi

[Gestire le reti virtuali](https://github.com/Azure-Samples/network-java-manage-virtual-network)   
[Gestire le interfacce di rete](https://github.com/Azure-Samples/network-java-manage-network-interface)   
[Gestire i gateway applicazione](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways)   
[Gestire i servizi di bilanciamento del carico con connessione Internet](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

Esplorare altri [esempi di codice Java per la rete di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=network) disponibili per l'uso nelle app.
