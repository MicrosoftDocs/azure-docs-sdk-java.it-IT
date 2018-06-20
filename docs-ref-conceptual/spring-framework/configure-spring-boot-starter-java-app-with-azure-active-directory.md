---
title: Come usare l'utilità di avvio Spring Boot per Azure Active Directory
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Active Directory.
services: active-directory
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: cf1cad0b87626058f7204a6565d09fb8901b7ce4
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954682"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="d0dbd-103">Come usare l'utilità di avvio Spring Boot per Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0dbd-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="d0dbd-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d0dbd-104">Overview</span></span>

<span data-ttu-id="d0dbd-105">Questo articolo illustra la creazione di un'app con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d0dbd-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0dbd-106">prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d0dbd-106">Prerequisites</span></span>

<span data-ttu-id="d0dbd-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="d0dbd-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="d0dbd-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="d0dbd-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="d0dbd-110">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="d0dbd-111">Creare un'applicazione personalizzata tramite Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="d0dbd-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="d0dbd-112">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="d0dbd-113">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi di **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul collegamento relativo a **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Specificare il nome del gruppo e dell'elemento][security-01]

1. <span data-ttu-id="d0dbd-115">Scorrere verso il basso fino alla sezione **Core**, selezionare la casella **Security** (Sicurezza) e nella sezione **Web** selezionare la casella **Web**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![Selezionare le utilità di avvio Security (Sicurezza) e Web][security-02]

