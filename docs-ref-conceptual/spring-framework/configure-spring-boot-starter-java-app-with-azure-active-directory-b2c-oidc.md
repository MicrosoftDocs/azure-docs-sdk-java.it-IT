---
title: Come usare Spring Boot Starter per Azure Active Directory B2C
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio per Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
editor: panli
ms.assetid: ''
ms.author: panli
ms.date: 02/28/2019
ms.devlang: java
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 21aa6c812a519d686356851a57f00688d9a24fa4
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625717"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a><span data-ttu-id="fd588-103">Esercitazione: Proteggere un'app Web Java con Spring Boot Starter per Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="fd588-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory B2C.</span></span>

## <a name="overview"></a><span data-ttu-id="fd588-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="fd588-104">Overview</span></span>

<span data-ttu-id="fd588-105">Questo articolo illustra come creare un'app Java con [Spring Initializr](https://start.spring.io/), che usa Spring Boot Starter per Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fd588-105">This article demonstrates creating a Java app with the [Spring Initializr](https://start.spring.io/) that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fd588-106">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="fd588-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fd588-107">Creare un'applicazione Java con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="fd588-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="fd588-108">Configurare Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="fd588-108">Configure Azure Active Directory B2C</span></span>
> * <span data-ttu-id="fd588-109">Proteggere l'applicazione con le classi e le annotazioni Spring Boot</span><span class="sxs-lookup"><span data-stu-id="fd588-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="fd588-110">Compilare e testare l'applicazione Java</span><span class="sxs-lookup"><span data-stu-id="fd588-110">Build and test your Java application</span></span>

<span data-ttu-id="fd588-111">Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="fd588-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd588-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fd588-112">Prerequisites</span></span>

<span data-ttu-id="fd588-113">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="fd588-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="fd588-114">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="fd588-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="fd588-115">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="fd588-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="fd588-116">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="fd588-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="fd588-117">Creare un'app con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="fd588-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="fd588-118">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="fd588-118">Browse to <https://start.spring.io/>.</span></span>

