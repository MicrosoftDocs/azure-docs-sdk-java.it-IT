---
title: Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Archiviazione di Azure.
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 1a219a066f0f89adbf3f541856b36b842520bfbb
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46505919"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="28724-103">Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="28724-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="28724-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="28724-104">Overview</span></span>

<span data-ttu-id="28724-105">Questo articolo illustra la creazione di un'applicazione personalizzata con **Spring Initializr**, quindi l'aggiunta dell'utilità di avvio per Archiviazione di Azure e infine l'uso dell'applicazione per caricare un BLOB nell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="28724-105">This article walks you through creating a custom application using the **Spring Initializr**, then adding the Azure storage starter to your application, and then using your application to upload a blob to your Azure storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28724-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="28724-106">Prerequisites</span></span>

<span data-ttu-id="28724-107">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="28724-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="28724-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28724-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="28724-109">[Interfaccia della riga di comando di Azure](http://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="28724-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="28724-110">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) aggiornato, versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="28724-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="28724-111">[Maven](http://maven.apache.org/) di Apache, versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="28724-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="28724-112">Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="28724-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a><span data-ttu-id="28724-113">Creare un account di archiviazione e un contenitore BLOB di Azure per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="28724-113">Create an Azure Storage Account and blob container for your application</span></span>

1. <span data-ttu-id="28724-114">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="28724-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="28724-115">Fare clic su **+Crea una risorsa**, quindi su **Archiviazione** e infine su **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="28724-115">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![Creare un account di archiviazione di Azure][IMG01]

1. <span data-ttu-id="28724-117">Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="28724-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="28724-118">Immettere un **nome** univoco, che diventerà parte dell'URI dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="28724-118">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="28724-119">Se si immette **wingtiptoysstorage** in **Nome**, ad esempio, l'URI sarà *wingtiptoysstorage.core.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="28724-119">For example: if you entered **wingtiptoysstorage** for the **Name**, the URI would be *wingtiptoysstorage.core.windows.net*.</span></span>
   * <span data-ttu-id="28724-120">Scegliere **Archivio BLOB** in **Tipologia account**.</span><span class="sxs-lookup"><span data-stu-id="28724-120">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="28724-121">Specificare la **località** per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="28724-121">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="28724-122">Scegliere la **sottoscrizione** da usare per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="28724-122">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="28724-123">Specificare se creare un nuovo **gruppo di risorse** per l'account di archiviazione o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="28724-123">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>
   
   ![Specificare le opzioni per l'account di archiviazione di Azure][IMG02]

1. <span data-ttu-id="28724-125">Dopo aver specificato le opzioni elencate sopra, fare clic su **Crea** per creare l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="28724-125">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

1. <span data-ttu-id="28724-126">Al termine della creazione dell'account di archiviazione nel portale di Azure, fare clic su **BLOB** e quindi su **+Contenitore**.</span><span class="sxs-lookup"><span data-stu-id="28724-126">When the Azure portal has created your storage account, click **Blobs**, then click **+Container**.</span></span>

   ![Creare un contenitore BLOB][IMG03]

1. <span data-ttu-id="28724-128">Immettere un **nome** per il contenitore BLOB e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="28724-128">Enter a **Name** for your blob container, and then click **OK**.</span></span>

   ![Specificare le opzioni per il contenitore BLOB][IMG04]

1. <span data-ttu-id="28724-130">Al termine della creazione, il contenitore BLOB sarà incluso nell'elenco nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="28724-130">The Azure portal will list your blob container after is has been created.</span></span>

   ![Verifica dell'elenco dei contenitori BLOB][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="28724-132">Creare un'applicazione Spring Boot semplice con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="28724-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="28724-133">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="28724-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="28724-134">Specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="28724-134">Specify the following options:</span></span>

   * <span data-ttu-id="28724-135">Generare un progetto **Maven** con **Java**.</span><span class="sxs-lookup"><span data-stu-id="28724-135">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="28724-136">Specificare **Spring Boot** versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="28724-136">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="28724-137">Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="28724-137">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="28724-138">Aggiungere la dipendenza **Web**.</span><span class="sxs-lookup"><span data-stu-id="28724-138">Add the **Web** dependency.</span></span>

      ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="28724-140">Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.wingtiptoys.storage*.</span><span class="sxs-lookup"><span data-stu-id="28724-140">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.storage*.</span></span>
   >

1. <span data-ttu-id="28724-141">Dopo aver specificato le opzioni elencate sopra, fare clic su **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="28724-141">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="28724-142">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="28724-142">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring][SI02]

1. <span data-ttu-id="28724-144">Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="28724-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a><span data-ttu-id="28724-145">Configurare l'app Spring Boot per l'uso dell'utilità di avvio per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="28724-145">Configure your Spring Boot app to use the Azure Storage starter</span></span>

1. <span data-ttu-id="28724-146">Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-146">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\pom.xml`

   <span data-ttu-id="28724-147">-oppure-</span><span class="sxs-lookup"><span data-stu-id="28724-147">-or-</span></span>

   `/users/example/home/storage/pom.xml`

1. <span data-ttu-id="28724-148">Aprire il file *pom.xml* in un editor di testo e aggiungere l'utilità di avvio Spring Cloud per Archiviazione di Azure all'elenco in `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="28724-148">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Storage starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![Modificare il file pom.xml][SI03]

1. <span data-ttu-id="28724-150">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="28724-150">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="28724-151">Creare un file di credenziali di Azure</span><span class="sxs-lookup"><span data-stu-id="28724-151">Create an Azure Credential File</span></span>

1. <span data-ttu-id="28724-152">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="28724-152">Open a command prompt.</span></span>

1. <span data-ttu-id="28724-153">Passare alla directory *resources* dell'app Spring Boot, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-153">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   <span data-ttu-id="28724-154">-oppure-</span><span class="sxs-lookup"><span data-stu-id="28724-154">-or-</span></span>

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. <span data-ttu-id="28724-155">Accedere all'account Azure:</span><span class="sxs-lookup"><span data-stu-id="28724-155">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="28724-156">Elencare le sottoscrizioni:</span><span class="sxs-lookup"><span data-stu-id="28724-156">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="28724-157">Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-157">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="28724-158">Creare il file di credenziali di Azure:</span><span class="sxs-lookup"><span data-stu-id="28724-158">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="28724-159">Questo comando creerà un file *my.azureauth* nella directory *resources* con contenuto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="28724-159">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a><span data-ttu-id="28724-160">Configurare l'app Spring Boot per l'uso dell'account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="28724-160">Configure your Spring Boot app to use your Azure Storage account</span></span>

1. <span data-ttu-id="28724-161">Individuare *application.properties* nella directory *resources* dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-161">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   <span data-ttu-id="28724-162">-oppure-</span><span class="sxs-lookup"><span data-stu-id="28724-162">-or-</span></span>

   `/users/example/home/storage/src/main/resources/application.properties`

1.  <span data-ttu-id="28724-163">Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="28724-163">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your storage account:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   <span data-ttu-id="28724-164">Dove:</span><span class="sxs-lookup"><span data-stu-id="28724-164">Where:</span></span>
   | <span data-ttu-id="28724-165">Campo</span><span class="sxs-lookup"><span data-stu-id="28724-165">Field</span></span> | <span data-ttu-id="28724-166">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="28724-166">Description</span></span> |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | <span data-ttu-id="28724-167">Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="28724-167">Specifies Azure credential file that you created earlier in this tutorial.</span></span> |
   | `spring.cloud.azure.resource-group` | <span data-ttu-id="28724-168">Specifica il gruppo di risorse di Azure contenente l'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="28724-168">Specifies the Azure Resource Group that contains your Azure Storage account.</span></span> |
   | `spring.cloud.azure.region` | <span data-ttu-id="28724-169">Specifica l'area geografica indicata al momento della creazione dell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="28724-169">Specifies the geographical region that you specified when you created your Azure Storage account.</span></span> |
   | `spring.cloud.azure.storage.account` | <span data-ttu-id="28724-170">Specifica l'account di archiviazione di Azure creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="28724-170">Specifies Azure Storage account that you created earlier in this tutorial.</span></span>

1. <span data-ttu-id="28724-171">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="28724-171">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a><span data-ttu-id="28724-172">Aggiungere codice di esempio per implementare le funzionalità di archiviazione di Azure di base</span><span class="sxs-lookup"><span data-stu-id="28724-172">Add sample code to implement basic Azure storage functionality</span></span>

<span data-ttu-id="28724-173">In questa sezione si creano le classi Java necessarie per archiviare un BLOB nell'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="28724-173">In this section, you create the necessary Java classes for storing a blob in your Azure storage account.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="28724-174">Modificare la classe dell'applicazione main</span><span class="sxs-lookup"><span data-stu-id="28724-174">Modify the main application class</span></span>

1. <span data-ttu-id="28724-175">Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-175">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   <span data-ttu-id="28724-176">-oppure-</span><span class="sxs-lookup"><span data-stu-id="28724-176">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. <span data-ttu-id="28724-177">Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="28724-177">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

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

1. <span data-ttu-id="28724-178">Salvare e chiudere il file Java dell'applicazione main.</span><span class="sxs-lookup"><span data-stu-id="28724-178">Save and close the main application Java file.</span></span>

### <a name="add-a-web-controller-class"></a><span data-ttu-id="28724-179">Aggiungere una classe controller Web</span><span class="sxs-lookup"><span data-stu-id="28724-179">Add a web controller class</span></span>

1. <span data-ttu-id="28724-180">Creare un nuovo file Java denominato *WebController.java* nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-180">Create a new Java file named *WebController.java* in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   <span data-ttu-id="28724-181">-oppure-</span><span class="sxs-lookup"><span data-stu-id="28724-181">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. <span data-ttu-id="28724-182">Aprire il file Java del controller Web in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="28724-182">Open the web controller Java file in a text editor, and add the following lines to the file:</span></span>

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
   
   <span data-ttu-id="28724-183">La sintassi `@Value("blob://[container]/[blob]")` definisce rispettivamente i nomi del contenitore e del BLOB in cui si vogliono archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="28724-183">Where the `@Value("blob://[container]/[blob]")` syntax respectively defines the names of the container and blob where you want to store the data.</span></span>

1. <span data-ttu-id="28724-184">Salvare e chiudere il file Java del controller Web.</span><span class="sxs-lookup"><span data-stu-id="28724-184">Save and close the web controller Java file.</span></span>

1. <span data-ttu-id="28724-185">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-185">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\storage`

   <span data-ttu-id="28724-186">-oppure-</span><span class="sxs-lookup"><span data-stu-id="28724-186">-or-</span></span>

   `cd /users/example/home/storage`

1. <span data-ttu-id="28724-187">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-187">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="28724-188">Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="28724-188">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   <span data-ttu-id="28724-189">a.</span><span class="sxs-lookup"><span data-stu-id="28724-189">a.</span></span> <span data-ttu-id="28724-190">Inviare una richiesta POST per aggiornare il contenuto di un file:</span><span class="sxs-lookup"><span data-stu-id="28724-190">Send a POST request to update a file's contents:</span></span>

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      <span data-ttu-id="28724-191">Verrà visualizzata una risposta che indica che il file è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="28724-191">You should see a response that the file was updated.</span></span>

   <span data-ttu-id="28724-192">b.</span><span class="sxs-lookup"><span data-stu-id="28724-192">b.</span></span> <span data-ttu-id="28724-193">Inviare una richiesta GET per verificare il contenuto del file:</span><span class="sxs-lookup"><span data-stu-id="28724-193">Send a GET request to verify the file's contents:</span></span>

      ```shell
      curl -X GET http://localhost:8080/
      ```

     <span data-ttu-id="28724-194">Verrà visualizzato il testo "Hello World" che è stato inserito.</span><span class="sxs-lookup"><span data-stu-id="28724-194">You should see the "Hello World" text that you posted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28724-195">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="28724-195">Next steps</span></span>

<span data-ttu-id="28724-196">Per altre informazioni sulle utilità di avvio Spring Boot aggiuntive disponibili per Microsoft Azure, vedere [Spring Boot Starters for Azure (Utilità di avvio Spring Boot per Azure)](spring-boot-starters-for-azure.md).</span><span class="sxs-lookup"><span data-stu-id="28724-196">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="28724-197">Per altre informazioni sull'integrazione delle funzionalità di Azure nelle applicazioni basate su Spring, vedere [Spring Framework in Azure](/java/azure/spring-framework/).</span><span class="sxs-lookup"><span data-stu-id="28724-197">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="28724-198">Per informazioni dettagliate sulle altre API di archiviazione di Azure che è possibile chiamare dalle applicazioni Spring Boot, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="28724-198">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="28724-199">Come usare l'archivio BLOB di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="28724-199">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="28724-200">Come usare l'archivio code di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="28724-200">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="28724-201">Come usare l'archivio tabelle di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="28724-201">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="28724-202">Come usare Archiviazione file di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="28724-202">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
