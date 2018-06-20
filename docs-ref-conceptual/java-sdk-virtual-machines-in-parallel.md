---
title: Creare macchine virtuali in diverse aree in parallelo | Microsoft Docs
description: Codice di esempio per creare macchine virtuali in diverse aree di Azure in parallelo con Azure SDK per Java
author: rloutlaw
manager: douge
ms.assetid: e5a36699-2d96-4571-84f9-a6af13f3c067
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: e20feb555c3a360eceae60c1569af9a00a5cd027
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931197"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a>Creare macchine virtuali in più aree dalle applicazioni Java

[Questo esempio](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) crea macchine virtuali in parallelo in diverse aree di Azure usando le [librerie di gestione di Azure per Java](https://github.com/Azure/azure-sdk-for-java).

> [!IMPORTANT]
> Nell'esempio vengono create in quattro aree 48 macchine virtuali in totale che eseguono Ubuntu 16.04 LTS e hanno [dimensioni STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes). Il codice di esempio elimina queste macchine virtuali prima di uscire. Assicurarsi di [verificare i limiti e le quote dei servizi](http://docs.microsoft.com/azure/azure-subscription-service-limits) prima di eseguire questo esempio con il numero predefinito di macchine virtuali.

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Quindi eseguire:

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java).

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a>Impostare le posizioni e i conteggi per le macchine virtuali

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

Questo elemento `Map` viene usato in un secondo momento nell'esempio per impostare la distribuzione delle macchine virtuali in tutto il mondo.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

Ogni risorsa presente nell'esempio viene gestita da questo gruppo di risorse. Le risorse possono quindi essere eliminate facilmente in un secondo momento eliminando il gruppo di risorse.

## <a name="define-the-virtual-machines"></a>Definire le macchine virtuali
```java
// list to store the VirtualMachine definitions
List<Creatable<VirtualMachine>> creatableVirtualMachines = new ArrayList<>();
    
// outer loop: iterate through each region included in the map
for (Map.Entry<Region, Integer> entry : virtualMachinesByLocation.entrySet()) {
    Region region = entry.getKey();
    Integer vmCount = entry.getValue();

    // Define one virtual network Creatable per region for the VMs to share
    String networkName = SdkContext.randomResourceName("vnetCOPD-", 20);
    Creatable<Network> networkCreatable = azure.networks().define(networkName)
           .withRegion(region)
           .withExistingResourceGroup(resourceGroup)
           .withAddressSpace("172.16.0.0/16");

    // Define one storage account Creatable per region for storing VM disks
    String storageAccountName = SdkContext.randomResourceName("stgcopd", 20);
    Creatable<StorageAccount> storageAccountCreatable = azure.storageAccounts()
        .define(storageAccountName)
              .withRegion(region)
              .withExistingResourceGroup(resourceGroup);

    // generate a common prefix for every VM name
    String linuxVMNamePrefix = SdkContext.randomResourceName("vm-", 15);

    // inner loop: iterate once for every VM instance in the region
    for (int i = 1; i <= vmCount; i++) {

        // Create one public IP address Creatable for each VM
        Creatable<PublicIpAddress> publicIpAddressCreatable = azure.publicIpAddresses()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withLeafDomainLabel(SdkContext.randomResourceName("pip", 10));

        publicIpCreatableKeys.add(publicIpAddressCreatable.key());

        // Create one virtual machine Creatable 
        Creatable<VirtualMachine> virtualMachineCreatable = azure.virtualMachines()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withNewPrimaryNetwork(networkCreatable)
             .withPrimaryPrivateIpAddressDynamic()
             .withNewPrimaryPublicIpAddress(publicIpAddressCreatable)
             .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
             .withRootUsername(userName)
             .withSsh(sshKey)
             .withSize(VirtualMachineSizeTypes.STANDARD_DS3_V2)
             .withNewStorageAccount(storageAccountCreatable);
        // add the virtual machine Creatable to the list     
        creatableVirtualMachines.add(virtualMachineCreatable); 
     }
}
```

