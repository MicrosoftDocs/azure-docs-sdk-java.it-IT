---
ms.openlocfilehash: ab31ee32ea940db2d7bcfa2fe36475d8a648bfc9
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592616"
---
| <span data-ttu-id="b9037-101">**Creare macchine virtuali**</span><span class="sxs-lookup"><span data-stu-id="b9037-101">**Create virtual machines**</span></span> || 
|---|---|
| <span data-ttu-id="b9037-102">[Gestire le macchine virtuali][1]</span><span class="sxs-lookup"><span data-stu-id="b9037-102">[Manage virtual machines][1]</span></span> | <span data-ttu-id="b9037-103">Creare, modificare, avviare, arrestare ed eliminare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="b9037-103">Create, modify, start, stop, and delete virtual machines.</span></span> |
| <span data-ttu-id="b9037-104">[Creare una macchina virtuale da un'immagine personalizzata][2]</span><span class="sxs-lookup"><span data-stu-id="b9037-104">[Create a virtual machine from a custom image][2]</span></span> | <span data-ttu-id="b9037-105">Creare un'immagine personalizzata di macchina virtuale e usarla per creare nuove macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="b9037-105">Create a custom virtual machine image and use it to create new virtual machines.</span></span> | 
| <span data-ttu-id="b9037-106">[Creare una macchina virtuale che usa un disco rigido virtuale specializzato da uno snapshot][3]</span><span class="sxs-lookup"><span data-stu-id="b9037-106">[Create a virtual machine using specialized VHD from a snapshot][3]</span></span> | <span data-ttu-id="b9037-107">Creare snapshot dal sistema operativo e dai dischi dati della macchina virtuale, creare dischi gestiti dagli snapshot e quindi creare una macchina virtuale collegando i dischi gestiti.</span><span class="sxs-lookup"><span data-stu-id="b9037-107">Create snapshot from the virtual machine's OS and data disks, create managed disks from the snapshots, and then create a virtual machine by attaching the managed disks.</span></span> |  
| <span data-ttu-id="b9037-108">[Creare macchine virtuali in parallelo nella stessa rete][4]</span><span class="sxs-lookup"><span data-stu-id="b9037-108">[Create virtual machines in parallel in the same network][4]</span></span> | <span data-ttu-id="b9037-109">Creare macchine virtuali nella stessa area sulla stessa rete virtuale con due subnet in parallelo.</span><span class="sxs-lookup"><span data-stu-id="b9037-109">Create virtual machines in the same region on the same virtual network with two subnets in parallel.</span></span> |
| <span data-ttu-id="b9037-110">[Creare macchine virtuali in diverse aree in parallelo][5]</span><span class="sxs-lookup"><span data-stu-id="b9037-110">[Create virtual machines across regions in parallel][5]</span></span> | <span data-ttu-id="b9037-111">Creare e applicare il bilanciamento del carico a un set di macchine virtuali in diverse aree di Azure.</span><span class="sxs-lookup"><span data-stu-id="b9037-111">Create and load-balance a set of virtual machines across multiple Azure regions.</span></span> |
| <span data-ttu-id="b9037-112">**Macchine virtuali di rete**</span><span class="sxs-lookup"><span data-stu-id="b9037-112">**Network virtual machines**</span></span> || 
| <span data-ttu-id="b9037-113">[Gestire le reti virtuali][6]</span><span class="sxs-lookup"><span data-stu-id="b9037-113">[Manage virtual networks][6]</span></span> | <span data-ttu-id="b9037-114">Configurare una rete virtuale con due subnet e limitare l'accesso Internet a tali subnet.</span><span class="sxs-lookup"><span data-stu-id="b9037-114">Set up a virtual network with two subnets and restrict Internet access to them.</span></span> |
| <span data-ttu-id="b9037-115">**Creare set di scalabilità**</span><span class="sxs-lookup"><span data-stu-id="b9037-115">**Create scale sets**</span></span> ||
| <span data-ttu-id="b9037-116">[Creare un set di scalabilità di macchine virtuali con un servizio di bilanciamento del carico][7]</span><span class="sxs-lookup"><span data-stu-id="b9037-116">[Create a virtual machine scale set with a load balancer][7]</span></span> | <span data-ttu-id="b9037-117">Creare un set di scalabilità di macchine virtuali, configurare un servizio di bilanciamento del carico e ottenere le stringhe di connessione SSH per le VM del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="b9037-117">Create a VM scale set, set up a load balancer, and get SSH connection strings to the scale set VMs.</span></span> |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://azure.microsoft.com/resources/samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md