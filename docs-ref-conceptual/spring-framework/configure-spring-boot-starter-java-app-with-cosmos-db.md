---
title: Come usare Spring Boot Starter con un'API DocumentDB per Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializer con l'API DocumentDB per Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 06553920aebb5f27e4d02279e7024d6766e0be94
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="bf589-103">Come usare Spring Boot Starter con l'API DocumentDB per Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bf589-103">How to use the Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="bf589-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="bf589-104">Overview</span></span>

<span data-ttu-id="bf589-105">Azure Cosmos DB è un servizio di database distribuito a livello globale che consente agli sviluppatori di usare i dati usando un'ampia gamma di API standard, come DocumentDB, MongoDB, Graph e Table.</span><span class="sxs-lookup"><span data-stu-id="bf589-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="bf589-106">Microsoft Spring Boot Starter consente agli sviluppatori di usare applicazioni Spring Boot che si integrano facilmente con Azure Cosmos DB usando le API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="bf589-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="bf589-107">Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure, l'uso di **[Spring Initializr]** per creare un'applicazione Java personalizzata e quindi l'aggiunta delle funzionalità di Spring Boot Starter all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB usando l'API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="bf589-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf589-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bf589-108">Prerequisites</span></span>

<span data-ttu-id="bf589-109">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="bf589-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="bf589-110">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="bf589-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="bf589-111">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="bf589-111">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="bf589-112">[Apache Maven](http://maven.apache.org/) versione 3.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="bf589-112">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="bf589-113">Creare un Azure Cosmos DB usando il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="bf589-113">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="bf589-114">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="bf589-114">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Portale di Azure][AZ01]

1. <span data-ttu-id="bf589-116">Fare clic su **Database** e quindi su **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="bf589-116">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Portale di Azure][AZ02]

1. <span data-ttu-id="bf589-118">Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf589-118">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="bf589-119">Immettere un **ID** univoco, che verrà usato come URI per il database,</span><span class="sxs-lookup"><span data-stu-id="bf589-119">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="bf589-120">ad esempio *wingtiptoysdata.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="bf589-120">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="bf589-121">Scegliere **SQL (Document DB)** per l'API.</span><span class="sxs-lookup"><span data-stu-id="bf589-121">Choose **SQL (Document DB)** for the API.</span></span>
   * <span data-ttu-id="bf589-122">Selezionare la **sottoscrizione** da usare per il database.</span><span class="sxs-lookup"><span data-stu-id="bf589-122">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="bf589-123">Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="bf589-123">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="bf589-124">Specificare il **percorso** per il database.</span><span class="sxs-lookup"><span data-stu-id="bf589-124">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="bf589-125">Dopo avere specificato queste opzioni, fare clic su **Crea** per creare il database.</span><span class="sxs-lookup"><span data-stu-id="bf589-125">When you have specified these options, click **Create** to create your database.</span></span>

   ![Portale di Azure][AZ03]

1. <span data-ttu-id="bf589-127">Al termine della creazione, il database viene elencato nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="bf589-127">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="bf589-128">È possibile fare clic sul database in una di queste posizioni per aprire la pagina delle proprietà per la cache.</span><span class="sxs-lookup"><span data-stu-id="bf589-128">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Portale di Azure][AZ04]

1. <span data-ttu-id="bf589-130">Quando viene visualizzata la pagina delle proprietà per il database, fare clic su **Chiavi di accesso** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati all'interno dell'applicazione Spring Boot.</span><span class="sxs-lookup"><span data-stu-id="bf589-130">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Portale di Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="bf589-132">Creare un'applicazione Spring Boot semplice con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="bf589-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="bf589-133">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="bf589-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="bf589-134">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="bf589-134">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the button to **Generate Project**.</span></span>

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="bf589-136">Spring Initializr usa i valori di **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.example.wintiptoys*.</span><span class="sxs-lookup"><span data-stu-id="bf589-136">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="bf589-137">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="bf589-137">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring Boot personalizzato][SI02]

1. <span data-ttu-id="bf589-139">Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="bf589-139">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![File del progetto Spring Boot personalizzato][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="bf589-141">Configurare l'app Spring Boot per usare Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="bf589-141">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="bf589-142">Individuare il file *pom.xml* nella directory dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bf589-142">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="bf589-143">-oppure-</span><span class="sxs-lookup"><span data-stu-id="bf589-143">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![Individuare il file pom.xml][PM01]

1. <span data-ttu-id="bf589-145">Aprire il file *pom.xml* in un editor di testo e aggiungere le righe seguenti all'elenco di `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="bf589-145">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Modifica del file pom.xml][PM02]

1. <span data-ttu-id="bf589-147">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="bf589-147">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="bf589-148">Configurare l'app Spring Boot per usare Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bf589-148">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="bf589-149">Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bf589-149">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="bf589-150">-oppure-</span><span class="sxs-lookup"><span data-stu-id="bf589-150">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Individuare il file application.properties][RE01]

1. <span data-ttu-id="bf589-152">Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:</span><span class="sxs-lookup"><span data-stu-id="bf589-152">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Modifica del file application.properties][RE02]

