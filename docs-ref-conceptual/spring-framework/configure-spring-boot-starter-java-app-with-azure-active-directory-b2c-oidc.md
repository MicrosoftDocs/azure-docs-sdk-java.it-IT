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
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>Esercitazione: Proteggere un'app Web Java con Spring Boot Starter per Azure Active Directory B2C.

## <a name="overview"></a>Panoramica

Questo articolo illustra come creare un'app Java con [Spring Initializr](https://start.spring.io/), che usa Spring Boot Starter per Azure Active Directory (Azure AD).

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare un'applicazione Java con Spring Initializr
> * Configurare Azure Active Directory B2C
> * Proteggere l'applicazione con le classi e le annotazioni Spring Boot
> * Compilare e testare l'applicazione Java

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-an-app-using-spring-initializr"></a>Creare un'app con Spring Initializr

1. Passare a <https://start.spring.io/>.

2. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi selezionare i moduli **Web** e **Security** (Sicurezza) di Spring Initializr.

   ![Specificare il nome del gruppo e dell'elemento](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. Fare clic su `Generate Project` e, quando richiesto, scaricare il progetto in un percorso nel computer locale.

## <a name="create-azure-active-directory-instance"></a>Creare l'istanza di Azure Active Directory

### <a name="create-the-active-directory-instance"></a>Creare l'istanza di Active Directory

1. Accedere a <https://portal.azure.com>.

2. Fare clic su **+Crea una risorsa**, quindi su **Identità** e infine su **Azure Active Directory B2C**.

   ![Creare la nuova istanza di Azure Active Directory B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. Immettere il **nome organizzazione** e il **nome di dominio iniziale**, registrare il **nome di dominio** come `${your-tenant-name}` e quindi fare clic su **Crea**.

   ![Ottenere il nome del tenant B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. Selezionare il nome dell'account nella parte superiore destra della barra degli strumenti del portale di Azure e quindi fare clic su **Cambia directory**.

   ![Scegliere l'istanza di Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. Selezionare la nuova istanza di Azure Active Directory nel menu a discesa.

   ![Scegliere l'istanza di Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. Cercare `b2c` e fare clic sul servizio `Azure AD B2C`.

   ![Individuare l'istanza di Azure Active Directory B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Aggiungere una registrazione per l'app Spring Boot

1. Scegliere **Azure AD B2C** dal menu del portale e fare clic su **Applicazioni**, quindi su **Aggiungi**.

   ![Aggiungere una nuova registrazione per l'app](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. Specificare il **nome** dell'applicazione, aggiungere `http://localhost:8080/home` per **URL di risposta**, registrare l'**ID applicazione** come `${your-client-id}` e quindi fare clic su **Salva**.

   ![Aggiungere l'URL di risposta dell'applicazione](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. Selezionare **Chiavi** nell'applicazione, fare clic su **Genera chiave** per generare `${your-client-secret}` e quindi su **Salva**.

4. Selezionare **Flussi utente** a sinistra e quindi fare clic su **Nuovo flusso utente**.

   ![Creare un flusso utente](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. Scegliere **Accedi o registrati**, **Criteri di modifica del profilo** e **Reimpostazione password** per creare i flussi utente. Specificare i valori per **Nome** e **Attributi e attestazioni utente** del flusso utente, quindi fare clic su **Crea**.

   ![Configurare il flusso utente](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a>Configurare e compilare l'app

1. Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.

2. Passare alla cartella padre per il progetto e aprire il file di progetto Maven `pom.xml` in un editor di testo.

3. Aggiungere le dipendenze per la sicurezza OAuth2 di Spring a `pom.xml`:

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

4. Salvare e chiudere il file *pom.xml*.

5. Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.yml* in un editor di testo.

6. Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:

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
   Dove:

   | Parametro | DESCRIZIONE |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | Contiene il valore di `${your-tenant-name` di AD B2C illustrato in precedenza. |
   | `azure.activedirectory.b2c.client-id` | Contiene il valore di `${your-client-id}` dell'applicazione completata in precedenza. |
   | `azure.activedirectory.b2c.client-secret` | Contiene il valore di `${your-client-secret}` dell'applicazione completata in precedenza. |
   | `azure.activedirectory.b2c.reply-url` | Contiene uno degli **URL di risposta** dell'applicazione completata in precedenza. |
   | `azure.activedirectory.b2c.logout-success-url` | Specificare l'URL relativo alla corretta disconnessione dell'applicazione. |
   | `azure.activedirectory.b2c.user-flows` | Contiene il nome dei flussi utente completati in precedenza.

   > [!NOTE]
   > 
   > Per un elenco completo dei valori disponibili nel file *application.yml*, vedere l'[esempio di Spring Boot per Azure Active Directory B2C][esempio di Spring Boot per AAD B2C] in GitHub.
   >

7. Salvare e chiudere il file *application.yml*.

8. Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione.

9. Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.

10. Immettere il codice seguente, quindi salvare e chiudere il file:

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

11. Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione.

12. Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.

13. Immettere il codice seguente, quindi salvare e chiudere il file:

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
14. Copiare `greeting.html` e `home.html` dall'[esempio di Spring Boot per Azure AD B2C](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates) e sostituire `${your-profile-edit-user-flow}` e `${your-password-reset-user-flow}` con il nome del flusso utente completato in precedenza.

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.

2. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <http://localhost:8080/> in un Web browser. Si dovrebbe essere reindirizzati alla pagina di accesso.

   ![Pagina di accesso](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. Fare clic sul collegamento con il nome del flusso utente `${your-sign-up-or-in}`. Si dovrebbe essere reindirizzati in Azure AD B2C per avviare il processo di autenticazione.

   ![Accesso ad Azure AD B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. Al termine dell'accesso, dovrebbe essere visualizzato l'esempio di `home page` nel browser.

   ![Accesso riuscito](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a>Summary

In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory B2C, si è configurato un nuovo tenant di Azure AD B2C registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)
