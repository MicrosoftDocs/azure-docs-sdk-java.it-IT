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
ms.date: 12/19/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: b3d97917deffa75bfab5f0d9ded64affd90139e1
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991395"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="c6f30-103">Esercitazione: Proteggere un'app Web Java con l'utilità di avvio Spring Boot per Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6f30-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="c6f30-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c6f30-104">Overview</span></span>

<span data-ttu-id="c6f30-105">Questo articolo illustra la creazione di un'app Java con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c6f30-105">This article demonstrates creating a Java app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c6f30-106">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="c6f30-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6f30-107">Creare un'applicazione Java con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="c6f30-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="c6f30-108">Configurare Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6f30-108">Configure Azure Active Directory</span></span>
> * <span data-ttu-id="c6f30-109">Proteggere l'applicazione con le classi e le annotazioni Spring Boot</span><span class="sxs-lookup"><span data-stu-id="c6f30-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="c6f30-110">Compilare e testare l'applicazione Java</span><span class="sxs-lookup"><span data-stu-id="c6f30-110">Build and test your Java application</span></span>

<span data-ttu-id="c6f30-111">Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="c6f30-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6f30-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c6f30-112">Prerequisites</span></span>

<span data-ttu-id="c6f30-113">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="c6f30-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="c6f30-114">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="c6f30-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="c6f30-115">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="c6f30-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="c6f30-116">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c6f30-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="c6f30-117">Creare un'app con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="c6f30-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="c6f30-118">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="c6f30-118">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="c6f30-119">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi fare clic sul collegamento **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="c6f30-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Specificare il nome del gruppo e dell'elemento][create-spring-app-01]

1. <span data-ttu-id="c6f30-121">Scorrere fino alla sezione **Core** e selezionare la casella **Security** (Sicurezza), quindi nella sezione **Web** selezionare la casella **Web** e infine scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-121">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**, then scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![Selezionare le utilità di avvio Security (Sicurezza), Web e Azure Active Directory][create-spring-app-02]

1. <span data-ttu-id="c6f30-123">Scorrere fino alla parte superiore o inferiore alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="c6f30-123">Scroll to the top or bottom of the page and click the button to **Generate Project**.</span></span>

   ![Generare il progetto Spring Boot][create-spring-app-03]

1. <span data-ttu-id="c6f30-125">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="c6f30-125">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="c6f30-126">Creare l'istanza di Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6f30-126">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="c6f30-127">Creare l'istanza di Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6f30-127">Create the Active Directory instance</span></span>

1. <span data-ttu-id="c6f30-128">Accedere a <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="c6f30-128">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="c6f30-129">Fare clic su **+Crea una risorsa**, quindi su **Identità** e infine su **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-129">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory**.</span></span>

   ![Creare la nuova istanza di Azure Active Directory][create-directory-01]

1. <span data-ttu-id="c6f30-131">Immettere il **Nome organizzazione** e il **Nome di dominio iniziale**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-131">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="c6f30-132">Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione,</span><span class="sxs-lookup"><span data-stu-id="c6f30-132">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="c6f30-133">ad esempio: `wingtiptoysdirectory.onmicrosoft.com`. Al termine, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-133">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![Specificare i nomi di Azure Active Directory][create-directory-02]

1. <span data-ttu-id="c6f30-135">Selezionare il nome dell'account nella parte superiore destra della barra degli strumenti del portale di Azure e quindi fare clic su **Cambia directory**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-135">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![Selezionare il nome dell'account Azure][create-directory-03]

