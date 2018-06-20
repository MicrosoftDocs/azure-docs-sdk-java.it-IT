---
title: Gestire reti virtuali di Azure con Java | Microsoft Docs
description: Codice di esempio per gestire reti virtuali di Azure nel codice Java
author: rloutlaw
manager: douge
ms.assetid: 92736911-3df6-46e7-b751-25bb36bf89b9
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 3d21cdd890912c1fc58fc65a79ba972b8327edeb
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931087"
---
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a><span data-ttu-id="e8fa8-103">Creare e gestire reti virtuali di Azure dalle app Java</span><span class="sxs-lookup"><span data-stu-id="e8fa8-103">Create and manage Azure virtual networks from your Java apps</span></span>

<span data-ttu-id="e8fa8-104">[Questo esempio](https://github.com/Azure-Samples/network-java-manage-virtual-network) crea una [rete virtuale](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) per isolare le risorse di Azure nel segmento di rete controllato.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-104">[This sample](https://github.com/Azure-Samples/network-java-manage-virtual-network) creates a [virtual network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) to isolate your Azure resources on network segment you control.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="e8fa8-105">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="e8fa8-105">Run the sample</span></span>

<span data-ttu-id="e8fa8-106">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="e8fa8-107">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="e8fa8-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

<span data-ttu-id="e8fa8-108">Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java).</span><span class="sxs-lookup"><span data-stu-id="e8fa8-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="e8fa8-109">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="e8fa8-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a><span data-ttu-id="e8fa8-110">Creare un gruppo di sicurezza di rete per bloccare il traffico Internet</span><span class="sxs-lookup"><span data-stu-id="e8fa8-110">Create a network security group to block Internet traffic</span></span>

```java
// this NSG definition block traffic to and from the public Internet
NetworkSecurityGroup backEndSubnetNsg = azure.networkSecurityGroups()
              .define(vnet1BackEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .defineRule("DenyInternetInComing")
                        .denyInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

<span data-ttu-id="e8fa8-111">Questa [regola di sicurezza di rete](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) blocca il traffico Internet pubblico in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-111">This [network security rule](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) blocks both inbound and outbound public Internet traffic.</span></span> <span data-ttu-id="e8fa8-112">Il gruppo di sicurezza di rete non ha effetto fino a quando non viene applicato a una subnet nella rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-112">This network security group will not have an effect until applied to a subnet in your virtual network.</span></span>

## <a name="create-a-virtual-network-with-two-subnets"></a><span data-ttu-id="e8fa8-113">Creare una rete virtuale con due subnet</span><span class="sxs-lookup"><span data-stu-id="e8fa8-113">Create a virtual network with two subnets</span></span>

```java
// create the a virtual network with two subnets
// assign the backend subnet a rule blocking public internet traffic
Network virtualNetwork1 = azure.networks().define(vnetName1)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withAddressSpace("192.168.0.0/16")
                    .withSubnet(vnet1FrontEndSubnetName, "192.168.1.0/24")
                    .defineSubnet(vnet1BackEndSubnetName)
                        .withAddressPrefix("192.168.2.0/24")
                        .withExistingNetworkSecurityGroup(backEndSubnetNsg)
                        .attach()
                    .create();
