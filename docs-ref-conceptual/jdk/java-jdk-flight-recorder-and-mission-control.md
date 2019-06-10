---
title: Java Flight Recorder e Mission Control
description: Istruzioni su come usare Java Flight Recorder e Mission Control per raccogliere e rivedere i dati delle app.
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: b27e0f741f1322b7e8e1df363dbb2f40a3d34d53
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568564"
---
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a>Uso di Java Flight Recorder (JFR) e Mission Control

Zulu Mission Control è una build di JDK Mission Control completamente testata, resa disponibile come open source da Oracle nel 2018 e gestita come progetto nell'ambiente OpenJDK. Insieme a Flight Recorder, Mission Control offre funzionalità interattive di monitoraggio e gestione con sovraccarico ridotto per i carichi di lavoro Java.

Zulu Mission Control è compatibile con i seguenti JDK/JRE:

* Zulu 12.1 e versioni successive
* Zulu 11.0 e versioni successive
* Zulu 8u202 (8.36) e versioni successive
* Oracle OpenJDK 11+15 e versioni successive
* Oracle Java 11.0 e versioni successive
* Oracle Java 8.0 e versioni successive

Seguire la procedura seguente per installare Zulu Mission Control, connettersi a una Java Virtual Machine (JVM) e acquisire visibilità in tempo reale su tutti gli aspetti di un'applicazione in esecuzione.

1.  [Installare un JDK/JRE compatibile con Zulu Mission Control](java-jdk-install.md).

2.  Scaricare Zulu Mission Control dal [sito di download Azul](https://www.azul.com/products/zulu-mission-control/), scegliere la versione appropriata per il sistema, salvarlo in locale e passare a questa directory.

3.  Espandere il file scaricato.

    **Linux:**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows:**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **MacOS:**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  Avviare l'applicazione Java con uno dei JDK compatibili. Ad esempio:

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  Avviare Zulu Mission Control

    **Linux:**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows:**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **MacOS:**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  Cambiare l'installazione di JVM per Mission Control (facoltativo)

    In Windows *zmc.exe* userà l'installazione di JVM predefinita configurata nel Registro di sistema. Zulu Mission Control deve essere avviato da un JDK completo per poter rilevare automaticamente le istanze locali di JVM. Se si tratta di un JRE, verrà visualizzato il messaggio di avviso seguente:

    > [!div class="mx-imgBorder"]
    ![Messaggio di avviso visualizzato se l'installazione di JDK è solo per JRE](../media/jdk/azul-jfr-1.png)

    Per cambiare la JVM usata da Mission Control, procedere come segue: 
    1.  Aprire il file di configurazione *zmc.ini* situato nella stessa directory di *zmc.exe*
    2.  Aggiungere due righe prima della riga `-vmargs`:
        * Nella prima riga scrivere `–vm`
        * Nella seconda riga scrivere il percorso dell'installazione di JDK. Ad esempio `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`.

7.  Individuare la JVM che esegue l'applicazione
    1.  Nel riquadro in alto a sinistra della finestra Zulu Mission Control fare clic sulla scheda **JVM Browser**.
    2.  Selezionare ed espandere la voce dell'elenco in alto a sinistra relativa all'istanza della JVM che esegue l'applicazione.

    > [!div class="mx-imgBorder"]
    ![Espandere la voce dell'elenco in alto a sinistra per l'istanza della JVM](../media/jdk/azul-jfr-2.png)


8.  Avviare una registrazione dei dati, se necessario
    1.  Se in Flight Recorder viene visualizzato il messaggio "No Recordings" (Nessuna registrazione) avviare una registrazione facendo clic con il pulsante destro del mouse sulla riga Flight Recorder nella scheda JVM Browser e scegliendo **Start Flight Recording** (Avvia registrazione dei dati).
    2.  Selezionare una registrazione a durata fissa oppure continua e una configurazione di profilatura (dettagliata) oppure continua (sovraccarico inferiore), quindi fare clic su **Finish** (Fine).

    > [!div class="mx-imgBorder"]
    ](../media/jdk/azul-jfr-3.png)Avviare la registrazione dei dati![

9.  Eseguire il dump della registrazione dei dati
    1.  Sotto la riga Flight Recorder nella scheda JVM Browser dovrebbe essere visualizzata una registrazione. Fare clic con il pulsante destro del mouse sulla riga corrispondente e scegliere **Dump whole recording** (Dump dell'intera registrazione).
    2.  Nel riquadro grande a destra della finestra Zulu Mission Control verrà visualizzata una nuova scheda. Questo riquadro rappresenta la registrazione di cui è stato appena eseguito il dump dalla JVM che esegue l'applicazione.

10. Esaminare la registrazione usando Zulu Mission Control
    1.  Se non è già attivata, selezionare la scheda **Outline** (Struttura) nel riquadro sinistro della finestra Zulu Mission Control. Questa scheda contiene diverse visualizzazioni dei dati raccolti con la registrazione.
 
    > [!div class="mx-imgBorder"]
    ![Esaminare la registrazione](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>Risorse

È anche disponibile un [video dimostrativo](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) presentato dal vice CTO di Azul Systems, Simon Ritter. Questo video illustra le procedure di configurazione e impostazione di Flight Recorder e Zulu Mission Control. La discussione su Flight Recorder inizia al minuto 31:30.

