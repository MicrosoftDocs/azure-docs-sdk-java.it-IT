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
# <a name="spring-boot-starters-for-azure"></a>Utilità di avvio Spring Boot per Azure

Questo articolo descrive le diverse utilità di avvio Spring Boot per [Spring Initializr] che offrono agli sviluppatori Java l'integrazione di funzionalità per l'uso di Microsoft Azure.

![Utilità di avvio Spring Boot per Azure][spring-boot-starters]

Sono attualmente disponibili le utilità di avvio Spring Boot seguenti per Azure:

* **[Supporto di Azure](#azure-support)**

   Fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory e così via.

* **[Azure Active Directory](#azure-active-directory)**

   Fornisce il supporto di integrazione per Spring Security con Azure Active Directory per l'autenticazione.

* **[Azure Key Vault](#azure-key-vault)**

   Fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.

* **[Archiviazione di Azure](#azure-storage)**

   Fornisce il supporto di Spring Boot per i servizi di archiviazione di Azure.

<a name="azure-support"></a>
## <a name="azure-support"></a>Supporto di Azure

Questa utilità di avvio Spring Boot offre il supporto della configurazione automatica per i servizi di Azure, ad esempio per il bus di servizio, Archiviazione, Active Directory, Cosmos DB, Key Vault e così via.

Per esempi dell'uso delle diverse funzionalità di Azure fornite con questa utilità di avvio, vedere:

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

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
## <a name="azure-active-directory"></a>Azure Active Directory

Questa utilità di avvio Spring Boot fornisce il supporto per la configurazione automatica di Spring Security per l'integrazione con Azure Active Directory per l'autenticazione.

Per esempi dell'uso delle funzionalità di Azure Active Directory fornite con questa utilità di avvio, vedere:

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

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
## <a name="azure-key-vault"></a>Azure Key Vault

Questa utilità di avvio Spring Boot fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.

Per esempi dell'uso delle funzionalità di Azure Key Vault fornite con questa utilità di avvio, vedere:

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

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
## <a name="azure-storage"></a>Archiviazione di Azure

Questa utilità di avvio Spring Boot fornisce il supporto di integrazione di Spring Boot per i servizi di archiviazione di Azure.

Per esempi dell'uso delle funzionalità di Archiviazione di Azure fornite con questa utilità di avvio, vedere:

* [Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

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

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni [Spring Boot] in Azure, vedere [Spring in Azure].

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.

<!-- URL List -->

[Azure per sviluppatori Java]: /java/azure/
[Uso di Azure DevOps e Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring in Azure]: /java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
