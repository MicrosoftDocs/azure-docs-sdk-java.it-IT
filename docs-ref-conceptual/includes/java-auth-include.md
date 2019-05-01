---
ms.openlocfilehash: 0a9b06932baa51c3cf003a4485a3a25261ffe91d
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592503"
---
Creare un [file di autenticazione](../java-sdk-azure-authenticate.md#mgmt-file) ed esportare una variabile di ambiente `AZURE_AUTH_LOCATION` nella riga di comando con il percorso completo del file.

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

Il file di autenticazione consente di configurare l'oggetto `Azure` del punto di ingresso usato dalle librerie di gestione per definire, creare e configurare le risorse di Azure.

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

[Altre informazioni](../java-sdk-azure-authenticate.md#mgmt-auth) sulle opzioni di autenticazione quando si usano le librerie di gestione di Azure per Java.