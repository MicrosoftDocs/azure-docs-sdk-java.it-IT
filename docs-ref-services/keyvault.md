---
title: Librerie di Azure Key Vault per Java
description: Panoramica delle librerie di Azure Key Vault per Java
keywords: Azure, Java, SDK, API, Key Vault, proteggere, chiavi, segreti, insieme di credenziali
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: keyvault
ms.openlocfilehash: 84ea77a19c326409f453f62359cf46c90398daeb
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799927"
---
# <a name="azure-key-vault-libraries-for-java"></a><span data-ttu-id="38739-104">Librerie di Azure Key Vault per Java</span><span class="sxs-lookup"><span data-stu-id="38739-104">Azure Key Vault libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="38739-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="38739-105">Overview</span></span>

<span data-ttu-id="38739-106">[Azure Key Vault](/azure/key-vault/) consente di proteggere e gestire le chiavi e i segreti di crittografia usati da servizi e applicazioni cloud.</span><span class="sxs-lookup"><span data-stu-id="38739-106">Safeguard and manage cryptographic keys and secrets used by cloud applications and services with [Azure Key Vault](/azure/key-vault/).</span></span>

<span data-ttu-id="38739-107">Per iniziare a usare Azure Key Vault, vedere [Introduzione ad Azure Key Vault](/azure/key-vault/key-vault-get-started).</span><span class="sxs-lookup"><span data-stu-id="38739-107">To get started with Azure Key Vault, see [Get started with Azure Key Vault](/azure/key-vault/key-vault-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="38739-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="38739-108">Client library</span></span>

<span data-ttu-id="38739-109">Creare, aggiornare ed eliminare le chiavi e i segreti contenuti in Azure Key Vault con le librerie client.</span><span class="sxs-lookup"><span data-stu-id="38739-109">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="38739-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="38739-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.1.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="38739-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="38739-111">Example</span></span>

<span data-ttu-id="38739-112">Recuperare una [chiave Web JSON](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) da un insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="38739-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = kvc.getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="38739-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="38739-113">Explore the Client APIs</span></span>](/java/api/overview/azure/keyvault/client)


## <a name="management-api"></a><span data-ttu-id="38739-114">API di gestione</span><span class="sxs-lookup"><span data-stu-id="38739-114">Management API</span></span>

<span data-ttu-id="38739-115">Usare le librerie di gestione di Azure Key Vault per creare insiemi di credenziali delle chiavi, autorizzare le applicazioni e gestire le autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="38739-115">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="38739-116">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="38739-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.15.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="38739-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="38739-117">Example</span></span>

<span data-ttu-id="38739-118">Autorizzare l'applicazione in esecuzione con l'[entità servizio](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` per elencare e recuperare i segreti da un insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="38739-118">Authorize and application running with [service principal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` to list and retrieve secrets from a key vault.</span></span> 

```java
vault1 = vault1.update()
            .defineAccessPolicy()
                .forServicePrincipal(clientId)
                .allowKeyAllPermissions()
                .allowSecretPermissions(SecretPermissions.GET)
                .allowSecretPermissions(SecretPermissions.LIST)
                .attach()
            .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="38739-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="38739-119">Explore the Management APIs</span></span>](/java/api/overview/azure/keyvault/management)


## <a name="samples"></a><span data-ttu-id="38739-120">Esempi</span><span class="sxs-lookup"><span data-stu-id="38739-120">Samples</span></span>

<span data-ttu-id="38739-121">Esplorare altri [esempi di codice Java per Azure Key Vault](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="38739-121">Explore more [sample Java code for Azure Key Vault](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault) you can use in your apps.</span></span>
