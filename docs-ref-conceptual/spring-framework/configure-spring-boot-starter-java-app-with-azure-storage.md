---
title: Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Archiviazione di Azure.
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: yungez;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: e10ecfb7f6d705aa3ccffc49d354d1019f7f1a0b
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure

## <a name="overview"></a>Panoramica

Questo articolo illustra come creare un'applicazione personalizzata con **Spring Initializr** e come usarla per accedere ad Archiviazione di Azure.

## <a name="prerequisites"></a>prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/).
* [Interfaccia della riga di comando di Azure](http://docs.microsoft.com/cli/azure/overview).
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) aggiornato, versione 1.7 o successiva.
* [Maven](http://maven.apache.org/) di Apache, versione 3.0 o versione successiva.

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Creare un'applicazione personalizzata tramite Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi di **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul collegamento relativo a **Switch to the full version** (Passa alla versione completa) di Spring Initializr.

   ![Opzioni di base di Spring Initializr](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > Spring Initializr userà i valori di **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.contoso.wingtiptoysdemo*.
   >

1. Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Storage** (Archiviazione di Azure).

   ![Opzioni complete di Spring Initializr](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).

   ![Opzioni complete di Spring Initializr](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

   ![Scaricare il progetto Spring Boot personalizzato](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>Accedere ad Azure e selezionare la sottoscrizione da usare

1. Aprire un prompt dei comandi.

1. Accedere all'account Azure con l'interfaccia della riga di comando di Azure:

   ```azurecli
   az login
   ```
   Seguire le istruzioni per completare il processo di accesso.

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```
   Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:

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

1. Specificare il GUID per l'account che si vuole usare con Azure, ad esempio:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a>Creare un account di Archiviazione di Azure

1. Creare un gruppo di risorse per le risorse di Azure che verranno usate in questo articolo, ad esempio:
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `name` | Specifica un nome univoco per il gruppo di risorse. |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del gruppo di risorse, ad esempio:  

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

1. Creare un account di archiviazione di Azure nel gruppo di risorse per l'app Spring Boot, ad esempio:
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `name` | Specifica un nome univoco per l'account di archiviazione. |
   | `resource-group` | Specifica il nome del gruppo di risorse creato nel passaggio precedente. |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato l'account di archiviazione. |
   | `sku` | Specifica uno dei valori seguenti: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`. |

   Azure restituirà una stringa JSON lunga contenente lo stato del provisioning, ad esempio:
   
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

1. Recuperare la stringa di connessione per l'account di archiviazione, ad esempio:
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   | ---|---|
   | `name` | Specifica un nome univoco dell'account di archiviazione che è stato creato nei passaggi precedenti. |
   | `resource-group` | Specifica il nome del gruppo di risorse creato nei passaggi precedenti. |

   Azure restituirà una stringa JSON contenente la stringa di connessione per l'account di archiviazione, ad esempio:

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a>Configurare e compilare l'applicazione Spring Boot

1. Estrarre i file dell'archivio di progetto scaricato in una directory.

1. Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.

1. Aggiungere la chiave per l'account di archiviazione, ad esempio:
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. Passare alla cartella */src/main/java/com/example/wingtiptoysdemo* nel progetto e aprire il file *WingtiptoysdemoApplication.java* in un editor di testo.

1. Sostituire il codice Java esistente con l'esempio seguente che elenca i BLOB in un contenitore:

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
   > L'esempio precedente trasferisce automaticamente le impostazioni dell'account di archiviazione definite nel file *application.properties*.
   >

1. Compilare ed eseguire l'applicazione:
   ```shell
   mvn clean package spring-boot:run
   ```
   
   L'applicazione creerà un contenitore e caricherà nel contenitore un file di testo come BLOB, che verrà elencato sotto l'account di archiviazione nel [portale di Azure](https://portal.azure.com).

   ![Elencare i BLOB nel portale di Azure](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > Durante la compilazione dell'applicazione può essere visualizzato il messaggio di errore seguente:
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
   > In questo caso è possibile disabilitare il test di Maven Surefire aggiungendo la voce del plug-in seguente nel file *pom.xml*:
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

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle utilità di avvio Spring Boot aggiuntive disponibili per Microsoft Azure, vedere [Spring Boot Starters for Azure (Utilità di avvio Spring Boot per Azure)](spring-boot-starters-for-azure.md).

Per altre informazioni sull'integrazione delle funzionalità di Azure nelle applicazioni basate su Spring, vedere [Spring Framework in Azure](/java/azure/spring-framework/).

Per informazioni dettagliate sulle altre API di archiviazione di Azure che è possibile chiamare dalle applicazioni Spring Boot, vedere gli articoli seguenti:
* [Come usare l'archivio BLOB di Azure da Java](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Come usare l'archivio code di Azure da Java](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Come usare l'archivio tabelle di Azure da Java](/azure/cosmos-db/table-storage-how-to-use-java)
* [Come usare Archiviazione file di Azure da Java](/azure/storage/files/storage-java-how-to-use-file-storage)
