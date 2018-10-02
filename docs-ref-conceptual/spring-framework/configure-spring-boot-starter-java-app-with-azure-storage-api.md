---
title: Come usare Spring Boot Starter con l'API di archiviazione di Azure
description: Informazioni su come configurare un'app Spring Boot Initializer con l'API di archiviazione di Azure.
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
ms.openlocfilehash: 8ee985f28b7fa80548e13681089e0a5a9226851d
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506459"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-storage-api"></a><span data-ttu-id="57961-103">Come usare Spring Boot Starter con l'API di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="57961-103">How to use the Spring Boot Starter with the Azure Storage API</span></span>

## <a name="overview"></a><span data-ttu-id="57961-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="57961-104">Overview</span></span>

<span data-ttu-id="57961-105">Questo articolo illustra come creare un'applicazione personalizzata con **Spring Initializr** e quindi usarla per accedere all'archivio di Azure con l'API di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="57961-105">This article walks you through creating a custom application using the **Spring Initializr**, and then using that application to access Azure storage by using the Azure Storage API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57961-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="57961-106">Prerequisites</span></span>

<span data-ttu-id="57961-107">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="57961-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="57961-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="57961-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="57961-109">[Interfaccia della riga di comando di Azure](http://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="57961-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="57961-110">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) aggiornato, versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="57961-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="57961-111">[Maven](http://maven.apache.org/) di Apache, versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="57961-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="57961-112">Creare un'applicazione personalizzata tramite Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="57961-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="57961-113">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="57961-113">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="57961-114">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi fare clic sul collegamento **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="57961-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Opzioni di base di Spring Initializr](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > <span data-ttu-id="57961-116">Spring Initializr userà i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.contoso.wingtiptoysdemo*.</span><span class="sxs-lookup"><span data-stu-id="57961-116">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.contoso.wingtiptoysdemo*.</span></span>
   >