1. <span data-ttu-id="d0dbd-117">Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![Selezionare l'utilità di avvio Azure Active Directory][security-03]

1. <span data-ttu-id="d0dbd-119">Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="d0dbd-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Generare il progetto Spring Boot][security-04]

1. <span data-ttu-id="d0dbd-121">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="d0dbd-122">Creare e configurare una nuova istanza di Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0dbd-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="d0dbd-123">Creare l'istanza di Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0dbd-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="d0dbd-124">Accedere a <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="d0dbd-125">Fare clic su **+Nuovo**, quindi su **Sicurezza e identità** e infine su **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![Creare la nuova istanza di Azure Active Directory][directory-01]

1. <span data-ttu-id="d0dbd-127">Immettere il **nome organizzazione** e il **nome di dominio iniziale** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-127">Enter your **Organization name** and your **Initial domain name**, and then click **Create**.</span></span>

   ![Specificare i nomi di Azure Active Directory][directory-02]

1. <span data-ttu-id="d0dbd-129">Scegliere la nuova istanza di Azure Active Directory dal menu a discesa della barra degli strumenti superiore del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-129">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![Scegliere l'istanza di Azure Active Directory][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="d0dbd-131">Aggiungere una registrazione per l'app Spring Boot</span><span class="sxs-lookup"><span data-stu-id="d0dbd-131">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="d0dbd-132">Scegliere **Azure Active Directory** dal menu del portale, fare clic su **Panoramica** e quindi fare cli su **Registrazioni per l'app**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-132">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![Aggiungere una nuova registrazione per l'app][directory-04]

1. <span data-ttu-id="d0dbd-134">Fare clic su **Registrazione nuova applicazione**, specificare il **nome** dell'applicazione, usare http://localhost:8080 per **URL di accesso** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-134">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![Creare una nuova registrazione di app][directory-05]

1. <span data-ttu-id="d0dbd-136">Dopo che è stata creata, fare clic su della registrazione per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-136">Click your application registration after it has been created.</span></span>

   ![Selezionare la registrazione per l'app][directory-06]

1. <span data-ttu-id="d0dbd-138">Nella pagina della registrazione per l'app copiare l'**ID applicazione** per usarlo in seguito, quindi fare clic su **Chiavi**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-138">When the page for your app registration, copy your **Application ID** for later, then click **Keys**.</span></span>

   ![Creare le chiavi della registrazione per l'app][directory-07]

1. <span data-ttu-id="d0dbd-140">Aggiungere una **descrizione**, specificare la **durata** di una nuova chiave e fare clic su **Salva**. Il valore della chiave verrà inserito automaticamente quando si fa clic sull'icona **Salva** ed è necessario copiare il valore della chiave per usarlo in seguito.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-140">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key for later.</span></span> <span data-ttu-id="d0dbd-141">Non sarà possibile recuperare questo valore in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-141">(You will not be able to retrieve this value later.)</span></span>

   ![Specificare i parametri della chiave di registrazione per l'app][directory-08]

1. <span data-ttu-id="d0dbd-143">Nella pagina principale della registrazione per l'app fare clic su **Autorizzazioni necessarie**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-143">From the main page for your app registration, click **Required permissions**.</span></span>

   ![Autorizzazioni necessarie per la registrazione per l'app][directory-09]

1. <span data-ttu-id="d0dbd-145">Fare clic su **Microsoft Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-145">Click **Windows Azure Active Directory**.</span></span>

   ![Selezionare Microsoft Azure Active Directory][directory-10]

1. <span data-ttu-id="d0dbd-147">Selezionare le caselle **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente** e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-147">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![Abilitare le autorizzazioni di accesso][directory-11]

1. <span data-ttu-id="d0dbd-149">Nella pagina **Autorizzazioni necessarie** fare clic su **Concedi autorizzazioni** e su **Sì** quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-149">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![Concedere le autorizzazioni di accesso][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="d0dbd-151">Configurare e compilare l'applicazione Spring Boot</span><span class="sxs-lookup"><span data-stu-id="d0dbd-151">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="d0dbd-152">Estrarre i file dell'archivio di progetto scaricato in una directory.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-152">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="d0dbd-153">Passare alla cartella padre nel progetto e aprire il file *pom.xml* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-153">Navigate to the parent folder in your project and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="d0dbd-154">Aggiungere la dipendenza per la sicurezza OAuth2 di Spring, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-154">Add the dependency for Spring OAuth2 security; for example:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. <span data-ttu-id="d0dbd-155">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-155">Save and close the  the *pom.xml* file.</span></span>

1. <span data-ttu-id="d0dbd-156">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="d0dbd-157">Aggiungere la chiave per l'account di archiviazione usando i valori precedenti, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-157">Add the key for your storage account using the values from earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   <span data-ttu-id="d0dbd-158">Dove:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-158">Where:</span></span>
   | <span data-ttu-id="d0dbd-159">Parametro</span><span class="sxs-lookup"><span data-stu-id="d0dbd-159">Parameter</span></span> | <span data-ttu-id="d0dbd-160">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="d0dbd-160">Description</span></span> |
   |---|---|
   | `azure.activedirectory.clientId` | <span data-ttu-id="d0dbd-161">Contiene l'**ID applicazione** precedente.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-161">Contains your **Application ID** from earlier.</span></span> |
   | `azure.activedirectory.clientSecret` | <span data-ttu-id="d0dbd-162">Contiene il valore della chiave della registrazione per l'app completata prima.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-162">Contains the key value from your app registration which you completed earlier.</span></span> |
   | `azure.activedirectory.activeDirectoryGroups` | <span data-ttu-id="d0dbd-163">Contiene un elenco di gruppi di Active Directory da usare per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-163">Contains a list of Active Directory groups to use for authentication.</span></span> |


1. <span data-ttu-id="d0dbd-164">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-164">Save and close the  the *application.properties* file.</span></span>

1. <span data-ttu-id="d0dbd-165">Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-165">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="d0dbd-166">Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-166">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="d0dbd-167">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-167">Enter the following code, then save and close the file:</span></span>

   ```java
   package com.wingtiptoys.security;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.web.authentication.preauth.PreAuthenticatedAuthenticationToken;
   
   @RestController
   public class HelloController {
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String hello() {
         return "Hello World!";
      }
   }
   ```

1. <span data-ttu-id="d0dbd-168">Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/security*.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-168">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="d0dbd-169">Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-169">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="d0dbd-170">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-170">Enter the following code, then save and close the file:</span></span>

   ```java
   package com.wingtiptoys.security;

   import com.microsoft.azure.spring.autoconfigure.aad.AADAuthenticationFilter;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.autoconfigure.security.oauth2.client.EnableOAuth2Sso;
   import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
   import org.springframework.security.web.csrf.CookieCsrfTokenRepository;
   
   @EnableOAuth2Sso
   @EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true)
   
   public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
      @Autowired
      private AADAuthenticationFilter aadAuthFilter;
      @Override
      protected void configure(HttpSecurity http) throws Exception {
         http.authorizeRequests().anyRequest().permitAll();
         http.addFilterBefore(aadAuthFilter, UsernamePasswordAuthenticationFilter.class);
      }
   }
   ```

## <a name="build-and-test-your-app"></a><span data-ttu-id="d0dbd-171">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="d0dbd-171">Build and test your app</span></span>

1. <span data-ttu-id="d0dbd-172">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="d0dbd-173">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   ```

   ![Compilare l'applicazione][build-application]

1. <span data-ttu-id="d0dbd-175">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-175">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="d0dbd-176">Dopo che l'applicazione è stata compilata e avviata da Maven, aprire <http://localhost:8080> in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-176">After your application is built and started by Maven, open <http://localhost:8080> in a web browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0dbd-177">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d0dbd-177">Next steps</span></span>

<span data-ttu-id="d0dbd-178">Per altre informazioni sull'uso di Azure Active Directory, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-178">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="d0dbd-179">[Documentazione di Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="d0dbd-179">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="d0dbd-180">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="d0dbd-180">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="d0dbd-181">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="d0dbd-181">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="d0dbd-182">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="d0dbd-182">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="d0dbd-183">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].</span><span class="sxs-lookup"><span data-stu-id="d0dbd-183">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="d0dbd-184">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-184">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="d0dbd-185">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-185">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="d0dbd-186">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-186">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="d0dbd-187">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="d0dbd-187">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Documentazione di Azure Active Directory]: /azure/active-directory/
[Azure Active Directory Documentation]: /azure/active-directory/
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[security-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-01.png
[security-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-02.png
[security-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-03.png
[security-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-04.png

[directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-01.png
[directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-02.png
[directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-03.png
[directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-04.png
[directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-05.png
[directory-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-06.png
[directory-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-07.png
[directory-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-08.png
[directory-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-09.png
[directory-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-10.png
[directory-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-11.png
[directory-12]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-12.png

[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