```

<span data-ttu-id="e8fa8-114">La subnet back-end nega l'accesso a Internet usando le regole impostate nel gruppo di sicurezza di rete.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-114">The backend subnet denies Internet access usingfollowing the rules set in the network security group.</span></span> <span data-ttu-id="e8fa8-115">La subnet front-end usa le [regole predefinite](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) che consentono il traffico in uscita verso Internet.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-115">The front end subnet uses the [default rules](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) which allow outbound traffic to the Internet.</span></span>

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a><span data-ttu-id="e8fa8-116">Creare un gruppo di sicurezza di rete per consentire il traffico HTTP in ingresso</span><span class="sxs-lookup"><span data-stu-id="e8fa8-116">Create a network security group to allow inbound HTTP traffic</span></span>
```java
// create a rule that allows inbound HTTP and blocks outbound Internet traffic
NetworkSecurityGroup frontEndSubnetNsg = azure.networkSecurityGroups()
               .define(vnet1FrontEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .defineRule("AllowHttpInComing")
                        .allowInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toPort(80)
                        .withProtocol(SecurityRuleProtocol.TCP)
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

<span data-ttu-id="e8fa8-117">Questa regola di sicurezza di rete consente il traffico in ingresso sulla porta 80 dalla rete Internet pubblica e blocca tutto il traffico in uscita dalla rete interna verso la rete Internet pubblica.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-117">This network security rule opens up inbound traffic on port 80 from the public Internet, and blocks all outbound traffic from inside the network to the public Internet.</span></span> 

## <a name="update-a-virtual-network"></a><span data-ttu-id="e8fa8-118">Aggiornare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="e8fa8-118">Update a virtual network</span></span>
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

<span data-ttu-id="e8fa8-119">Aggiornare la subnet front-end per consentire il traffico HTTP in ingresso usando la regola di sicurezza di rete creata nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-119">Update the front end subnet to allow inbound HTTP traffic using the network security rule created in the previous step.</span></span>

## <a name="create-a-virtual-machine-on-a-subnet"></a><span data-ttu-id="e8fa8-120">Creare una macchina virtuale in una subnet</span><span class="sxs-lookup"><span data-stu-id="e8fa8-120">Create a virtual machine on a subnet</span></span>
```java
// attach the new VM to the front end subnet on the virtual network
VirtualMachine frontEndVM = azure.virtualMachines().define(frontEndVmName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withExistingPrimaryNetwork(virtualNetwork1) 
                    .withSubnet(vnet1FrontEndSubnetName)
                    .withPrimaryPrivateIpAddressDynamic()
                    .withNewPrimaryPublicIpAddress(publicIpAddressLeafDnsForFrontEndVm)
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();
```

<span data-ttu-id="e8fa8-121">`withExistingPrimaryNetwork()` e `withSubnet()` configurano la macchina virtuale per l'uso della subnet front-end nella rete virtuale creata nei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-121">`withExistingPrimaryNetwork()` and `withSubnet()` configure the virtual machine to use the front-end subnet on the virtual network created in the previous steps.</span></span>

## <a name="list-virtual-networks-in-a-resource-group"></a><span data-ttu-id="e8fa8-122">Elencare le reti virtuali in un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="e8fa8-122">List virtual networks in a resource group</span></span>
```java
// iterate over every virtual network in the resource group 
for (Network virtualNetwork : azure.networks().listByResourceGroup(rgName)) {
    // for each subnet on the virtual network, log the network address prefix 
    for (Map.Entry<String, Subnet> entry : virtualNetwork.subnets().entrySet()) {
        String subnetName = entry.getKey();
        Subnet subnet = entry.getValue();
        System.out.println("Address prefix for subnet " + subnetName + 
             " is " + subnet.addressPrefix());
    }
}
```       

<span data-ttu-id="e8fa8-123">È possibile elencare e controllare l'oggetto `Network` usando la raccolta esterna o scorrere ogni risorsa figlio per ogni rete usando il ciclo ForEach annidato, come illustrato in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-123">You can list and inspect `Network` object using the outer collection or iterate through each child resource for each network using the nested for-each loop as seen in this example.</span></span>

## <a name="delete-a-virtual-network"></a><span data-ttu-id="e8fa8-124">Eliminare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="e8fa8-124">Delete a virtual network</span></span>
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

<span data-ttu-id="e8fa8-125">Rimuovendo una rete virtuale, si eliminano le subnet nella rete, ma non si eliminano le regole del gruppo di sicurezza di rete applicate alle subnet.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-125">Removing a virtual network deletes the subnets on the network but does not delete the network security group rules applied to the subnets.</span></span> <span data-ttu-id="e8fa8-126">Tali definizioni possono essere riapplicate ad altre subnet.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-126">Those definitions can be reapplied to other subnets.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="e8fa8-127">Spiegazione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="e8fa8-127">Sample explanation</span></span>

<span data-ttu-id="e8fa8-128">Questo esempio crea una rete virtuale con due subnet e con una macchina virtuale in ogni subnet.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-128">This sample creates a virtual network with two subnets and with one virtual machine on each subnet.</span></span> <span data-ttu-id="e8fa8-129">La subnet back-end non ha accesso alla rete Internet pubblica.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-129">The back subnet is cut off from the public Internet.</span></span> <span data-ttu-id="e8fa8-130">La subnet front-end accetta il traffico HTTP in ingresso da Internet.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-130">The front-facing subnet accepts inbound HTTP traffic from the Internet.</span></span> <span data-ttu-id="e8fa8-131">Entrambe le macchine virtuali nella rete virtuale comunicano tra loro tramite le regole del gruppo di sicurezza di rete predefinite.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-131">Both virtual machines in the virtual network communicate with each other through the default network security group rules.</span></span>

| <span data-ttu-id="e8fa8-132">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="e8fa8-132">Class used in sample</span></span> | <span data-ttu-id="e8fa8-133">Note</span><span class="sxs-lookup"><span data-stu-id="e8fa8-133">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="e8fa8-134">Rete</span><span class="sxs-lookup"><span data-stu-id="e8fa8-134">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="e8fa8-135">Rappresentazione di oggetto locale della rete virtuale creata da `azure.networks().define()...create()`.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-135">Local object representation of the virtual network created from `azure.networks().define()...create()` .</span></span> <span data-ttu-id="e8fa8-136">Usare la catena Fluent `update()...apply()` per aggiornare una rete virtuale esistente.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-136">Use the `update()...apply()` fluent chain to update an existing virtual network.</span></span>
| [<span data-ttu-id="e8fa8-137">Subnet</span><span class="sxs-lookup"><span data-stu-id="e8fa8-137">Subnet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | <span data-ttu-id="e8fa8-138">Creare le subnet nella rete virtuale quando si definisce o si aggiorna la rete tramite `withSubnet()`.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-138">Create subnets on the virtual network when defining or updating the network using `withSubnet()`.</span></span> <span data-ttu-id="e8fa8-139">Ottenere le rappresentazioni di oggetti di una subnet da `Network.subnets().get()` o `Network.subnets().entrySet()`.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-139">Get object representations of a subnet from `Network.subnets().get()` or `Network.subnets().entrySet()`.</span></span> <span data-ttu-id="e8fa8-140">Questi oggetti hanno metodi per eseguire query sulle proprietà della subnet.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-140">These objects have methods to query subnet properties.</span></span>
| [<span data-ttu-id="e8fa8-141">NetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="e8fa8-141">NetworkSecurityGroup</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | <span data-ttu-id="e8fa8-142">Classe creata usando la catena Fluent `azure.networkSecurityGroups().define()...create()` e quindi applicata alle subnet mediante l'aggiornamento o la creazione di subnet in una rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="e8fa8-142">Created using the `azure.networkSecurityGroups().define()...create()` fluent chain and then applied to subnets through the updating or creating subnets in a virtual network.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e8fa8-143">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e8fa8-143">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]