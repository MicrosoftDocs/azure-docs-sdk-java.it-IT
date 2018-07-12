---
title: Aggiungere un certificato radice per Azure all'archivio CA Java
description: Informazioni su come aggiungere un certificato radice dell'autorità di certificazione (CA) all'archivio certificati CA Java (cacerts) per usarlo con Microsoft Azure.
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 07/02/2018
ms.author: robmcm
ms.openlocfilehash: 3f2de63f7eb1422ff1dd6db45d68e02f4af188b8
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37864041"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a>Aggiunta di un certificato radice all'archivio certificati CA Java

Le applicazioni che usano i servizi di Azure, ad esempio il bus di servizio di Azure, devono considerare attendibile il certificato radice Baltimore CyberTrust. Questo certificato potrebbe essere già installato nel sistema. In caso contrario, i passaggi di questa esercitazione illustreranno come usare lo strumento **keytool** di Oracle per aggiungere il certificato radice dell'autorità di certificazione (CA) necessario all'archivio certificati CA Java (cacerts) che verrà usato per i servizi di Azure.

L'utilità keytool di Oracle è uno _strumento di gestione di chiavi e certificati_ che consente agli sviluppatori di gestire l'elenco dei certificati attendibili da usare con Java. È possibile usare lo strumento Keytool per aggiungere il certificato della CA prima di comprimere il JDK e di aggiungerlo alla cartella **approot** del progetto Azure. In alternativa, è possibile eseguire un'attività di avvio in Azure che usa lo strumento Keytool per aggiungere il certificato.

A partire dal 15 aprile 2013 è iniziata la migrazione di Azure dal certificato radice GTE CyberTrust Global al certificato radice Baltimore CyberTrust. I passaggi seguenti illustrano come usare keytool per aggiungere il certificato radice Baltimore CyberTrust all'archivio certificati CA Java (cacerts).

> [!NOTE]
> 
> È possibile usare i passaggi descritti in questo articolo per configurare Java SDK in modo da considerare attendibili certificati radice di altre autorità di certificazione attendibili. Ad esempio, si può scegliere un certificato radice nell'elenco dei [certificati radice GeoTrust](http://www.geotrust.com/resources/root-certificates/).
> 

## <a name="determining-which-root-certificates-are-installed"></a>Individuazione dei certificati radice installati

Il certificato Baltimore potrebbe essere già installato nell'archivio cacerts. È quindi necessario seguire questa procedura per determinare se è già presente.

1. Al prompt dei comandi di amministratore passare alla cartella **jdk\jre\lib\security** di JDK e quindi eseguire questo comando per elencare i certificati installati nel sistema:

   ```shell
   keytool -list -keystore cacerts
   ```

1. Se viene richiesta la password dell'archivio, la password predefinita è **changeit**.

   > [!NOTE]
   > 
   > Se si vuole cambiare la password dell'archivio, vedere la documentazione di keytool all'indirizzo <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.
   > 

1. Se non viene visualizzato un certificato con l'identificazione personale `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, usare la procedura descritta nella sezione seguente per scaricare e installare il certificato.

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a>Per aggiungere un certificato radice all'archivio cacerts

1. Scaricare il certificato radice Baltimore CyberTrust da <https://cacert.omniroot.com/bc2025.crt> e salvarlo in un file locale con estensione **cer** nella cartella **jdk\jre\lib\security**. Per questo esempio, si supponga di aver scaricato il certificato radice Baltimore CyberTrust con il nome **bc2025.cer**.

   > [!NOTE]
   > 
   > Il certificato radice Baltimore CyberTrust ha il numero di serie `02:00:00:b9` e l'identificazione personale SHA1 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.
   > 

2. Importare il certificato nell'archivio cacerts con il comando seguente:

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   Dove:

   |  Parametro   |                              DESCRIZIONE                               |
   |--------------|------------------------------------------------------------------------|
   | `keystore`   | Specifica l'archivio certificati.                                       |
   | `importcert` | Specifica che si sta importando un certificato.                        |
   | `alias`      | Specifica un alias per il certificato.                                |
   | `file`       | Specifica il nome file del certificato radice che viene importato. |


3. Se viene richiesto di considerare attendibile il certificato, verificare che l'identificazione personale sia `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` e digitare **y** se è corretta.

4. Eseguire il comando seguente per verificare se il certificato CA è stato importato correttamente:

   ```shell
   keytool -list -keystore cacerts
   ```

Dopo aver aggiunto il certificato radice a JDK, è possibile comprimere il contenuto di JDK e aggiungerlo alla cartella **approot** del progetto Azure.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'utilità keytool, vedere <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

Per altre informazioni su Java, vedere [Azure for Java developers](/java/azure) (Azure per sviluppatori Java).

<!-- For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx). -->
