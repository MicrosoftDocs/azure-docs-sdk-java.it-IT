---
title: Gestire macchine virtuali di Azure con Java | Microsoft Docs
description: Codice di esempio per la gestione di macchine virtuali di Azure con Azure SDK per Java
author: rloutlaw
manager: douge
ms.assetid: 88629aee-6279-433e-a08b-4f8e290446d0
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 388b68bfb0fdac70efed5ad5d0f7c957ce915449
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592566"
---
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a><span data-ttu-id="fc116-103">Gestire le macchine virtuali di Azure dalle applicazioni Java</span><span class="sxs-lookup"><span data-stu-id="fc116-103">Manage Azure virtual machines from your Java applications</span></span>

<span data-ttu-id="fc116-104">[Questo esempio](https://github.com/Azure-Samples/compute-java-manage-vm/) usa le [librerie di gestione di Azure per Java](https://github.com/Azure/azure-sdk-for-java) per creare e usare macchine virtuali di Azure.</span><span class="sxs-lookup"><span data-stu-id="fc116-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-vm/) uses the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java) to create and work with Azure virtual machines.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="fc116-105">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="fc116-105">Run the sample</span></span>

<span data-ttu-id="fc116-106">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="fc116-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="fc116-107">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="fc116-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

<span data-ttu-id="fc116-108">Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java).</span><span class="sxs-lookup"><span data-stu-id="fc116-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="fc116-109">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="fc116-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a><span data-ttu-id="fc116-110">Creare una macchina virtuale Windows</span><span class="sxs-lookup"><span data-stu-id="fc116-110">Create a Windows virtual machine</span></span>

```java
// Prepare a data disk for VM
Disk dataDisk = azure.disks().define(SdkContext.randomResourceName("dsk", 30))
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withData()
            .withSizeInGB(50)
            .create();

// create the windows virtual machine with the data disk            
VirtualMachine windowsVM = azure.virtualMachines().define(windowsVmName)
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularWindowsImage(KnownWindowsVirtualMachineImage.WINDOWS_SERVER_2012_R2_DATACENTER)
            .withAdminUsername(userName)
            .withAdminPassword(password)
            .withNewDataDisk(10)
            .withNewDataDisk(dataDiskCreatable)
            .withExistingDataDisk(dataDisk)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

<span data-ttu-id="fc116-111">Questo codice:</span><span class="sxs-lookup"><span data-stu-id="fc116-111">This code:</span></span>   

0. <span data-ttu-id="fc116-112">Definisce un oggetto Creatable `Disk` con una dimensione di 50 GB e un nome casuale per l'uso con una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fc116-112">Defines a `Disk` Creatable with a 50GB size and random name for use with a virtual machine.</span></span>
0. <span data-ttu-id="fc116-113">Usa la catena `azure.virtualMachines().define()..create()` per creare la macchina virtuale Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="fc116-113">Uses the `azure.virtualMachines().define()..create()` chain to create the Windows Server 2012 virtual machine.</span></span> <span data-ttu-id="fc116-114">L'API crea l'elemento `Disk` definito nel passaggio precedente insieme alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fc116-114">The API creates the `Disk` defined in the previous step the same time as the virtual machine.</span></span> <span data-ttu-id="fc116-115">Alla macchina virtuale viene anche collegato un disco dati da 10 GB tramite `withNewDataDisk(10)`.</span><span class="sxs-lookup"><span data-stu-id="fc116-115">A 10GB data disk is also attached to the virtual machine through `withNewDataDisk(10)`.</span></span>

<span data-ttu-id="fc116-116">Vedere altre informazioni sull'uso di [oggetti<T> Creatable](java-sdk-azure-concepts.md#Creatables) per definire le rappresentazioni locali delle risorse e crearle solo nel momento in cui si rendono necessarie per altre risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="fc116-116">Learn more about using [Creatable<T> objects](java-sdk-azure-concepts.md#Creatables) to define local representations of resources and create them just as other Azure resources need them.</span></span>

## <a name="stop-start-and-restart-a-virtual-machine"></a><span data-ttu-id="fc116-117">Arrestare, avviare e riavviare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="fc116-117">Stop, start, and restart a virtual machine</span></span>

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

<span data-ttu-id="fc116-118">`powerOff()` arresta il sistema operativo della macchina virtuale, ma non dealloca le relative risorse.</span><span class="sxs-lookup"><span data-stu-id="fc116-118">`powerOff()` stops the virtual machine operating system but does not deallocate its resources.</span></span>

## <a name="add-a-virtual-machine-to-an-existing-network"></a><span data-ttu-id="fc116-119">Aggiungere una macchina virtuale a una rete esistente</span><span class="sxs-lookup"><span data-stu-id="fc116-119">Add a virtual machine to an existing network</span></span>

```java
// Get the virtual network the current virtual machine is using
Network network = windowsVM.getPrimaryNetworkInterface().primaryIPConfiguration().getNetwork();

