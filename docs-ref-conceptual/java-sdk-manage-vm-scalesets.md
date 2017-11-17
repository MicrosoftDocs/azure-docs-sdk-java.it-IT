---
title: "Gestire set di scalabilità di macchine virtuali con Java | Microsoft Docs"
description: "Codice di esempio per la gestione di set di scalabilità di macchine virtuali di Azure con Azure SDK per Java"
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
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a><span data-ttu-id="e1a3c-103">Gestire set di scalabilità di macchine virtuali di Azure dalle applicazioni Java</span><span class="sxs-lookup"><span data-stu-id="e1a3c-103">Manage Azure virtual machine scale sets from your Java applications</span></span>

<span data-ttu-id="e1a3c-104">[Questo esempio](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets) crea un [set di scalabilità di macchine virtuali](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) usando le [librerie di gestione Java](https://github.com/Azure/azure-sdk-for-java).</span><span class="sxs-lookup"><span data-stu-id="e1a3c-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets) creates a  [virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="e1a3c-105">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="e1a3c-105">Run the sample</span></span>

<span data-ttu-id="e1a3c-106">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="e1a3c-107">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="e1a3c-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

<span data-ttu-id="e1a3c-108">Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java).</span><span class="sxs-lookup"><span data-stu-id="e1a3c-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="e1a3c-109">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="e1a3c-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a><span data-ttu-id="e1a3c-110">Creare una rete virtuale per il set di scalabilità</span><span class="sxs-lookup"><span data-stu-id="e1a3c-110">Create a virtual network for the scale set</span></span>

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

<span data-ttu-id="e1a3c-111">Configurare una rete virtuale e il servizio di bilanciamento del carico prima di creare la definizione del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-111">Set up a virtual network and load balancer before creating the scale set definition.</span></span> <span data-ttu-id="e1a3c-112">Il set di scalabilità usa queste risorse per la configurazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-112">The scale set uses these resources for its initial configuration.</span></span>

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a><span data-ttu-id="e1a3c-113">Creare un servizio di bilanciamento del carico per distribuire il carico nel set di scalabilità</span><span class="sxs-lookup"><span data-stu-id="e1a3c-113">Create a load balancer to distribute load across the scale set</span></span>

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

 <span data-ttu-id="e1a3c-114">Il servizio di bilanciamento del carico definisce due pool di indirizzi di rete back-end, uno per bilanciare il carico in HTTP (`backendPoolName1`) e l'altro per bilanciare il carico in HTTPS (`backendPoolName2`).</span><span class="sxs-lookup"><span data-stu-id="e1a3c-114">The load balancer defines two backend network address pools-one to balance load across HTTP (`backendPoolName1`) and the other to balance load across HTTPS (`backendPoolName2`).</span></span>  <span data-ttu-id="e1a3c-115">I metodi `defineHttpProbe()` configurano gli [endpoint del probe di integrità](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) nei servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-115">The `defineHttpProbe()` methods set up [health probe endpoints](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) on the load balancers.</span></span> <span data-ttu-id="e1a3c-116">Le regole NAT espongono le porte 22 e 23 nelle macchine virtuali del set di scalabilità per l'accesso SSH e telnet.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-116">NAT rules expose ports 22 and 23 on the scale set virtual machines for telnet and SSH access.</span></span>

## <a name="create-a-scale-set"></a><span data-ttu-id="e1a3c-117">Creare un set di scalabilità</span><span class="sxs-lookup"><span data-stu-id="e1a3c-117">Create a scale set</span></span>
 
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

<span data-ttu-id="e1a3c-118">Usare la definizione di rete virtuale e le definizioni del servizio di bilanciamento del carico create nel passaggio precedente per creare un set di scalabilità con tre istanze di Linux (`withCapacity(3)`) e tre dischi dati da 100 GB ognuna.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-118">Use the virtual network definition and load balancer definitions created in the previous step to create a scale set with three Linux instances (`withCapacity(3)`) and three 100GB data disks each.</span></span> <span data-ttu-id="e1a3c-119">La catena del metodo `defineNewExtension()` installa il server web Apache in ogni macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-119">The `defineNewExtension()` method chain installs the Apache web server on each VM.</span></span>

## <a name="list-virtual-machine-scale-set-network-interfaces"></a><span data-ttu-id="e1a3c-120">Elencare le interfacce di rete del set di scalabilità di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="e1a3c-120">List virtual machine scale set network interfaces</span></span>

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a><span data-ttu-id="e1a3c-121">Ottenere le stringhe di connessione SSH per ogni macchina virtuale del set di scalabilità</span><span class="sxs-lookup"><span data-stu-id="e1a3c-121">Get SSH connection strings for each scale set virtual machine</span></span>

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

<span data-ttu-id="e1a3c-122">Il pool NAT creato in precedenza ha eseguito il mapping delle porte telnet e SSH, rispettivamente la 22 e la 23, delle macchine virtuali alle porte del servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-122">The NAT pool created earlier mapped the SSH and telnet ports (22 and 23, respectively) on the virtual machines to ports on the load balancer.</span></span> <span data-ttu-id="e1a3c-123">Questo codice compila la stringa di connessione SSH per ogni macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-123">This code builds the SSH connection string for each virtual machine.</span></span>

## <a name="stop-the-virtual-machine-scale-set"></a><span data-ttu-id="e1a3c-124">Arrestare il set di scalabilità di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="e1a3c-124">Stop the virtual machine scale set</span></span>

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

