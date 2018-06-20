---
title: Gestire set di scalabilità di macchine virtuali con Java | Microsoft Docs
description: Codice di esempio per la gestione di set di scalabilità di macchine virtuali di Azure con Azure SDK per Java
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 4653726b387369c18942b6c11392f15b9f0351f3
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931157"
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a>Gestire set di scalabilità di macchine virtuali di Azure dalle applicazioni Java

[Questo esempio](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets) crea un [set di scalabilità di macchine virtuali](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) usando le [librerie di gestione Java](https://github.com/Azure/azure-sdk-for-java). 

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Quindi eseguire:

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java).

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a>Creare una rete virtuale per il set di scalabilità

```java
Network network = azure.networks().define(vnetName)
                    .withRegion(region)
                    .withNewResourceGroup(rgName)
                    .withAddressSpace("172.16.0.0/16")
                    .defineSubnet("Front-end")
                    .withAddressPrefix("172.16.1.0/24")
                    .attach()
                    .create();
```

Configurare una rete virtuale e il servizio di bilanciamento del carico prima di creare la definizione del set di scalabilità. Il set di scalabilità usa queste risorse per la configurazione iniziale.

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a>Creare un servizio di bilanciamento del carico per distribuire il carico nel set di scalabilità

```java
LoadBalancer loadBalancer1 = azure.loadBalancers().define(loadBalancerName1)
                    .withRegion(region)
                    .withExistingResourceGroup(rgName)
                    // assign a public IP address to the load balancer
                    .definePublicFrontend(frontendName)
                        .withExistingPublicIPAddress(publicIPAddress)
                        .attach()
                    // Add two backend address pools
                    .defineBackend(backendPoolName1)
                        .attach()
                    .defineBackend(backendPoolName2)
                        .attach()
                    // Add two health probes on 80 and 443
                    .defineHttpProbe(httpProbe)
                        .withRequestPath("/")
                        .withPort(80)
                        .attach()
                    .defineHttpProbe(httpsProbe)
                        .withRequestPath("/")
                        .withPort(443)
                        .attach()

                    // balance HTTP and HTTPs traffic
                    .defineLoadBalancingRule(httpLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(80)
                        .withProbe(httpProbe)
                        .withBackend(backendPoolName1)
                        .attach()
                    .defineLoadBalancingRule(httpsLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(443)
                        .withProbe(httpsProbe)
                        .withBackend(backendPoolName2)
                        .attach()

                    // Add NAT definitions to enable SSH and telnet to the VMs 
                    .defineInboundNatPool(natPool50XXto22)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(5000, 5099)
                        .withBackendPort(22)
                        .attach()
                    .defineInboundNatPool(natPool60XXto23)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(6000, 6099)
                        .withBackendPort(23)
                        .attach()
                    .create();
```

 Il servizio di bilanciamento del carico definisce due pool di indirizzi di rete back-end, uno per bilanciare il carico in HTTP (`backendPoolName1`) e l'altro per bilanciare il carico in HTTPS (`backendPoolName2`).  I metodi `defineHttpProbe()` configurano gli [endpoint del probe di integrità](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) nei servizi di bilanciamento del carico. Le regole NAT espongono le porte 22 e 23 nelle macchine virtuali del set di scalabilità per l'accesso SSH e telnet.

## <a name="create-a-scale-set"></a>Creare un set di scalabilità
 
```java
 // Create a virtual machine scale set with three virtual machines
 // And, install Apache Web servers on them
VirtualMachineScaleSet virtualMachineScaleSet = azure.virtualMachineScaleSets()
       .define(vmssName)
                .withRegion(region)
                .withExistingResourceGroup(rgName)
                .withSku(VirtualMachineScaleSetSkuTypes.STANDARD_D3_V2)
                .withExistingPrimaryNetworkSubnet(network, "Front-end")
                .withExistingPrimaryInternetFacingLoadBalancer(loadBalancer1)
                .withPrimaryInternetFacingLoadBalancerBackends(backendPoolName1, backendPoolName2)
                .withPrimaryInternetFacingLoadBalancerInboundNatPools(natPool50XXto22, natPool60XXto23)
                .withoutPrimaryInternalLoadBalancer()
                .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                .withRootUsername(userName)
                .withSsh(sshKey)
                .withNewDataDisk(100)
                .withNewDataDisk(100, 1, CachingTypes.READ_WRITE)
                .withNewDataDisk(100, 2, 
                     CachingTypes.READ_WRITE, StorageAccountTypes.STANDARD_LRS)
                .withCapacity(3)
                // Use a VM extension to install Apache Web servers
                .defineNewExtension("CustomScriptForLinux")
                    .withPublisher("Microsoft.OSTCExtensions")
                    .withType("CustomScriptForLinux")
                    .withVersion("1.4")
                    .withMinorVersionAutoUpgrade()
                    .withPublicSetting("fileUris", fileUris)
                    .withPublicSetting("commandToExecute", installCommand)
                    .attach()
                .create();
```

