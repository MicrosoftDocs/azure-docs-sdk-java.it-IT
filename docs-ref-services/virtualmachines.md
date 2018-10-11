---
title: Librerie delle macchine virtuali di Azure per Java
description: ''
keywords: Azure, Java, SDK, API, calcolo, macchine virtuali
author: douge
ms.author: douge
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: compute
ms.openlocfilehash: a54bc40e1d28ba6ee1d8b0638cb259adbb69d78d
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893042"
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="15736-103">Librerie delle macchine virtuali di Azure</span><span class="sxs-lookup"><span data-stu-id="15736-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="15736-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="15736-104">Overview</span></span>

<span data-ttu-id="15736-105">Risorse di calcolo su richiesta e scalabili in esecuzione su Linux o Windows.</span><span class="sxs-lookup"><span data-stu-id="15736-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="15736-106">Per iniziare a usare le macchine virtuali di Azure, vedere [Creare una macchina virtuale Linux con il portale di Azure](/azure/virtual-machines/linux/quick-create-portal).</span><span class="sxs-lookup"><span data-stu-id="15736-106">To get started with Azure virtual machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="15736-107">API di gestione</span><span class="sxs-lookup"><span data-stu-id="15736-107">Management API</span></span>

<span data-ttu-id="15736-108">Creare, configurare e aumentare le macchine virtuali Windows e Linux in Azure dal codice con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="15736-108">Create, configure, and scale out Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="15736-109">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="15736-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.3.0</version>
</dependency>
```   


## <a name="example"></a><span data-ttu-id="15736-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="15736-110">Example</span></span>

<span data-ttu-id="15736-111">Creare una nuova macchina virtuale Linux in un nuovo gruppo di risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="15736-111">Create a new Linux virtual machine in a new Azure resource group.</span></span>

```java
VirtualMachine newLinuxVm = azure.virtualMachines().define(linuxVmName)
            .withRegion(Region.US_EAST)
            .withNewResourceGroup("myResourceGroup")
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
            .withRootUsername(userName)
            .withSshKey(key)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="15736-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="15736-112">Explore the Management APIs</span></span>](/java/api/overview/azure/virtualmachines/management)


## <a name="samples"></a><span data-ttu-id="15736-113">Esempi</span><span class="sxs-lookup"><span data-stu-id="15736-113">Samples</span></span>

<span data-ttu-id="15736-114">[Gestire le macchine virtuali][1] </span><span class="sxs-lookup"><span data-stu-id="15736-114">[Manage virtual machines][1] </span></span>  
<span data-ttu-id="15736-115">[Gestire le reti virtuali][6] </span><span class="sxs-lookup"><span data-stu-id="15736-115">[Manage virtual networks][6] </span></span>  
<span data-ttu-id="15736-116">[Creare una macchina virtuale da un'immagine personalizzata][2] </span><span class="sxs-lookup"><span data-stu-id="15736-116">[Create a virtual machine from a custom image][2] </span></span>  
<span data-ttu-id="15736-117">[Creare macchine virtuali in diverse aree in parallelo][5]  </span><span class="sxs-lookup"><span data-stu-id="15736-117">[Create virtual machines across regions in parallel][5]  </span></span>  
<span data-ttu-id="15736-118">[Creare un set di scalabilità di macchine virtuali con un servizio di bilanciamento del carico][7]</span><span class="sxs-lookup"><span data-stu-id="15736-118">[Create a virtual machine scale set with a load balancer][7]</span></span>    

[1]: ../docs-ref-conceptual/java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[5]: ../docs-ref-conceptual/java-sdk-virtual-machines-in-parallel.md
[6]: ../docs-ref-conceptual/java-sdk-manage-virtual-networks.md
[7]: ../docs-ref-conceptual/java-sdk-manage-vm-scalesets.md

<span data-ttu-id="15736-119">Esplorare altri [esempi di codice Java per le macchine virtuali di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=VM) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="15736-119">Explore more [sample Java code for Azure virtual machines](https://azure.microsoft.com/resources/samples/?platform=java&term=VM) you can use in your apps.</span></span>