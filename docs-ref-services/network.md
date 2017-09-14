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
ms.openlocfilehash: de03cf7c073c3a0dd636e23a4554987e7cad11f7
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2017
---
# <a name="azure-network-libraries-for-java"></a><span data-ttu-id="b5d81-104">Librerie della rete di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="b5d81-104">Azure Network libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="b5d81-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b5d81-105">Overview</span></span>

<span data-ttu-id="b5d81-106">Connettere le risorse di Azure, filtrare e bilanciare il traffico e gestire il routing con la [rete di Azure](/azure/networking/networking-overview).</span><span class="sxs-lookup"><span data-stu-id="b5d81-106">Connect Azure resources, filter and balance traffic, and manage routing with [Azure Networking](/azure/networking/networking-overview).</span></span>

<span data-ttu-id="b5d81-107">Per iniziare a usare la rete di Azure, vedere [Creare la prima rete virtuale](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span><span class="sxs-lookup"><span data-stu-id="b5d81-107">To get started with Azure Networking, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-api"></a><span data-ttu-id="b5d81-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="b5d81-108">Management API</span></span>

<span data-ttu-id="b5d81-109">Creare e gestire [reti virtuali](/azure/virtual-network/virtual-networks-overview) di Azure, route [ExpressRoute](/azure/expressroute/) e [gateway applicazione](/azure/application-gateway/) con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="b5d81-109">Create and manage Azure [virtual networks](/azure/virtual-network/virtual-networks-overview) , [ExpressRoutes](/azure/expressroute/) , and [Application Gateways](/azure/application-gateway/) with the management API.</span></span>

<span data-ttu-id="b5d81-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="b5d81-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.2.1</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="b5d81-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="b5d81-111">Example</span></span>

<span data-ttu-id="b5d81-112">Creare una nuova rete virtuale con una sola subnet.</span><span class="sxs-lookup"><span data-stu-id="b5d81-112">Create a new virtual network with a single subnet.</span></span>

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
> [<span data-ttu-id="b5d81-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="b5d81-113">Explore the Management APIs</span></span>](/java/api/overview/azure/networking/managementapi)

## <a name="samples"></a><span data-ttu-id="b5d81-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="b5d81-114">Samples</span></span>

<span data-ttu-id="b5d81-115">[Gestire le reti virtuali](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span><span class="sxs-lookup"><span data-stu-id="b5d81-115">[Manage virtual networks](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span></span>  
<span data-ttu-id="b5d81-116">[Gestire le interfacce di rete](https://github.com/Azure-Samples/network-java-manage-network-interface) </span><span class="sxs-lookup"><span data-stu-id="b5d81-116">[Manage network interfaces](https://github.com/Azure-Samples/network-java-manage-network-interface) </span></span>  
<span data-ttu-id="b5d81-117">[Gestire i gateway applicazione](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span><span class="sxs-lookup"><span data-stu-id="b5d81-117">[Manage Application Gateways](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span></span>  
[<span data-ttu-id="b5d81-118">Gestire i servizi di bilanciamento del carico con connessione Internet</span><span class="sxs-lookup"><span data-stu-id="b5d81-118">Manage internet facing load balancers</span></span>](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

<span data-ttu-id="b5d81-119">Esplorare altri [esempi di codice Java per la rete di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=network) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="b5d81-119">Explore more [sample Java code for Azure Networking](https://azure.microsoft.com/resources/samples/?platform=java&term=network) you can use in your apps.</span></span>
