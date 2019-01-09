---
title: Utilità di avvio Spring Boot per Azure
description: Questo articolo descrive le diverse utilità di avvio di Spring Boot disponibili per Azure.
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 69c0381313994796af31d5301ceadb9f6f40dcb5
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991555"
---
# <a name="spring-boot-starters-for-azure"></a><span data-ttu-id="d3a27-103">Utilità di avvio Spring Boot per Azure</span><span class="sxs-lookup"><span data-stu-id="d3a27-103">Spring Boot Starters for Azure</span></span>

<span data-ttu-id="d3a27-104">Questo articolo descrive le diverse utilità di avvio Spring Boot per [Spring Initializr] che offrono agli sviluppatori Java l'integrazione di funzionalità per l'uso di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d3a27-104">This article describes the various Spring Boot Starters for the [Spring Initializr] that provide Java developers with integration features for working with Microsoft Azure.</span></span>

![Utilità di avvio Spring Boot per Azure][spring-boot-starters]

<span data-ttu-id="d3a27-106">Sono attualmente disponibili le utilità di avvio Spring Boot seguenti per Azure:</span><span class="sxs-lookup"><span data-stu-id="d3a27-106">The following Spring Boot Starters are currently available for Azure:</span></span>

* <span data-ttu-id="d3a27-107">**[Supporto di Azure](#azure-support)**</span><span class="sxs-lookup"><span data-stu-id="d3a27-107">**[Azure Support](#azure-support)**</span></span>

   <span data-ttu-id="d3a27-108">Fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory e così via.</span><span class="sxs-lookup"><span data-stu-id="d3a27-108">Provides auto-configuration support for Azure Services; e.g. Service Bus, Storage, Active Directory, etc.</span></span>

* <span data-ttu-id="d3a27-109">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="d3a27-109">**[Azure Active Directory](#azure-active-directory)**</span></span>

   <span data-ttu-id="d3a27-110">Fornisce il supporto di integrazione per Spring Security con Azure Active Directory per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d3a27-110">Provides integration support for Spring Security with Azure Active Directory for authentication.</span></span>

* <span data-ttu-id="d3a27-111">**[Azure Key Vault](#azure-key-vault)**</span><span class="sxs-lookup"><span data-stu-id="d3a27-111">**[Azure Key Vault](#azure-key-vault)**</span></span>

   <span data-ttu-id="d3a27-112">Fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d3a27-112">Provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

* <span data-ttu-id="d3a27-113">**[Archiviazione di Azure](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="d3a27-113">**[Azure Storage](#azure-storage)**</span></span>

   <span data-ttu-id="d3a27-114">Fornisce il supporto di Spring Boot per i servizi di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="d3a27-114">Provides Spring Boot support for Azure Storage services.</span></span>

<a name="azure-support"></a>
## <a name="azure-support"></a><span data-ttu-id="d3a27-115">Supporto di Azure</span><span class="sxs-lookup"><span data-stu-id="d3a27-115">Azure Support</span></span>

<span data-ttu-id="d3a27-116">Questa utilità di avvio Spring Boot offre il supporto della configurazione automatica per i servizi di Azure, ad esempio per il bus di servizio, Archiviazione, Active Directory, Cosmos DB, Key Vault e così via.</span><span class="sxs-lookup"><span data-stu-id="d3a27-116">This Spring Boot Starter provides auto-configuration support for Azure Services; for example: Service Bus, Storage, Active Directory, Cosmos DB, Key Vault, etc.</span></span>

<span data-ttu-id="d3a27-117">Per esempi dell'uso delle diverse funzionalità di Azure fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="d3a27-117">For examples of how to use the various Azure features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

<span data-ttu-id="d3a27-118">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="d3a27-118">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="d3a27-119">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="d3a27-119">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="d3a27-120">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="d3a27-120">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* <span data-ttu-id="d3a27-121">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="d3a27-121">The following section is added to the file:</span></span>

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
## <a name="azure-active-directory"></a><span data-ttu-id="d3a27-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d3a27-122">Azure Active Directory</span></span>

<span data-ttu-id="d3a27-123">Questa utilità di avvio Spring Boot fornisce il supporto per la configurazione automatica di Spring Security per l'integrazione con Azure Active Directory per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d3a27-123">This Spring Boot Starter provides auto-configuration support for Spring Security in order to provide integration with Azure Active Directory for authentication.</span></span>

<span data-ttu-id="d3a27-124">Per esempi dell'uso delle funzionalità di Azure Active Directory fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="d3a27-124">For examples of how to use the Azure Active Directory features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

<span data-ttu-id="d3a27-125">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="d3a27-125">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="d3a27-126">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="d3a27-126">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="d3a27-127">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="d3a27-127">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="d3a27-128">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="d3a27-128">The following section is added to the file:</span></span>

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
## <a name="azure-key-vault"></a><span data-ttu-id="d3a27-129">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d3a27-129">Azure Key Vault</span></span>

<span data-ttu-id="d3a27-130">Questa utilità di avvio Spring Boot fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d3a27-130">This Spring Boot Starter provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

<span data-ttu-id="d3a27-131">Per esempi dell'uso delle funzionalità di Azure Key Vault fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="d3a27-131">For examples of how to use the Azure Key Vault features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

<span data-ttu-id="d3a27-132">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="d3a27-132">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="d3a27-133">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="d3a27-133">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="d3a27-134">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="d3a27-134">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="d3a27-135">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="d3a27-135">The following section is added to the file:</span></span>

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
## <a name="azure-storage"></a><span data-ttu-id="d3a27-136">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="d3a27-136">Azure Storage</span></span>

<span data-ttu-id="d3a27-137">Questa utilità di avvio Spring Boot fornisce il supporto di integrazione di Spring Boot per i servizi di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="d3a27-137">This Spring Boot Starter provides Spring Boot integration support for Azure Storage services.</span></span>

<span data-ttu-id="d3a27-138">Per esempi dell'uso delle funzionalità di Archiviazione di Azure fornite con questa utilità di avvio, vedere:</span><span class="sxs-lookup"><span data-stu-id="d3a27-138">For examples of how to use the Azure Storage features that are provided by this starter, see the following:</span></span>

* [<span data-ttu-id="d3a27-139">Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="d3a27-139">How to use the Spring Boot Starter for Azure Storage</span></span>](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

<span data-ttu-id="d3a27-140">Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="d3a27-140">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="d3a27-141">Viene aggiunta la proprietà seguente all'elemento `<properties>`:</span><span class="sxs-lookup"><span data-stu-id="d3a27-141">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="d3a27-142">La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="d3a27-142">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="d3a27-143">Viene aggiunta la sezione seguente al file:</span><span class="sxs-lookup"><span data-stu-id="d3a27-143">The following section is added to the file:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d3a27-144">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d3a27-144">Next steps</span></span>

<span data-ttu-id="d3a27-145">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="d3a27-145">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d3a27-146">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="d3a27-146">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="d3a27-147">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d3a27-147">Additional Resources</span></span>

<span data-ttu-id="d3a27-148">Per altre informazioni sull'uso delle applicazioni [Spring Boot] in Azure, vedere [Spring in Azure].</span><span class="sxs-lookup"><span data-stu-id="d3a27-148">For more information about using [Spring Boot] applications on Azure, see [Spring on Azure].</span></span>

<span data-ttu-id="d3a27-149">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="d3a27-149">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="d3a27-150">Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="d3a27-150">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<!-- URL List -->

[Azure per sviluppatori Java]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Uso di Azure DevOps e Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring in Azure]: /java/azure/spring-framework/
[Spring on Azure]: /java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
