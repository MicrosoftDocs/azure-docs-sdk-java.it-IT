---
title: Eseguire l'autenticazione con le librerie di gestione di Azure per Java
description: "Eseguire l'autenticazione con un'entità servizio nelle librerie di gestione di Azure per Java"
keywords: "Azure, Java, SDK, API, Maven, Gradle, autenticazione, active directory, entità servizio"
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
ms.openlocfilehash: dd1273d5fc79f513e8dae5420eb6a6988d327d58
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a><span data-ttu-id="50138-104">Eseguire l'autenticazione con le librerie di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="50138-104">Authenticate with the Azure libraries for Java</span></span> 

## <a name="connect-to-services-with-connection-strings"></a><span data-ttu-id="50138-105">Connettersi ai servizi con le stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="50138-105">Connect to services with connection strings</span></span>

<span data-ttu-id="50138-106">La maggior parte delle librerie di servizi di Azure usa una stringa di connessione o una chiave di protezione per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="50138-106">Most Azure service libraries use a connection string or secure key for authentication.</span></span> <span data-ttu-id="50138-107">Il database SQL include, ad esempio, le informazioni specificate per nome utente e password nella stringa di connessione JDBC:</span><span class="sxs-lookup"><span data-stu-id="50138-107">For example, SQL Database includes username and password information in the JDBC connection string:</span></span>

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

<span data-ttu-id="50138-108">Archiviazione di Azure usa una chiave di archiviazione per autorizzare l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="50138-108">Azure Storage uses a storage key to authorize the application:</span></span>

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

<span data-ttu-id="50138-109">Le stringhe di connessione dei servizi vengono usate per eseguire l'autenticazione ad altri servizi di Azure, ad esempio [DocumentDB](https://docs.microsoft.com/azure/documentdb/documentdb-java-application#a-iduseserviceastep-4-using-the-documentdb-service-in-a-java-application), [Cache Redis](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started) e [Bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues).</span><span class="sxs-lookup"><span data-stu-id="50138-109">Service connection strings are used to authenticate to other Azure services like [DocumentDB](https://docs.microsoft.com/azure/documentdb/documentdb-java-application#a-iduseserviceastep-4-using-the-documentdb-service-in-a-java-application), [Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started), and [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues).</span></span> <span data-ttu-id="50138-110">Per ottenere le stringhe di connessione, è possibile usare il portale di Azure o l'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="50138-110">You can get the connection strings using the Azure portal or the CLI.</span></span>  <span data-ttu-id="50138-111">È anche possibile usare le librerie di gestione di Azure per Java per eseguire query nelle risorse per creare stringhe di connessione nel codice.</span><span class="sxs-lookup"><span data-stu-id="50138-111">You can also use the Azure management libraries for Java to query resources to build connection strings in your code.</span></span> 

<span data-ttu-id="50138-112">Questo codice, ad esempio, usa le librerie di gestione per creare una stringa di connessione dell'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="50138-112">For example, this code uses the management libraries to create a storage account connection string:</span></span>

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

<span data-ttu-id="50138-113">Altre librerie richiedono che l'applicazione venga eseguita con un'[entità servizio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) che autorizza l'esecuzione dell'applicazione con le credenziali concesse.</span><span class="sxs-lookup"><span data-stu-id="50138-113">Other libraries require your application to run with a [service prinicpal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) authorizing the application to run with granted credentials.</span></span> <span data-ttu-id="50138-114">Questa configurazione è simile alla procedura di autenticazione basata su oggetti per le librerie di gestione illustrata di seguito.</span><span class="sxs-lookup"><span data-stu-id="50138-114">This configuration is similar to the object-based authentication steps for the management library listed below.</span></span>

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a><span data-ttu-id="50138-115">Eseguire l'autenticazione con le librerie di gestione di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="50138-115">Authenticate with the Azure management libraries for Java</span></span>

<span data-ttu-id="50138-116">Sono disponibili due opzioni per autenticare l'applicazione con Azure quando si usano le librerie di gestione Java per creare e gestire le risorse.</span><span class="sxs-lookup"><span data-stu-id="50138-116">Two options are available to authenticate your application with Azure when using the Java management libraries to create and manage resources.</span></span>

### <a name="authenticate-with-an-applicationtokencredentials-object"></a><span data-ttu-id="50138-117">Eseguire l'autenticazione con un oggetto ApplicationTokenCredentials</span><span class="sxs-lookup"><span data-stu-id="50138-117">Authenticate with an ApplicationTokenCredentials object</span></span>

