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
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'applicazione personalizzata con **Spring Initializr**, quindi l'aggiunta dell'utilità di avvio per Archiviazione di Azure e infine l'uso dell'applicazione per caricare un BLOB nell'account di archiviazione di Azure.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/).
* [Interfaccia della riga di comando di Azure](http://docs.microsoft.com/cli/azure/overview).
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Maven](http://maven.apache.org/) di Apache, versione 3.0 o versione successiva.

> [!IMPORTANT]
>
> Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a>Creare un account di archiviazione e un contenitore BLOB di Azure per l'applicazione

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **+Crea una risorsa**, quindi su **Archiviazione** e infine su **Account di archiviazione**.

   ![Creare un account di archiviazione di Azure][IMG01]

1. Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:

   * Immettere un **nome** univoco, che diventerà parte dell'URI dell'account di archiviazione. Se si immette **wingtiptoysstorage** in **Nome**, ad esempio, l'URI sarà *wingtiptoysstorage.core.windows.net*.
   * Scegliere **Archivio BLOB** in **Tipologia account**.
   * Specificare la **località** per l'account di archiviazione.
   * Scegliere la **sottoscrizione** da usare per l'account di archiviazione.
   * Specificare se creare un nuovo **gruppo di risorse** per l'account di archiviazione o sceglierne uno esistente.

   ![Specificare le opzioni per l'account di archiviazione di Azure][IMG02]

1. Dopo aver specificato le opzioni elencate sopra, fare clic su **Crea** per creare l'account di archiviazione.

1. Al termine della creazione dell'account di archiviazione nel portale di Azure, fare clic su **BLOB** e quindi su **+Contenitore**.

   ![Creare un contenitore BLOB][IMG03]

1. Immettere un **nome** per il contenitore BLOB e quindi fare clic su **OK**.

   ![Specificare le opzioni per il contenitore BLOB][IMG04]

1. Al termine della creazione, il contenitore BLOB sarà incluso nell'elenco nel portale di Azure.

   ![Verifica dell'elenco dei contenitori BLOB][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare le opzioni seguenti:

   * Generare un progetto **Maven** con **Java**.
   * Specificare **Spring Boot** versione 2.0 o successiva.
   * Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.
   * Aggiungere la dipendenza **Web**.

      ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.wingtiptoys.storage*.
   >

1. Dopo aver specificato le opzioni elencate sopra, fare clic su **Generate Project** (Genera progetto).

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

   ![Scaricare il progetto Spring][SI02]

1. Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a>Configurare l'app Spring Boot per l'uso dell'utilità di avvio per Archiviazione di Azure

1. Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:

   `C:\SpringBoot\storage\pom.xml`

   -oppure-

   `/users/example/home/storage/pom.xml`

1. Aprire il file *pom.xml* in un editor di testo e aggiungere l'utilità di avvio Spring Cloud per Archiviazione di Azure all'elenco in `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![Modificare il file pom.xml][SI03]

1. Salvare e chiudere il file *pom.xml*.

## <a name="create-an-azure-credential-file"></a>Creare un file di credenziali di Azure

1. Aprire un prompt dei comandi.

1. Passare alla directory *resources* dell'app Spring Boot, ad esempio:

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   -oppure-

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. Accedere all'account Azure:

   ```azurecli
   az login
   ```

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```
   Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:

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

1. Creare il file di credenziali di Azure:

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   Questo comando creerà un file *my.azureauth* nella directory *resources* con contenuto simile all'esempio seguente:

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a>Configurare l'app Spring Boot per l'uso dell'account di archiviazione di Azure

1. Individuare *application.properties* nella directory *resources* dell'app, ad esempio:

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   -oppure-

   `/users/example/home/storage/src/main/resources/application.properties`

2. Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'account di archiviazione:

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   Dove:

   |                   Campo                   |                                            DESCRIZIONE                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.             |
   |    `spring.cloud.azure.resource-group`    |           Specifica il gruppo di risorse di Azure contenente l'account di archiviazione di Azure.            |
   |        `spring.cloud.azure.region`        | Specifica l'area geografica indicata al momento della creazione dell'account di archiviazione di Azure. |
   |   `spring.cloud.azure.storage.account`    |            Specifica l'account di archiviazione di Azure creato in precedenza in questa esercitazione.             |


3. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a>Aggiungere codice di esempio per implementare le funzionalità di archiviazione di Azure di base

In questa sezione si creano le classi Java necessarie per archiviare un BLOB nell'account di archiviazione di Azure.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   -oppure-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

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

1. Salvare e chiudere il file Java dell'applicazione main.

### <a name="add-a-web-controller-class"></a>Aggiungere una classe controller Web

1. Creare un nuovo file Java denominato *WebController.java* nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   -oppure-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. Aprire il file Java del controller Web in un editor di testo e aggiungere le righe seguenti al file:

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

   La sintassi `@Value("blob://[container]/[blob]")` definisce rispettivamente i nomi del contenitore e del BLOB in cui si vogliono archiviare i dati.

1. Salvare e chiudere il file Java del controller Web.

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:

   `cd C:\SpringBoot\storage`

   -oppure-

   `cd /users/example/home/storage`

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*, ad esempio:

   a. Inviare una richiesta POST per aggiornare il contenuto di un file:

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      Verrà visualizzata una risposta che indica che il file è stato aggiornato.

   b. Inviare una richiesta GET per verificare il contenuto del file:

      ```shell
      curl -X GET http://localhost:8080/
      ```

     Verrà visualizzato il testo "Hello World" che è stato inserito.

## <a name="summary"></a>Riepilogo

In questa esercitazione si è creata una nuova applicazione Java con **[Spring Initializr]**, si è aggiunta l'utilità di avvio per Archiviazione di Azure all'applicazione e quindi si è configurata l'applicazione per il caricamento di un BLOB nell'account di archiviazione di Azure.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sulle utilità di avvio Spring Boot aggiuntive disponibili per Microsoft Azure, vedere [Spring Boot Starters for Azure (Utilità di avvio Spring Boot per Azure)](spring-boot-starters-for-azure.md).

Per informazioni dettagliate sulle altre API di archiviazione di Azure che è possibile chiamare dalle applicazioni Spring Boot, vedere gli articoli seguenti:
* [Come usare l'archivio BLOB di Azure da Java](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Come usare l'archivio code di Azure da Java](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Come usare l'archivio tabelle di Azure da Java](/azure/cosmos-db/table-storage-how-to-use-java)
* [Come usare Archiviazione file di Azure da Java](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
