---
title: Concetti e modelli di utilizzo delle librerie di gestione di Azure per Java
description: 
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
ms.assetid: f452468b-7aae-4944-abad-0b1aaf19170d
ms.openlocfilehash: 052c4de1e8f9ff0ece5f36d1c3514bad8c04cfec
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-management-library-concepts"></a><span data-ttu-id="47715-103">Concetti relativi alle librerie di gestione di Azure</span><span class="sxs-lookup"><span data-stu-id="47715-103">Azure management library concepts</span></span>

## <a name="build-resources-through-a-fluent-interface"></a><span data-ttu-id="47715-104">Creare risorse tramite un'interfaccia Fluent</span><span class="sxs-lookup"><span data-stu-id="47715-104">Build resources through a fluent interface</span></span>

<span data-ttu-id="47715-105">Un'interfaccia Fluent è un modello che crea oggetti usando una catena di metodi che configura correttamente gli attributi dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="47715-105">A fluent interface is a pattern that creates objects using a method chain that correctly configures the object's attributes.</span></span> <span data-ttu-id="47715-106">Ad esempio, per creare un nuovo account di archiviazione di Azure:</span><span class="sxs-lookup"><span data-stu-id="47715-106">For example, to create a new Azure Storage account</span></span>

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

<span data-ttu-id="47715-107">Durante lo scorrimento della catena di metodi, l'ambiente di sviluppo integrato (IDE, Integrated Development Environment) suggerisce il metodo successivo da chiamare nella conversazione Fluent.</span><span class="sxs-lookup"><span data-stu-id="47715-107">As you go through the method chain, your IDE suggests the next method to call in the fluent conversation.</span></span>   

![GIF del completamento del comando di IntelliJ tramite una catena Fluent](media/intelliJFluent.gif)

<span data-ttu-id="47715-109">Inserire nella catena i metodi suggeriti dall'IDE, purché risultino significativi per le risorse di Azure definite.</span><span class="sxs-lookup"><span data-stu-id="47715-109">Chain the methods suggested by the IDE as long as they make sense for the Azure resource being defined.</span></span> <span data-ttu-id="47715-110">Se nella catena non è presente un metodo necessario, l'IDE evidenzierà la catena con un errore.</span><span class="sxs-lookup"><span data-stu-id="47715-110">If you are missing a required method in the chain your IDE will highlight it with an error.</span></span>

## <a name="resource-collections"></a><span data-ttu-id="47715-111">Raccolte di risorse</span><span class="sxs-lookup"><span data-stu-id="47715-111">Resource collections</span></span>

<span data-ttu-id="47715-112">La libreria di gestione ha un singolo punto di ingresso tramite l'oggetto `com.microsoft.azure.management.Azure` di primo livello per la creazione e l'aggiornamento delle risorse.</span><span class="sxs-lookup"><span data-stu-id="47715-112">The management library has a single point of entry through the top-level `com.microsoft.azure.management.Azure` object to create and update resources.</span></span> <span data-ttu-id="47715-113">Selezionare i tipi di risorse da usare tramite i metodi della raccolta di risorse definiti nell'oggetto `Azure`.</span><span class="sxs-lookup"><span data-stu-id="47715-113">Select which type of resources to work with using the resource collection methods defined in the `Azure` object.</span></span> <span data-ttu-id="47715-114">Ad esempio, il database SQL:</span><span class="sxs-lookup"><span data-stu-id="47715-114">For example, SQL Database:</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a><span data-ttu-id="47715-115">Elenchi e iterazioni</span><span class="sxs-lookup"><span data-stu-id="47715-115">Lists and iterations</span></span>

<span data-ttu-id="47715-116">Ogni raccolta di risorse include un metodo `list()` per restituire ogni istanza di tale risorsa nella sottoscrizione corrente.</span><span class="sxs-lookup"><span data-stu-id="47715-116">Each resource collection has a `list()` method to return every instance of that resource in your current subscription.</span></span> <span data-ttu-id="47715-117">Ad esempio, `azure.sqlServers().list()` restituisce tutti i database SQL nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="47715-117">For example, `azure.sqlServers().list()` returns all SQL databases in the subscription.</span></span>

<span data-ttu-id="47715-118">Usare il metodo `listByResourceGroup(String groupname)` per definire un [gruppo di risorse di Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) specifico come ambito dell'elenco restituito.</span><span class="sxs-lookup"><span data-stu-id="47715-118">Use the `listByResourceGroup(String groupname)` method to scope the returned List to a specific [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span>  