<span data-ttu-id="e1a3c-125">Le macchine virtuali arrestate continuano a utilizzare le risorse riservate.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-125">Stopped virtual machines continue to consume reserved resources.</span></span> <span data-ttu-id="e1a3c-126">Usare `deallocate()` per arrestare il sistema operativo nelle macchine virtuali e rilasciare le relative risorse di calcolo.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-126">Use `deallocate()` to stop the operating system on the virtual machines and release their compute resources.</span></span>

## <a name="deallocate-the-virtual-machine-scale-set"></a><span data-ttu-id="e1a3c-127">Deallocare il set di scalabilità di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="e1a3c-127">Deallocate the virtual machine scale set</span></span>

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

<span data-ttu-id="e1a3c-128">Deallocate() arresta il sistema operativo nelle macchine virtuali e consente di liberare le risorse calcolo e di rete, ad esempio gli indirizzi IP, usate dalle istanze del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-128">Deallocate() shuts down the operating system on the virtual machines and frees up the compute and network resources (such as IP addresses) used by the scale set instances.</span></span> <span data-ttu-id="e1a3c-129">Per qualsiasi disco, incluso quello del sistema operativo, collegato alle macchine virtuali, si continuano ad accumulare costi di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-129">You continue to accrue storage charges for any disks (including the OS) attached to the virtual machines.</span></span>

## <a name="start-a-virtual-machine-scale-set"></a><span data-ttu-id="e1a3c-130">Avviare un set di scalabilità di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="e1a3c-130">Start a virtual machine scale set</span></span>

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a><span data-ttu-id="e1a3c-131">Aggiornare il numero di istanze di macchine virtuali nel set di scalabilità</span><span class="sxs-lookup"><span data-stu-id="e1a3c-131">Update the number of virtual machines instances in the scale set</span></span>
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

<span data-ttu-id="e1a3c-132">Usare `withCapacity()` per ridimensionare il numero di macchine virtuali nel set di scalabilità e `withSku()` per ridimensionare la capacità di ogni macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-132">Scale the number of virtual machines in the scale set using `withCapacity()` and scale the capacity of each virtual machine using `withSku()`.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="e1a3c-133">Spiegazione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="e1a3c-133">Sample explanation</span></span>

<span data-ttu-id="e1a3c-134">Il [codice di esempio](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) crea prima di tutto una rete virtuale per consentire al set di scalabilità di comunicare e un servizio di bilanciamento del carico per distribuire il traffico tra le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-134">[The sample code](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) first creates a virtual network for the scale set to communicate across and a load balancer to distribute traffic across the virtual machines.</span></span> <span data-ttu-id="e1a3c-135">La catena del metodo `azure.virtualMachineScaleSets().define()...create()` crea il set di scalabilità con tre istanze di Linux in esecuzione nel server Web Apache.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-135">The `azure.virtualMachineScaleSets().define()...create()` method chain creates the scale set with three Linux instances running the Apache web server.</span></span>    
   
| <span data-ttu-id="e1a3c-136">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="e1a3c-136">Class used in sample</span></span> | <span data-ttu-id="e1a3c-137">Note</span><span class="sxs-lookup"><span data-stu-id="e1a3c-137">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="e1a3c-138">VirtualMachineScaleSet</span><span class="sxs-lookup"><span data-stu-id="e1a3c-138">VirtualMachineScaleSet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | <span data-ttu-id="e1a3c-139">Consente di eseguire query, avviare, arrestare, aggiornare ed eliminare tutte le macchine virtuali del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-139">Query, start, stop, update and delete all virtual machines in the scale set.</span></span>
| [<span data-ttu-id="e1a3c-140">VirtualMachineScaleSetVM</span><span class="sxs-lookup"><span data-stu-id="e1a3c-140">VirtualMachineScaleSetVM</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | <span data-ttu-id="e1a3c-141">Recuperata da `virtualMachineScaleSet.virtualMachines().get()` o `list()`, consente di eseguire query, avviare, arrestare, configurare ed eliminare le macchine virtuali del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-141">Retrieved from `virtualMachineScaleSet.virtualMachines().get()` or `list()`, allows you to query, start, stop, configure and delete virtual machines in the scale set.</span></span>
| [<span data-ttu-id="e1a3c-142">VirtualMachineScaleSetNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="e1a3c-142">VirtualMachineScaleSetNetworkInterface</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | <span data-ttu-id="e1a3c-143">Restituita da `virtualMachineScaleSet.listNetworkInterfaces()`, offre una rappresentazione di sola lettura di un'interfaccia di rete in una macchina virtuale in un set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-143">Returned from `virtualMachineScaleSet.listNetworkInterfaces()`, read-only representation of a network interface on a virtual machine in a scale set.</span></span>
| [<span data-ttu-id="e1a3c-144">VirtualMachineScaleSetSkuTypes</span><span class="sxs-lookup"><span data-stu-id="e1a3c-144">VirtualMachineScaleSetSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | <span data-ttu-id="e1a3c-145">Classe dei campi statici che consente di impostare il [livello del set di scalabilità di macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) usato per definire la quantità di risorse utilizzabile per i membri del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-145">Class of static fields used to set the [virtual machine scale set tier](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) used to define how much resources scale set members can consume.</span></span>
| [<span data-ttu-id="e1a3c-146">VirtualMachineScaleSetNicIpConfiguration</span><span class="sxs-lookup"><span data-stu-id="e1a3c-146">VirtualMachineScaleSetNicIpConfiguration</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | <span data-ttu-id="e1a3c-147">Usata eseguire query sulla configurazione IP associata a un'interfaccia di rete in una macchina virtuale del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e1a3c-147">Used to query the IP configuration associated with a network interface on a scale set virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1a3c-148">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e1a3c-148">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]