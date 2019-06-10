---
title: Installare il JDK Azul Zulu per Azure e Azure Stack
description: Come installare i JDK (Java Development Kit) Azul Zulu per lo sviluppo di Azure con Windows, Linux e Mac
author: erickson-doug
manager: douge
ms.author: douge
ms.date: 4/19/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 33d2206f9a0a1cc9fadc60b17c1a93f66c592fe3
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568594"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a><span data-ttu-id="95094-103">Installare JDK per Azure e Azure Stack</span><span class="sxs-lookup"><span data-stu-id="95094-103">Install the JDK for Azure and Azure Stack</span></span>

<span data-ttu-id="95094-104">Le build Azul Zulu Enterprise di OpenJDK sono distribuzioni di OpenJDK gratuite, multipiattaforma e pronte per la produzione per Azure e Azure Stack, supportate da Microsoft e Azul Systems.</span><span class="sxs-lookup"><span data-stu-id="95094-104">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="95094-105">Contengono tutti i componenti necessari per compilare ed eseguire applicazioni Java SE.</span><span class="sxs-lookup"><span data-stu-id="95094-105">They contain all the components for building and running Java SE applications.</span></span>

<span data-ttu-id="95094-106">Per ogni [sistema operativo client sono supportati più tipi di pacchetti di download](https://www.azul.com/downloads/azure-only/zulu/).</span><span class="sxs-lookup"><span data-stu-id="95094-106">There are [multiple download package types supported for each client OS](https://www.azul.com/downloads/azure-only/zulu/).</span></span> <span data-ttu-id="95094-107">È anche possibile ottenere un'immagine di macchina virtuale dalla raccolta di Azure Marketplace per le piattaforme seguenti:</span><span class="sxs-lookup"><span data-stu-id="95094-107">You can also get a virtual machine image from the Azure Marketplace Gallery for the following platforms:</span></span>

  * [<span data-ttu-id="95094-108">Azul Zulu: Java 8 in Ubuntu 18.04</span><span class="sxs-lookup"><span data-stu-id="95094-108">Azul Zulu: Java 8 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
  * [<span data-ttu-id="95094-109">Azul Zulu: Java 8 in Windows Server 2019</span><span class="sxs-lookup"><span data-stu-id="95094-109">Azul Zulu: Java 8 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-windows-2019)
  
  * [<span data-ttu-id="95094-110">Azul Zulu: Java 11 in Ubuntu 18.04</span><span class="sxs-lookup"><span data-stu-id="95094-110">Azul Zulu: Java 11 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
  * [<span data-ttu-id="95094-111">Azul Zulu: Java 11 in Windows Server 2019</span><span class="sxs-lookup"><span data-stu-id="95094-111">Azul Zulu: Java 11 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-windows-2019)


> [!NOTE]
> <span data-ttu-id="95094-112">Queste istruzioni riguardano la versione Java 8 a 64 bit del JDK.</span><span class="sxs-lookup"><span data-stu-id="95094-112">These instructions target the 64-bit Java 8 version of the JDK.</span></span> <span data-ttu-id="95094-113">Azul fornisce anche l'ambiente JRE (Java Run-time Environment) come installazione autonoma.</span><span class="sxs-lookup"><span data-stu-id="95094-113">Azul also provides the Java Run-time Environment (JRE) as a stand-alone installation.</span></span> <span data-ttu-id="95094-114">Il JRE è incluso nell'installazione del JDK.</span><span class="sxs-lookup"><span data-stu-id="95094-114">The JRE is included with the JDK install.</span></span>
>
>  <span data-ttu-id="95094-115">I pacchetti di Java 11 sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).</span><span class="sxs-lookup"><span data-stu-id="95094-115">Java 11 packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-windows"></a><span data-ttu-id="95094-116">Scaricare e installare i JDK Azul Zulu per Windows</span><span class="sxs-lookup"><span data-stu-id="95094-116">Download and install the Azul Zulu JDKs for Windows</span></span> 

1. <span data-ttu-id="95094-117">[Scaricare il JDK 8 Azul Zulu a 64 bit come file MSI](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi) in un percorso del client, ad esempio `C:\Users\<your_login>\Downloads`.</span><span class="sxs-lookup"><span data-stu-id="95094-117">[Download the 64-bit Azul Zulu JDK 8 as an MSI](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi) to a location on your client, such as `C:\Users\<your_login>\Downloads`.</span></span> <span data-ttu-id="95094-118">I pacchetti ZIP sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).</span><span class="sxs-lookup"><span data-stu-id="95094-118">(.ZIP packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="95094-119">Passare alla directory e fare doppio clic sul file MSI scaricato per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="95094-119">Navigate to the directory and double-click the downloaded MSI file to begin installation.</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-mac"></a><span data-ttu-id="95094-120">Scaricare e installare i JDK Azul Zulu per Mac</span><span class="sxs-lookup"><span data-stu-id="95094-120">Download and install the Azul Zulu JDKs for Mac</span></span> 

<span data-ttu-id="95094-121">Per scaricare un file ZIP per Mac, seguire questa procedura.</span><span class="sxs-lookup"><span data-stu-id="95094-121">These steps download a ZIP file to your Mac.</span></span> <span data-ttu-id="95094-122">È anche disponibile una versione DMG.</span><span class="sxs-lookup"><span data-stu-id="95094-122">There is also a DMG version available.</span></span>

1. <span data-ttu-id="95094-123">[Scaricare il JDK 8 Azul Zulu a 64 bit come file ZIP](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip) in un percorso del client, ad esempio `/Library/Java/JavaVirtualMachines/`.</span><span class="sxs-lookup"><span data-stu-id="95094-123">[Download the 64-bit Azul Zulu JDK 8 as a ZIP file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip) to a location on your client, such as `/Library/Java/JavaVirtualMachines/`.</span></span> <span data-ttu-id="95094-124">I pacchetti DMG sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).</span><span class="sxs-lookup"><span data-stu-id="95094-124">(.DMG packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="95094-125">Avviare il Finder, passare alla directory di download e fare doppio clic sul file ZIP.</span><span class="sxs-lookup"><span data-stu-id="95094-125">Launch Finder, navigate to the download directory, and double-click the ZIP file.</span></span> <span data-ttu-id="95094-126">In alternativa, è possibile avviare una finestra di comando del terminale, passare alla directory ed eseguire:</span><span class="sxs-lookup"><span data-stu-id="95094-126">Alternatively, you can launch a terminal command window, navigate to the directory, and run:</span></span>

```cli
unzip <name_of_zulu_package>.zip
```

## <a name="download-and-install-the-azul-zulu-jdks-for-alpine-linux"></a><span data-ttu-id="95094-127">Scaricare e installare i JDK Azul Zulu per Alpine Linux</span><span class="sxs-lookup"><span data-stu-id="95094-127">Download and install the Azul Zulu JDKs for Alpine Linux</span></span>

1. <span data-ttu-id="95094-128">[Scaricare il JDK 8 Azul Zulu a 64 bit come file TAR](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz) in un percorso del client, ad esempio `/usr/lib/jvm`.</span><span class="sxs-lookup"><span data-stu-id="95094-128">[Download the 64-bit Azul Zulu JDK 8 as a TAR file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz) to a location on your client, such as `/usr/lib/jvm`.</span></span> <span data-ttu-id="95094-129">I pacchetti RPM e DEB sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).</span><span class="sxs-lookup"><span data-stu-id="95094-129">(.RPM and .DEB packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="95094-130">Passare alla directory ed eseguire il comando seguente per decomprimere ed espandere il file:</span><span class="sxs-lookup"><span data-stu-id="95094-130">Go to your directory and run the following command to unzip and expand the file:</span></span>

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a><span data-ttu-id="95094-131">Verificare l'installazione</span><span class="sxs-lookup"><span data-stu-id="95094-131">Confirm your installation</span></span>

<span data-ttu-id="95094-132">Per verificare l'installazione, eseguire `java -version` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="95094-132">To confirm your installation, go to the command-line and run `java -version`.</span></span>

<span data-ttu-id="95094-133">L'output del comando dovrebbe essere:</span><span class="sxs-lookup"><span data-stu-id="95094-133">The output of the command should be:</span></span>

```cli
$ java -version

openjdk version "1.8.0_212"
OpenJDK Runtime Environment (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 1.8.0_212-b04)
OpenJDK 64-Bit Server VM (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 25.212-b04, mixed mode)

```

## <a name="download-and-install-the-azul-zulu-jdks-from-a-yum-repository"></a><span data-ttu-id="95094-134">Scaricare e installare i JDK Azul Zulu da un repository Yum</span><span class="sxs-lookup"><span data-stu-id="95094-134">Download and install the Azul Zulu JDKs from a Yum repository</span></span>

<span data-ttu-id="95094-135">I JDK Azul Zulu vengono forniti in un [repository Yum](http://repos.azul.com/azure-only/zulu-azure.repo) da Azul.</span><span class="sxs-lookup"><span data-stu-id="95094-135">The Azul Zulu JDKs are provided in a [Yum repository](http://repos.azul.com/azure-only/zulu-azure.repo) by Azul.</span></span>

<span data-ttu-id="95094-136">**Per installare il JDK Azul Zulu per Java 8, eseguire i comandi seguenti dall'interfaccia della riga di comando:**</span><span class="sxs-lookup"><span data-stu-id="95094-136">**To install the Azul Zulu JDK for Java 8, run the following commands from your CLI:**</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="95094-137">Per Java 11, eseguire:</span><span class="sxs-lookup"><span data-stu-id="95094-137">For Java 11, run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

<span data-ttu-id="95094-138">Per Java 12 (anteprima), eseguire:</span><span class="sxs-lookup"><span data-stu-id="95094-138">For Java 12 (Preview), run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-12-azure-jdk
```

<span data-ttu-id="95094-139">**Per aggiornare un pacchetto JDK 8 Zulu da un repository Yum:**</span><span class="sxs-lookup"><span data-stu-id="95094-139">**To update a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="95094-140">Se si usa la versione 11 o 12, cambiare il numero di versione nel comando qui sopra.</span><span class="sxs-lookup"><span data-stu-id="95094-140">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="95094-141">**Per rimuovere un pacchetto JDK 8 Zulu da un repository Yum:**</span><span class="sxs-lookup"><span data-stu-id="95094-141">**To remove a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -y erase zulu-8-azure-jdk
```
<span data-ttu-id="95094-142">Se si usa la versione 11 o 12, cambiare il numero di versione nel comando qui sopra.</span><span class="sxs-lookup"><span data-stu-id="95094-142">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a><span data-ttu-id="95094-143">Scaricare e installare i JDK Azul Zulu da un repository apt-get</span><span class="sxs-lookup"><span data-stu-id="95094-143">Download and install the Azul Zulu JDKs from an apt-get repository</span></span>

<span data-ttu-id="95094-144">I JDK Azul Zulu vengono anche forniti in un [repository apt-get](http://repos.azul.com/azure-only/zulu/apt) da Azul.</span><span class="sxs-lookup"><span data-stu-id="95094-144">The Azul Zulu JDKs are also provided in an [apt-get repository](http://repos.azul.com/azure-only/zulu/apt) by Azul.</span></span>

<span data-ttu-id="95094-145">**Per installare il JDK Azul Zulu per Java 8 con apt-get, eseguire i comandi seguenti dall'interfaccia della riga di comando:**</span><span class="sxs-lookup"><span data-stu-id="95094-145">**To install the Azul Zulu JDK for Java 8 with apt-get, run the following commands from your CLI:**</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="95094-146">Per Java 11, eseguire:</span><span class="sxs-lookup"><span data-stu-id="95094-146">For Java 11, run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

<span data-ttu-id="95094-147">Per Java 12 (anteprima), eseguire:</span><span class="sxs-lookup"><span data-stu-id="95094-147">For Java 12 (Preview), run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-12-azure-jdk
```

<span data-ttu-id="95094-148">**Per aggiornare un pacchetto JDK 8 Zulu da un repository apt-get:**</span><span class="sxs-lookup"><span data-stu-id="95094-148">**To update a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="95094-149">La versione precedente verrà rimossa automaticamente.</span><span class="sxs-lookup"><span data-stu-id="95094-149">The previous release will be automatically removed.</span></span>
<span data-ttu-id="95094-150">Se si usa la versione 11 o 12, cambiare il numero di versione nel comando qui sopra.</span><span class="sxs-lookup"><span data-stu-id="95094-150">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="95094-151">**Per rimuovere un pacchetto JDK 8 Zulu da un repository apt-get:**</span><span class="sxs-lookup"><span data-stu-id="95094-151">**To remove a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

<span data-ttu-id="95094-152">Se si usa la versione 11 o 12, cambiare il numero di versione nel comando qui sopra.</span><span class="sxs-lookup"><span data-stu-id="95094-152">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="95094-153">Per le istruzioni più dettagliate su come preparare, installare e gestire i JDK Azul Zulu per lo sviluppo Azure, vedere la [documentazione ufficiale di Zulu](https://docs.azul.com/zulu/zuludocs/index.htm).</span><span class="sxs-lookup"><span data-stu-id="95094-153">For more detailed guidance on preparing, installing, and managing your Azul Zulu JDKs for Azure development, read [the official Zulu docs](https://docs.azul.com/zulu/zuludocs/index.htm).</span></span>

