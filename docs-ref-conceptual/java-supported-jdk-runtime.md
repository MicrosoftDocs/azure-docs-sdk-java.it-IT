---
title: Java JDK e supporto a lungo termine per lo sviluppo di Azure
description: Download e indicazione del supporto di Azure per lo sviluppo e l'esecuzione di applicazioni Java.
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 10/15/2017
ms.author: routlaw
ms.openlocfilehash: 2865219f350990e8b07f7d2cd99f536168a6b8d4
ms.sourcegitcommit: 7df2d442ad7cbdb235e5dd35302a9b73379c23d5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026997"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a>Ottenere download e supporto di Java JDK nelle attività di sviluppo per Azure

Gli sviluppatori Java in Azure e Azure Stack possono compilare ed eseguire applicazioni Java di produzione usando le [build Zulu Enterprise di OpenJDK di Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) senza incorrere in costi di supporto aggiuntivi. È possibile usare qualsiasi runtime Java in Azure, ma usando Zulu si ottengono aggiornamenti di manutenzione gratuiti ed è possibile richiedere assistenza a Microsoft con un [piano di supporto di Azure qualificato](https://azure.microsoft.com/support/plans/).

## <a name="supported-java-versions-and-update-schedule"></a>Versioni Java supportate e piano degli aggiornamenti

Azul Systems fornirà [build Zulu Enterprise di OpenJDK per Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) completamente supportate per tutte le versioni di Java con supporto a lungo termine (LTS), a partire da Java SE 7, 8 e 11. Per altre informazioni, vedere il [comunicato stampa di Azul](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).


Queste versioni di JDK avranno aggiornamenti trimestrali di sicurezza e correzioni di bug, nonché patch e aggiornamenti critici straordinari, se necessario.  Questo supporto include il backporting a Java 7 e 8 degli aggiornamenti di sicurezza e delle correzioni di bug applicate alle versioni più recenti di Java come Java 11, garantendo la stabilità e la sicurezza delle versioni precedenti di Java.  I clienti di Azure hanno diritto a questi aggiornamenti di sicurezza e correzioni di bug per la piattaforma senza incorrere in costi di sottoscrizione non pianificati per Java SE. Le date di supporto per ogni versione di Java SE sono evidenziate nell'immagine seguente.

![Sequenza temporale del supporto JDK per Azure](media/azure-jdk-support.png)

Azul Systems mantiene una [roadmap Java SE](https://www.azul.com/products/azul_support_roadmap/) per queste versioni.

## <a name="use-for-local-development"></a>Usare per lo sviluppo locale 

Gli sviluppatori possono scaricare Java JDK per Azure e Azure Stack dal sito Web di [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/). I download sono disponibili per Windows, Linux e macOS. Gli sviluppatori che usano Linux possono ottenere i pacchetti anche tramite i gestori di pacchetti [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) e [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo).

Il supporto per Azul Zulu JDK supportato da Azure è disponibile quando si sviluppa per Azure o Azure Stack con un [piano di supporto per Azure qualificato](https://azure.microsoft.com/support/plans/).

## <a name="use-in-docker-containers"></a>Usare nei contenitori Docker

È possibile creare immagini Docker illimitate usando le build Zulu Enterprise di OpenJDK su qualsiasi distribuzione. Le immagini di Zulu Docker basate su Azul Zulu Enterprise per Azure JDK sono disponibili nel [repository Docker pubblico di Microsoft](https://hub.docker.com/r/microsoft/java-jdk/). I documenti Dockerfile usati per creare queste immagini sono disponibili nel [repository GitHub Java di Microsoft](https://github.com/Microsoft/java/tree/master/docker).

Per inserire le app in un contenitore usando queste immagini, sarà necessario impostare un'istruzione `FROM` nel proprio documento Dockerfile e configurare il contenitore con le dipendenze dell'applicazione. Ad esempio, per eseguire un'applicazione Java SE contenuta in un pacchetto JAR con binding alla porta 8080:

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a>Supporto di runtime del servizio di Azure

I servizi della piattaforma di Azure come [servizio app](/azure/app-service/containers/), [Funzioni](/azure/azure-functions/functions-create-first-java-maven), [Service Fabric](/azure/service-fabric/) e [HDInsight](/azure/hdinsight/) usano le build Zulu Enterprise di OpenJDK con l'applicazione automatica di patch di sicurezza e correzioni di bug per le versioni minori di Java.