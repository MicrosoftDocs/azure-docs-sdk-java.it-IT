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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892902"
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="2848d-104">Librerie di Gestione traffico di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="2848d-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="2848d-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2848d-105">Overview</span></span>

<span data-ttu-id="2848d-106">[DNS di Azure](/azure/dns/dns-overview) consente di offrire la risoluzione dei nomi di dominio e gestire i record DNS con le credenziali, le API, gli strumenti e la fatturazione usati per gli altri servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="2848d-106">Provide domain name resolution and manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services with [Azure DNS](/azure/dns/dns-overview).</span></span>

<span data-ttu-id="2848d-107">Per iniziare a usare DNS di Azure, vedere [Introduzione a DNS di Azure con l'interfaccia della riga di comando di Azure 2.0](/azure/dns/dns-getstarted-cli).</span><span class="sxs-lookup"><span data-stu-id="2848d-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure CLI 2.0](/azure/dns/dns-getstarted-cli).</span></span>

## <a name="management-api"></a><span data-ttu-id="2848d-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="2848d-108">Management API</span></span>

<span data-ttu-id="2848d-109">Creare zone DNS e aggiungere record alle zone con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="2848d-109">Create DNS zones and add records to zones with the management API.</span></span>

<span data-ttu-id="2848d-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="2848d-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="2848d-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="2848d-111">Example</span></span>

<span data-ttu-id="2848d-112">Creare una zona DNS radice e aggiungere un record CNAME `www` in un gruppo di risorse esistente.</span><span class="sxs-lookup"><span data-stu-id="2848d-112">Create a root DNS zone and add a `www` CNAME record in an existing resource group.</span></span>

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="2848d-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="2848d-113">Explore the Management APIs</span></span>](/java/api/overview/azure/dns/management)

## <a name="samples"></a><span data-ttu-id="2848d-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="2848d-114">Samples</span></span>

[<span data-ttu-id="2848d-115">Ospitare e gestire i domini con DNS di Azure</span><span class="sxs-lookup"><span data-stu-id="2848d-115">Host and manage your domains with Azure DNS</span></span>](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

<span data-ttu-id="2848d-116">Esplorare altri [esempi di codice Java per DNS di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=dns) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="2848d-116">Explore more [sample Java code for Azure DNS](https://azure.microsoft.com/resources/samples/?platform=java&term=dns) you can use in your apps.</span></span>

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
