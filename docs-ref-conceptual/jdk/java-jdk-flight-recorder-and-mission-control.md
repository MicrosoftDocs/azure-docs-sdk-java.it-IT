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
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a><span data-ttu-id="9180a-103">Uso di Java Flight Recorder (JFR) e Mission Control</span><span class="sxs-lookup"><span data-stu-id="9180a-103">Using Java Flight Recorder (JFR) and Mission Control</span></span>

<span data-ttu-id="9180a-104">Zulu Mission Control è una build di JDK Mission Control completamente testata, resa disponibile come open source da Oracle nel 2018 e gestita come progetto nell'ambiente OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="9180a-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="9180a-105">Insieme a Flight Recorder, Mission Control offre funzionalità interattive di monitoraggio e gestione con sovraccarico ridotto per i carichi di lavoro Java.</span><span class="sxs-lookup"><span data-stu-id="9180a-105">Coupled with Flight Recorder, Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="9180a-106">Zulu Mission Control è compatibile con i seguenti JDK/JRE:</span><span class="sxs-lookup"><span data-stu-id="9180a-106">Zulu Mission Control is compatible with the following JDKs/JREs:</span></span>

* <span data-ttu-id="9180a-107">Zulu 12.1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="9180a-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="9180a-108">Zulu 11.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="9180a-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="9180a-109">Zulu 8u202 (8.36) e versioni successive</span><span class="sxs-lookup"><span data-stu-id="9180a-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="9180a-110">Oracle OpenJDK 11+15 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="9180a-110">Oracle OpenJDK 11+15 and later</span></span>
* <span data-ttu-id="9180a-111">Oracle Java 11.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="9180a-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="9180a-112">Oracle Java 8.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="9180a-112">Oracle Java 8.0 and later</span></span>

<span data-ttu-id="9180a-113">Seguire la procedura seguente per installare Zulu Mission Control, connettersi a una Java Virtual Machine (JVM) e acquisire visibilità in tempo reale su tutti gli aspetti di un'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9180a-113">Follow the steps below to install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application.</span></span>

1.  <span data-ttu-id="9180a-114">[Installare un JDK/JRE compatibile con Zulu Mission Control](java-jdk-install.md).</span><span class="sxs-lookup"><span data-stu-id="9180a-114">[Install a Zulu Mission Control compatible JDK/JRE](java-jdk-install.md).</span></span>

