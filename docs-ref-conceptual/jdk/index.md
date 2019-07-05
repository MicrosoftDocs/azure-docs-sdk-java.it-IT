---
title: Java JDK e supporto a lungo termine per lo sviluppo di Azure
description: Questo articolo include i collegamenti per il download e l'indicazione del supporto tecnico di Azure per lo sviluppo e l'esecuzione di applicazioni Java.
author: bmitchell287
manager: douge
ms.devlang: java
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: brendm
ms.openlocfilehash: 1bb93f775686b5b0f127c586dda802f4eb5f775d
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533631"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a>Supporto a lungo termine di Java per Azure e Azure Stack

Gli sviluppatori Java in Microsoft Azure e Azure Stack possono compilare ed eseguire applicazioni Java di produzione usando [Azul Zulu Enterprise per Azure](https://www.azul.com/downloads/azure-only/zulu/) senza incorrere in costi di supporto aggiuntivi. È possibile usare qualsiasi runtime Java in Azure, ma se si sceglie Zulu si ottengono aggiornamenti di manutenzione gratuiti ed è possibile risolvere problemi relativi al supporto rivolgendosi a Microsoft.

> [!div class="nextstepaction"]
> [Scaricare e installare Java](java-jdk-install.md)

## <a name="long-term-support"></a>Supporto a lungo termine

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a>Anteprima tecnica

* [Java 12](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>Cos'è Zulu OpenJDK per Azure?

Le build Azul Zulu Enterprise di OpenJDK sono distribuzioni di OpenJDK gratuite, multipiattaforma e pronte per la produzione per Azure e Azure Stack, supportate da Microsoft e Azul Systems. Queste distribuzioni:

* Sono build di OpenJDK completamente open source fornite come pacchetti JDK (Java Development Kit), JRE (Java Runtime Environment) e JRE headless. Questi binari sono build commerciali di Java Standard Edition (SE) pienamente compatibili e conformi che è possibile usare con applicazioni o componenti Java in Azure e Azure Stack.
* Vengono fornite con supporto a lungo termine, tra cui correzioni di bug, miglioramenti delle prestazioni e patch di sicurezza.
* Sono disponibili per lo sviluppo e l'esecuzione di applicazioni Java in Windows, Linux e macOS.
* Sono disponibili come immagini di contenitori in Docker Hub e come macchine virtuali Windows e Linux in Azure Marketplace.
* Vengono usate in Azure per supportare molti servizi di Azure, ad esempio:
  * Servizio app di Azure (Windows)
  * Servizio app di Azure (Linux)
  * Funzioni di Azure
  * Azure Service Fabric
  * HDInsight di Azure
  * Ricerca di Azure
  * Azure DevOps
  * Azure Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>Versioni Java supportate e piano degli aggiornamenti

Azul Systems fornisce [build Zulu Enterprise di OpenJDK per Azure](https://www.azul.com/downloads/azure-only/zulu/) completamente supportate per tutte le versioni di Java con supporto a lungo termine, a partire da Java SE 7, 8 e 11. Per altre informazioni, vedere il [comunicato stampa di Azul](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).

|Supporto a lungo termine di Java SE  |Data di fine supporto  |
|---------|----------|
|[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7) |Luglio 2023 |
|[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8) |Marzo 2025|
|[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11) |Settembre 2026|
|[![Java 12](../media/jdk/java-12.png)]() |**Anteprima**|

Queste versioni di JDK prevedono aggiornamenti trimestrali di sicurezza e correzioni di bug, oltre a patch e aggiornamenti critici straordinari, se necessario. Il supporto include il backporting a Java 7 e 8 degli aggiornamenti di sicurezza e delle correzioni di bug applicate alle versioni più recenti di Java, come Java 11, garantendo la continua stabilità e la sicurezza delle versioni precedenti di Java. I clienti di Azure possono ottenere questi aggiornamenti di sicurezza e le correzioni di bug per la piattaforma senza incorrere in costi di sottoscrizione non pianificati per Java SE.

Azul Systems mantiene una [roadmap Java SE](https://www.azul.com/products/azul_support_roadmap/) per queste versioni.

## <a name="benefits-for-developers"></a>Vantaggi per gli sviluppatori

Le versioni di JDK Azul Zulu offrono questi vantaggi:

* Supporto assicurato sia da Microsoft che da Azul Systems.

   * I binari Zulu sono pronti per la produzione e supportati da Microsoft e Azul Systems.
   * Zulu viene fornito con supporto a lungo termine gratuito per Java 7, 8 e 11. Il supporto a lungo termine verrà fornito anche per Java 17. È possibile aggiornare le versioni di Java solo quando è necessario.
   * Java 7 è supportato fino a luglio 2023. Java 8 e 11 sono supportati oltre il 2024.
   * Microsoft è impegnata a eseguire Zulu internamente nei computer usati per fornire molti servizi di Azure.

* Pronte per la produzione.

   * Build di OpenJDK completamente open source.
   * Soluzioni sostitutive per molte distribuzioni di Java SE senza richiedere modifiche.
   * JDK, JRE e JRE-headless.
   * Java 7, 8 e 11.
   * Conformità verificata alle specifiche di Java SE tramite Technology Compatibility Kit (TCK) della community OpenJDK.
   * Gli sviluppatori continueranno a ricevere aggiornamenti di produzione per Java SE, tra cui correzioni di bug, miglioramenti delle prestazioni e patch di sicurezza, per Java SE 7, 8 e 11.

* Supporto multipiattaforma. Zulu supporta binari per più piattaforme e versioni, tra cui:

   * Client Windows
     * 10
     * 8.1
     * 8, 7
   * Windows Server
     * 2016 R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux, tra cui
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * macOS X
   * Disponibili in più tipi di pacchetti:
     * MSI, ZIP, TAR, DEB, RPM e DMG

    Le immagini di contenitori Docker certificate per Zulu JDK, JRE e JRE headless con più immagini del sistema operativo di base sono disponibili in Docker.

    Hub:

    * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
    * [JRE](https://hub.docker.com/_/microsoft-java-jre)
    * [JRE headless](https://hub.docker.com/_/microsoft-java-jre-headless)

* Nessun costo.

   * Microsoft fornisce tutto il necessario per creare e ridimensionare le app Java in Azure senza costi aggiuntivi. Tramite Zulu si riceveranno gratuitamente aggiornamenti di sicurezza e correzioni di bug della piattaforma per le app Java.
   * [Java Flight Recorder e Mission Control](java-jdk-flight-recorder-and-mission-control.md) sono disponibili in Zulu Java 8, 11 e 12 (anteprima).

* Disponibile come anteprima tecnica di versioni senza supporto a lungo termine.

   * Le anteprime tecniche consentono di testare progressivamente le nuove funzionalità non appena vengono distribuite in versioni a breve termine, che eventualmente passeranno al supporto a lungo termine di Java 17.

* Modifiche di OpenJDK inviate in upstream.

   * I committer di Azul Systems eseguono il push delle modifiche di Zulu in OpenJDK, per cui il repository upstream è completo e inclusivo.

Come sempre, gli sviluppatori Java possono trasferire in Java i propri runtime Java, tra cui JDK Oracle e Red Hat e sfruttarne l'infrastruttura sicura e i servizi ricchi di funzionalità. È inoltre disponibile l'edizione di produzione di Oracle Java SE per l'esecuzione di carichi di lavoro Java in macchine virtuali Windows o Linux in Azure.

## <a name="use-java-jdks-for-local-development"></a>Usare JDK Java per lo sviluppo locale 

È possibile [scaricare i JDK Java per Azure e Azure Stack](https://www.azul.com/downloads/azure-only/zulu/) per l'uso in ambienti di sviluppo locali. I download sono disponibili per Windows, Linux e macOS. Se si usa Linux, è possibile ottenere i pacchetti anche tramite i gestori di pacchetti [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) e [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo).

Per altre istruzioni, vedere l'articolo sulle [immagini Docker per Azure](java-jdk-docker-images.md).
