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
# <a name="azure-management-library-concepts"></a>Concetti relativi alle librerie di gestione di Azure

## <a name="build-resources-through-a-fluent-interface"></a>Creare risorse tramite un'interfaccia Fluent

Un'interfaccia Fluent è un modello che crea oggetti usando una catena di metodi che configura correttamente gli attributi dell'oggetto. Ad esempio, per creare un nuovo account di archiviazione di Azure:

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

Durante lo scorrimento della catena di metodi, l'ambiente di sviluppo integrato (IDE, Integrated Development Environment) suggerisce il metodo successivo da chiamare nella conversazione Fluent.   

![GIF del completamento del comando di IntelliJ tramite una catena Fluent](media/intelliJFluent.gif)

Inserire nella catena i metodi suggeriti dall'IDE, purché risultino significativi per le risorse di Azure definite. Se nella catena non è presente un metodo necessario, l'IDE evidenzierà la catena con un errore.

## <a name="resource-collections"></a>Raccolte di risorse

La libreria di gestione ha un singolo punto di ingresso tramite l'oggetto `com.microsoft.azure.management.Azure` di primo livello per la creazione e l'aggiornamento delle risorse. Selezionare i tipi di risorse da usare tramite i metodi della raccolta di risorse definiti nell'oggetto `Azure`. Ad esempio, il database SQL:

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a>Elenchi e iterazioni

Ogni raccolta di risorse include un metodo `list()` per restituire ogni istanza di tale risorsa nella sottoscrizione corrente. Ad esempio, `azure.sqlServers().list()` restituisce tutti i database SQL nella sottoscrizione.

Usare il metodo `listByResourceGroup(String groupname)` per definire un [gruppo di risorse di Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) specifico come ambito dell'elenco restituito.  

Eseguire una ricerca e l'iterazione della raccolta `PagedList<T>` restituita, in modo analogo a un oggetto `List<T>` normale:

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a>Raccolte restituite dalle query

Le librerie di gestione restituiscono tipi di raccolte specifiche dalle query in base alla struttura dei risultati.

- `List<T>`: dati non ordinati su cui è facile eseguire ricerche e l'iterazione.
- `Map<T>`: le mappe sono coppie chiave/valore con chiavi univoche, ma non necessariamente valori univoci. Un esempio di mappa è costituito dalle impostazioni dell'app per un'app Web del servizio app.
- `Set<T>`: i set hanno chiavi e valori univoci. Un esempio di set è costituito dalle reti collegate a una macchina virtuale, che hanno un identificatore univoco (chiave) e una configurazione di rete univoca (valore).

## <a name="actionable-verbs"></a>Verbi su cui è possibile eseguire azioni

I metodi i cui nomi includono verbi consentono di eseguire azioni immediate in Azure. Questi metodi funzionano in modo sincrono e bloccano l'esecuzione nel thread corrente fino al completamento. 

| Verbo   |  Esempio di utilizzo |
|--------|---------------|
| create | `azure.virtualMachines().create(listOfVMCreatables)` |
| apply  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| delete | `azure.disks().deleteById(id)` | 
| list   | `azure.sqlServers().list()` | 
| get    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> `define()` e `update()` sono verbi ma non bloccano l'esecuzione, a meno che non siano seguiti da `create()` o `apply()`.
 
Esistono versioni asincrone di alcuni di questi metodi con il suffisso `Async` e con [estensioni reattive](https://github.com/ReactiveX/RxJava). 

Alcuni oggetti hanno altri metodi che consentono di modificare lo stato della risorsa in Azure. Ad esempio, `restart()` in `VirtualMachine`.

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
Questi metodi non hanno sempre versioni asincrone e bloccheranno l'esecuzione nel rispettivo thread fino al completamento.

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a>Creazione differita delle risorse

La creazione di risorse di Azure può presentare alcune difficoltà, ad esempio nel caso in cui una nuova risorsa dipenda da un'altra risorsa che non esiste ancora. Un esempio di questo scenario è costituito dalla necessità di riservare un indirizzo IP pubblico e di configurare un disco quando si crea una nuova macchina virtuale. Non si vuole verificare di avere riservato l'indirizzo o di avere creato il disco. Si vuole solo assicurare che la macchina virtuale abbia tali risorse quando viene creata.

Gli oggetti `Creatable<T>` consentono di definire le risorse di Azure da usare nel codice, senza attendere che vengano create nella sottoscrizione. Le librerie di gestione consentono di posticipare la creazione di oggetti `Creatable<T>` fino a quando non sono necessari.

Generare oggetti `Creatable<T>` per le risorse di Azure tramite il verbo `define()`:

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

La risorsa di Azure definita da `Creatable<PublicIPAddress>` in questo esempio non esiste ancora nella sottoscrizione quando il codice viene eseguito.  Usare l'oggetto `publicIPAddressCreatable` per creare altre risorse con questo indirizzo IP. 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

Le risorse `Creatable<T>` vengono generate quando viene creata in Azure qualsiasi risorsa definita usando l'oggetto con `create()`. Continuando l'esempio relativo all'indirizzo IP e alla macchina virtuale:

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

Se si passa `Creatable<T>` alle chiamate `create()`, viene restituito un oggetto `CreatedResources` invece di un singolo oggetto risorsa.  L'oggetto `CreatedResources<T>` consente di accedere a tutte le risorse create dalla chiamata `create()`, non solo alla risorsa tipizzata nella chiamata. Per accedere all'indirizzo IP pubblico creato in Azure per la macchina virtuale creata nell'esempio precedente:

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a>Gestione delle eccezioni

Le classi Exception delle librerie di gestione estendono `com.microsoft.rest.RestException`. Intercettare le eccezioni generate dalle librerie di gestione con un blocco `catch (RestException exception)` dopo l'istruzione `try` rilevante.

## <a name="logs-and-trace"></a>Log e traccia

Configurare la quantità di registrazione dalla libreria di gestione quando si crea l'oggetto `Azure` del punto di ingresso usando `withLogLevel()`. Esistono i livelli di traccia seguenti:

| Livello di traccia | Registrazione abilitata 
| ------------ | ---------------
| com.microsoft.rest.LogLevel.NONE | Nessun output
| com.microsoft.rest.LogLevel.BASIC | Registra gli URL delle chiamate REST sottostanti, i codici di risposta e gli orari
| com.microsoft.rest.LogLevel.BODY | Tutti gli elementi del livello BASIC e i corpi di richiesta e risposta per le chiamate REST
| com.microsoft.rest.LogLevel.HEADERS | Tutti gli elementi del livello BASIC e le intestazioni di richiesta e risposta per le chiamate REST
| com.microsoft.rest.LogLevel.BODY_AND_HEADERS | Tutti gli elementi dei livelli di registrazione BODY e HEADERS

Associare un'[implementazione della registrazione SLF4J](https://www.slf4j.org/manual.html) se è necessario registrare l'output in un framework di registrazione come [Log4J 2](https://logging.apache.org/log4j/2.x/).