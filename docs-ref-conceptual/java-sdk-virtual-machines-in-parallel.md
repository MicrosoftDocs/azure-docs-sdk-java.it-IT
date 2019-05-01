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
ms.openlocfilehash: 497ea5c4e2efd73335f05f90b700fb70e2809de6
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592511"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a><span data-ttu-id="4c68b-103">Creare macchine virtuali in più aree dalle applicazioni Java</span><span class="sxs-lookup"><span data-stu-id="4c68b-103">Create virtual machines across multiple regions from your Java applications</span></span>

<span data-ttu-id="4c68b-104">[Questo esempio](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) crea macchine virtuali in parallelo in diverse aree di Azure usando le [librerie di gestione di Azure per Java](https://github.com/Azure/azure-sdk-for-java).</span><span class="sxs-lookup"><span data-stu-id="4c68b-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) creates virtual machines in parallel across different Azure regions using the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4c68b-105">Nell'esempio vengono create in quattro aree 48 macchine virtuali in totale che eseguono Ubuntu 16.04 LTS e hanno [dimensioni STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span><span class="sxs-lookup"><span data-stu-id="4c68b-105">The sample creates a total of 48 VMs running Ubuntu 16.04 LTS of [size STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes) across four regions.</span></span> <span data-ttu-id="4c68b-106">Il codice di esempio elimina queste macchine virtuali prima di uscire.</span><span class="sxs-lookup"><span data-stu-id="4c68b-106">The sample code deletes these virtual machines before exiting.</span></span> <span data-ttu-id="4c68b-107">Assicurarsi di [verificare i limiti e le quote dei servizi](http://docs.microsoft.com/azure/azure-subscription-service-limits) prima di eseguire questo esempio con il numero predefinito di macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="4c68b-107">Make sure to [check your service limits and quota](http://docs.microsoft.com/azure/azure-subscription-service-limits) before running this sample with the default number of VMs.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="4c68b-108">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="4c68b-108">Run the sample</span></span>

<span data-ttu-id="4c68b-109">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="4c68b-109">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="4c68b-110">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="4c68b-110">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

<span data-ttu-id="4c68b-111">Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java).</span><span class="sxs-lookup"><span data-stu-id="4c68b-111">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="4c68b-112">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="4c68b-112">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a><span data-ttu-id="4c68b-113">Impostare le posizioni e i conteggi per le macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="4c68b-113">Set locations and counts for the virtual machines</span></span>

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

<span data-ttu-id="4c68b-114">Questo elemento `Map` viene usato in un secondo momento nell'esempio per impostare la distribuzione delle macchine virtuali in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="4c68b-114">This `Map` is used later in the sample to set the distrubtion of the VMs worldwide.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="4c68b-115">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="4c68b-115">Create a resource group</span></span> 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

<span data-ttu-id="4c68b-116">Ogni risorsa presente nell'esempio viene gestita da questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="4c68b-116">Each resource in the sample is managed by this resource group.</span></span> <span data-ttu-id="4c68b-117">Le risorse possono quindi essere eliminate facilmente in un secondo momento eliminando il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="4c68b-117">This makes the resources easy to clean up later by deleting the resource group.</span></span>

## <a name="define-the-virtual-machines"></a><span data-ttu-id="4c68b-118">Definire le macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="4c68b-118">Define the virtual machines</span></span>
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

