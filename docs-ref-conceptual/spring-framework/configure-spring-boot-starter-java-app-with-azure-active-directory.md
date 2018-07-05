---
title: Come usare l'utilità di avvio Spring Boot per Azure Active Directory
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Active Directory.
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 06/20/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: adcbc78cc129daf589bf070741308e4024432e5d
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090834"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a>Come usare l'utilità di avvio Spring Boot per Azure Active Directory

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'app con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [Vantaggi per gli abbonati MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Creare un'applicazione personalizzata tramite Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi di **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul collegamento relativo a **Switch to the full version** (Passa alla versione completa) di Spring Initializr.

   ![Specificare il nome del gruppo e dell'elemento][security-01]

1. Scorrere verso il basso fino alla sezione **Core**, selezionare la casella **Security** (Sicurezza) e nella sezione **Web** selezionare la casella **Web**.

   ![Selezionare le utilità di avvio Security (Sicurezza) e Web][security-02]

1. Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Active Directory**.

   ![Selezionare l'utilità di avvio Azure Active Directory][security-03]

1. Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).

   ![Generare il progetto Spring Boot][security-04]

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a>Creare e configurare una nuova istanza di Azure Active Directory

### <a name="create-the-active-directory-instance"></a>Creare l'istanza di Active Directory

1. Accedere a <https://portal.azure.com>.

1. Fare clic su **+Nuovo**, quindi su **Sicurezza e identità** e infine su **Azure Active Directory**.

   ![Creare la nuova istanza di Azure Active Directory][directory-01]

1. Immettere il **nome organizzazione** e il **nome di dominio iniziale** e quindi fare clic su **Crea**.

   ![Specificare i nomi di Azure Active Directory][directory-02]

1. Scegliere la nuova istanza di Azure Active Directory dal menu a discesa della barra degli strumenti superiore del portale di Azure.

   ![Scegliere l'istanza di Azure Active Directory][directory-03]

1. Selezionare **Azure Active Directory** nel menu del portale, fare clic su **Proprietà** e copiare il valore di **ID directory**, perché verrà usato più avanti in questo articolo.

   ![Copiare l'ID di Azure Active Directory][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Aggiungere una registrazione per l'app Spring Boot

1. Scegliere **Azure Active Directory** dal menu del portale, fare clic su **Panoramica** e quindi fare cli su **Registrazioni per l'app**.

   ![Aggiungere una nuova registrazione per l'app][directory-04]

1. Fare clic su **Registrazione nuova applicazione**, specificare il **nome** dell'applicazione, usare http://localhost:8080 come **URL di accesso** e quindi fare clic su **Crea**.

   ![Creare una nuova registrazione di app][directory-05]

1. Dopo che è stata creata, fare clic su della registrazione per l'applicazione.

   ![Selezionare la registrazione per l'app][directory-06]

1. Nella pagina per la registrazione dell'app copiare il valore di **ID applicazione** per usarlo in seguito, quindi fare clic su **Impostazioni** e infine su **Chiavi**.

   ![Creare le chiavi della registrazione per l'app][directory-07]

1. Aggiungere una **descrizione**, specificare la **durata** di una nuova chiave e fare clic su **Salva**. Il valore della chiave verrà inserito automaticamente quando si fa clic sull'icona **Salva** ed è necessario copiare il valore della chiave per usarlo in seguito. Non sarà possibile recuperare questo valore in un secondo momento.

   ![Specificare i parametri della chiave di registrazione per l'app][directory-08]

1. Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **Autorizzazioni necessarie**.

   ![Autorizzazioni necessarie per la registrazione per l'app][directory-09]

1. Fare clic su **Microsoft Azure Active Directory**.

   ![Selezionare Microsoft Azure Active Directory][directory-10]

1. Selezionare le caselle **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente** e quindi fare clic su **Salva**.

   ![Abilitare le autorizzazioni di accesso][directory-11]

1. Nella pagina **Autorizzazioni necessarie** fare clic su **Concedi autorizzazioni** e su **Sì** quando richiesto.

   ![Concedere le autorizzazioni di accesso][directory-12]

1. Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **URL di risposta**.

   ![Modificare gli URL di risposta][directory-14]

1. Immettere "http://localhost:8080/login/oauth2/code/azure" come nuovo URL di risposta e quindi fare clic su **Salva**.

   ![Aggiungere un nuovo URL di risposta][directory-15]

## <a name="configure-and-compile-your-spring-boot-application"></a>Configurare e compilare l'applicazione Spring Boot

1. Estrarre i file dell'archivio di progetto scaricato in una directory.

2. Passare alla cartella padre nel progetto e aprire il file *pom.xml* in un editor di testo.

3. Aggiungere le dipendenze per la sicurezza OAuth2 di Spring, ad esempio:

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

4. Salvare e chiudere il file *pom.xml*.

5. Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.

6. Aggiungere la chiave per l'account di archiviazione usando i valori precedenti, ad esempio:

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.active-directory-groups=Users
   ```
   Dove:

   | Parametro | DESCRIZIONE |
   |---|---|
   | `azure.activedirectory.tenant-id` | Contiene il valore di **ID directory** di Active Directory copiato in precedenza. |
   | `spring.security.oauth2.client.registration.azure.client-id` | Contiene il valore di **ID applicazione** della registrazione dell'app completata in precedenza. |
   | `spring.security.oauth2.client.registration.azure.client-secret` | Contiene il **valore** della chiave di registrazione dell'app completata in precedenza. |
   | `azure.activedirectory.active-directory-groups` | Contiene un elenco di gruppi di Active Directory da usare per l'autenticazione. |

   > [!NOTE]
   > 
   > Per un elenco completo dei valori disponibili nel file *application.properties*, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.
   >

7. Salvare e chiudere il file *application.properties*.

8. Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.

9. Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.

10. Immettere il codice seguente, quindi salvare e chiudere il file:

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```
   > [!NOTE]
   > 
   > Il nome di gruppo specificato nel metodo `@PreAuthorize("hasRole('')")` deve contenere uno dei gruppi specificati nel campo `azure.activedirectory.active-directory-groups` del file *application.properties*.
   >

   > [!NOTE]
   > 
   > È possibile specificare impostazioni di autorizzazione diverse per mapping di richieste diversi, ad esempio:
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```
   >    

11. Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/security*.

12. Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.

13. Immettere il codice seguente, quindi salvare e chiudere il file:

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![Compilare l'applicazione][build-application]

1. Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <http://localhost:8080> in un Web browser. Verranno richiesti un nome utente e una password.

   ![Accesso all'applicazione][application-login]

1. Al termine dell'accesso, verrà visualizzato il testo "Hello World" di esempio dal controller.

   ![Accesso riuscito][hello-world]

   > [!NOTE]
   > 
   > Agli account utente non autorizzati verrà visualizzato il messaggio **HTTP 403** di accesso non autorizzato.
   >

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'uso di Azure Active Directory, vedere gli articoli seguenti:

* [Documentazione di Azure Active Directory].

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

Per un esempio più dettagliato, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.

<!-- URL List -->

[Documentazione di Azure Active Directory]: /azure/active-directory/
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Vantaggi per gli abbonati MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

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
[directory-13]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-13.png
[directory-14]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-14.png
[directory-15]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-15.png

[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