1. <span data-ttu-id="bf589-154">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="bf589-154">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="bf589-155">Aggiungere il codice di esempio per implementare le funzionalità di base del database</span><span class="sxs-lookup"><span data-stu-id="bf589-155">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="bf589-156">In questa sezione si creano due classi Java per l'archiviazione dei dati utente e si modifica la classe principale dell'applicazione per creare un'istanza della classe utente e salvarla nel database.</span><span class="sxs-lookup"><span data-stu-id="bf589-156">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="bf589-157">Definire una classe di base per l'archiviazione dei dati utente</span><span class="sxs-lookup"><span data-stu-id="bf589-157">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="bf589-158">Creare un nuovo file denominato *User.java* nella stessa directory del file dell'applicazione Java main.</span><span class="sxs-lookup"><span data-stu-id="bf589-158">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="bf589-159">Aprire il file *User.java* in un editor di testo e aggiungere le righe seguenti al file per definire una classe User generica che consenta di archiviare e recuperare i valori nel database:</span><span class="sxs-lookup"><span data-stu-id="bf589-159">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="bf589-160">Salvare e chiudere il file *User.java*.</span><span class="sxs-lookup"><span data-stu-id="bf589-160">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="bf589-161">Definire un'interfaccia del repository di dati</span><span class="sxs-lookup"><span data-stu-id="bf589-161">Define a data repository interface</span></span>

1. <span data-ttu-id="bf589-162">Creare un nuovo file denominato *UserRepository.java* nella stessa directory del file dell'applicazione Java main.</span><span class="sxs-lookup"><span data-stu-id="bf589-162">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="bf589-163">Aprire il file *UserRepository.java* in un editor di testo e aggiungere le righe seguenti al file per definire l'interfaccia utente del repository che estende l'interfaccia del repository predefinita di DocumentDB:</span><span class="sxs-lookup"><span data-stu-id="bf589-163">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="bf589-164">Salvare e chiudere il file *UserRepository.java*.</span><span class="sxs-lookup"><span data-stu-id="bf589-164">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="bf589-165">Modificare la classe dell'applicazione main</span><span class="sxs-lookup"><span data-stu-id="bf589-165">Modify the main application class</span></span>

1. <span data-ttu-id="bf589-166">Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bf589-166">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="bf589-167">-oppure-</span><span class="sxs-lookup"><span data-stu-id="bf589-167">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Individuare il file Java dell'applicazione][JV01]

1. <span data-ttu-id="bf589-169">Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="bf589-169">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. <span data-ttu-id="bf589-170">Salvare e chiudere il file Java dell'applicazione main.</span><span class="sxs-lookup"><span data-stu-id="bf589-170">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="bf589-171">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="bf589-171">Build and test your app</span></span>

1. <span data-ttu-id="bf589-172">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bf589-172">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="bf589-173">-oppure-</span><span class="sxs-lookup"><span data-stu-id="bf589-173">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="bf589-174">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bf589-174">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="bf589-175">Verranno visualizzati diversi messaggi di runtime e verrà mostrato il messaggio `User: testFirstName testLastName` per indicare che i valori archiviati e recuperati correttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="bf589-175">Your application will display several runtime messages, and you should see the message `User: testFirstName testLastName` displayed to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ![Output positivo dall'applicazione][JV02]

1. <span data-ttu-id="bf589-177">FACOLTATIVO: è possibile usare il portale di Azure per visualizzare il contenuto di Azure Cosmos DB nella pagina delle proprietà del database facendo clic su **Esplora documenti** e quindi selezionando l'elemento nell'elenco per visualizzare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="bf589-177">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Document Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![Utilizzo di Esplora documenti per visualizzare i dati][JV03]

## <a name="next-steps"></a><span data-ttu-id="bf589-179">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bf589-179">Next steps</span></span>

<span data-ttu-id="bf589-180">Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf589-180">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="bf589-181">[Documentazione di Azure Cosmos DB].</span><span class="sxs-lookup"><span data-stu-id="bf589-181">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="bf589-182">[Azure Cosmos DB: Creare un'app per le API DocumentDB con Java e il portale di Azure][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="bf589-182">[Azure Cosmos DB: Build a DocumentDB API app with Java and the Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="bf589-183">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf589-183">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="bf589-184">Spring Boot DocumenDB Starter for Azure</span><span class="sxs-lookup"><span data-stu-id="bf589-184">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="bf589-185">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="bf589-185">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="bf589-186">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="bf589-186">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="bf589-187">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].</span><span class="sxs-lookup"><span data-stu-id="bf589-187">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="bf589-188">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="bf589-188">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="bf589-189">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="bf589-189">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="bf589-190">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="bf589-190">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="bf589-191">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="bf589-191">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV03.png
