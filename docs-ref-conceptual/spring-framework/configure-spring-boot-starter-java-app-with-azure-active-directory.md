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
ms.openlocfilehash: da44a40b7b52e75bb0a946b46ddfc033bfef54e9
ms.sourcegitcommit: 473c3aec55f3e9b131dc87c62e2eac218ce9564e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51571718"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="e1154-103">Esercitazione: Proteggere un'app Web Java con l'utilità di avvio Spring Boot per Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1154-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="e1154-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e1154-104">Overview</span></span>

<span data-ttu-id="e1154-105">Questo articolo illustra la creazione di un'app con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e1154-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e1154-106">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="e1154-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e1154-107">Creare un'applicazione Java con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="e1154-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="e1154-108">Configurare Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1154-108">Configure Azure Active Directory</span></span>
> * <span data-ttu-id="e1154-109">Proteggere l'applicazione con le classi e le annotazioni Spring Boot</span><span class="sxs-lookup"><span data-stu-id="e1154-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="e1154-110">Compilare e testare l'applicazione Java</span><span class="sxs-lookup"><span data-stu-id="e1154-110">Build and test your Java application</span></span>

<span data-ttu-id="e1154-111">Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="e1154-111">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1154-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e1154-112">Prerequisites</span></span>

<span data-ttu-id="e1154-113">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="e1154-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="e1154-114">[Java Development Kit (JDK)](https://aka.ms/azure-jdks) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="e1154-114">A [Java Development Kit (JDK)](https://aka.ms/azure-jdks), version 1.7 or later.</span></span>
* <span data-ttu-id="e1154-115">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="e1154-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-application-using-the-spring-initializr"></a><span data-ttu-id="e1154-116">Creare un'applicazione con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="e1154-116">Create an application using the Spring Initializr</span></span>

1. <span data-ttu-id="e1154-117">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="e1154-117">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="e1154-118">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi fare clic sul collegamento **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="e1154-118">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Specificare il nome del gruppo e dell'elemento][security-01]

1. <span data-ttu-id="e1154-120">Scorrere verso il basso fino alla sezione **Core**, selezionare la casella **Security** (Sicurezza) e nella sezione **Web** selezionare la casella **Web**.</span><span class="sxs-lookup"><span data-stu-id="e1154-120">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![Selezionare le utilità di avvio Security (Sicurezza) e Web][security-02]

1. <span data-ttu-id="e1154-122">Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e1154-122">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![Selezionare l'utilità di avvio Azure Active Directory][security-03]

1. <span data-ttu-id="e1154-124">Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="e1154-124">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Generare il progetto Spring Boot][security-04]

1. <span data-ttu-id="e1154-126">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1154-126">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="e1154-127">Creare e configurare una nuova istanza di Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1154-127">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="e1154-128">Creare l'istanza di Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1154-128">Create the Active Directory instance</span></span>

1. <span data-ttu-id="e1154-129">Accedere a <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="e1154-129">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="e1154-130">Fare clic su **+Nuovo**, quindi su **Sicurezza e identità** e infine su **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e1154-130">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![Creare la nuova istanza di Azure Active Directory][directory-01]

1. <span data-ttu-id="e1154-132">Immettere il **Nome organizzazione** e il **Nome di dominio iniziale**.</span><span class="sxs-lookup"><span data-stu-id="e1154-132">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="e1154-133">Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione,</span><span class="sxs-lookup"><span data-stu-id="e1154-133">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="e1154-134">ad esempio: `wingtiptoysdirectory.onmicrosoft.com`. Al termine, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1154-134">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![Specificare i nomi di Azure Active Directory][directory-02]

1. <span data-ttu-id="e1154-136">Scegliere la nuova istanza di Azure Active Directory dal menu a discesa della barra degli strumenti superiore del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e1154-136">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![Scegliere l'istanza di Azure Active Directory][directory-03]

1. <span data-ttu-id="e1154-138">Selezionare **Azure Active Directory** nel menu del portale, fare clic su **Proprietà** e copiare il valore di **ID directory**, perché verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e1154-138">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![Copiare l'ID di Azure Active Directory][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="e1154-140">Aggiungere una registrazione per l'app Spring Boot</span><span class="sxs-lookup"><span data-stu-id="e1154-140">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="e1154-141">Scegliere **Azure Active Directory** dal menu del portale, fare clic su **Panoramica** e quindi fare cli su **Registrazioni per l'app**.</span><span class="sxs-lookup"><span data-stu-id="e1154-141">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![Aggiungere una nuova registrazione per l'app][directory-04]

2. <span data-ttu-id="e1154-143">Fare clic su **Registrazione nuova applicazione**, specificare il **nome** dell'applicazione, usare http://localhost:8080 come **URL di accesso** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1154-143">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![Creare una nuova registrazione di app][directory-05]

