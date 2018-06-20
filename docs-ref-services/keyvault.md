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
ms.openlocfilehash: 396d02b8bba5878ffb24f5f8994ae29aef36cfdc
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823824"
---
# <a name="azure-key-vault-libraries-for-java"></a>Librerie di Azure Key Vault per Java

## <a name="overview"></a>Panoramica

[Azure Key Vault](/azure/key-vault/) consente di proteggere e gestire le chiavi e i segreti di crittografia usati da servizi e applicazioni cloud.

Per iniziare a usare Azure Key Vault, vedere [Introduzione ad Azure Key Vault](/azure/key-vault/key-vault-get-started).

## <a name="client-library"></a>Libreria client

Creare, aggiornare ed eliminare le chiavi e i segreti contenuti in Azure Key Vault con le librerie client.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```   

## <a name="example"></a>Esempio

Recuperare una [chiave Web JSON](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) da un insieme di credenziali delle chiavi.

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = kvc.getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [Esplorare le API client](/java/api/overview/azure/keyvault/client)


## <a name="management-api"></a>API di gestione

Usare le librerie di gestione di Azure Key Vault per creare insiemi di credenziali delle chiavi, autorizzare le applicazioni e gestire le autorizzazioni. 

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a>Esempio

Autorizzare l'applicazione in esecuzione con l'[entità servizio](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` per elencare e recuperare i segreti da un insieme di credenziali delle chiavi. 

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
> [Esplorare le API di gestione](/java/api/overview/azure/keyvault/management)


## <a name="samples"></a>Esempi

[Gestire gli insiemi di credenziali delle chiavi][1]   

[1]: https://github.com/Azure-Samples/key-vault-java-manage-key-vaults

Esplorare altri [esempi di codice Java per Azure Key Vault](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault) disponibili per l'uso nelle app.
