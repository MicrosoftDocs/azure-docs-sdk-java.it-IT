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
# <a name="use-java-flight-recorder-and-mission-control"></a><span data-ttu-id="296f4-103">Usare Java Flight Recorder e Mission Control</span><span class="sxs-lookup"><span data-stu-id="296f4-103">Use Java Flight Recorder and Mission Control</span></span>

<span data-ttu-id="296f4-104">Zulu Mission Control è una build di JDK Mission Control completamente testata, resa disponibile come open source da Oracle nel 2018 e gestita come progetto nell'ambiente OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="296f4-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open-sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="296f4-105">Insieme a Java Flight Recorder (JFR), Mission Control offre funzionalità interattive di monitoraggio e gestione con sovraccarico ridotto per i carichi di lavoro Java.</span><span class="sxs-lookup"><span data-stu-id="296f4-105">Coupled with Java Flight Recorder (JFR), Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="296f4-106">Zulu Mission Control è compatibile con i JDK (Java Development Kit) e JRE (Java Runtime Environment) seguenti:</span><span class="sxs-lookup"><span data-stu-id="296f4-106">Zulu Mission Control is compatible with the following Java Development Kits (JDKs) and Java Runtime Environments (JREs):</span></span>

* <span data-ttu-id="296f4-107">Zulu 12.1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="296f4-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="296f4-108">Zulu 11.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="296f4-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="296f4-109">Zulu 8u202 (8.36) e versioni successive</span><span class="sxs-lookup"><span data-stu-id="296f4-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="296f4-110">Oracle OpenJDK 11 e 15 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="296f4-110">Oracle OpenJDK 11 and 15 and later</span></span>
* <span data-ttu-id="296f4-111">Oracle Java 11.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="296f4-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="296f4-112">Oracle Java 8.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="296f4-112">Oracle Java 8.0 and later</span></span>

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a><span data-ttu-id="296f4-113">Installare Zulu Mission Control e connettersi a una JVM</span><span class="sxs-lookup"><span data-stu-id="296f4-113">Install Zulu Mission Control and connect to a JVM</span></span>

<span data-ttu-id="296f4-114">Per installare Zulu Mission Control, connettersi a una Java Virtual Machine (JVM) e acquisire visibilità in tempo reale su tutti gli aspetti di un'applicazione in esecuzione, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="296f4-114">To install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application, do the following:</span></span>

1.  <span data-ttu-id="296f4-115">[Installare un JDK e un JRE compatibile con Zulu Mission Control](java-jdk-install.md).</span><span class="sxs-lookup"><span data-stu-id="296f4-115">[Install a Zulu Mission Control-compatible JDK and JRE](java-jdk-install.md).</span></span>