Usare la definizione di rete virtuale e le definizioni del servizio di bilanciamento del carico create nel passaggio precedente per creare un set di scalabilità con tre istanze di Linux (`withCapacity(3)`) e tre dischi dati da 100 GB ognuna. La catena del metodo `defineNewExtension()` installa il server web Apache in ogni macchina virtuale.

## <a name="list-virtual-machine-scale-set-network-interfaces"></a>Elencare le interfacce di rete del set di scalabilità di macchine virtuali

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a>Ottenere le stringhe di connessione SSH per ogni macchina virtuale del set di scalabilità

```java
for (VirtualMachineScaleSetVM instance : virtualMachineScaleSet.virtualMachines().list()) {
    System.out.println("Scale set virtual machine instance #" + instance.instanceId());
    System.out.println(instance.id());
    PagedList<VirtualMachineScaleSetNetworkInterface> networkInterfaces = 
         instance.listNetworkInterfaces();
    // Pick the first NIC on the instance and use its primary IP address
    VirtualMachineScaleSetNetworkInterface networkInterface = networkInterfaces.get(0);
    for (VirtualMachineScaleSetNicIPConfiguration ipConfig : networkInterface.ipConfigurations().values()) {
        if (ipConfig.isPrimary()) {
            List<LoadBalancerInboundNatRule> natRules = ipConfig.listAssociatedLoadBalancerInboundNatRules();
            for (LoadBalancerInboundNatRule natRule : natRules) {
                // find rule matching the inbound SSH port on the backend for the IP address
                // and return the SSH connection string to that port on the load balancer
                if (natRule.backendPort() == 22) {
                    System.out.println("SSH connection string: " + userName + 
                        "@" + publicIPAddress.fqdn() + ":" + natRule.frontendPort());
                break;
                }
            }
            break;
        }
    }
}
```

Il pool NAT creato in precedenza ha eseguito il mapping delle porte telnet e SSH, rispettivamente la 22 e la 23, delle macchine virtuali alle porte del servizio di bilanciamento del carico. Questo codice compila la stringa di connessione SSH per ogni macchina virtuale.

## <a name="stop-the-virtual-machine-scale-set"></a>Arrestare il set di scalabilità di macchine virtuali

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

Le macchine virtuali arrestate continuano a utilizzare le risorse riservate. Usare `deallocate()` per arrestare il sistema operativo nelle macchine virtuali e rilasciare le relative risorse di calcolo.

## <a name="deallocate-the-virtual-machine-scale-set"></a>Deallocare il set di scalabilità di macchine virtuali

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

Deallocate() arresta il sistema operativo nelle macchine virtuali e consente di liberare le risorse calcolo e di rete, ad esempio gli indirizzi IP, usate dalle istanze del set di scalabilità. Per qualsiasi disco, incluso quello del sistema operativo, collegato alle macchine virtuali, si continuano ad accumulare costi di archiviazione.

## <a name="start-a-virtual-machine-scale-set"></a>Avviare un set di scalabilità di macchine virtuali

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a>Aggiornare il numero di istanze di macchine virtuali nel set di scalabilità
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

Usare `withCapacity()` per ridimensionare il numero di macchine virtuali nel set di scalabilità e `withSku()` per ridimensionare la capacità di ogni macchina virtuale.

## <a name="sample-explanation"></a>Spiegazione dell'esempio

Il [codice di esempio](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) crea prima di tutto una rete virtuale per consentire al set di scalabilità di comunicare e un servizio di bilanciamento del carico per distribuire il traffico tra le macchine virtuali. La catena del metodo `azure.virtualMachineScaleSets().define()...create()` crea il set di scalabilità con tre istanze di Linux in esecuzione nel server Web Apache.    
   
| Classe usata nell'esempio | Note
|-------|-------|
| [VirtualMachineScaleSet](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | Consente di eseguire query, avviare, arrestare, aggiornare ed eliminare tutte le macchine virtuali del set di scalabilità.
| [VirtualMachineScaleSetVM](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | Recuperata da `virtualMachineScaleSet.virtualMachines().get()` o `list()`, consente di eseguire query, avviare, arrestare, configurare ed eliminare le macchine virtuali del set di scalabilità.
| [VirtualMachineScaleSetNetworkInterface](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | Restituita da `virtualMachineScaleSet.listNetworkInterfaces()`, offre una rappresentazione di sola lettura di un'interfaccia di rete in una macchina virtuale in un set di scalabilità.
| [VirtualMachineScaleSetSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | Classe dei campi statici che consente di impostare il [livello del set di scalabilità di macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) usato per definire la quantità di risorse utilizzabile per i membri del set di scalabilità.
| [VirtualMachineScaleSetNicIpConfiguration](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | Usata eseguire query sulla configurazione IP associata a un'interfaccia di rete in una macchina virtuale del set di scalabilità.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]