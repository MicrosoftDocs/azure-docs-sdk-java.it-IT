---
title: "Utilità di avvio Spring Boot per Azure"
description: "Questo articolo descrive le diverse utilità di avvio di Spring Boot disponibili per Azure."
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 678d4b279cecb83c95b3bf0f6bcdf1581924aa62
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
---
# <a name="spring-boot-starters-for-azure"></a><span data-ttu-id="eb019-103">Utilità di avvio Spring Boot per Azure</span><span class="sxs-lookup"><span data-stu-id="eb019-103">Spring Boot Starters for Azure</span></span>

<span data-ttu-id="eb019-104">Questo articolo descrive le diverse utilità di avvio Spring Boot per [Spring Initializr] che offrono agli sviluppatori Java l'integrazione di funzionalità per l'uso di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="eb019-104">This article describes the various Spring Boot Starters for the [Spring Initializr] that provide Java developers with integration features for working with Microsoft Azure.</span></span>

![Utilità di avvio Spring Boot per Azure][spring-boot-starters]

<span data-ttu-id="eb019-106">Sono attualmente disponibili le utilità di avvio Spring Boot seguenti per Azure:</span><span class="sxs-lookup"><span data-stu-id="eb019-106">The following Spring Boot Starters are currently available for Azure:</span></span>

* <span data-ttu-id="eb019-107">**[Supporto di Azure](#azure-support)**</span><span class="sxs-lookup"><span data-stu-id="eb019-107">**[Azure Support](#azure-support)**</span></span>

   <span data-ttu-id="eb019-108">Fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory e così via.</span><span class="sxs-lookup"><span data-stu-id="eb019-108">Provides auto-configuration support for Azure Services; e.g. Service Bus, Storage, Active Directory, etc.</span></span>

* <span data-ttu-id="eb019-109">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="eb019-109">**[Azure Active Directory](#azure-active-directory)**</span></span>

   <span data-ttu-id="eb019-110">Fornisce il supporto di integrazione per Spring Security con Azure Active Directory per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="eb019-110">Provides integration support for Spring Security with Azure Active Directory for authentication.</span></span>

* <span data-ttu-id="eb019-111">**[Azure Key Vault](#azure-key-vault)**</span><span class="sxs-lookup"><span data-stu-id="eb019-111">**[Azure Key Vault](#azure-key-vault)**</span></span>

   <span data-ttu-id="eb019-112">Fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="eb019-112">Provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

* <span data-ttu-id="eb019-113">**[Archiviazione di Azure](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="eb019-113">**[Azure Storage](#azure-storage)**</span></span>

   <span data-ttu-id="eb019-114">Fornisce il supporto di Spring Boot per i servizi di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="eb019-114">Provides Spring Boot support for Azure Storage services.</span></span>

<a name="azure-support"></a>
## <a name="azure-support"></a><span data-ttu-id="eb019-115">Supporto di Azure</span><span class="sxs-lookup"><span data-stu-id="eb019-115">Azure Support</span></span>

<span data-ttu-id="eb019-116">Questa utilità di avvio Spring Boot fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory, Cosmos DB, Key Vault e così via.</span><span class="sxs-lookup"><span data-stu-id="eb019-116">This Spring Boot Starter provides auto-configuration support for Azure Services; for example: Service Bus, Storage, Active Directory, Cosmos DB, Key Vault, etc.</span></span>

<span data-ttu-id="eb019-117">Per esempi dell'uso delle diverse funzionalità di Azure fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="eb019-117">For examples of how to use the various Azure features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="eb019-118"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples></span><span class="sxs-lookup"><span data-stu-id="eb019-118"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples></span></span>

<span data-ttu-id="eb019-119">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="eb019-119">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="eb019-120">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="eb019-120">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="eb019-121">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="eb019-121">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* <span data-ttu-id="eb019-122">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="eb019-122">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a><span data-ttu-id="eb019-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eb019-123">Azure Active Directory</span></span>

<span data-ttu-id="eb019-124">Questa utilità di avvio Spring Boot fornisce il supporto per la configurazione automatica di Spring Security per l'integrazione con Azure Active Directory per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="eb019-124">This Spring Boot Starter provides auto-configuration support for Spring Security in order to provide integration with Azure Active Directory for authentication.</span></span>

<span data-ttu-id="eb019-125">Per esempi dell'uso delle funzionalità di Azure Active Directory fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="eb019-125">For examples of how to use the Azure Active Directory features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="eb019-126"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="eb019-126"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample></span></span>

<span data-ttu-id="eb019-127">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="eb019-127">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="eb019-128">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="eb019-128">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="eb019-129">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="eb019-129">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="eb019-130">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="eb019-130">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a><span data-ttu-id="eb019-131">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="eb019-131">Azure Key Vault</span></span>

<span data-ttu-id="eb019-132">Questa utilità di avvio Spring Boot fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="eb019-132">This Spring Boot Starter provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

<span data-ttu-id="eb019-133">Per esempi dell'uso delle funzionalità di Azure Key Vault fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="eb019-133">For examples of how to use the Azure Key Vault features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="eb019-134"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="eb019-134"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample></span></span>

<span data-ttu-id="eb019-135">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="eb019-135">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="eb019-136">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="eb019-136">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="eb019-137">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="eb019-137">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="eb019-138">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="eb019-138">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a><span data-ttu-id="eb019-139">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="eb019-139">Azure Storage</span></span>

<span data-ttu-id="eb019-140">Questa utilità di avvio Spring Boot fornisce il supporto di integrazione di Spring Boot per i servizi di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="eb019-140">This Spring Boot Starter provides Spring Boot integration support for Azure Storage services.</span></span>

<span data-ttu-id="eb019-141">Per esempi dell'uso delle funzionalità di Archiviazione di Azure fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="eb019-141">For examples of how to use the Azure Storage features that are provided by this starter, see the following:</span></span>

* [<span data-ttu-id="eb019-142">Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="eb019-142">How to use the Spring Boot Starter for Azure Storage</span></span>](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <span data-ttu-id="eb019-143"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="eb019-143"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample></span></span>

<span data-ttu-id="eb019-144">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="eb019-144">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="eb019-145">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="eb019-145">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="eb019-146">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="eb019-146">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="eb019-147">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="eb019-147">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a><span data-ttu-id="eb019-148">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="eb019-148">Next steps</span></span>

<span data-ttu-id="eb019-149">Per altre informazioni sull'uso delle applicazioni [Spring Boot] in Azure, vedere [Spring in Azure].</span><span class="sxs-lookup"><span data-stu-id="eb019-149">For more information about using [Spring Boot] applications on Azure, see [Spring on Azure].</span></span>

<span data-ttu-id="eb019-150">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].</span><span class="sxs-lookup"><span data-stu-id="eb019-150">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="eb019-151">Per informazioni sulla Guida introduttiva con le proprie applicazioni Spring Boot, vedere **Spring Initializr** (Inizializzazione di SpringBoot) all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="eb019-151">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<!-- URL List -->

[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring in Azure]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