<span data-ttu-id="47715-119">Eseguire una ricerca e l'iterazione della raccolta `PagedList<T>` restituita, in modo analogo a un oggetto `List<T>` normale:</span><span class="sxs-lookup"><span data-stu-id="47715-119">Search and iterate over the returned `PagedList<T>` collection just as you would a normal `List<T>`:</span></span>

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a><span data-ttu-id="47715-120">Raccolte restituite dalle query</span><span class="sxs-lookup"><span data-stu-id="47715-120">Collections returned from queries</span></span>

<span data-ttu-id="47715-121">Le librerie di gestione restituiscono tipi di raccolte specifiche dalle query in base alla struttura dei risultati.</span><span class="sxs-lookup"><span data-stu-id="47715-121">The management libraries return specific collection types from queries based on the structure of the results.</span></span>

- <span data-ttu-id="47715-122">`List<T>`: dati non ordinati su cui è facile eseguire ricerche e l'iterazione.</span><span class="sxs-lookup"><span data-stu-id="47715-122">`List<T>`: Unordered data that is easy to search and iterate over.</span></span>
- <span data-ttu-id="47715-123">`Map<T>`: le mappe sono coppie chiave/valore con chiavi univoche, ma non necessariamente valori univoci.</span><span class="sxs-lookup"><span data-stu-id="47715-123">`Map<T>`: Maps are key/value pairs with unique keys, but not necessarily unique values.</span></span> <span data-ttu-id="47715-124">Un esempio di mappa è costituito dalle impostazioni dell'app per un'app Web del servizio app.</span><span class="sxs-lookup"><span data-stu-id="47715-124">An example of a Map would be app settings for an App Service Web App.</span></span>
- <span data-ttu-id="47715-125">`Set<T>`: i set hanno chiavi e valori univoci.</span><span class="sxs-lookup"><span data-stu-id="47715-125">`Set<T>`: Sets have unique keys and values.</span></span> <span data-ttu-id="47715-126">Un esempio di set è costituito dalle reti collegate a una macchina virtuale, che hanno un identificatore univoco (chiave) e una configurazione di rete univoca (valore).</span><span class="sxs-lookup"><span data-stu-id="47715-126">An example of a Set would be networks attached to a virtual machine, which would have both an unique identifier (the key) and a unique network configuration (the value).</span></span>

## <a name="actionable-verbs"></a><span data-ttu-id="47715-127">Verbi su cui è possibile eseguire azioni</span><span class="sxs-lookup"><span data-stu-id="47715-127">Actionable verbs</span></span>

<span data-ttu-id="47715-128">I metodi i cui nomi includono verbi consentono di eseguire azioni immediate in Azure.</span><span class="sxs-lookup"><span data-stu-id="47715-128">Methods with verbs in their names take immediate action in Azure.</span></span> <span data-ttu-id="47715-129">Questi metodi funzionano in modo sincrono e bloccano l'esecuzione nel thread corrente fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="47715-129">These methods work synchronously and block execution in the current thread until they complete.</span></span> 

| <span data-ttu-id="47715-130">Verbo</span><span class="sxs-lookup"><span data-stu-id="47715-130">Verb</span></span>   |  <span data-ttu-id="47715-131">Esempio di utilizzo</span><span class="sxs-lookup"><span data-stu-id="47715-131">Sample Usage</span></span> |
|--------|---------------|
| <span data-ttu-id="47715-132">create</span><span class="sxs-lookup"><span data-stu-id="47715-132">create</span></span> | `azure.virtualMachines().create(listOfVMCreatables)` |
| <span data-ttu-id="47715-133">apply</span><span class="sxs-lookup"><span data-stu-id="47715-133">apply</span></span>  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| <span data-ttu-id="47715-134">delete</span><span class="sxs-lookup"><span data-stu-id="47715-134">delete</span></span> | `azure.disks().deleteById(id)` | 
| <span data-ttu-id="47715-135">list</span><span class="sxs-lookup"><span data-stu-id="47715-135">list</span></span>   | `azure.sqlServers().list()` | 
| <span data-ttu-id="47715-136">get</span><span class="sxs-lookup"><span data-stu-id="47715-136">get</span></span>    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> <span data-ttu-id="47715-137">`define()` e `update()` sono verbi ma non bloccano l'esecuzione, a meno che non siano seguiti da `create()` o `apply()`.</span><span class="sxs-lookup"><span data-stu-id="47715-137">`define()` and `update()` are verbs but do not block unless followed by a `create()` or `apply()`.</span></span>
 