1.  <span data-ttu-id="296f4-116">[Scaricare Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/), scegliere la versione appropriata per il sistema, salvarla in locale e passare a tale directory.</span><span class="sxs-lookup"><span data-stu-id="296f4-116">[Download Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

1.  <span data-ttu-id="296f4-117">Espandere il file scaricato.</span><span class="sxs-lookup"><span data-stu-id="296f4-117">Expand the downloaded file.</span></span>

    <span data-ttu-id="296f4-118">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="296f4-118">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="296f4-119">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="296f4-119">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="296f4-120">**macOS:**</span><span class="sxs-lookup"><span data-stu-id="296f4-120">**macOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  <span data-ttu-id="296f4-121">Avviare l'applicazione Java con uno dei JDK compatibili.</span><span class="sxs-lookup"><span data-stu-id="296f4-121">Start your Java application by using one of the compatible JDKs.</span></span> <span data-ttu-id="296f4-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="296f4-122">For example:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  <span data-ttu-id="296f4-123">Avviare Zulu Mission Control.</span><span class="sxs-lookup"><span data-stu-id="296f4-123">Start Zulu Mission Control.</span></span>

    <span data-ttu-id="296f4-124">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="296f4-124">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="296f4-125">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="296f4-125">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="296f4-126">**macOS:**</span><span class="sxs-lookup"><span data-stu-id="296f4-126">**macOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  <span data-ttu-id="296f4-127">(Facoltativo) Cambiare l'installazione di JVM per Mission Control.</span><span class="sxs-lookup"><span data-stu-id="296f4-127">(Optional) Switch the JVM installation for Mission Control.</span></span>

    <span data-ttu-id="296f4-128">Nei dispositivi Windows *zmc.exe* usa l'installazione di JVM predefinita configurata nel Registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="296f4-128">On Windows devices, *zmc.exe* uses the default JVM installation that's configured in the registry.</span></span> <span data-ttu-id="296f4-129">Zulu Mission Control deve essere avviato da un JDK completo per poter rilevare automaticamente le istanze locali di JVM.</span><span class="sxs-lookup"><span data-stu-id="296f4-129">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="296f4-130">Se l'installazione è un JRE, non verrà rilevato alcuna JVM e verrà visualizzato l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="296f4-130">If the installation is a JRE, no JVM will be detected, and you will receive the following warning:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="296f4-131">![Messaggio di avviso visualizzato se l'installazione di JDK è solo per JRE](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="296f4-131">![Warning if JDK installation is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="296f4-132">Per cambiare la JVM usata da Mission Control, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="296f4-132">To change the JVM that's used by Mission Control, do the following:</span></span> 

    <span data-ttu-id="296f4-133">a.</span><span class="sxs-lookup"><span data-stu-id="296f4-133">a.</span></span> <span data-ttu-id="296f4-134">Aprire il file di configurazione *zmc.ini* presente nella stessa directory del file *zmc.exe*.</span><span class="sxs-lookup"><span data-stu-id="296f4-134">Open the *zmc.ini* configuration file, which is in the same directory as the *zmc.exe* file.</span></span>

    <span data-ttu-id="296f4-135">b.</span><span class="sxs-lookup"><span data-stu-id="296f4-135">b.</span></span> <span data-ttu-id="296f4-136">Aggiungere due righe prima della riga `-vmargs`:</span><span class="sxs-lookup"><span data-stu-id="296f4-136">Before the line `-vmargs`, add two lines:</span></span>  

       * <span data-ttu-id="296f4-137">Nella prima riga digitare `–vm`.</span><span class="sxs-lookup"><span data-stu-id="296f4-137">On the first line, enter `–vm`.</span></span>  
       * <span data-ttu-id="296f4-138">Nella seconda riga digitare il percorso dell'installazione di JDK, ad esempio `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`.</span><span class="sxs-lookup"><span data-stu-id="296f4-138">On the second line, enter the path to your JDK installation (for example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

1.  <span data-ttu-id="296f4-139">Per individuare la JVM che esegue l'applicazione, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="296f4-139">Locate the JVM that's running your application by doing the following:</span></span>

    <span data-ttu-id="296f4-140">a.</span><span class="sxs-lookup"><span data-stu-id="296f4-140">a.</span></span> <span data-ttu-id="296f4-141">Nel riquadro sinistro della finestra Zulu Mission Control selezionare la scheda **JVM Browser** (Browser JVM).</span><span class="sxs-lookup"><span data-stu-id="296f4-141">In the left pane of the Zulu Mission Control window, select the **JVM Browser** tab.</span></span>

    <span data-ttu-id="296f4-142">b.</span><span class="sxs-lookup"><span data-stu-id="296f4-142">b.</span></span> <span data-ttu-id="296f4-143">Nell'elenco selezionare ed espandere l'istanza della JVM che esegue l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="296f4-143">In the list, select and expand the JVM instance that's running your application.</span></span>

    ![Istanza della JVM nell'elenco espanso](../media/jdk/azul-jfr-2.png)


1.  <span data-ttu-id="296f4-145">Avviare una registrazione dei dati, se necessario.</span><span class="sxs-lookup"><span data-stu-id="296f4-145">Start a flight recording, if necessary.</span></span>

    <span data-ttu-id="296f4-146">a.</span><span class="sxs-lookup"><span data-stu-id="296f4-146">a.</span></span> <span data-ttu-id="296f4-147">Se nel riquadro sinistro sotto **Flight Recorder** viene visualizzato il messaggio *No Recordings* (Nessuna registrazione), avviare una registrazione facendo clic con il pulsante destro del mouse su **Flight Recorder** e selezionando **Start Flight Recording** (Avvia registrazione dei dati).</span><span class="sxs-lookup"><span data-stu-id="296f4-147">In the left pane, under **Flight Recorder**, if a *No Recordings* message is displayed, start a recording by right-clicking **Flight Recorder** and then selecting **Start Flight Recording**.</span></span>

    <span data-ttu-id="296f4-148">b.</span><span class="sxs-lookup"><span data-stu-id="296f4-148">b.</span></span> <span data-ttu-id="296f4-149">Selezionare **Time fixed recording** (Registrazione a durata fissa) oppure **Continuous recording** (Registrazione continua) e quindi una configurazione **Profiling** (Profilatura) (dettagliata) oppure **Continuous** (Continua) (sovraccarico minore), infine selezionare **Finish** (Fine).</span><span class="sxs-lookup"><span data-stu-id="296f4-149">Select either **Time fixed recording** or **Continuous recording**, and either a **Profiling** configuration (fine-grained) or a **Continuous** configuration (lower overhead), and then select **Finish**.</span></span>

    ![Avviare una registrazione dei dati](../media/jdk/azul-jfr-3.png)

    <span data-ttu-id="296f4-151">Sotto la riga **Flight Recorder** nella scheda JVM Browser (Browser JVM) dovrebbe essere visualizzata una registrazione.</span><span class="sxs-lookup"><span data-stu-id="296f4-151">A flight recording should appear below the **Flight Recorder** line in the JVM browser.</span></span>

1. <span data-ttu-id="296f4-152">Eseguire il dump della registrazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="296f4-152">Dump the flight recording.</span></span> <span data-ttu-id="296f4-153">A questo scopo, fare clic con il pulsante destro del mouse sulla riga corrispondente e selezionare **Dump whole recording** (Dump dell'intera registrazione).</span><span class="sxs-lookup"><span data-stu-id="296f4-153">To do so, right-click the line that represents the flight recording, and then select **Dump whole recording**.</span></span>

    <span data-ttu-id="296f4-154">Nel riquadro grande a destra della finestra Zulu Mission Control viene visualizzata una nuova scheda.</span><span class="sxs-lookup"><span data-stu-id="296f4-154">A new tab appears in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="296f4-155">Questo riquadro rappresenta la registrazione di cui è stato appena eseguito il dump dalla JVM che esegue l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="296f4-155">This pane represents the flight recording that was just dumped from the JVM that's running your application.</span></span>

1. <span data-ttu-id="296f4-156">Esaminare la registrazione usando Zulu Mission Control.</span><span class="sxs-lookup"><span data-stu-id="296f4-156">Examine the flight recording by using Zulu Mission Control.</span></span> <span data-ttu-id="296f4-157">A questo scopo, selezionare la scheda **Outline** (Struttura) nel riquadro sinistro della finestra Zulu Mission Control.</span><span class="sxs-lookup"><span data-stu-id="296f4-157">To do so, select the **Outline** tab in the left pane of the Zulu Mission Control window.</span></span> <span data-ttu-id="296f4-158">Questa scheda contiene diverse visualizzazioni dei dati raccolti durante la registrazione.</span><span class="sxs-lookup"><span data-stu-id="296f4-158">This tab displays various views of the data that's collected in the flight recording.</span></span>
 
    ![Esaminare la registrazione dei dati](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a><span data-ttu-id="296f4-160">Risorse</span><span class="sxs-lookup"><span data-stu-id="296f4-160">Resources</span></span>

<span data-ttu-id="296f4-161">Per altre informazioni, visitare il sito di Azul Systems e guardare il [webinar di Azul su Flight Recorder e Mission Control open source: gestione e misurazione delle prestazioni di OpenJDK 8](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/).</span><span class="sxs-lookup"><span data-stu-id="296f4-161">To learn more, go to the Azul Systems site and view [Azul Webinar: Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/).</span></span> <span data-ttu-id="296f4-162">Il video è presentato dal vice CTO di Azul Systems, Simon Ritter.</span><span class="sxs-lookup"><span data-stu-id="296f4-162">The video is narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="296f4-163">La discussione su Flight Recorder inizia al minuto 31:30.</span><span class="sxs-lookup"><span data-stu-id="296f4-163">The Flight Recorder discussion starts at 31:30.</span></span>