<span data-ttu-id="4c68b-119">Il ciclo `for` esterno precedente esegue l'iterazione in ogni area, definendo un oggetto Creatable di rete virtuale e un oggetto Creatable di account di archiviazione per l'uso da parte di tutte le macchine virtuali di quell'area.</span><span class="sxs-lookup"><span data-stu-id="4c68b-119">The outer `for` loop above iterates through each region, defining a virtual network Creatable and storage account Creatable for use by all virtual machines in that region.</span></span> <span data-ttu-id="4c68b-120">Vedere altre informazioni sull'uso di oggetti [Creatable](java-sdk-azure-concepts.md#Creatables) per creare risorse solo se necessarie quando si usano le librerie di gestione.</span><span class="sxs-lookup"><span data-stu-id="4c68b-120">Learn more about using [Creatables](java-sdk-azure-concepts.md#Creatables) to create resources only as needed when using the management libraries.</span></span>

<span data-ttu-id="4c68b-121">Il ciclo `for` interno ottiene un oggetto Creatable di indirizzo IP pubblico per la macchina virtuale e quindi definisce un oggetto Creatable di macchina virtuale usando gli oggetti Creatable relativi a rete virtuale, account di archiviazione e indirizzo IP pubblico definiti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4c68b-121">The inner `for` loop gets a public IP address Creatable for the virtual machine and then defines a virtual machine Creatable using the Creatables for the virtual network, storage account, and public IP address defined previously.</span></span>  <span data-ttu-id="4c68b-122">Questo oggetto Creatable VirtualMachine viene quindi aggiunto all'elenco `creatableVirtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="4c68b-122">This VirtualMachine Creatable is then added to the `creatableVirtualMachines` list.</span></span>

## <a name="create-the-virtual-machines"></a><span data-ttu-id="4c68b-123">Creare le macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="4c68b-123">Create the virtual machines</span></span>

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

<span data-ttu-id="4c68b-124">La chiamata `azure.virtualMachines().create(creatableVirtualMachines)` crea nelle aree in parallelo tutte le macchine virtuali definite nell'elenco `creatableVirtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="4c68b-124">The `azure.virtualMachines().create(creatableVirtualMachines)` call creates all of the virtual machines defined in the `creatableVirtualMachines` List in parallel across the regions.</span></span>

<span data-ttu-id="4c68b-125">Usare l'oggetto `CreatedResources<VirtualMachine>` restituito per accedere alle risorse create nella sottoscrizione di Azure durante il metodo `create()`, non solo il tipo `VirtualMachine` restituito.</span><span class="sxs-lookup"><span data-stu-id="4c68b-125">Use the returned `CreatedResources<VirtualMachine>` object to access any resources created in the Azure subscription during the the `create()` method, not just the returned `VirtualMachine` type.</span></span> <span data-ttu-id="4c68b-126">Eseguire il cast del valore restituito da `createdRelatedResources()` al tipo corretto.</span><span class="sxs-lookup"><span data-stu-id="4c68b-126">Cast the returned value from `createdRelatedResources()` to the correct type.</span></span> 

<span data-ttu-id="4c68b-127">Vedere altre informazioni sull'uso di `Creatable<T>` e `CreatedResources` nell'articolo relativo ai [concetti delle librerie](java-sdk-azure-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="4c68b-127">Learn more about working with `Creatable<T>` and `CreatedResources` in our [library concepts article](java-sdk-azure-concepts.md).</span></span>

## <a name="delete-the-resource-group"></a><span data-ttu-id="4c68b-128">Eliminare il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="4c68b-128">Delete the resource group</span></span> 

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

<span data-ttu-id="4c68b-129">Questo blocco elimina le risorse create nell'esempio prima dell'uscita.</span><span class="sxs-lookup"><span data-stu-id="4c68b-129">This block deletes resources created in the sample before the sample exits.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="4c68b-130">Spiegazione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="4c68b-130">Sample explanation</span></span>

<span data-ttu-id="4c68b-131">Visualizzare l'esempio di codice completo in [GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel).</span><span class="sxs-lookup"><span data-stu-id="4c68b-131">View the complete sample code on [Github](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel).</span></span>

<span data-ttu-id="4c68b-132">L'esempio usa oggetti `Creatable` per definire un account di archiviazione e una rete virtuale per ogni area che ospita le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="4c68b-132">The sample uses `Creatable` objects to define a virtual network and storage account for each region hosting the virtual machines.</span></span> <span data-ttu-id="4c68b-133">Vengono quindi definiti oggetti `Creatable` per l'indirizzo IP pubblico di ogni macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="4c68b-133">`Creatable` objects are then defined for the public IP address for each virtual machine.</span></span> <span data-ttu-id="4c68b-134">L'esempio definisce le macchine virtuali con questi oggetti `Creatable` e aggiunge la definizione delle VM all'elenco `virtualMachineCreatable`.</span><span class="sxs-lookup"><span data-stu-id="4c68b-134">The sample defines the virtual machines using these `Creatable` objects, and sample adds the VM definition to the `virtualMachineCreatable` list.</span></span>

<span data-ttu-id="4c68b-135">Dopo che il codice ha aggiunto la definizione di ogni macchina virtuale all'elenco, `azure.virtualMachines().create(creatableVirtualMachines)` crea ogni macchina virtuale in parallelo in Azure.</span><span class="sxs-lookup"><span data-stu-id="4c68b-135">After the code adds every virtual machine definition to the list, `azure.virtualMachines().create(creatableVirtualMachines)` creates each virtual machine in parallel in Azure.</span></span>

<span data-ttu-id="4c68b-136">L'esempio di codice ottiene quindi dall'oggetto CreatedResources restituito gli indirizzi IP per tutte le macchine virtuali create, per creare un'istanza di [Gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) per la distribuzione del carico tra le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="4c68b-136">The sample code then gets the IP addresses for all of the created virtual machines from the returned CreatedResources object to create a [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) to distribute load across the virtual machines.</span></span> 

<span data-ttu-id="4c68b-137">Il blocco `finally` elimina le risorse dalla sottoscrizione di Azure anche in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="4c68b-137">The `finally` block deletes the resources from your Azure subscription even in the case of an error.</span></span>

| <span data-ttu-id="4c68b-138">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="4c68b-138">Class used in sample</span></span> | <span data-ttu-id="4c68b-139">Note</span><span class="sxs-lookup"><span data-stu-id="4c68b-139">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="4c68b-140">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="4c68b-140">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="4c68b-141">Eseguire query sulle proprietà e gestire lo stato delle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="4c68b-141">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="4c68b-142">I risultati vengono recuperati sotto forma di elenco da `azure.virtualMachines().list()` o per nome o ID `azure.virtualMachines().getByResourceGroup()`</span><span class="sxs-lookup"><span data-stu-id="4c68b-142">Retrieved in list form from `azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="4c68b-143">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="4c68b-143">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="4c68b-144">Valori statici che eseguono il mapping a [opzioni relative alle dimensioni delle macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) per l'uso come parametro di `withSize()` quando si definisce una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="4c68b-144">Static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) for use as a parameter to `withSize()` when defining a virtual machine.</span></span>
| [<span data-ttu-id="4c68b-145">PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="4c68b-145">PublicIpAddress</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | <span data-ttu-id="4c68b-146">Definito, ma non immediatamente creato, per ogni macchina virtuale tramite `azure.publicIpAddresses().define()`.</span><span class="sxs-lookup"><span data-stu-id="4c68b-146">Defined, but not immediately created, for each virtual machine through `azure.publicIpAddresses().define()`.</span></span> <span data-ttu-id="4c68b-147">Archiviare la chiave per ogni `Creatable` e recuperarla in un secondo momento tramite `createdRelatedResource()`</span><span class="sxs-lookup"><span data-stu-id="4c68b-147">Store the key for each `Creatable` and retrieve later through `createdRelatedResource()`</span></span>
| [<span data-ttu-id="4c68b-148">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="4c68b-148">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="4c68b-149">Set di opzioni di macchine virtuali Linux usato come parametro per il metodo `withPopularLinuxImage()` durante la definizione di una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="4c68b-149">Set of Linux virtual machine options used as a parameter to `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="4c68b-150">Rete</span><span class="sxs-lookup"><span data-stu-id="4c68b-150">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="4c68b-151">L'esempio definisce una rete virtuale per ogni area tramite `azure.networks().define()`.</span><span class="sxs-lookup"><span data-stu-id="4c68b-151">The sample defines one virtual network for each region through  `azure.networks().define()` .</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4c68b-152">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4c68b-152">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]