1. <span data-ttu-id="57961-117">Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Storage** (Archiviazione di Azure).</span><span class="sxs-lookup"><span data-stu-id="57961-117">Scroll down to the **Azure** section and check the box for **Azure Storage**.</span></span>

   ![Opzioni complete di Spring Initializr](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. <span data-ttu-id="57961-119">Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="57961-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Opzioni complete di Spring Initializr](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. <span data-ttu-id="57961-121">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="57961-121">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring Boot personalizzato](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="57961-123">Accedere ad Azure e selezionare la sottoscrizione da usare</span><span class="sxs-lookup"><span data-stu-id="57961-123">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="57961-124">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="57961-124">Open a command prompt.</span></span>

1. <span data-ttu-id="57961-125">Accedere all'account Azure con l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="57961-125">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="57961-126">Seguire le istruzioni per completare il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="57961-126">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="57961-127">Elencare le sottoscrizioni:</span><span class="sxs-lookup"><span data-stu-id="57961-127">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="57961-128">Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-128">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="57961-129">Specificare il GUID per l'account che si vuole usare con Azure, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-129">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="57961-130">Creare un account di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="57961-130">Create an Azure Storage account</span></span>

1. <span data-ttu-id="57961-131">Creare un gruppo di risorse per le risorse di Azure che verranno usate in questo articolo, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-131">Create a resource group for the Azure resources you will use in this article; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="57961-132">Dove:</span><span class="sxs-lookup"><span data-stu-id="57961-132">Where:</span></span>

   | <span data-ttu-id="57961-133">Parametro</span><span class="sxs-lookup"><span data-stu-id="57961-133">Parameter</span></span> | <span data-ttu-id="57961-134">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="57961-134">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="57961-135">Specifica un nome univoco per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="57961-135">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="57961-136">Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="57961-136">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="57961-137">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del gruppo di risorse, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-137">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. <span data-ttu-id="57961-138">Creare un account di archiviazione di Azure nel gruppo di risorse per l'app Spring Boot, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-138">Create an Azure storage account in the in the resource group for your Spring Boot app; for example:</span></span>
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   <span data-ttu-id="57961-139">Dove:</span><span class="sxs-lookup"><span data-stu-id="57961-139">Where:</span></span>

   | <span data-ttu-id="57961-140">Parametro</span><span class="sxs-lookup"><span data-stu-id="57961-140">Parameter</span></span> | <span data-ttu-id="57961-141">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="57961-141">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="57961-142">Specifica un nome univoco per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="57961-142">Specifies a unique name for your storage account.</span></span> |
   | `resource-group` | <span data-ttu-id="57961-143">Specifica il nome del gruppo di risorse creato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="57961-143">Specifies the name of the resource group group you created in the previous step.</span></span> |
   | `location` | <span data-ttu-id="57961-144">Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="57961-144">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your storage account will be hosted.</span></span> |
   | `sku` | <span data-ttu-id="57961-145">Specifica uno dei valori seguenti: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`.</span><span class="sxs-lookup"><span data-stu-id="57961-145">Specifies one of the following: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`.</span></span> |

   <span data-ttu-id="57961-146">Azure restituirà una stringa JSON lunga contenente lo stato del provisioning, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-146">Azure will return a long JSON string which contains the provisioning state; for example: |</span></span>

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "identity": null,
     "kind": "Storage"
       ...
       ... (A long list of values will be displayed here.)
       ...
     "statusOfPrimary": "available",
     "statusOfSecondary": null,
     "tags": {},
     "type": "Microsoft.Storage/storageAccounts"
   }
   ```

3. <span data-ttu-id="57961-147">Recuperare la stringa di connessione per l'account di archiviazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-147">Retrieve the connection string for your storage account; for example:</span></span>
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   <span data-ttu-id="57961-148">Dove:</span><span class="sxs-lookup"><span data-stu-id="57961-148">Where:</span></span>

   | <span data-ttu-id="57961-149">Parametro</span><span class="sxs-lookup"><span data-stu-id="57961-149">Parameter</span></span> | <span data-ttu-id="57961-150">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="57961-150">Description</span></span> |
   | ---|---|
   | `name` | <span data-ttu-id="57961-151">Specifica un nome univoco dell'account di archiviazione che è stato creato nei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="57961-151">Specifies a unique name of the storage account you created in previous steps.</span></span> |
   | `resource-group` | <span data-ttu-id="57961-152">Specifica il nome del gruppo di risorse creato nei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="57961-152">Specifies the name of the resource group you created in previous steps.</span></span> |

   <span data-ttu-id="57961-153">Azure restituirà una stringa JSON contenente la stringa di connessione per l'account di archiviazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-153">Azure will return a JSON string which contains the connection string for your storage account; for example:</span></span>

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="57961-154">Configurare e compilare l'applicazione Spring Boot</span><span class="sxs-lookup"><span data-stu-id="57961-154">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="57961-155">Estrarre i file dell'archivio di progetto scaricato in una directory.</span><span class="sxs-lookup"><span data-stu-id="57961-155">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="57961-156">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="57961-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="57961-157">Aggiungere la chiave per l'account di archiviazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="57961-157">Add the key for your storage account; for example:</span></span>
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. <span data-ttu-id="57961-158">Passare alla cartella */src/main/java/com/example/wingtiptoysdemo* nel progetto e aprire il file *WingtiptoysdemoApplication.java* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="57961-158">Navigate to the */src/main/java/com/example/wingtiptoysdemo* folder in your project and open the *WingtiptoysdemoApplication.java* file in a text editor.</span></span>

1. <span data-ttu-id="57961-159">Sostituire il codice Java esistente con l'esempio seguente che elenca i BLOB in un contenitore:</span><span class="sxs-lookup"><span data-stu-id="57961-159">Replace the existing Java code with the following example that lists the blobs in a container:</span></span>

   ```java
   package com.example.wingtiptoysdemo;

   import com.microsoft.azure.storage.*;
   import com.microsoft.azure.storage.blob.*;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   import java.net.URISyntaxException;

   @SpringBootApplication
   public class WingtiptoysdemoApplication implements CommandLineRunner {

      @Autowired
      private CloudStorageAccount cloudStorageAccount;

      final String containerName = "mycontainer";

      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysdemoApplication.class, args);
      }

      public void run(String... var1)
             throws URISyntaxException, StorageException {
          // Create a container (if it does not exist).
          createContainerIfNotExists(containerName);
          // Upload a blob to the container.
          uploadTextBlob(containerName);
      }

      private void createContainerIfNotExists(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Create the container if it does not exist.
            container.createIfNotExists();
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }

      private void uploadTextBlob(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Get a blob reference for a text file.
            CloudBlockBlob blob = container.getBlockBlobReference("test.txt");
            // Upload some text into the blob.
            blob.uploadText("Hello World!");
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }
   }
   ```
   > [!NOTE]
   >
   > <span data-ttu-id="57961-160">L'esempio precedente trasferisce automaticamente le impostazioni dell'account di archiviazione definite nel file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="57961-160">The above example autowires the storage account settings that you defined in the *application.properties* file.</span></span>
   >

1. <span data-ttu-id="57961-161">Compilare ed eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="57961-161">Compile and run the application:</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

   <span data-ttu-id="57961-162">L'applicazione creerà un contenitore e caricherà nel contenitore un file di testo come BLOB, che verrà elencato sotto l'account di archiviazione nel [portale di Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="57961-162">The application will create a container and upload a text file as a blob to the container, which will be listed under your storage account in the [Azure portal](https://portal.azure.com).</span></span>

   ![Elencare i BLOB nel portale di Azure](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > <span data-ttu-id="57961-164">Durante la compilazione dell'applicazione può essere visualizzato il messaggio di errore seguente:</span><span class="sxs-lookup"><span data-stu-id="57961-164">When you compile your application, you might see the following error message:</span></span>
   > 
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] BUILD FAILURE`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] Total time: 2.616 s`<br/>
   > `[INFO] Finished at: 2017-11-11T13:14:15Z`<br/>
   > `[INFO] Final Memory: 26M/213M`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2`<br/>
   > `.18.1:test (default-test) on project wingtiptoysdemo: Execution default-test of`<br/>
   > `goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test failed: The for`<br/>
   > `ked VM terminated without properly saying goodbye. VM crash or System.exit called?`<br/>
   > `[ERROR] Command was /bin/sh -c cd /home/robert/SpringBoot/wingtiptoysdemo && /u`<br/>
   > `sr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -jar /home/robert/SpringBoot/wingt`<br/>
   > `iptoysdemo/target/surefire/surefirebooter6371623993063346766.jar /home/robert/S`<br/>
   > `pringBoot/wingtiptoysdemo/target/surefire/surefire5107893623933537917tmp /home/`<br/>
   > `robert/SpringBoot/wingtiptoysdemo/target/surefire/surefire_01414159391084128068tmp`<br/>
   > `[ERROR] -> [Help 1]`<br/>
   > 
   > <span data-ttu-id="57961-165">In questo caso è possibile disabilitare il test di Maven Surefire aggiungendo la voce del plug-in seguente nel file *pom.xml*:</span><span class="sxs-lookup"><span data-stu-id="57961-165">If this happens, you might want to disable the Maven Surefire testing; to do so, add the following plugin entry in your *pom.xml* file:</span></span>
   > 
   > ```xml
   > <plugin>
   >   <groupId>org.apache.maven.plugins</groupId>
   >   <artifactId>maven-surefire-plugin</artifactId>
   >   <version>2.20.1</version>
   >   <configuration>
   >     <skipTests>true</skipTests>
   >   </configuration>
   > </plugin>
   > ```
   > 

## <a name="next-steps"></a><span data-ttu-id="57961-166">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="57961-166">Next steps</span></span>

<span data-ttu-id="57961-167">Per altre informazioni sulle utilità di avvio Spring Boot aggiuntive disponibili per Microsoft Azure, vedere [Spring Boot Starters for Azure (Utilità di avvio Spring Boot per Azure)](spring-boot-starters-for-azure.md).</span><span class="sxs-lookup"><span data-stu-id="57961-167">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="57961-168">Per altre informazioni sull'integrazione delle funzionalità di Azure nelle applicazioni basate su Spring, vedere [Spring Framework in Azure](/java/azure/spring-framework/).</span><span class="sxs-lookup"><span data-stu-id="57961-168">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="57961-169">Per informazioni dettagliate sulle altre API di archiviazione di Azure che è possibile chiamare dalle applicazioni Spring Boot, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="57961-169">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="57961-170">Come usare l'archivio BLOB di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="57961-170">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="57961-171">Come usare l'archivio code di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="57961-171">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="57961-172">Come usare l'archivio tabelle di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="57961-172">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="57961-173">Come usare Archiviazione file di Azure da Java</span><span class="sxs-lookup"><span data-stu-id="57961-173">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)
