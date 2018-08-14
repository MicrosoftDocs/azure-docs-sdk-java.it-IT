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
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a>Creare e gestire reti virtuali di Azure dalle app Java

[Questo esempio](https://github.com/Azure-Samples/network-java-manage-virtual-network) crea una [rete virtuale](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) per isolare le risorse di Azure nel segmento di rete controllato.

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Quindi eseguire:

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java).

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a>Creare un gruppo di sicurezza di rete per bloccare il traffico Internet

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

Questa [regola di sicurezza di rete](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) blocca il traffico Internet pubblico in ingresso e in uscita. Il gruppo di sicurezza di rete non ha effetto fino a quando non viene applicato a una subnet nella rete virtuale.

## <a name="create-a-virtual-network-with-two-subnets"></a>Creare una rete virtuale con due subnet

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

La subnet back-end nega l'accesso a Internet usando le regole impostate nel gruppo di sicurezza di rete. La subnet front-end usa le [regole predefinite](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) che consentono il traffico in uscita verso Internet.

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a>Creare un gruppo di sicurezza di rete per consentire il traffico HTTP in ingresso
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

Questa regola di sicurezza di rete consente il traffico in ingresso sulla porta 80 dalla rete Internet pubblica e blocca tutto il traffico in uscita dalla rete interna verso la rete Internet pubblica. 

## <a name="update-a-virtual-network"></a>Aggiornare una rete virtuale
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

Aggiornare la subnet front-end per consentire il traffico HTTP in ingresso usando la regola di sicurezza di rete creata nel passaggio precedente.

## <a name="create-a-virtual-machine-on-a-subnet"></a>Creare una macchina virtuale in una subnet
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

`withExistingPrimaryNetwork()` e `withSubnet()` configurano la macchina virtuale per l'uso della subnet front-end nella rete virtuale creata nei passaggi precedenti.

## <a name="list-virtual-networks-in-a-resource-group"></a>Elencare le reti virtuali in un gruppo di risorse
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

È possibile elencare e controllare l'oggetto `Network` usando la raccolta esterna o scorrere ogni risorsa figlio per ogni rete usando il ciclo ForEach annidato, come illustrato in questo esempio.

## <a name="delete-a-virtual-network"></a>Eliminare una rete virtuale
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

Rimuovendo una rete virtuale, si eliminano le subnet nella rete, ma non si eliminano le regole del gruppo di sicurezza di rete applicate alle subnet. Tali definizioni possono essere riapplicate ad altre subnet.

## <a name="sample-explanation"></a>Spiegazione dell'esempio

Questo esempio crea una rete virtuale con due subnet e con una macchina virtuale in ogni subnet. La subnet back-end non ha accesso alla rete Internet pubblica. La subnet front-end accetta il traffico HTTP in ingresso da Internet. Entrambe le macchine virtuali nella rete virtuale comunicano tra loro tramite le regole del gruppo di sicurezza di rete predefinite.

| Classe usata nell'esempio | Note
|-------|-------|
| [Rete](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | Rappresentazione di oggetto locale della rete virtuale creata da `azure.networks().define()...create()`. Usare la catena Fluent `update()...apply()` per aggiornare una rete virtuale esistente.
| [Subnet](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | Creare le subnet nella rete virtuale quando si definisce o si aggiorna la rete tramite `withSubnet()`. Ottenere le rappresentazioni di oggetti di una subnet da `Network.subnets().get()` o `Network.subnets().entrySet()`. Questi oggetti hanno metodi per eseguire query sulle proprietà della subnet.
| [NetworkSecurityGroup](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | Classe creata usando la catena Fluent `azure.networkSecurityGroups().define()...create()` e quindi applicata alle subnet mediante l'aggiornamento o la creazione di subnet in una rete virtuale. 

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]