<span data-ttu-id="47715-138">Esistono versioni asincrone di alcuni di questi metodi con il suffisso `Async` e con [estensioni reattive](https://github.com/ReactiveX/RxJava).</span><span class="sxs-lookup"><span data-stu-id="47715-138">Asynchronous versions of some of these  methods exist with the `Async` suffix using [Reactive extensions](https://github.com/ReactiveX/RxJava).</span></span> 

<span data-ttu-id="47715-139">Alcuni oggetti hanno altri metodi che consentono di modificare lo stato della risorsa in Azure.</span><span class="sxs-lookup"><span data-stu-id="47715-139">Some objects have other methods with that change the state of the resource in Azure.</span></span> <span data-ttu-id="47715-140">Ad esempio, `restart()` in `VirtualMachine`.</span><span class="sxs-lookup"><span data-stu-id="47715-140">For example, `restart()` on a `VirtualMachine`.</span></span>

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
<span data-ttu-id="47715-141">Questi metodi non hanno sempre versioni asincrone e bloccheranno l'esecuzione nel rispettivo thread fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="47715-141">These methods do not always have asynchronous versions and will block execution on their thread until they complete.</span></span>

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a><span data-ttu-id="47715-142">Creazione differita delle risorse</span><span class="sxs-lookup"><span data-stu-id="47715-142">Lazy resource creation</span></span>

<span data-ttu-id="47715-143">La creazione di risorse di Azure può presentare alcune difficoltà, ad esempio nel caso in cui una nuova risorsa dipenda da un'altra risorsa che non esiste ancora.</span><span class="sxs-lookup"><span data-stu-id="47715-143">A challenge when creating Azure resources arises when a new resource depends on another resource that doesn't yet exist.</span></span> <span data-ttu-id="47715-144">Un esempio di questo scenario è costituito dalla necessità di riservare un indirizzo IP pubblico e di configurare un disco quando si crea una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="47715-144">An example of this scenario is reserving a public IP address and setting up a disk when creating a new virtual machine.</span></span> <span data-ttu-id="47715-145">Non si vuole verificare di avere riservato l'indirizzo o di avere creato il disco. Si vuole solo assicurare che la macchina virtuale abbia tali risorse quando viene creata.</span><span class="sxs-lookup"><span data-stu-id="47715-145">You don't want to verify reserving the address or the creating the disk, you just want to ensure the virtual machine has those resources when it is created.</span></span>

<span data-ttu-id="47715-146">Gli oggetti `Creatable<T>` consentono di definire le risorse di Azure da usare nel codice, senza attendere che vengano create nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="47715-146">`Creatable<T>` objects let you define Azure resources for use in your code without waiting around for them to be created in your subscription.</span></span> <span data-ttu-id="47715-147">Le librerie di gestione consentono di posticipare la creazione di oggetti `Creatable<T>` fino a quando non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="47715-147">The management libraries defer creating  `Creatable<T>` objects until they are needed.</span></span>

<span data-ttu-id="47715-148">Generare oggetti `Creatable<T>` per le risorse di Azure tramite il verbo `define()`:</span><span class="sxs-lookup"><span data-stu-id="47715-148">Generate `Creatable<T>` objects for Azure resources through the `define()` verb:</span></span>

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

<span data-ttu-id="47715-149">La risorsa di Azure definita da `Creatable<PublicIPAddress>` in questo esempio non esiste ancora nella sottoscrizione quando il codice viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="47715-149">The Azure resource defined by the `Creatable<PublicIPAddress>` in this example does not yet exist in your subscription when this code executes.</span></span>  <span data-ttu-id="47715-150">Usare l'oggetto `publicIPAddressCreatable` per creare altre risorse con questo indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="47715-150">Use the `publicIPAddressCreatable` object to create other Azure resources with this IP address.</span></span> 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

<span data-ttu-id="47715-151">Le risorse `Creatable<T>` vengono generate quando viene creata in Azure qualsiasi risorsa definita usando l'oggetto con `create()`.</span><span class="sxs-lookup"><span data-stu-id="47715-151">The `Creatable<T>` resources are generated in your subscription when any resource that is defined using the object is built in Azure using `create()`.</span></span> <span data-ttu-id="47715-152">Continuando l'esempio relativo all'indirizzo IP e alla macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="47715-152">Continuing the IP address and virtual machine example:</span></span>

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

<span data-ttu-id="47715-153">Se si passa `Creatable<T>` alle chiamate `create()`, viene restituito un oggetto `CreatedResources` invece di un singolo oggetto risorsa.</span><span class="sxs-lookup"><span data-stu-id="47715-153">Passing the `Creatable<T>` to `create()` calls returns a `CreatedResources` object instead of a single resource object.</span></span>  <span data-ttu-id="47715-154">L'oggetto `CreatedResources<T>` consente di accedere a tutte le risorse create dalla chiamata `create()`, non solo alla risorsa tipizzata nella chiamata.</span><span class="sxs-lookup"><span data-stu-id="47715-154">The `CreatedResources<T>` object lets you access all resources created by the `create()` call, not just the typed resource in the call.</span></span> <span data-ttu-id="47715-155">Per accedere all'indirizzo IP pubblico creato in Azure per la macchina virtuale creata nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="47715-155">To access the public IP address created in Azure for the virtual machine created in the above example:</span></span>

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a><span data-ttu-id="47715-156">Gestione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="47715-156">Exception handling</span></span>

<span data-ttu-id="47715-157">Le classi Exception delle librerie di gestione estendono `com.microsoft.rest.RestException`.</span><span class="sxs-lookup"><span data-stu-id="47715-157">The management libraries' Exception classes extend `com.microsoft.rest.RestException`.</span></span> <span data-ttu-id="47715-158">Intercettare le eccezioni generate dalle librerie di gestione con un blocco `catch (RestException exception)` dopo l'istruzione `try` rilevante.</span><span class="sxs-lookup"><span data-stu-id="47715-158">Catch exceptions generated by the management libraries with a `catch (RestException exception)` block after the relevant `try` statement.</span></span>

## <a name="logs-and-trace"></a><span data-ttu-id="47715-159">Log e traccia</span><span class="sxs-lookup"><span data-stu-id="47715-159">Logs and trace</span></span>

<span data-ttu-id="47715-160">Configurare la quantità di registrazione dalla libreria di gestione quando si crea l'oggetto `Azure` del punto di ingresso usando `withLogLevel()`.</span><span class="sxs-lookup"><span data-stu-id="47715-160">Configure the amount of logging from the management library when you build the entry point `Azure` object using `withLogLevel()`.</span></span> <span data-ttu-id="47715-161">Esistono i livelli di traccia seguenti:</span><span class="sxs-lookup"><span data-stu-id="47715-161">The following trace levels exist:</span></span>

| <span data-ttu-id="47715-162">Livello di traccia</span><span class="sxs-lookup"><span data-stu-id="47715-162">Trace level</span></span> | <span data-ttu-id="47715-163">Registrazione abilitata</span><span class="sxs-lookup"><span data-stu-id="47715-163">Logging enabled</span></span> 
| ------------ | ---------------
| <span data-ttu-id="47715-164">com.microsoft.rest.LogLevel.NONE</span><span class="sxs-lookup"><span data-stu-id="47715-164">com.microsoft.rest.LogLevel.NONE</span></span> | <span data-ttu-id="47715-165">Nessun output</span><span class="sxs-lookup"><span data-stu-id="47715-165">No output</span></span>
| <span data-ttu-id="47715-166">com.microsoft.rest.LogLevel.BASIC</span><span class="sxs-lookup"><span data-stu-id="47715-166">com.microsoft.rest.LogLevel.BASIC</span></span> | <span data-ttu-id="47715-167">Registra gli URL delle chiamate REST sottostanti, i codici di risposta e gli orari</span><span class="sxs-lookup"><span data-stu-id="47715-167">Logs the URLs to underlying REST calls, response codes and times</span></span>
| <span data-ttu-id="47715-168">com.microsoft.rest.LogLevel.BODY</span><span class="sxs-lookup"><span data-stu-id="47715-168">com.microsoft.rest.LogLevel.BODY</span></span> | <span data-ttu-id="47715-169">Tutti gli elementi del livello BASIC e i corpi di richiesta e risposta per le chiamate REST</span><span class="sxs-lookup"><span data-stu-id="47715-169">Everything in BASIC plus request and response bodies for the REST calls</span></span>
| <span data-ttu-id="47715-170">com.microsoft.rest.LogLevel.HEADERS</span><span class="sxs-lookup"><span data-stu-id="47715-170">com.microsoft.rest.LogLevel.HEADERS</span></span> | <span data-ttu-id="47715-171">Tutti gli elementi del livello BASIC e le intestazioni di richiesta e risposta per le chiamate REST</span><span class="sxs-lookup"><span data-stu-id="47715-171">Everything in BASIC plus the request and response headers REST calls</span></span>
| <span data-ttu-id="47715-172">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span><span class="sxs-lookup"><span data-stu-id="47715-172">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span></span> | <span data-ttu-id="47715-173">Tutti gli elementi dei livelli di registrazione BODY e HEADERS</span><span class="sxs-lookup"><span data-stu-id="47715-173">Everything in both BODY and HEADERS log level</span></span>

<span data-ttu-id="47715-174">Associare un'[implementazione della registrazione SLF4J](https://www.slf4j.org/manual.html) se è necessario registrare l'output in un framework di registrazione come [Log4J 2](https://logging.apache.org/log4j/2.x/).</span><span class="sxs-lookup"><span data-stu-id="47715-174">Bind a [SLF4J logging implementation](https://www.slf4j.org/manual.html) if you need to log output to a logging framework like [Log4J 2](https://logging.apache.org/log4j/2.x/).</span></span>