3. <span data-ttu-id="e1154-145">Dopo che è stata creata, fare clic su della registrazione per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e1154-145">Click your application registration after it has been created.</span></span>

   ![Selezionare la registrazione per l'app][directory-06]

4. <span data-ttu-id="e1154-147">Quando viene visualizzata la pagina per la registrazione dell'app, copiare il valore di **ID applicazione**, perché verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e1154-147">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="e1154-148">Fare clic su **Impostazioni** e quindi su **Chiara**.</span><span class="sxs-lookup"><span data-stu-id="e1154-148">Click **Settings**, and then click **Keys**.</span></span>

   ![Creare le chiavi della registrazione per l'app][directory-07]

5. <span data-ttu-id="e1154-150">Aggiungere una **Descrizione**, specificare la **Durata** di una nuova chiave e fare clic su **Salva**. Il valore della chiave verrà inserito automaticamente quando si fa clic sull'icona **Salva** ed è necessario copiare il valore della chiave per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e1154-150">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="e1154-151">Non sarà possibile recuperare questo valore in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="e1154-151">(You will not be able to retrieve this value later.)</span></span>

   ![Specificare i parametri della chiave di registrazione per l'app][directory-08]

6. <span data-ttu-id="e1154-153">Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **Autorizzazioni necessarie**.</span><span class="sxs-lookup"><span data-stu-id="e1154-153">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![Autorizzazioni necessarie per la registrazione per l'app][directory-09]

7. <span data-ttu-id="e1154-155">Fare clic su **Microsoft Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e1154-155">Click **Windows Azure Active Directory**.</span></span>

   ![Selezionare Microsoft Azure Active Directory][directory-10]

8. <span data-ttu-id="e1154-157">Selezionare le caselle **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente** e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="e1154-157">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![Abilitare le autorizzazioni di accesso][directory-11]

9. <span data-ttu-id="e1154-159">Nella pagina **Autorizzazioni necessarie** fare clic su **Concedi autorizzazioni** e su **Sì** quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="e1154-159">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![Concedere le autorizzazioni di accesso][directory-12]

10. <span data-ttu-id="e1154-161">Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **URL di risposta**.</span><span class="sxs-lookup"><span data-stu-id="e1154-161">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![Modificare gli URL di risposta][directory-14]

11. <span data-ttu-id="e1154-163">Immettere "<http://localhost:8080/login/oauth2/code/azure>" come nuovo URL di risposta e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="e1154-163">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![Aggiungere un nuovo URL di risposta][directory-15]

12. <span data-ttu-id="e1154-165">Nella pagina principale per la registrazione dell'app fare clic su **Manifesto**, quindi impostare il valore del parametro `oauth2AllowImplicitFlow` su `true` e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="e1154-165">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![Configurare il manifesto dell'app][directory-16]

    > [!NOTE]
    > 
    > <span data-ttu-id="e1154-167">Per altre informazioni sul parametro `oauth2AllowImplicitFlow` e altre impostazioni dell'applicazione, vedere [Manifesto dell'applicazione Azure Active Directory][AAD app manifest].</span><span class="sxs-lookup"><span data-stu-id="e1154-167">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="e1154-168">Aggiungere un account utente alla directory e quindi aggiungere tale account al gruppo</span><span class="sxs-lookup"><span data-stu-id="e1154-168">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="e1154-169">Nella pagina **Panoramica** di Active Directory fare clic su **Utenti**.</span><span class="sxs-lookup"><span data-stu-id="e1154-169">From the **Overview** page of your Active Directory, click **Users**.</span></span>

   ![Aprire il pannello Utenti][directory-17]

1. <span data-ttu-id="e1154-171">Quando viene visualizzato il pannello **Utenti**, fare clic su **Nuovo utente**.</span><span class="sxs-lookup"><span data-stu-id="e1154-171">When the **Users** panel is displayed, click **New user**.</span></span>

   ![Aggiungere un nuovo account utente][directory-18]

1. <span data-ttu-id="e1154-173">Quando viene visualizzato il pannello **Utente**, immettere il **Nome** e il **Nome utente**.</span><span class="sxs-lookup"><span data-stu-id="e1154-173">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![Immettere le informazioni sull'account utente][directory-19]

   > [!NOTE]
   > 
   > <span data-ttu-id="e1154-175">È necessario specificare l'URL della directory salvato in precedenza in questa esercitazione quando si immette il nome utente. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e1154-175">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="e1154-176">Fare clic su **Gruppi**, quindi selezionare i gruppi che verranno usati per l'autorizzazione nell'applicazione e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="e1154-176">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="e1154-177">Per le finalità di questa esercitazione, aggiungere l'account al gruppo _Utenti_.</span><span class="sxs-lookup"><span data-stu-id="e1154-177">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![Selezionare i gruppi dell'utente][directory-20]

1. <span data-ttu-id="e1154-179">Fare clic su **Mostra password** e copiare la password, perché verrà usata quando si accede all'applicazione più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e1154-179">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span>

   ![Mostrare la password][directory-21]

1. <span data-ttu-id="e1154-181">Fare clic su **Crea** per aggiungere il nuovo account utente alla directory.</span><span class="sxs-lookup"><span data-stu-id="e1154-181">Click **Create** to add the new user account to your directory.</span></span>

   ![Creare il nuovo account utente][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="e1154-183">Configurare e compilare l'applicazione Spring Boot</span><span class="sxs-lookup"><span data-stu-id="e1154-183">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="e1154-184">Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.</span><span class="sxs-lookup"><span data-stu-id="e1154-184">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="e1154-185">Passare alla cartella padre per il progetto e aprire il file di progetto Maven `pom.xml` in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="e1154-185">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

1. <span data-ttu-id="e1154-186">Aggiungere le dipendenze per la sicurezza OAuth2 di Spring a `pom.xml`:</span><span class="sxs-lookup"><span data-stu-id="e1154-186">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

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

1. <span data-ttu-id="e1154-187">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="e1154-187">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="e1154-188">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="e1154-188">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="e1154-189">Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e1154-189">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="e1154-190">Dove:</span><span class="sxs-lookup"><span data-stu-id="e1154-190">Where:</span></span>

   | <span data-ttu-id="e1154-191">Parametro</span><span class="sxs-lookup"><span data-stu-id="e1154-191">Parameter</span></span> | <span data-ttu-id="e1154-192">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="e1154-192">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="e1154-193">Contiene il valore di **ID directory** di Active Directory copiato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e1154-193">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="e1154-194">Contiene il valore di **ID applicazione** della registrazione dell'app completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e1154-194">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="e1154-195">Contiene il **valore** della chiave di registrazione dell'app completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e1154-195">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="e1154-196">Contiene un elenco di gruppi di Active Directory da usare per l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="e1154-196">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="e1154-197">Per un elenco completo dei valori disponibili nel file *application.properties*, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.</span><span class="sxs-lookup"><span data-stu-id="e1154-197">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="e1154-198">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="e1154-198">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="e1154-199">Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.</span><span class="sxs-lookup"><span data-stu-id="e1154-199">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="e1154-200">Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="e1154-200">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="e1154-201">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="e1154-201">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="e1154-202">Il nome di gruppo specificato nel metodo `@PreAuthorize("hasRole('')")` deve contenere uno dei gruppi specificati nel campo `azure.activedirectory.active-directory-groups` del file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="e1154-202">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   >

   > [!NOTE]
   > 
   > <span data-ttu-id="e1154-203">È possibile specificare impostazioni di autorizzazione diverse per mapping di richieste diversi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e1154-203">You can specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="e1154-204">Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/security*.</span><span class="sxs-lookup"><span data-stu-id="e1154-204">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="e1154-205">Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="e1154-205">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="e1154-206">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="e1154-206">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="e1154-207">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="e1154-207">Build and test your app</span></span>

1. <span data-ttu-id="e1154-208">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1154-208">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="e1154-209">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e1154-209">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![Compilare l'applicazione][build-application]

1. <span data-ttu-id="e1154-211">Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <http://localhost:8080> in un Web browser. Verranno richiesti un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="e1154-211">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![Accesso all'applicazione][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="e1154-213">È possibile che venga richiesta la modifica della password se questo è il primo accesso per un nuovo account utente.</span><span class="sxs-lookup"><span data-stu-id="e1154-213">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![Modifica della password][update-password]
   > 

1. <span data-ttu-id="e1154-215">Al termine dell'accesso, verrà visualizzato il testo "Hello World" di esempio dal controller.</span><span class="sxs-lookup"><span data-stu-id="e1154-215">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![Accesso riuscito][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="e1154-217">Agli account utente non autorizzati verrà visualizzato il messaggio **HTTP 403** di accesso non autorizzato.</span><span class="sxs-lookup"><span data-stu-id="e1154-217">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="e1154-218">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e1154-218">Next steps</span></span>

<span data-ttu-id="e1154-219">In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory, si è configurato un nuovo tenant di Azure AD registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.</span><span class="sxs-lookup"><span data-stu-id="e1154-219">In this tutorial, you created a new Java web application using the Azure Active Directory starter, configured a new Azure AD tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span> <span data-ttu-id="e1154-220">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="e1154-220">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e1154-221">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="e1154-221">Spring on Azure</span></span>](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
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
