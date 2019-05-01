---
ms.openlocfilehash: 0a9b06932baa51c3cf003a4485a3a25261ffe91d
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592503"
---
<span data-ttu-id="580df-101">Creare un [file di autenticazione](../java-sdk-azure-authenticate.md#mgmt-file) ed esportare una variabile di ambiente `AZURE_AUTH_LOCATION` nella riga di comando con il percorso completo del file.</span><span class="sxs-lookup"><span data-stu-id="580df-101">Create an [authentication file](../java-sdk-azure-authenticate.md#mgmt-file) and export an environment variable `AZURE_AUTH_LOCATION` on the command line with the full path to the file.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

<span data-ttu-id="580df-102">Il file di autenticazione consente di configurare l'oggetto `Azure` del punto di ingresso usato dalle librerie di gestione per definire, creare e configurare le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="580df-102">The authentication file is used to configure the entry point `Azure` object used by the management libraries to define, create, and configure Azure resources.</span></span>

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

<span data-ttu-id="580df-103">[Altre informazioni](../java-sdk-azure-authenticate.md#mgmt-auth) sulle opzioni di autenticazione quando si usano le librerie di gestione di Azure per Java.</span><span class="sxs-lookup"><span data-stu-id="580df-103">[Learn more](../java-sdk-azure-authenticate.md#mgmt-auth) about authentication options when using the Azure management libraries for Java.</span></span>