2. <span data-ttu-id="fd588-119">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi selezionare i moduli **Web** e **Security** (Sicurezza) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="fd588-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select the **Web** and **Security** module of the Spring Initializr.</span></span>

   ![Specificare il nome del gruppo e dell'elemento](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. <span data-ttu-id="fd588-121">Fare clic su `Generate Project` e, quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="fd588-121">Click `Generate Project`, download the project to a path on your local computer when prompted.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="fd588-122">Creare l'istanza di Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd588-122">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="fd588-123">Creare l'istanza di Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd588-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="fd588-124">Accedere a <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="fd588-124">Log into <https://portal.azure.com>.</span></span>

2. <span data-ttu-id="fd588-125">Fare clic su **+Crea una risorsa**, quindi su **Identità** e infine su **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="fd588-125">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory B2C**.</span></span>

   ![Creare la nuova istanza di Azure Active Directory B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. <span data-ttu-id="fd588-127">Immettere il **nome organizzazione** e il **nome di dominio iniziale**, registrare il **nome di dominio** come `${your-tenant-name}` e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="fd588-127">Enter your **Organization name** and your **Initial domain name**, record the **domain name** as your `${your-tenant-name}` and click **Create**.</span></span>

   ![Ottenere il nome del tenant B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. <span data-ttu-id="fd588-129">Selezionare il nome dell'account nella parte superiore destra della barra degli strumenti del portale di Azure e quindi fare clic su **Cambia directory**.</span><span class="sxs-lookup"><span data-stu-id="fd588-129">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![Scegliere l'istanza di Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. <span data-ttu-id="fd588-131">Selezionare la nuova istanza di Azure Active Directory nel menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="fd588-131">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![Scegliere l'istanza di Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. <span data-ttu-id="fd588-133">Cercare `b2c` e fare clic sul servizio `Azure AD B2C`.</span><span class="sxs-lookup"><span data-stu-id="fd588-133">Search `b2c` and click `Azure AD B2C` service.</span></span>

   ![Individuare l'istanza di Azure Active Directory B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="fd588-135">Aggiungere una registrazione per l'app Spring Boot</span><span class="sxs-lookup"><span data-stu-id="fd588-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="fd588-136">Scegliere **Azure AD B2C** dal menu del portale e fare clic su **Applicazioni**, quindi su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fd588-136">Select **Azure AD B2C** from the portal menu, click **Applications**, and then click **Add**.</span></span>

   ![Aggiungere una nuova registrazione per l'app](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. <span data-ttu-id="fd588-138">Specificare il **nome** dell'applicazione, aggiungere `http://localhost:8080/home` per **URL di risposta**, registrare l'**ID applicazione** come `${your-client-id}` e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="fd588-138">Specify your application **Name**, add `http://localhost:8080/home` for the **Reply URL**, record the **Application ID** as your `${your-client-id}` and then click **Save**.</span></span>

   ![Aggiungere l'URL di risposta dell'applicazione](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. <span data-ttu-id="fd588-140">Selezionare **Chiavi** nell'applicazione, fare clic su **Genera chiave** per generare `${your-client-secret}` e quindi su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="fd588-140">Select **Keys** from your application, click **Generate key** to generate `${your-client-secret}` and then **Save**.</span></span>

4. <span data-ttu-id="fd588-141">Selezionare **Flussi utente** a sinistra e quindi fare clic su **Nuovo flusso utente**.</span><span class="sxs-lookup"><span data-stu-id="fd588-141">Select **User flows** on your left, and then **Click** \*\*New user flow \*\*.</span></span>

   ![Creare un flusso utente](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. <span data-ttu-id="fd588-143">Scegliere **Accedi o registrati**, **Criteri di modifica del profilo** e **Reimpostazione password** per creare i flussi utente.</span><span class="sxs-lookup"><span data-stu-id="fd588-143">Choose **Sign up or in**, **Profile editing** and **Password reset** to create user flows respectively.</span></span> <span data-ttu-id="fd588-144">Specificare i valori per **Nome** e **Attributi e attestazioni utente** del flusso utente, quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="fd588-144">Specify your user flow **Name** and **User attributes and claims**, click **Create**.</span></span>

   ![Configurare il flusso utente](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="fd588-146">Configurare e compilare l'app</span><span class="sxs-lookup"><span data-stu-id="fd588-146">Configure and compile your app</span></span>

1. <span data-ttu-id="fd588-147">Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.</span><span class="sxs-lookup"><span data-stu-id="fd588-147">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

2. <span data-ttu-id="fd588-148">Passare alla cartella padre per il progetto e aprire il file di progetto Maven `pom.xml` in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="fd588-148">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

3. <span data-ttu-id="fd588-149">Aggiungere le dipendenze per la sicurezza OAuth2 di Spring a `pom.xml`:</span><span class="sxs-lookup"><span data-stu-id="fd588-149">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
       <version>2.1.6.M2</version>
   </dependency>
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf.extras</groupId>
       <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   </dependency>
   ```

4. <span data-ttu-id="fd588-150">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="fd588-150">Save and close the *pom.xml* file.</span></span>

5. <span data-ttu-id="fd588-151">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.yml* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="fd588-151">Navigate to the *src/main/resources* folder in your project and open the *application.yml* file in a text editor.</span></span>

6. <span data-ttu-id="fd588-152">Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fd588-152">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

   ```yaml
   azure:
     activedirectory:
       b2c:
         tenant: ${your-tenant-name}
         client-id: ${your-client-id}
         client-secret: ${your-client-secret}
         reply-url: ${your-reply-url-from-aad} # should be absolute url.
         logout-success-url: ${you-logout-success-url}
         user-flows:
           sign-up-or-sign-in: ${your-sign-up-or-in-user-flow}
           profile-edit: ${your-profile-edit-user-flow}     # optional
           password-reset: ${your-password-reset-user-flow} # optional
   ```
   <span data-ttu-id="fd588-153">Dove:</span><span class="sxs-lookup"><span data-stu-id="fd588-153">Where:</span></span>

   | <span data-ttu-id="fd588-154">Parametro</span><span class="sxs-lookup"><span data-stu-id="fd588-154">Parameter</span></span> | <span data-ttu-id="fd588-155">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="fd588-155">Description</span></span> |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | <span data-ttu-id="fd588-156">Contiene il valore di `${your-tenant-name` di AD B2C illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fd588-156">Contains your AD B2C's `${your-tenant-name` from earlier.</span></span> |
   | `azure.activedirectory.b2c.client-id` | <span data-ttu-id="fd588-157">Contiene il valore di `${your-client-id}` dell'applicazione completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fd588-157">Contains the `${your-client-id}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.client-secret` | <span data-ttu-id="fd588-158">Contiene il valore di `${your-client-secret}` dell'applicazione completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fd588-158">Contains the `${your-client-secret}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.reply-url` | <span data-ttu-id="fd588-159">Contiene uno degli **URL di risposta** dell'applicazione completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fd588-159">Contains one of the **Reply URL** from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.logout-success-url` | <span data-ttu-id="fd588-160">Specificare l'URL relativo alla corretta disconnessione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fd588-160">Specify the URL when your application logout successfully.</span></span> |
   | `azure.activedirectory.b2c.user-flows` | <span data-ttu-id="fd588-161">Contiene il nome dei flussi utente completati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fd588-161">Contains the name of the user flows that you completed earlier.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="fd588-162">Per un elenco completo dei valori disponibili nel file *application.yml*, vedere l'[esempio di Spring Boot per Azure Active Directory B2C][esempio di Spring Boot per AAD B2C] in GitHub.</span><span class="sxs-lookup"><span data-stu-id="fd588-162">For a full list of values that are available in your *application.yml* file, see the [Azure Active Directory B2C Spring Boot Sample][AAD B2C Spring Boot Sample] on GitHub.</span></span>
   >

7. <span data-ttu-id="fd588-163">Salvare e chiudere il file *application.yml*.</span><span class="sxs-lookup"><span data-stu-id="fd588-163">Save and close the *application.yml* file.</span></span>

8. <span data-ttu-id="fd588-164">Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fd588-164">Create a folder named *controller* in the Java source folder for your application.</span></span>

9. <span data-ttu-id="fd588-165">Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="fd588-165">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

10. <span data-ttu-id="fd588-166">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="fd588-166">Enter the following code, then save and close the file:</span></span>

    ```java
    package sample.aad.controller;
    
    import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
    import org.springframework.security.oauth2.core.user.OAuth2User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    
    @Controller
    public class WebController {
    
        private void initializeModel(Model model, OAuth2AuthenticationToken token) {
            if (token != null) {
                final OAuth2User user = token.getPrincipal();
    
                model.addAttribute("grant_type", user.getAuthorities());
                model.addAllAttributes(user.getAttributes());
            }
        }
    
        @GetMapping(value = "/")
        public String index(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "home";
        }
    
        @GetMapping(value = "/greeting")
        public String greeting(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "greeting";
        }
    
        @GetMapping(value = "/home")
        public String home(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "home";
        }
    }
    ```

11. <span data-ttu-id="fd588-167">Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fd588-167">Create a folder named *security* in the Java source folder for your application.</span></span>

12. <span data-ttu-id="fd588-168">Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="fd588-168">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

13. <span data-ttu-id="fd588-169">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="fd588-169">Enter the following code, then save and close the file:</span></span>

    ```java
    package sample.aad.security;
    
    import com.microsoft.azure.spring.autoconfigure.b2c.AADB2COidcLoginConfigurer;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    
    @EnableWebSecurity
    public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {
    
        private final AADB2COidcLoginConfigurer configurer;
    
        public WebSecurityConfiguration(AADB2COidcLoginConfigurer configurer) {
            this.configurer = configurer;
        }
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .anyRequest()
                    .authenticated()
                    .and()
                    .apply(configurer)
            ;
        }
    }
    ```
14. <span data-ttu-id="fd588-170">Copiare `greeting.html` e `home.html` dall'[esempio di Spring Boot per Azure AD B2C](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates) e sostituire `${your-profile-edit-user-flow}` e `${your-password-reset-user-flow}` con il nome del flusso utente completato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fd588-170">Copy the `greeting.html` and `home.html` from [Azure AD B2C Spring Boot Sample](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates), and replace the `${your-profile-edit-user-flow}` and `${your-password-reset-user-flow}` with your user flow name respectively that completed earlier.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="fd588-171">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="fd588-171">Build and test your app</span></span>

1. <span data-ttu-id="fd588-172">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.</span><span class="sxs-lookup"><span data-stu-id="fd588-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

2. <span data-ttu-id="fd588-173">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fd588-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. <span data-ttu-id="fd588-174">Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <http://localhost:8080/> in un Web browser. Si dovrebbe essere reindirizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="fd588-174">After your application is built and started by Maven, open <http://localhost:8080/> in a web browser; you should be redirected to login page.</span></span>

   ![Pagina di accesso](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. <span data-ttu-id="fd588-176">Fare clic sul collegamento con il nome del flusso utente `${your-sign-up-or-in}`. Si dovrebbe essere reindirizzati in Azure AD B2C per avviare il processo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="fd588-176">Click linke with name of `${your-sign-up-or-in}` user flow, you should be rediected Azure AD B2C to start the authentication process.</span></span>

   ![Accesso ad Azure AD B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. <span data-ttu-id="fd588-178">Al termine dell'accesso, dovrebbe essere visualizzato l'esempio di `home page` nel browser.</span><span class="sxs-lookup"><span data-stu-id="fd588-178">After you have logged in successfully, you should see the sample `home page` from the browser,</span></span>

   ![Accesso riuscito](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a><span data-ttu-id="fd588-180">Summary</span><span class="sxs-lookup"><span data-stu-id="fd588-180">Summary</span></span>

<span data-ttu-id="fd588-181">In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory B2C, si è configurato un nuovo tenant di Azure AD B2C registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.</span><span class="sxs-lookup"><span data-stu-id="fd588-181">In this tutorial, you created a new Java web application using the Azure Active Directory B2C starter, configured a new Azure AD B2C tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd588-182">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fd588-182">Next steps</span></span>

<span data-ttu-id="fd588-183">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="fd588-183">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd588-184">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="fd588-184">Spring on Azure</span></span>](/java/azure/spring-framework)