<span data-ttu-id="50138-118">Creare un'istanza di `ApplicationTokenCredentials` per fornire le credenziali dell'entità servizio all'oggetto `Azure` di primo livello dal codice.</span><span class="sxs-lookup"><span data-stu-id="50138-118">Create an instance of `ApplicationTokenCredentials` to supply the service principal credentials to the top-level `Azure` object from inside your code.</span></span>

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

<span data-ttu-id="50138-119">`client`, `tenant` e `key` sono gli stessi valori dell'entità servizio usati con l'[autenticazione basata su file](#mgmt-file).</span><span class="sxs-lookup"><span data-stu-id="50138-119">The `client`, `tenant` and `key` are the same service principal values used with [file-based authentication](#mgmt-file).</span></span> <span data-ttu-id="50138-120">Il valore `AzureEnvironment.AZURE` crea le credenziali per il cloud pubblico di Azure.</span><span class="sxs-lookup"><span data-stu-id="50138-120">The `AzureEnvironment.AZURE` value creates credentials against the Azure public cloud.</span></span> <span data-ttu-id="50138-121">Sostituirlo con un valore diverso se è necessario accedere a un altro cloud (ad esempio, `AzureEnvironment.AZURE_GERMANY`).</span><span class="sxs-lookup"><span data-stu-id="50138-121">Change this to a different value if you need to access another cloud (for example, `AzureEnvironment.AZURE_GERMANY`).</span></span>  

 <span data-ttu-id="50138-122">Leggere i valori dell'entità servizio dalle variabili di ambiente o da un archivio di gestione segreto, ad esempio [Key Vault](/azure/key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="50138-122">Read the service principal values from environment variables or a secret management store like [Key Vault](/azure/key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="50138-123">Non impostare questi valori come stringhe in testo non crittografato nel codice per evitare l'esposizione accidentale delle credenziali nella cronologia del controllo della versione.</span><span class="sxs-lookup"><span data-stu-id="50138-123">Avoid setting these values as cleartext strings in your code to prevent accidentally exposing credentials in your version control history.</span></span>   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a><span data-ttu-id="50138-124">Autenticazione basata su file (anteprima)</span><span class="sxs-lookup"><span data-stu-id="50138-124">File based authentication (Preview)</span></span>

<span data-ttu-id="50138-125">Il modo più semplice per eseguire l'autenticazione consiste nel creare un file delle proprietà contenente le credenziali per un'[entità servizio di Azure](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) usando il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="50138-125">The simplest way to authenticate is to create a properties file that contains credentials for an [Azure service principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) using the following format:</span></span>

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

- <span data-ttu-id="50138-126">subscription: usare il valore *id* da `az account show` nell'interfaccia della riga di comando di Azure 2.0.</span><span class="sxs-lookup"><span data-stu-id="50138-126">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="50138-127">client: usare il valore *appId* dell'output di un'entità servizio creata per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="50138-127">client: use the *appId* value from the output taken from a service principal created to run the application.</span></span> <span data-ttu-id="50138-128">Se non è disponibile un'entità servizio per l'app, [crearne una con l'interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="50138-128">If you don't have a service principal for your app, [create one with the Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>
- <span data-ttu-id="50138-129">key: usare il valore *password* dell'output dell'interfaccia della riga di comando di creazione dell'entità servizio</span><span class="sxs-lookup"><span data-stu-id="50138-129">key: use the *password* value from the service principal create CLI output</span></span> 
- <span data-ttu-id="50138-130">tenant: usare il valore *tenant* dell'output dell'interfaccia della riga di comando di creazione dell'entità servizio</span><span class="sxs-lookup"><span data-stu-id="50138-130">tenant: use the *tenant* value from the service principal create CLI output</span></span>

<span data-ttu-id="50138-131">Salvare questo file nel sistema in una posizione sicura e leggibile dal codice.</span><span class="sxs-lookup"><span data-stu-id="50138-131">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="50138-132">Impostare una variabile di ambiente con il percorso completo del file nella shell:</span><span class="sxs-lookup"><span data-stu-id="50138-132">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="50138-133">Creare l'oggetto `Azure` del punto di ingresso per iniziare a usare le librerie.</span><span class="sxs-lookup"><span data-stu-id="50138-133">Create the entry point `Azure` object to start working with the libraries.</span></span> <span data-ttu-id="50138-134">Leggere la posizione del file delle proprietà tramite la variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="50138-134">Read the location of the properties file through the environment variable.</span></span>

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