// Create a Linux VM in the same subnet
VirtualMachine linuxVM = azure.virtualMachines().define(linuxVmName)
           .withRegion(region)
           .withExistingResourceGroup(rgName)
           .withExistingPrimaryNetwork(network)
           .withSubnet("subnet1") // default subnet name when no name specified at creation
           .withPrimaryPrivateIPAddressDynamic()
           .withoutPrimaryPublicIPAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withRootPassword(password)
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
```

<span data-ttu-id="fc116-120">Usare `withPopularLinuxImage` per definire una VM Linux invece di una VM Windows.</span><span class="sxs-lookup"><span data-stu-id="fc116-120">Use `withPopularLinuxImage` to define a Linux VM instead of a Windows one.</span></span>


## <a name="list-virtual-machines"></a><span data-ttu-id="fc116-121">Elenco di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="fc116-121">List virtual machines</span></span>

```java
// get a list of VMs in the same resource group as an existing VM
String resourceGroupName = windowsVM.resourceGroupName();
PagedList<VirtualMachine> resourceGroupVMs = azure.virtualMachines()
        .listByResourceGroup(resourceGroupName); 

// for each vitual machine in the resource group, log their name and plan
for (VirtualMachine virtualMachine : azure.virtualMachines().listByResourceGroup(resourceGroupName)) {
    System.out.println("VM " + virtualMachine.computerName() + 
        " has plan " + virtualMachine.plan());
}
```

<span data-ttu-id="fc116-122">Elencare tutte le macchine virtuali di una sottoscrizione tramite `azure.virtualMachines().list()` ed eseguire l'iterazione tramite la mappa restituita da `tags()` per gestire le raccolte di macchine virtuali con tag nei gruppi di risorse.</span><span class="sxs-lookup"><span data-stu-id="fc116-122">List all virtual machines for a subscription using `azure.virtualMachines().list()` and iterate through the Map returned by `tags()` to manage tagged collections of virtual machines across resource groups.</span></span>

## <a name="update-a-virtual-machine"></a><span data-ttu-id="fc116-123">Aggiornare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="fc116-123">Update a virtual machine</span></span>

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

<span data-ttu-id="fc116-124">Aggiornare la configurazione della macchina virtuale usando `update()...apply()` e gli stessi metodi usati per configurare la macchina virtuale quando viene creata con `define()...create()`.</span><span class="sxs-lookup"><span data-stu-id="fc116-124">Update the virtual machine configuration using `update()...apply()` and the same methods used to configure the virtual machine when created through `define()...create()`.</span></span>

## <a name="delete-a-virtual-machine"></a><span data-ttu-id="fc116-125">Eliminare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="fc116-125">Delete a virtual machine</span></span>

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a><span data-ttu-id="fc116-126">Spiegazione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="fc116-126">Sample explanation</span></span>

<span data-ttu-id="fc116-127">[Il codice di esempio](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) crea una macchina virtuale Windows con un disco dati da 50 GB.</span><span class="sxs-lookup"><span data-stu-id="fc116-127">[The sample code](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) creates a Windows virtual machine with a 50GB data disk.</span></span> <span data-ttu-id="fc116-128">L'esempio crea quindi un secondo disco dati da 10 GB e lo collega a questa macchina virtuale Windows.</span><span class="sxs-lookup"><span data-stu-id="fc116-128">The sample then creates a second 10GB data disk and attaches it to this Windows virtual machine.</span></span>
<span data-ttu-id="fc116-129">L'esempio crea quindi una macchina virtuale Linux nella stessa rete della macchina virtuale Windows.</span><span class="sxs-lookup"><span data-stu-id="fc116-129">Then the sample creates a Linux virtual machine in the same virtual network as the Windows virtual machine.</span></span>

<span data-ttu-id="fc116-130">L'esempio registra informazioni su entrambe le macchine virtuali ed elimina entrambe le macchine prima del completamento.</span><span class="sxs-lookup"><span data-stu-id="fc116-130">The sample logs information about both virtual machines and deletes them both before completing.</span></span>

| <span data-ttu-id="fc116-131">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="fc116-131">Class used in sample</span></span> | <span data-ttu-id="fc116-132">Note</span><span class="sxs-lookup"><span data-stu-id="fc116-132">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="fc116-133">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="fc116-133">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="fc116-134">Eseguire query sulle proprietà e gestire lo stato delle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="fc116-134">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="fc116-135">I risultati vengono recuperati sotto forma di elenco con `azure.virtualMachines().list()` o per nome o ID `azure.virtualMachines().getByResourceGroup()`</span><span class="sxs-lookup"><span data-stu-id="fc116-135">Retrieved in list form  with`azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="fc116-136">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="fc116-136">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="fc116-137">Classe con valori statici che eseguono il mapping a [opzioni relative alle dimensioni delle macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), usata per il metodo `withSize()` per definire le risorse allocate alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fc116-137">Class with static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), used by the `withSize()` method to define the resources allocated to the VM.</span></span>
| [<span data-ttu-id="fc116-138">Disco</span><span class="sxs-lookup"><span data-stu-id="fc116-138">Disk</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | <span data-ttu-id="fc116-139">Creare un disco per archiviare i dati usando `withData()` o l'immagine del sistema operativo con il metodo `withLinux` o `withWindows` appropriato quando si definisce il disco.</span><span class="sxs-lookup"><span data-stu-id="fc116-139">Create a disk to store data using `withData()` or operating system image using the appropriate `withLinux` or `withWindows` method when defining the disk.</span></span> <span data-ttu-id="fc116-140">Collegare i dischi alle macchine virtuali al momento della creazione (`using withNewDataDisk` o `withExistingDataDisk`) oppure dopo usando `update()..apply()` sull'oggetto VirtualMachine.</span><span class="sxs-lookup"><span data-stu-id="fc116-140">Attach disks to virtual machines either at the time of creation (`using withNewDataDisk` or `withExistingDataDisk`) or after creation by `update()..apply()` on the VirtualMachine object.</span></span>
| [<span data-ttu-id="fc116-141">DiskSkuTypes</span><span class="sxs-lookup"><span data-stu-id="fc116-141">DiskSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | <span data-ttu-id="fc116-142">Classe con valori statici per definire un disco con un piano di archiviazione Standard o [Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="fc116-142">Class with static values to define a disk with a standard or [premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) storage plan.</span></span>
| [<span data-ttu-id="fc116-143">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="fc116-143">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="fc116-144">Classe con un set di opzioni di macchine virtuali Linux per l'uso con il metodo `withPopularLinuxImage()` durante la definizione di una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fc116-144">Class with a set of Linux virtual machine options for use with the `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="fc116-145">KnownWindowsVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="fc116-145">KnownWindowsVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | <span data-ttu-id="fc116-146">Classe con un set di opzioni di immagini di macchine virtuali Windows per l'uso con il metodo `withPopularWindowsImage()` durante la definizione di una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fc116-146">Class with a set of Windows virtual machine image options for use with the `withPopularWindowsImage()` method when defining a virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc116-147">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fc116-147">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]