Il ciclo `for` esterno precedente esegue l'iterazione in ogni area, definendo un oggetto Creatable di rete virtuale e un oggetto Creatable di account di archiviazione per l'uso da parte di tutte le macchine virtuali di quell'area. Vedere altre informazioni sull'uso di oggetti [Creatable](java-sdk-azure-concepts.md#Creatables) per creare risorse solo se necessarie quando si usano le librerie di gestione.

Il ciclo `for` interno ottiene un oggetto Creatable di indirizzo IP pubblico per la macchina virtuale e quindi definisce un oggetto Creatable di macchina virtuale usando gli oggetti Creatable relativi a rete virtuale, account di archiviazione e indirizzo IP pubblico definiti in precedenza.  Questo oggetto Creatable VirtualMachine viene quindi aggiunto all'elenco `creatableVirtualMachines`.

## <a name="create-the-virtual-machines"></a>Creare le macchine virtuali

```java
// create all virtual machines defined in the list, return all Creatable objects used
// including networks, public IP addresses, and storage accounts
CreatedResources<VirtualMachine> virtualMachines = azure.virtualMachines().create(creatableVirtualMachines);

// list the IDs of each virtual machine created 
for (VirtualMachine virtualMachine : virtualMachines.values()) {
    System.out.println(virtualMachine.id());
}

// call createdRelatedResource(key) to get the resources used to define the virtual machines. 
// Save the key at the time you define the Creatable to use CreatedResources like this
for (String publicIpCreatableKey : publicIpCreatableKeys) {
    PublicIPAddress pip = 
         (PublicIPAddress) virtualMachines.createdRelatedResource(publicIpCreatableKey);
}
```

La chiamata `azure.virtualMachines().create(creatableVirtualMachines)` crea nelle aree in parallelo tutte le macchine virtuali definite nell'elenco `creatableVirtualMachines`.

Usare l'oggetto `CreatedResources<VirtualMachine>` restituito per accedere alle risorse create nella sottoscrizione di Azure durante il metodo `create()`, non solo il tipo `VirtualMachine` restituito. Eseguire il cast del valore restituito da `createdRelatedResources()` al tipo corretto. 

Vedere altre informazioni sull'uso di `Creatable<T>` e `CreatedResources` nell'articolo relativo ai [concetti delle librerie](java-sdk-azure-concepts.md).

## <a name="delete-the-resource-group"></a>Eliminare il gruppo di risorse. 

```java
// finally block deletes the resource group before the code exits
// deleting a resource group deletes all resources created in it
finally {
    try {
        System.out.println("Deleting Resource Group: " + rgName);
        azure.resourceGroups().deleteByName(rgName);
        System.out.println("Deleted Resource Group: " + rgName);
    } catch (NullPointerException npe) {
        System.out.println("Did not create any resources in Azure. No clean up is necessary");
    } catch (Exception g) {
        g.printStackTrace();
    }
}
```

Questo blocco elimina le risorse create nell'esempio prima dell'uscita.

## <a name="sample-explanation"></a>Spiegazione dell'esempio

Visualizzare l'esempio di codice completo in [GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel).

L'esempio usa oggetti `Creatable` per definire un account di archiviazione e una rete virtuale per ogni area che ospita le macchine virtuali. Vengono quindi definiti oggetti `Creatable` per l'indirizzo IP pubblico di ogni macchina virtuale. L'esempio definisce le macchine virtuali con questi oggetti `Creatable` e aggiunge la definizione delle VM all'elenco `virtualMachineCreatable`.

Dopo che il codice ha aggiunto la definizione di ogni macchina virtuale all'elenco, `azure.virtualMachines().create(creatableVirtualMachines)` crea ogni macchina virtuale in parallelo in Azure.

L'esempio di codice ottiene quindi dall'oggetto CreatedResources restituito gli indirizzi IP per tutte le macchine virtuali create, per creare un'istanza di [Gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) per la distribuzione del carico tra le macchine virtuali. 

Il blocco `finally` elimina le risorse dalla sottoscrizione di Azure anche in caso di errore.

| Classe usata nell'esempio | Note
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | Eseguire query sulle proprietà e gestire lo stato delle macchine virtuali. I risultati vengono recuperati sotto forma di elenco da `azure.virtualMachines().list()` o per nome o ID `azure.virtualMachines().getByResourceGroup()`
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | Valori statici che eseguono il mapping a [opzioni relative alle dimensioni delle macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) per l'uso come parametro di `withSize()` quando si definisce una macchina virtuale.
| [PublicIpAddress](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | Definito, ma non immediatamente creato, per ogni macchina virtuale tramite `azure.publicIpAddresses().define()`. Archiviare la chiave per ogni `Creatable` e recuperarla in un secondo momento tramite `createdRelatedResource()`
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | Set di opzioni di macchine virtuali Linux usato come parametro per il metodo `withPopularLinuxImage()` durante la definizione di una macchina virtuale.
| [Rete](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | L'esempio definisce una rete virtuale per ogni area tramite `azure.networks().define()`. 

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]