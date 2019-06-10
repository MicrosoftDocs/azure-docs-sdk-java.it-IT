---
title: Usare immagini Docker con un JDK per lo sviluppo Java in Azure
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: ee8df2a08b23d090965cb42e2c15b934d4785e7c
ms.sourcegitcommit: 03379369346974c6e80f86e7129b885112b5c1a9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/29/2019
ms.locfileid: "64910233"
---
# <a name="use-docker-with-a-jdk-for-azure"></a><span data-ttu-id="41ef3-102">Usare Docker con JDK per Azure</span><span class="sxs-lookup"><span data-stu-id="41ef3-102">Use Docker with a JDK for Azure</span></span> 

<span data-ttu-id="41ef3-103">Le immagini Docker predefinite per Java 7, 8 e 11 sono disponibili tramite [Docker Hub](https://hub.docker.com/_/microsoft-java-se).</span><span class="sxs-lookup"><span data-stu-id="41ef3-103">Pre-built Docker images for Java 7, 8, and 11 are available through [Docker Hub](https://hub.docker.com/_/microsoft-java-se).</span></span>

<span data-ttu-id="41ef3-104">Le immagini di contenitori Docker certificate per Zulu JDK, JRE e JRE headless con più immagini del sistema operativo di base sono disponibili in Docker Hub:</span><span class="sxs-lookup"><span data-stu-id="41ef3-104">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker Hub:</span></span>

* [<span data-ttu-id="41ef3-105">JDK</span><span class="sxs-lookup"><span data-stu-id="41ef3-105">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
* [<span data-ttu-id="41ef3-106">JRE</span><span class="sxs-lookup"><span data-stu-id="41ef3-106">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
* [<span data-ttu-id="41ef3-107">JRE headless</span><span class="sxs-lookup"><span data-stu-id="41ef3-107">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a><span data-ttu-id="41ef3-108">Esecuzione di un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="41ef3-108">Running a Docker image</span></span>

<span data-ttu-id="41ef3-109">Le immagini Docker possono essere eseguite con la sintassi `$ docker run mcr.microsoft.com/java/jdk:tag java`, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="41ef3-109">Docker images can be run using the syntax `$ docker run mcr.microsoft.com/java/jdk:tag java` as shown in the following example.</span></span>

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a><span data-ttu-id="41ef3-110">Creazione di un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="41ef3-110">Creating a Docker image</span></span>

<span data-ttu-id="41ef3-111">È possibile creare un'immagine usando le immagini di Docker Hub ufficiali di Microsoft, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="41ef3-111">You can create an image using Microsoft's official Docker Hub images as shown in the following examples.</span></span>

### <a name="create-a-docker-file"></a><span data-ttu-id="41ef3-112">Creare un file Docker</span><span class="sxs-lookup"><span data-stu-id="41ef3-112">Create a Docker file</span></span>

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a><span data-ttu-id="41ef3-113">Creare un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="41ef3-113">Build a Docker image</span></span>

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a><span data-ttu-id="41ef3-114">Eseguire la nuova immagine</span><span class="sxs-lookup"><span data-stu-id="41ef3-114">Run the new image</span></span>

```cli
docker run hello-world
```

<span data-ttu-id="41ef3-115">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="41ef3-115">You will see the following output:</span></span>

```output
Hello World - From Microsoft Azure !!!
```
