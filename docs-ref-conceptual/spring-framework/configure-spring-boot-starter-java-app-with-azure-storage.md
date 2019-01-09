---
title: Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Archiviazione di Azure.
services: storage
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: cdd157abdb993517f7c880a7edaff10f0e3d1033
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991585"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="504a5-103">Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="504a5-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="504a5-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="504a5-104">Overview</span></span>

<span data-ttu-id="504a5-105">Questo articolo illustra la creazione di un'applicazione personalizzata con **Spring Initializr**, quindi l'aggiunta dell'utilità di avvio per Archiviazione di Azure e infine l'uso dell'applicazione per caricare un BLOB nell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-105">This article walks you through creating a custom application using the **Spring Initializr**, then adding the Azure storage starter to your application, and then using your application to upload a blob to your Azure storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="504a5-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="504a5-106">Prerequisites</span></span>

<span data-ttu-id="504a5-107">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="504a5-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="504a5-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="504a5-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="504a5-109">[Interfaccia della riga di comando di Azure](http://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="504a5-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="504a5-110">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="504a5-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="504a5-111">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="504a5-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="504a5-112">[Maven](http://maven.apache.org/) di Apache, versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="504a5-112">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="504a5-113">Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="504a5-113">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a><span data-ttu-id="504a5-114">Creare un account di archiviazione e un contenitore BLOB di Azure per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="504a5-114">Create an Azure Storage Account and blob container for your application</span></span>

1. <span data-ttu-id="504a5-115">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="504a5-115">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="504a5-116">Fare clic su **+Crea una risorsa**, quindi su **Archiviazione** e infine su **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="504a5-116">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![Creare un account di archiviazione di Azure][IMG01]

1. <span data-ttu-id="504a5-118">Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="504a5-118">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="504a5-119">Immettere un **nome** univoco, che diventerà parte dell'URI dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-119">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="504a5-120">Se si immette **wingtiptoysstorage** in **Nome**, ad esempio, l'URI sarà *wingtiptoysstorage.core.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="504a5-120">For example: if you entered **wingtiptoysstorage** for the **Name**, the URI would be *wingtiptoysstorage.core.windows.net*.</span></span>
   * <span data-ttu-id="504a5-121">Scegliere **Archivio BLOB** in **Tipologia account**.</span><span class="sxs-lookup"><span data-stu-id="504a5-121">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="504a5-122">Specificare la **località** per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-122">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="504a5-123">Scegliere la **sottoscrizione** da usare per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-123">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="504a5-124">Specificare se creare un nuovo **gruppo di risorse** per l'account di archiviazione o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="504a5-124">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>

   ![Specificare le opzioni per l'account di archiviazione di Azure][IMG02]

1. <span data-ttu-id="504a5-126">Dopo aver specificato le opzioni elencate sopra, fare clic su **Crea** per creare l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-126">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

1. <span data-ttu-id="504a5-127">Al termine della creazione dell'account di archiviazione nel portale di Azure, fare clic su **BLOB** e quindi su **+Contenitore**.</span><span class="sxs-lookup"><span data-stu-id="504a5-127">When the Azure portal has created your storage account, click **Blobs**, then click **+Container**.</span></span>

   ![Creare un contenitore BLOB][IMG03]

1. <span data-ttu-id="504a5-129">Immettere un **nome** per il contenitore BLOB e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="504a5-129">Enter a **Name** for your blob container, and then click **OK**.</span></span>

   ![Specificare le opzioni per il contenitore BLOB][IMG04]

1. <span data-ttu-id="504a5-131">Al termine della creazione, il contenitore BLOB sarà incluso nell'elenco nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-131">The Azure portal will list your blob container after is has been created.</span></span>

   ![Verifica dell'elenco dei contenitori BLOB][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="504a5-133">Creare un'applicazione Spring Boot semplice con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="504a5-133">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="504a5-134">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="504a5-134">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="504a5-135">Specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="504a5-135">Specify the following options:</span></span>

   * <span data-ttu-id="504a5-136">Generare un progetto **Maven** con **Java**.</span><span class="sxs-lookup"><span data-stu-id="504a5-136">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="504a5-137">Specificare **Spring Boot** versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="504a5-137">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="504a5-138">Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-138">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="504a5-139">Aggiungere la dipendenza **Web**.</span><span class="sxs-lookup"><span data-stu-id="504a5-139">Add the **Web** dependency.</span></span>

      ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="504a5-141">Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.wingtiptoys.storage*.</span><span class="sxs-lookup"><span data-stu-id="504a5-141">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.storage*.</span></span>
   >

1. <span data-ttu-id="504a5-142">Dopo aver specificato le opzioni elencate sopra, fare clic su **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="504a5-142">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="504a5-143">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="504a5-143">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring][SI02]

1. <span data-ttu-id="504a5-145">Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="504a5-145">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a><span data-ttu-id="504a5-146">Configurare l'app Spring Boot per l'uso dell'utilità di avvio per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="504a5-146">Configure your Spring Boot app to use the Azure Storage starter</span></span>

1. <span data-ttu-id="504a5-147">Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-147">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\pom.xml`

   <span data-ttu-id="504a5-148">-oppure-</span><span class="sxs-lookup"><span data-stu-id="504a5-148">-or-</span></span>

   `/users/example/home/storage/pom.xml`

1. <span data-ttu-id="504a5-149">Aprire il file *pom.xml* in un editor di testo e aggiungere l'utilità di avvio Spring Cloud per Archiviazione di Azure all'elenco in `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="504a5-149">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Storage starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![Modificare il file pom.xml][SI03]

1. <span data-ttu-id="504a5-151">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="504a5-151">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="504a5-152">Creare un file di credenziali di Azure</span><span class="sxs-lookup"><span data-stu-id="504a5-152">Create an Azure Credential File</span></span>

1. <span data-ttu-id="504a5-153">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="504a5-153">Open a command prompt.</span></span>

1. <span data-ttu-id="504a5-154">Passare alla directory *resources* dell'app Spring Boot, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-154">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   <span data-ttu-id="504a5-155">-oppure-</span><span class="sxs-lookup"><span data-stu-id="504a5-155">-or-</span></span>

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. <span data-ttu-id="504a5-156">Accedere all'account Azure:</span><span class="sxs-lookup"><span data-stu-id="504a5-156">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="504a5-157">Elencare le sottoscrizioni:</span><span class="sxs-lookup"><span data-stu-id="504a5-157">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="504a5-158">Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-158">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the subscription you want to use with Azure; for example:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. <span data-ttu-id="504a5-159">Creare il file di credenziali di Azure:</span><span class="sxs-lookup"><span data-stu-id="504a5-159">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="504a5-160">Questo comando creerà un file *my.azureauth* nella directory *resources* con contenuto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="504a5-160">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a><span data-ttu-id="504a5-161">Configurare l'app Spring Boot per l'uso dell'account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="504a5-161">Configure your Spring Boot app to use your Azure Storage account</span></span>

1. <span data-ttu-id="504a5-162">Individuare *application.properties* nella directory *resources* dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-162">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   <span data-ttu-id="504a5-163">-oppure-</span><span class="sxs-lookup"><span data-stu-id="504a5-163">-or-</span></span>

   `/users/example/home/storage/src/main/resources/application.properties`

2. <span data-ttu-id="504a5-164">Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="504a5-164">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your storage account:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   <span data-ttu-id="504a5-165">Dove:</span><span class="sxs-lookup"><span data-stu-id="504a5-165">Where:</span></span>

   |                   <span data-ttu-id="504a5-166">Campo</span><span class="sxs-lookup"><span data-stu-id="504a5-166">Field</span></span>                   |                                            <span data-ttu-id="504a5-167">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="504a5-167">Description</span></span>                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            <span data-ttu-id="504a5-168">Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-168">Specifies Azure credential file that you created earlier in this tutorial.</span></span>             |
   |    `spring.cloud.azure.resource-group`    |           <span data-ttu-id="504a5-169">Specifica il gruppo di risorse di Azure contenente l'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-169">Specifies the Azure Resource Group that contains your Azure Storage account.</span></span>            |
   |        `spring.cloud.azure.region`        | <span data-ttu-id="504a5-170">Specifica l'area geografica indicata al momento della creazione dell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-170">Specifies the geographical region that you specified when you created your Azure Storage account.</span></span> |
   |   `spring.cloud.azure.storage.account`    |            <span data-ttu-id="504a5-171">Specifica l'account di archiviazione di Azure creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="504a5-171">Specifies Azure Storage account that you created earlier in this tutorial.</span></span>             |


3. <span data-ttu-id="504a5-172">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="504a5-172">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a><span data-ttu-id="504a5-173">Aggiungere codice di esempio per implementare le funzionalità di archiviazione di Azure di base</span><span class="sxs-lookup"><span data-stu-id="504a5-173">Add sample code to implement basic Azure storage functionality</span></span>

<span data-ttu-id="504a5-174">In questa sezione si creano le classi Java necessarie per archiviare un BLOB nell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-174">In this section, you create the necessary Java classes for storing a blob in your Azure storage account.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="504a5-175">Modificare la classe dell'applicazione main</span><span class="sxs-lookup"><span data-stu-id="504a5-175">Modify the main application class</span></span>

1. <span data-ttu-id="504a5-176">Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-176">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   <span data-ttu-id="504a5-177">-oppure-</span><span class="sxs-lookup"><span data-stu-id="504a5-177">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. <span data-ttu-id="504a5-178">Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="504a5-178">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="504a5-179">Salvare e chiudere il file Java dell'applicazione main.</span><span class="sxs-lookup"><span data-stu-id="504a5-179">Save and close the main application Java file.</span></span>

### <a name="add-a-web-controller-class"></a><span data-ttu-id="504a5-180">Aggiungere una classe controller Web</span><span class="sxs-lookup"><span data-stu-id="504a5-180">Add a web controller class</span></span>

1. <span data-ttu-id="504a5-181">Creare un nuovo file Java denominato *WebController.java* nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-181">Create a new Java file named *WebController.java* in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   <span data-ttu-id="504a5-182">-oppure-</span><span class="sxs-lookup"><span data-stu-id="504a5-182">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. <span data-ttu-id="504a5-183">Aprire il file Java del controller Web in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="504a5-183">Open the web controller Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;

   @RestController
   public class WebController {

      @Value("blob://test/myfile.txt")
      private Resource blobFile;

      @GetMapping(value = "/")
      public String readBlobFile() throws IOException {
         return StreamUtils.copyToString(
            this.blobFile.getInputStream(),
            Charset.defaultCharset()) + "\n";
      }

      @PostMapping(value = "/")
      public String writeBlobFile(@RequestBody String data) throws IOException {
         try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
         }
         return "File was updated.\n";
      }
   }
   ```

   <span data-ttu-id="504a5-184">La sintassi `@Value("blob://[container]/[blob]")` definisce rispettivamente i nomi del contenitore e del BLOB in cui si vogliono archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="504a5-184">Where the `@Value("blob://[container]/[blob]")` syntax respectively defines the names of the container and blob where you want to store the data.</span></span>

1. <span data-ttu-id="504a5-185">Salvare e chiudere il file Java del controller Web.</span><span class="sxs-lookup"><span data-stu-id="504a5-185">Save and close the web controller Java file.</span></span>

1. <span data-ttu-id="504a5-186">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-186">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\storage`

   <span data-ttu-id="504a5-187">-oppure-</span><span class="sxs-lookup"><span data-stu-id="504a5-187">-or-</span></span>

   `cd /users/example/home/storage`

1. <span data-ttu-id="504a5-188">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-188">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="504a5-189">Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="504a5-189">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   <span data-ttu-id="504a5-190">a.</span><span class="sxs-lookup"><span data-stu-id="504a5-190">a.</span></span> <span data-ttu-id="504a5-191">Inviare una richiesta POST per aggiornare il contenuto di un file:</span><span class="sxs-lookup"><span data-stu-id="504a5-191">Send a POST request to update a file's contents:</span></span>

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      <span data-ttu-id="504a5-192">Verrà visualizzata una risposta che indica che il file è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="504a5-192">You should see a response that the file was updated.</span></span>

   <span data-ttu-id="504a5-193">b.</span><span class="sxs-lookup"><span data-stu-id="504a5-193">b.</span></span> <span data-ttu-id="504a5-194">Inviare una richiesta GET per verificare il contenuto del file:</span><span class="sxs-lookup"><span data-stu-id="504a5-194">Send a GET request to verify the file's contents:</span></span>

      ```shell
      curl -X GET http://localhost:8080/
      ```

     <span data-ttu-id="504a5-195">Verrà visualizzato il testo "Hello World" che è stato inserito.</span><span class="sxs-lookup"><span data-stu-id="504a5-195">You should see the "Hello World" text that you posted.</span></span>

## <a name="summary"></a><span data-ttu-id="504a5-196">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="504a5-196">Summary</span></span>

<span data-ttu-id="504a5-197">In questa esercitazione si è creata una nuova applicazione Java con **[Spring Initializr]**, si è aggiunta l'utilità di avvio per Archiviazione di Azure all'applicazione e quindi si è configurata l'applicazione per il caricamento di un BLOB nell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-197">In this tutorial, you created a new Java application using the **[Spring Initializr]**, added the Azure storage starter to your application, and then configured your application to upload a blob to your Azure storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="504a5-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="504a5-198">Next steps</span></span>

<span data-ttu-id="504a5-199">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="504a5-199">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="504a5-200">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="504a5-200">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="504a5-201">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="504a5-201">Additional Resources</span></span>

<span data-ttu-id="504a5-202">Per altre informazioni sulle utilità di avvio Spring Boot aggiuntive disponibili per Microsoft Azure, vedere [Spring Boot Starters for Azure (Utilità di avvio Spring Boot per Azure)](spring-boot-starters-for-azure.md).</span><span class="sxs-lookup"><span data-stu-id="504a5-202">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="504a5-203">Per informazioni dettagliate sulle altre API di archiviazione di Azure che è possibile chiamare dalle applicazioni Spring Boot, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="504a5-203">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="504a5-204">Come usare l'archivio BLOB di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="504a5-204">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="504a5-205">Come usare l'archivio code di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="504a5-205">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="504a5-206">Come usare l'archivio tabelle di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="504a5-206">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="504a5-207">Come usare Archiviazione file di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="504a5-207">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
