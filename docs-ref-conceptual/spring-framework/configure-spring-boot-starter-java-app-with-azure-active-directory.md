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
ms.date: 07/02/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 665768ffe7bec977d553ffa62e1dbd6b968eb9de
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799907"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="9c772-103">Come usare l'utilità di avvio Spring Boot per Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c772-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="9c772-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9c772-104">Overview</span></span>

<span data-ttu-id="9c772-105">Questo articolo illustra la creazione di un'app con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c772-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c772-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9c772-106">Prerequisites</span></span>

<span data-ttu-id="9c772-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="9c772-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="9c772-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="9c772-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="9c772-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="9c772-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="9c772-110">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="9c772-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="9c772-111">Creare un'applicazione personalizzata tramite Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="9c772-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="9c772-112">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="9c772-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="9c772-113">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi fare clic sul collegamento **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="9c772-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Specificare il nome del gruppo e dell'elemento][security-01]

1. <span data-ttu-id="9c772-115">Scorrere verso il basso fino alla sezione **Core**, selezionare la casella **Security** (Sicurezza) e nella sezione **Web** selezionare la casella **Web**.</span><span class="sxs-lookup"><span data-stu-id="9c772-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![Selezionare le utilità di avvio Security (Sicurezza) e Web][security-02]

1. <span data-ttu-id="9c772-117">Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9c772-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![Selezionare l'utilità di avvio Azure Active Directory][security-03]

1. <span data-ttu-id="9c772-119">Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="9c772-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Generare il progetto Spring Boot][security-04]

1. <span data-ttu-id="9c772-121">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="9c772-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="9c772-122">Creare e configurare una nuova istanza di Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c772-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="9c772-123">Creare l'istanza di Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c772-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="9c772-124">Accedere a <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="9c772-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="9c772-125">Fare clic su **+Nuovo**, quindi su **Sicurezza e identità** e infine su **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9c772-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![Creare la nuova istanza di Azure Active Directory][directory-01]

1. <span data-ttu-id="9c772-127">Immettere il **Nome organizzazione** e il **Nome di dominio iniziale**.</span><span class="sxs-lookup"><span data-stu-id="9c772-127">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="9c772-128">Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione,</span><span class="sxs-lookup"><span data-stu-id="9c772-128">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="9c772-129">ad esempio: `wingtiptoysdirectory.onmicrosoft.com`. Al termine, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9c772-129">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![Specificare i nomi di Azure Active Directory][directory-02]

1. <span data-ttu-id="9c772-131">Scegliere la nuova istanza di Azure Active Directory dal menu a discesa della barra degli strumenti superiore del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="9c772-131">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![Scegliere l'istanza di Azure Active Directory][directory-03]

1. <span data-ttu-id="9c772-133">Selezionare **Azure Active Directory** nel menu del portale, fare clic su **Proprietà** e copiare il valore di **ID directory**, perché verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9c772-133">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![Copiare l'ID di Azure Active Directory][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="9c772-135">Aggiungere una registrazione per l'app Spring Boot</span><span class="sxs-lookup"><span data-stu-id="9c772-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="9c772-136">Scegliere **Azure Active Directory** dal menu del portale, fare clic su **Panoramica** e quindi fare cli su **Registrazioni per l'app**.</span><span class="sxs-lookup"><span data-stu-id="9c772-136">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![Aggiungere una nuova registrazione per l'app][directory-04]

2. <span data-ttu-id="9c772-138">Fare clic su **Registrazione nuova applicazione**, specificare il **nome** dell'applicazione, usare http://localhost:8080 come **URL di accesso** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9c772-138">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![Creare una nuova registrazione di app][directory-05]

3. <span data-ttu-id="9c772-140">Dopo che è stata creata, fare clic su della registrazione per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c772-140">Click your application registration after it has been created.</span></span>

   ![Selezionare la registrazione per l'app][directory-06]

4. <span data-ttu-id="9c772-142">Quando viene visualizzata la pagina per la registrazione dell'app, copiare il valore di **ID applicazione**, perché verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9c772-142">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="9c772-143">Fare clic su **Impostazioni** e quindi su **Chiara**.</span><span class="sxs-lookup"><span data-stu-id="9c772-143">Click **Settings**, and then click **Keys**.</span></span>

   ![Creare le chiavi della registrazione per l'app][directory-07]

5. <span data-ttu-id="9c772-145">Aggiungere una **Descrizione**, specificare la **Durata** di una nuova chiave e fare clic su **Salva**. Il valore della chiave verrà inserito automaticamente quando si fa clic sull'icona **Salva** ed è necessario copiare il valore della chiave per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9c772-145">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="9c772-146">Non sarà possibile recuperare questo valore in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="9c772-146">(You will not be able to retrieve this value later.)</span></span>

   ![Specificare i parametri della chiave di registrazione per l'app][directory-08]

6. <span data-ttu-id="9c772-148">Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **Autorizzazioni necessarie**.</span><span class="sxs-lookup"><span data-stu-id="9c772-148">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![Autorizzazioni necessarie per la registrazione per l'app][directory-09]

7. <span data-ttu-id="9c772-150">Fare clic su **Microsoft Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9c772-150">Click **Windows Azure Active Directory**.</span></span>

   ![Selezionare Microsoft Azure Active Directory][directory-10]

8. <span data-ttu-id="9c772-152">Selezionare le caselle **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente** e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="9c772-152">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![Abilitare le autorizzazioni di accesso][directory-11]

9. <span data-ttu-id="9c772-154">Nella pagina **Autorizzazioni necessarie** fare clic su **Concedi autorizzazioni** e su **Sì** quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="9c772-154">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![Concedere le autorizzazioni di accesso][directory-12]

10. <span data-ttu-id="9c772-156">Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **URL di risposta**.</span><span class="sxs-lookup"><span data-stu-id="9c772-156">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![Modificare gli URL di risposta][directory-14]

11. <span data-ttu-id="9c772-158">Immettere "<http://localhost:8080/login/oauth2/code/azure>" come nuovo URL di risposta e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="9c772-158">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![Aggiungere un nuovo URL di risposta][directory-15]

12. <span data-ttu-id="9c772-160">Nella pagina principale per la registrazione dell'app fare clic su **Manifesto**, quindi impostare il valore del parametro `oauth2AllowImplicitFlow` su `true` e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="9c772-160">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![Configurare il manifesto dell'app][directory-16]

    > [!NOTE]
    > 
    > <span data-ttu-id="9c772-162">Per altre informazioni sul parametro `oauth2AllowImplicitFlow` e altre impostazioni dell'applicazione, vedere [Manifesto dell'applicazione Azure Active Directory][AAD app manifest].</span><span class="sxs-lookup"><span data-stu-id="9c772-162">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="9c772-163">Aggiungere un account utente alla directory e quindi aggiungere tale account al gruppo</span><span class="sxs-lookup"><span data-stu-id="9c772-163">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="9c772-164">Nella pagina **Panoramica** di Active Directory fare clic su **Utenti**.</span><span class="sxs-lookup"><span data-stu-id="9c772-164">From the **Overview** page of your Active Directory, click **Users**.</span></span>

   ![Aprire il pannello Utenti][directory-17]

1. <span data-ttu-id="9c772-166">Quando viene visualizzato il pannello **Utenti**, fare clic su **Nuovo utente**.</span><span class="sxs-lookup"><span data-stu-id="9c772-166">When the **Users** panel is displayed, click **New user**.</span></span>

   ![Aggiungere un nuovo account utente][directory-18]

1. <span data-ttu-id="9c772-168">Quando viene visualizzato il pannello **Utente**, immettere il **Nome** e il **Nome utente**.</span><span class="sxs-lookup"><span data-stu-id="9c772-168">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![Immettere le informazioni sull'account utente][directory-19]

   > [!NOTE]
   > 
   > <span data-ttu-id="9c772-170">È necessario specificare l'URL della directory salvato in precedenza in questa esercitazione quando si immette il nome utente. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9c772-170">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="9c772-171">Fare clic su **Gruppi**, quindi selezionare i gruppi che verranno usati per l'autorizzazione nell'applicazione e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="9c772-171">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="9c772-172">Per le finalità di questa esercitazione, aggiungere l'account al gruppo _Utenti_.</span><span class="sxs-lookup"><span data-stu-id="9c772-172">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![Selezionare i gruppi dell'utente][directory-20]

1. <span data-ttu-id="9c772-174">Fare clic su **Mostra password** e copiare la password, perché verrà usata quando si accede all'applicazione più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9c772-174">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span>

   ![Mostrare la password][directory-21]

1. <span data-ttu-id="9c772-176">Fare clic su **Crea** per aggiungere il nuovo account utente alla directory.</span><span class="sxs-lookup"><span data-stu-id="9c772-176">Click **Create** to add the new user account to your directory.</span></span>

   ![Creare il nuovo account utente][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="9c772-178">Configurare e compilare l'applicazione Spring Boot</span><span class="sxs-lookup"><span data-stu-id="9c772-178">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="9c772-179">Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.</span><span class="sxs-lookup"><span data-stu-id="9c772-179">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="9c772-180">Passare alla cartella padre per il progetto e aprire il file *pom.xml* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="9c772-180">Navigate to the parent folder for your project, and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="9c772-181">Aggiungere le dipendenze per la sicurezza OAuth2 di Spring, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9c772-181">Add the dependencies for Spring OAuth2 security; for example:</span></span>

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

1. <span data-ttu-id="9c772-182">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="9c772-182">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="9c772-183">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="9c772-183">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="9c772-184">Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9c772-184">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```
   <span data-ttu-id="9c772-185">Dove:</span><span class="sxs-lookup"><span data-stu-id="9c772-185">Where:</span></span>

   | <span data-ttu-id="9c772-186">Parametro</span><span class="sxs-lookup"><span data-stu-id="9c772-186">Parameter</span></span> | <span data-ttu-id="9c772-187">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="9c772-187">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="9c772-188">Contiene il valore di **ID directory** di Active Directory copiato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c772-188">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="9c772-189">Contiene il valore di **ID applicazione** della registrazione dell'app completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c772-189">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="9c772-190">Contiene il **valore** della chiave di registrazione dell'app completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c772-190">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="9c772-191">Contiene un elenco di gruppi di Active Directory da usare per l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="9c772-191">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="9c772-192">Per un elenco completo dei valori disponibili nel file *application.properties*, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c772-192">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="9c772-193">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="9c772-193">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="9c772-194">Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.</span><span class="sxs-lookup"><span data-stu-id="9c772-194">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="9c772-195">Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="9c772-195">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="9c772-196">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="9c772-196">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="9c772-197">Il nome di gruppo specificato nel metodo `@PreAuthorize("hasRole('')")` deve contenere uno dei gruppi specificati nel campo `azure.activedirectory.active-directory-groups` del file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="9c772-197">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   >

   > [!NOTE]
   > 
   > <span data-ttu-id="9c772-198">È possibile specificare impostazioni di autorizzazione diverse per mapping di richieste diversi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9c772-198">You can specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="9c772-199">Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/security*.</span><span class="sxs-lookup"><span data-stu-id="9c772-199">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="9c772-200">Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="9c772-200">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="9c772-201">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="9c772-201">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="9c772-202">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="9c772-202">Build and test your app</span></span>

1. <span data-ttu-id="9c772-203">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.</span><span class="sxs-lookup"><span data-stu-id="9c772-203">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="9c772-204">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9c772-204">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![Compilare l'applicazione][build-application]

1. <span data-ttu-id="9c772-206">Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <http://localhost:8080> in un Web browser. Verranno richiesti un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="9c772-206">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![Accesso all'applicazione][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="9c772-208">È possibile che venga richiesta la modifica della password se questo è il primo accesso per un nuovo account utente.</span><span class="sxs-lookup"><span data-stu-id="9c772-208">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![Modifica della password][update-password]
   > 

1. <span data-ttu-id="9c772-210">Al termine dell'accesso, verrà visualizzato il testo "Hello World" di esempio dal controller.</span><span class="sxs-lookup"><span data-stu-id="9c772-210">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![Accesso riuscito][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="9c772-212">Agli account utente non autorizzati verrà visualizzato il messaggio **HTTP 403** di accesso non autorizzato.</span><span class="sxs-lookup"><span data-stu-id="9c772-212">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="9c772-213">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9c772-213">Next steps</span></span>

<span data-ttu-id="9c772-214">Per altre informazioni sull'uso di Azure Active Directory, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c772-214">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="9c772-215">[Documentazione di Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="9c772-215">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="9c772-216">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c772-216">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="9c772-217">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="9c772-217">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="9c772-218">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="9c772-218">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="9c772-219">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="9c772-219">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="9c772-220">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="9c772-220">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="9c772-221">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="9c772-221">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="9c772-222">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="9c772-222">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="9c772-223">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="9c772-223">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="9c772-224">Per un esempio più dettagliato, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c772-224">For a more-detailed sample, see the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>

<!-- URL List -->

[Documentazione di Azure Active Directory]: /azure/active-directory/
[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure per sviluppatori Java]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
[directory-16]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-16.png
[directory-17]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-17.png
[directory-18]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-18.png
[directory-19]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-19.png
[directory-20]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-20.png
[directory-21]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-21.png
[directory-22]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-22.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