1. <span data-ttu-id="c6f30-137">Selezionare la nuova istanza di Azure Active Directory nel menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="c6f30-137">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![Scegliere l'istanza di Azure Active Directory][create-directory-04]

1. <span data-ttu-id="c6f30-139">Selezionare **Azure Active Directory** nel menu del portale, fare clic su **Proprietà** e copiare il valore di **ID directory**, perché verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c6f30-139">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![Copiare l'ID di Azure Active Directory][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="c6f30-141">Aggiungere una registrazione per l'app Spring Boot</span><span class="sxs-lookup"><span data-stu-id="c6f30-141">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="c6f30-142">Selezionare **Azure Active Directory** nel menu del portale e fare clic su **Registrazioni per l'app** e quindi su **Registrazione nuova applicazione**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-142">Select **Azure Active Directory** from the portal menu, click **App registrations**, and then click **New application registration**, .</span></span>

   ![Aggiungere una nuova registrazione per l'app][create-app-registration-01]

2. <span data-ttu-id="c6f30-144">Specificare il **nome** dell'applicazione, usare http://localhost:8080 come **URL di accesso** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-144">Specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![Creare una nuova registrazione di app][create-app-registration-02]

4. <span data-ttu-id="c6f30-146">Quando viene visualizzata la pagina per la registrazione dell'app, copiare il valore di **ID applicazione**, perché verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c6f30-146">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="c6f30-147">Fare clic su **Impostazioni** e quindi su **Chiara**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-147">Click **Settings**, and then click **Keys**.</span></span>

   ![Creare le chiavi della registrazione per l'app][create-app-registration-03]

5. <span data-ttu-id="c6f30-149">Aggiungere una **Descrizione**, specificare la **Durata** di una nuova chiave e fare clic su **Salva**. Il valore della chiave verrà inserito automaticamente quando si fa clic sull'icona **Salva** ed è necessario copiare il valore della chiave per configurare il file *application.properties* più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c6f30-149">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="c6f30-150">Non sarà possibile recuperare questo valore in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="c6f30-150">(You will not be able to retrieve this value later.)</span></span>

   ![Specificare i parametri della chiave di registrazione per l'app][create-app-registration-04]

6. <span data-ttu-id="c6f30-152">Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **Autorizzazioni necessarie**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-152">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![Autorizzazioni necessarie per la registrazione per l'app][create-app-registration-05]

7. <span data-ttu-id="c6f30-154">Fare clic su **Microsoft Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-154">Click **Windows Azure Active Directory**.</span></span>

   ![Selezionare Microsoft Azure Active Directory][create-app-registration-06]

8. <span data-ttu-id="c6f30-156">Selezionare le caselle **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente** e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-156">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![Abilitare le autorizzazioni di accesso][create-app-registration-07]

9. <span data-ttu-id="c6f30-158">Nella pagina **Autorizzazioni necessarie** fare clic su **Concedi autorizzazioni** e su **Sì** quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="c6f30-158">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![Concedere le autorizzazioni di accesso][create-app-registration-08]

10. <span data-ttu-id="c6f30-160">Nella pagina principale per la registrazione dell'app fare clic su **Impostazioni** e quindi su **URL di risposta**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-160">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![Modificare gli URL di risposta][create-app-registration-09]

11. <span data-ttu-id="c6f30-162">Immettere "<http://localhost:8080/login/oauth2/code/azure>" come nuovo URL di risposta e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-162">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![Aggiungere un nuovo URL di risposta][create-app-registration-10]

12. <span data-ttu-id="c6f30-164">Nella pagina principale per la registrazione dell'app fare clic su **Manifesto**, quindi impostare il valore del parametro `oauth2AllowImplicitFlow` su `true` e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-164">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![Configurare il manifesto dell'app][create-app-registration-11]

    > [!NOTE]
    > 
    > <span data-ttu-id="c6f30-166">Per altre informazioni sul parametro `oauth2AllowImplicitFlow` e altre impostazioni dell'applicazione, vedere [Manifesto dell'applicazione Azure Active Directory][AAD app manifest].</span><span class="sxs-lookup"><span data-stu-id="c6f30-166">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="c6f30-167">Aggiungere un account utente alla directory e quindi aggiungere tale account al gruppo</span><span class="sxs-lookup"><span data-stu-id="c6f30-167">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="c6f30-168">Nella pagina **Panoramica** di Active Directory fare clic su **Tutti gli utenti** e quindi su **Nuovo utente**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-168">From the **Overview** page of your Active Directory, click **All Users**, and then click **New user**.</span></span>

   ![Aggiungere un nuovo account utente][create-user-01]

1. <span data-ttu-id="c6f30-170">Quando viene visualizzato il pannello **Utente**, immettere il **Nome** e il **Nome utente**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-170">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![Immettere le informazioni sull'account utente][create-user-02]

   > [!NOTE]
   > 
   > <span data-ttu-id="c6f30-172">È necessario specificare l'URL della directory salvato in precedenza in questa esercitazione quando si immette il nome utente. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c6f30-172">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="c6f30-173">Fare clic su **Gruppi**, quindi selezionare i gruppi che verranno usati per l'autorizzazione nell'applicazione e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="c6f30-173">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="c6f30-174">Per le finalità di questa esercitazione, aggiungere l'account al gruppo _Utenti_.</span><span class="sxs-lookup"><span data-stu-id="c6f30-174">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![Selezionare i gruppi dell'utente][create-user-03]

1. <span data-ttu-id="c6f30-176">Fare clic su **Mostra password** e copiare la password, perché verrà usata quando si accede all'applicazione più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c6f30-176">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span> <span data-ttu-id="c6f30-177">Dopo aver copiato la password, fare clic su **Crea** per aggiungere il nuovo account utente alla directory.</span><span class="sxs-lookup"><span data-stu-id="c6f30-177">When you have copied the password, click **Create** to add the new user account to your directory.</span></span>

   ![Mostrare la password][create-user-04]

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="c6f30-179">Configurare e compilare l'app</span><span class="sxs-lookup"><span data-stu-id="c6f30-179">Configure and compile your app</span></span>

1. <span data-ttu-id="c6f30-180">Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.</span><span class="sxs-lookup"><span data-stu-id="c6f30-180">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="c6f30-181">Passare alla cartella padre per il progetto e aprire il file di progetto Maven `pom.xml` in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c6f30-181">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

1. <span data-ttu-id="c6f30-182">Aggiungere le dipendenze per la sicurezza OAuth2 di Spring a `pom.xml`:</span><span class="sxs-lookup"><span data-stu-id="c6f30-182">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

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

1. <span data-ttu-id="c6f30-183">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="c6f30-183">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="c6f30-184">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c6f30-184">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="c6f30-185">Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c6f30-185">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="c6f30-186">Dove:</span><span class="sxs-lookup"><span data-stu-id="c6f30-186">Where:</span></span>

   | <span data-ttu-id="c6f30-187">Parametro</span><span class="sxs-lookup"><span data-stu-id="c6f30-187">Parameter</span></span> | <span data-ttu-id="c6f30-188">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="c6f30-188">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="c6f30-189">Contiene il valore di **ID directory** di Active Directory copiato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c6f30-189">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="c6f30-190">Contiene il valore di **ID applicazione** della registrazione dell'app completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c6f30-190">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="c6f30-191">Contiene il **valore** della chiave di registrazione dell'app completata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c6f30-191">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="c6f30-192">Contiene un elenco di gruppi di Active Directory da usare per l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c6f30-192">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="c6f30-193">Per un elenco completo dei valori disponibili nel file *application.properties*, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c6f30-193">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="c6f30-194">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="c6f30-194">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="c6f30-195">Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.</span><span class="sxs-lookup"><span data-stu-id="c6f30-195">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="c6f30-196">Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c6f30-196">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="c6f30-197">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="c6f30-197">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="c6f30-198">Il nome di gruppo specificato nel metodo `@PreAuthorize("hasRole('')")` deve contenere uno dei gruppi specificati nel campo `azure.activedirectory.active-directory-groups` del file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="c6f30-198">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   > 
   > <span data-ttu-id="c6f30-199">È anche possibile specificare impostazioni di autorizzazione diverse per mapping di richieste diversi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c6f30-199">You can also specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="c6f30-200">Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/security*.</span><span class="sxs-lookup"><span data-stu-id="c6f30-200">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="c6f30-201">Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c6f30-201">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="c6f30-202">Immettere il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="c6f30-202">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="c6f30-203">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="c6f30-203">Build and test your app</span></span>

1. <span data-ttu-id="c6f30-204">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.</span><span class="sxs-lookup"><span data-stu-id="c6f30-204">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="c6f30-205">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c6f30-205">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![Compilare l'applicazione][build-application]

1. <span data-ttu-id="c6f30-207">Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <http://localhost:8080> in un Web browser. Verranno richiesti un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="c6f30-207">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![Accesso all'applicazione][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="c6f30-209">È possibile che venga richiesta la modifica della password se questo è il primo accesso per un nuovo account utente.</span><span class="sxs-lookup"><span data-stu-id="c6f30-209">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![Modifica della password][update-password]
   > 

1. <span data-ttu-id="c6f30-211">Al termine dell'accesso, verrà visualizzato il testo "Hello World" di esempio dal controller.</span><span class="sxs-lookup"><span data-stu-id="c6f30-211">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![Accesso riuscito][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="c6f30-213">Agli account utente non autorizzati verrà visualizzato il messaggio **HTTP 403** di accesso non autorizzato.</span><span class="sxs-lookup"><span data-stu-id="c6f30-213">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="summary"></a><span data-ttu-id="c6f30-214">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c6f30-214">Summary</span></span>

<span data-ttu-id="c6f30-215">In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory, si è configurato un nuovo tenant di Azure AD registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.</span><span class="sxs-lookup"><span data-stu-id="c6f30-215">In this tutorial, you created a new Java web application using the Azure Active Directory starter, configured a new Azure AD tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6f30-216">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c6f30-216">Next steps</span></span>

<span data-ttu-id="c6f30-217">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="c6f30-217">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c6f30-218">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="c6f30-218">Spring on Azure</span></span>](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