2.  <span data-ttu-id="9180a-115">Scaricare Zulu Mission Control dal [sito di download Azul](https://www.azul.com/products/zulu-mission-control/), scegliere la versione appropriata per il sistema, salvarlo in locale e passare a questa directory.</span><span class="sxs-lookup"><span data-stu-id="9180a-115">Download Zulu Mission Control from [the Azul download site](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

3.  <span data-ttu-id="9180a-116">Espandere il file scaricato.</span><span class="sxs-lookup"><span data-stu-id="9180a-116">Expand the downloaded file.</span></span>

    <span data-ttu-id="9180a-117">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="9180a-117">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="9180a-118">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="9180a-118">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="9180a-119">**MacOS:**</span><span class="sxs-lookup"><span data-stu-id="9180a-119">**MacOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  <span data-ttu-id="9180a-120">Avviare l'applicazione Java con uno dei JDK compatibili.</span><span class="sxs-lookup"><span data-stu-id="9180a-120">Start your Java application using one of the compatible JDKs.</span></span> <span data-ttu-id="9180a-121">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9180a-121">E.g.:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  <span data-ttu-id="9180a-122">Avviare Zulu Mission Control</span><span class="sxs-lookup"><span data-stu-id="9180a-122">Start Zulu Mission Control</span></span>

    <span data-ttu-id="9180a-123">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="9180a-123">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="9180a-124">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="9180a-124">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="9180a-125">**MacOS:**</span><span class="sxs-lookup"><span data-stu-id="9180a-125">**MacOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  <span data-ttu-id="9180a-126">Cambiare l'installazione di JVM per Mission Control (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="9180a-126">Switch the JVM installation for Mission Control (Optional)</span></span>

    <span data-ttu-id="9180a-127">In Windows *zmc.exe* userà l'installazione di JVM predefinita configurata nel Registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="9180a-127">On Windows, *zmc.exe* will use the default JVM installation configured in the registry.</span></span> <span data-ttu-id="9180a-128">Zulu Mission Control deve essere avviato da un JDK completo per poter rilevare automaticamente le istanze locali di JVM.</span><span class="sxs-lookup"><span data-stu-id="9180a-128">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="9180a-129">Se si tratta di un JRE, verrà visualizzato il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="9180a-129">If this is a JRE, you will see the warning below:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9180a-130">![Messaggio di avviso visualizzato se l'installazione di JDK è solo per JRE](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="9180a-130">![Warning if JDK install is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="9180a-131">Per cambiare la JVM usata da Mission Control, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="9180a-131">To change the JVM used by Mission Control, follow these steps:</span></span> 
    1.  <span data-ttu-id="9180a-132">Aprire il file di configurazione *zmc.ini* situato nella stessa directory di *zmc.exe*</span><span class="sxs-lookup"><span data-stu-id="9180a-132">Open *zmc.ini* configuration file, located in the same directory as the *zmc.exe*</span></span>
    2.  <span data-ttu-id="9180a-133">Aggiungere due righe prima della riga `-vmargs`:</span><span class="sxs-lookup"><span data-stu-id="9180a-133">Before the line `-vmargs`, add two lines:</span></span>
        * <span data-ttu-id="9180a-134">Nella prima riga scrivere `–vm`</span><span class="sxs-lookup"><span data-stu-id="9180a-134">On the first line, write `–vm`</span></span>
        * <span data-ttu-id="9180a-135">Nella seconda riga scrivere il percorso dell'installazione di JDK.</span><span class="sxs-lookup"><span data-stu-id="9180a-135">On the second line, write the path to your JDK installation.</span></span> <span data-ttu-id="9180a-136">Ad esempio `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`.</span><span class="sxs-lookup"><span data-stu-id="9180a-136">(For example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

7.  <span data-ttu-id="9180a-137">Individuare la JVM che esegue l'applicazione</span><span class="sxs-lookup"><span data-stu-id="9180a-137">Locate the JVM running your application</span></span>
    1.  <span data-ttu-id="9180a-138">Nel riquadro in alto a sinistra della finestra Zulu Mission Control fare clic sulla scheda **JVM Browser**.</span><span class="sxs-lookup"><span data-stu-id="9180a-138">In the upper left pane of the Zulu Mission Control window click on the tab labelled **JVM Browser**.</span></span>
    2.  <span data-ttu-id="9180a-139">Selezionare ed espandere la voce dell'elenco in alto a sinistra relativa all'istanza della JVM che esegue l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9180a-139">Select and expand the list item in the upper left for your the JVM instance running your application.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9180a-140">![Espandere la voce dell'elenco in alto a sinistra per l'istanza della JVM](../media/jdk/azul-jfr-2.png)</span><span class="sxs-lookup"><span data-stu-id="9180a-140">![Expand the list item in the upper-left for your JVM instance](../media/jdk/azul-jfr-2.png)</span></span>


8.  <span data-ttu-id="9180a-141">Avviare una registrazione dei dati, se necessario</span><span class="sxs-lookup"><span data-stu-id="9180a-141">Start a Flight Recording, if necessary</span></span>
    1.  <span data-ttu-id="9180a-142">Se in Flight Recorder viene visualizzato il messaggio "No Recordings" (Nessuna registrazione) avviare una registrazione facendo clic con il pulsante destro del mouse sulla riga Flight Recorder nella scheda JVM Browser e scegliendo **Start Flight Recording** (Avvia registrazione dei dati).</span><span class="sxs-lookup"><span data-stu-id="9180a-142">If the Flight Recorder displays "No Recordings", start one by right-clicking on the Flight Recorder line in the JVM Browser tab and selecting **Start Flight Recording...**</span></span>
    2.  <span data-ttu-id="9180a-143">Selezionare una registrazione a durata fissa oppure continua e una configurazione di profilatura (dettagliata) oppure continua (sovraccarico inferiore), quindi fare clic su **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="9180a-143">Select either a fixed duration recording or a continuous recording, and either a Profiling configuration (fine-grained) or a Continuous configuration (lower overhead), then click **Finish**.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9180a-144">](../media/jdk/azul-jfr-3.png)Avviare la registrazione dei dati![</span><span class="sxs-lookup"><span data-stu-id="9180a-144">![Start a Flight Recording](../media/jdk/azul-jfr-3.png)</span></span>

9.  <span data-ttu-id="9180a-145">Eseguire il dump della registrazione dei dati</span><span class="sxs-lookup"><span data-stu-id="9180a-145">Dump the Flight Recording</span></span>
    1.  <span data-ttu-id="9180a-146">Sotto la riga Flight Recorder nella scheda JVM Browser dovrebbe essere visualizzata una registrazione.</span><span class="sxs-lookup"><span data-stu-id="9180a-146">A Flight Recording should appear below the Flight Recorder line in the JVM Browser.</span></span> <span data-ttu-id="9180a-147">Fare clic con il pulsante destro del mouse sulla riga corrispondente e scegliere **Dump whole recording** (Dump dell'intera registrazione).</span><span class="sxs-lookup"><span data-stu-id="9180a-147">Right-click on the line representing the Flight Recording and select **Dump whole recording**.</span></span>
    2.  <span data-ttu-id="9180a-148">Nel riquadro grande a destra della finestra Zulu Mission Control verrà visualizzata una nuova scheda.</span><span class="sxs-lookup"><span data-stu-id="9180a-148">A new tab will appear in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="9180a-149">Questo riquadro rappresenta la registrazione di cui è stato appena eseguito il dump dalla JVM che esegue l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9180a-149">This pane represents the Flight Recording just dumped from the JVM running your application.</span></span>

10. <span data-ttu-id="9180a-150">Esaminare la registrazione usando Zulu Mission Control</span><span class="sxs-lookup"><span data-stu-id="9180a-150">Examine the Flight Recording using Zulu Mission Control</span></span>
    1.  <span data-ttu-id="9180a-151">Se non è già attivata, selezionare la scheda **Outline** (Struttura) nel riquadro sinistro della finestra Zulu Mission Control.</span><span class="sxs-lookup"><span data-stu-id="9180a-151">If not already activated, select the tab labelled **Outline** in the left pane of the Zulu Mission Control Window.</span></span> <span data-ttu-id="9180a-152">Questa scheda contiene diverse visualizzazioni dei dati raccolti con la registrazione.</span><span class="sxs-lookup"><span data-stu-id="9180a-152">This tab contains different views of the data collected in the Flight Recording.</span></span>
 
    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9180a-153">![Esaminare la registrazione](../media/jdk/azul-jfr-4.png)</span><span class="sxs-lookup"><span data-stu-id="9180a-153">![Review the Fliight Recording](../media/jdk/azul-jfr-4.png)</span></span>

## <a name="resources"></a><span data-ttu-id="9180a-154">Risorse</span><span class="sxs-lookup"><span data-stu-id="9180a-154">Resources</span></span>

<span data-ttu-id="9180a-155">È anche disponibile un [video dimostrativo](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) presentato dal vice CTO di Azul Systems, Simon Ritter.</span><span class="sxs-lookup"><span data-stu-id="9180a-155">We have also prepared a [demonstration video](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="9180a-156">Questo video illustra le procedure di configurazione e impostazione di Flight Recorder e Zulu Mission Control.</span><span class="sxs-lookup"><span data-stu-id="9180a-156">The video walks you through the configuration and setup of both Flight Recorder and Zulu Mission Control.</span></span> <span data-ttu-id="9180a-157">La discussione su Flight Recorder inizia al minuto 31:30.</span><span class="sxs-lookup"><span data-stu-id="9180a-157">The Flight Recorder discussion starts at 31:30.</span></span>

