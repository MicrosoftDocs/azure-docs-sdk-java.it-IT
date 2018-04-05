---
title: Eseguire l'autenticazione con le librerie di gestione di Azure per Java
description: Eseguire l'autenticazione con un'entità servizio nelle librerie di gestione di Azure per Java
keywords: Azure, Java, SDK, API, Maven, Gradle, autenticazione, active directory, entità servizio
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 10f457e3-578b-4655-8cd1-51339226ee7d
ms.openlocfilehash: 3808c6d56b04f28c84a89a25219e4ec523f87964
ms.sourcegitcommit: 61030d025614b084e897809e603b2ec79900ec8d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2018
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a>Eseguire l'autenticazione con le librerie di Azure per Java 

## <a name="connect-to-services-with-connection-strings"></a>Connettersi ai servizi con le stringhe di connessione

La maggior parte delle librerie di servizi di Azure usa una stringa di connessione o una chiave di protezione per l'autenticazione. Il database SQL include, ad esempio, le informazioni specificate per nome utente e password nella stringa di connessione JDBC:

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

Archiviazione di Azure usa una chiave di archiviazione per autorizzare l'applicazione:

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

Le stringhe di connessione dei servizi vengono usate per eseguire l'autenticazione ad altri servizi di Azure, ad esempio [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService), [Cache Redis](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started) e [Bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues). Per ottenere le stringhe di connessione, è possibile usare il portale di Azure o l'interfaccia della riga di comando.  È anche possibile usare le librerie di gestione di Azure per Java per eseguire query nelle risorse per creare stringhe di connessione nel codice. 

Questo codice, ad esempio, usa le librerie di gestione per creare una stringa di connessione dell'account di archiviazione:

```java
// create a new storage account
StorageAccount storage = azure.storageAccounts().getByResourceGroup("myResourceGroup","myStorageAccount");

// create a storage container to hold the file
List<StorageAccountKey> keys = storage.getKeys();
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storage.name()
        + ";AccountKey=" + keys.get(0).value()
        + ";EndpointSuffix=core.windows.net";
```

Altre librerie richiedono che l'applicazione venga eseguita con un'[entità servizio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) che autorizza l'esecuzione dell'applicazione con le credenziali concesse. Questa configurazione è simile alla procedura di autenticazione basata su oggetti per le librerie di gestione illustrata di seguito.

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a>Eseguire l'autenticazione con le librerie di gestione di Azure per Java

Sono disponibili due opzioni per autenticare l'applicazione con Azure quando si usano le librerie di gestione Java per creare e gestire le risorse.

### <a name="authenticate-with-an-applicationtokencredentials-object"></a>Eseguire l'autenticazione con un oggetto ApplicationTokenCredentials

Creare un'istanza di `ApplicationTokenCredentials` per fornire le credenziali dell'entità servizio all'oggetto `Azure` di primo livello dal codice.

```java
import com.microsoft.azure.credentials.ApplicationTokenCredentials;
import com.microsoft.azure.AzureEnvironment;

// ...

ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(client, 
        tenant,
        key, 
        AzureEnvironment.AZURE);
        
Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credentials)
        .withDefaultSubscription();
```

`client`, `tenant` e `key` sono gli stessi valori dell'entità servizio usati con l'[autenticazione basata su file](#mgmt-file). Il valore `AzureEnvironment.AZURE` crea le credenziali per il cloud pubblico di Azure. Sostituirlo con un valore diverso se è necessario accedere a un altro cloud (ad esempio, `AzureEnvironment.AZURE_GERMANY`).  

 Leggere i valori dell'entità servizio dalle variabili di ambiente o da un archivio di gestione segreto, ad esempio [Key Vault](/azure/key-vault/key-vault-whatis.md). Non impostare questi valori come stringhe in testo non crittografato nel codice per evitare l'esposizione accidentale delle credenziali nella cronologia del controllo della versione.   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a>Autenticazione basata su file (anteprima)

Il modo più semplice per eseguire l'autenticazione consiste nel creare un file delle proprietà contenente le credenziali per un'[entità servizio di Azure](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) usando il formato seguente:

```text
# sample management library properties file
subscription=########-####-####-####-############
client=########-####-####-####-############
key=XXXXXXXXXXXXXXXX
tenant=########-####-####-####-############
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

- subscription: usare il valore *id* da `az account show` nell'interfaccia della riga di comando di Azure 2.0.
- client: usare il valore *appId* dell'output di un'entità servizio creata per eseguire l'applicazione. Se non è disponibile un'entità servizio per l'app, [crearne una con l'interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli).
- key: usare il valore *password* dell'output dell'interfaccia della riga di comando di creazione dell'entità servizio 
- tenant: usare il valore *tenant* dell'output dell'interfaccia della riga di comando di creazione dell'entità servizio

Salvare questo file nel sistema in una posizione sicura e leggibile dal codice. Impostare una variabile di ambiente con il percorso completo del file nella shell:

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

Creare l'oggetto `Azure` del punto di ingresso per iniziare a usare le librerie. Leggere la posizione del file delle proprietà tramite la variabile di ambiente.

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



