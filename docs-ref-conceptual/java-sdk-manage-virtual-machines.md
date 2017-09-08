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
ms.openlocfilehash: e3048b3317477f4b1fb8edf93e4bebad6b7fafce
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a>Gestire le macchine virtuali di Azure dalle applicazioni Java

[Questo esempio](https://github.com/Azure-Samples/compute-java-manage-vm/) usa le [librerie di gestione di Azure per Java](https://github.com/Azure/azure-sdk-for-java) per creare e usare macchine virtuali di Azure.

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Quindi eseguire:

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java).

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a>Creare una macchina virtuale Windows

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

Questo codice:   

0. Definisce un oggetto Creatable `Disk` con una dimensione di 50 GB e un nome casuale per l'uso con una macchina virtuale.
0. Usa la catena `azure.virtualMachines().define()..create()` per creare la macchina virtuale Windows Server 2012. L'API crea l'elemento `Disk` definito nel passaggio precedente insieme alla macchina virtuale. Alla macchina virtuale viene anche collegato un disco dati da 10 GB tramite `withNewDataDisk(10)`.

Vedere altre informazioni sull'uso di [oggetti<T> Creatable](java-sdk-azure-concepts.md#Creatables) per definire le rappresentazioni locali delle risorse e crearle solo nel momento in cui si rendono necessarie per altre risorse di Azure.

## <a name="stop-start-and-restart-a-virtual-machine"></a>Arrestare, avviare e riavviare una macchina virtuale

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

`powerOff()` arresta il sistema operativo della macchina virtuale, ma non dealloca le relative risorse.

## <a name="add-a-virtual-machine-to-an-existing-network"></a>Aggiungere una macchina virtuale a una rete esistente

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

Usare `withPopularLinuxImage` per definire una VM Linux invece di una VM Windows.


## <a name="list-virtual-machines"></a>Elenco di macchine virtuali

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

Elencare tutte le macchine virtuali di una sottoscrizione tramite `azure.virtualMachines().list()` ed eseguire l'iterazione tramite la mappa restituita da `tags()` per gestire le raccolte di macchine virtuali con tag nei gruppi di risorse.

## <a name="update-a-virtual-machine"></a>Aggiornare una macchina virtuale

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

Aggiornare la configurazione della macchina virtuale usando `update()...apply()` e gli stessi metodi usati per configurare la macchina virtuale quando viene creata con `define()...create()`.

## <a name="delete-a-virtual-machine"></a>Eliminare una macchina virtuale

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a>Spiegazione dell'esempio

[Il codice di esempio](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) crea una macchina virtuale Windows con un disco dati da 50 GB. L'esempio crea quindi un secondo disco dati da 10 GB e lo collega a questa macchina virtuale Windows.
L'esempio crea quindi una macchina virtuale Linux nella stessa rete della macchina virtuale Windows.

L'esempio registra informazioni su entrambe le macchine virtuali ed elimina entrambe le macchine prima del completamento.

| Classe usata nell'esempio | Note
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | Eseguire query sulle proprietà e gestire lo stato delle macchine virtuali. I risultati vengono recuperati sotto forma di elenco con `azure.virtualMachines().list()` o per nome o ID `azure.virtualMachines().getByResourceGroup()`
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | Classe con valori statici che eseguono il mapping a [opzioni relative alle dimensioni delle macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), usata per il metodo `withSize()` per definire le risorse allocate alla macchina virtuale.
| [Disco](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | Creare un disco per archiviare i dati usando `withData()` o l'immagine del sistema operativo con il metodo `withLinux` o `withWindows` appropriato quando si definisce il disco. Collegare i dischi alle macchine virtuali al momento della creazione (`using withNewDataDisk` o `withExistingDataDisk`) oppure dopo usando `update()..apply()` sull'oggetto VirtualMachine.
| [DiskSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | Classe con valori statici per definire un disco con un piano di archiviazione Standard o [Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage).
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | Classe con un set di opzioni di macchine virtuali Linux per l'uso con il metodo `withPopularLinuxImage()` durante la definizione di una macchina virtuale.
| [KnownWindowsVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | Classe con un set di opzioni di immagini di macchine virtuali Windows per l'uso con il metodo `withPopularWindowsImage()` durante la definizione di una macchina virtuale.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]