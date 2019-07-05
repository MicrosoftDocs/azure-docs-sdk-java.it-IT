---
title: Java Flight Recorder e Mission Control
description: Istruzioni su come usare Java Flight Recorder e Mission Control per raccogliere e rivedere i dati delle app.
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 64f64f2e5891fccf9d62510f39bd99d73457d590
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533630"
---
# <a name="use-java-flight-recorder-and-mission-control"></a>Usare Java Flight Recorder e Mission Control

Zulu Mission Control è una build di JDK Mission Control completamente testata, resa disponibile come open source da Oracle nel 2018 e gestita come progetto nell'ambiente OpenJDK. Insieme a Java Flight Recorder (JFR), Mission Control offre funzionalità interattive di monitoraggio e gestione con sovraccarico ridotto per i carichi di lavoro Java.

Zulu Mission Control è compatibile con i JDK (Java Development Kit) e JRE (Java Runtime Environment) seguenti:

* Zulu 12.1 e versioni successive
* Zulu 11.0 e versioni successive
* Zulu 8u202 (8.36) e versioni successive
* Oracle OpenJDK 11 e 15 e versioni successive
* Oracle Java 11.0 e versioni successive
* Oracle Java 8.0 e versioni successive

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a>Installare Zulu Mission Control e connettersi a una JVM

Per installare Zulu Mission Control, connettersi a una Java Virtual Machine (JVM) e acquisire visibilità in tempo reale su tutti gli aspetti di un'applicazione in esecuzione, seguire questa procedura:

1.  [Installare un JDK e un JRE compatibile con Zulu Mission Control](java-jdk-install.md).

1.  [Scaricare Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/), scegliere la versione appropriata per il sistema, salvarla in locale e passare a tale directory.

1.  Espandere il file scaricato.

    **Linux:**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows:**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **macOS:**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  Avviare l'applicazione Java con uno dei JDK compatibili. Ad esempio:

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  Avviare Zulu Mission Control.

    **Linux:**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows:**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **macOS:**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  (Facoltativo) Cambiare l'installazione di JVM per Mission Control.

    Nei dispositivi Windows *zmc.exe* usa l'installazione di JVM predefinita configurata nel Registro di sistema. Zulu Mission Control deve essere avviato da un JDK completo per poter rilevare automaticamente le istanze locali di JVM. Se l'installazione è un JRE, non verrà rilevato alcuna JVM e verrà visualizzato l'avviso seguente:

    > [!div class="mx-imgBorder"]
    ![Messaggio di avviso visualizzato se l'installazione di JDK è solo per JRE](../media/jdk/azul-jfr-1.png)

    Per cambiare la JVM usata da Mission Control, eseguire le operazioni seguenti: 

    a. Aprire il file di configurazione *zmc.ini* presente nella stessa directory del file *zmc.exe*.

    b. Aggiungere due righe prima della riga `-vmargs`:  

       * Nella prima riga digitare `–vm`.  
       * Nella seconda riga digitare il percorso dell'installazione di JDK, ad esempio `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`.

1.  Per individuare la JVM che esegue l'applicazione, eseguire le operazioni seguenti:

    a. Nel riquadro sinistro della finestra Zulu Mission Control selezionare la scheda **JVM Browser** (Browser JVM).

    b. Nell'elenco selezionare ed espandere l'istanza della JVM che esegue l'applicazione.

    ![Istanza della JVM nell'elenco espanso](../media/jdk/azul-jfr-2.png)


1.  Avviare una registrazione dei dati, se necessario.

    a. Se nel riquadro sinistro sotto **Flight Recorder** viene visualizzato il messaggio *No Recordings* (Nessuna registrazione), avviare una registrazione facendo clic con il pulsante destro del mouse su **Flight Recorder** e selezionando **Start Flight Recording** (Avvia registrazione dei dati).

    b. Selezionare **Time fixed recording** (Registrazione a durata fissa) oppure **Continuous recording** (Registrazione continua) e quindi una configurazione **Profiling** (Profilatura) (dettagliata) oppure **Continuous** (Continua) (sovraccarico minore), infine selezionare **Finish** (Fine).

    ![Avviare una registrazione dei dati](../media/jdk/azul-jfr-3.png)

    Sotto la riga **Flight Recorder** nella scheda JVM Browser (Browser JVM) dovrebbe essere visualizzata una registrazione.

1. Eseguire il dump della registrazione dei dati. A questo scopo, fare clic con il pulsante destro del mouse sulla riga corrispondente e selezionare **Dump whole recording** (Dump dell'intera registrazione).

    Nel riquadro grande a destra della finestra Zulu Mission Control viene visualizzata una nuova scheda. Questo riquadro rappresenta la registrazione di cui è stato appena eseguito il dump dalla JVM che esegue l'applicazione.

1. Esaminare la registrazione usando Zulu Mission Control. A questo scopo, selezionare la scheda **Outline** (Struttura) nel riquadro sinistro della finestra Zulu Mission Control. Questa scheda contiene diverse visualizzazioni dei dati raccolti durante la registrazione.
 
    ![Esaminare la registrazione dei dati](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>Risorse

Per altre informazioni, visitare il sito di Azul Systems e guardare il [webinar di Azul su Flight Recorder e Mission Control open source: gestione e misurazione delle prestazioni di OpenJDK 8](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/). Il video è presentato dal vice CTO di Azul Systems, Simon Ritter. La discussione su Flight Recorder inizia al minuto 31:30.

