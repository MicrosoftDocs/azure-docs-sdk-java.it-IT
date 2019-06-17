---
title: Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializer con l'API SQL di Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: f00afbdd09ce617f863ed758f4bdddcb40701e27
ms.sourcegitcommit: 5bbf64121a99019207ed8cca29280fc5183c7314
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2019
ms.locfileid: "66840840"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="cf9ff-103">Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cf9ff-103">How to use the Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="cf9ff-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="cf9ff-104">Overview</span></span>

<span data-ttu-id="cf9ff-105">Azure Cosmos DB è un servizio di database distribuito a livello globale che consente agli sviluppatori di usare i dati usando un'ampia gamma di API standard, come SQL, MongoDB, Graph e Table.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as SQL, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="cf9ff-106">Microsoft Spring Boot Starter consente agli sviluppatori di usare applicazioni Spring Boot che si integrano facilmente con Azure Cosmos DB usando l'API SQL.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.</span></span>

<span data-ttu-id="cf9ff-107">Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure, l'uso di **[Spring Initializr]** per creare un'applicazione Spring Boot personalizzata e quindi l'aggiunta delle funzionalità di [Spring Boot Cosmos DB Starter per Azure] all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB usando Spring Data e l'API SQL di Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom Spring Boot application, and then add the [Spring Boot Cosmos DB Starter for Azure] to your custom application to store data in and retrieve data from your Azure Cosmos DB by using Spring Data and the Cosmos DB SQL API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf9ff-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cf9ff-108">Prerequisites</span></span>

<span data-ttu-id="cf9ff-109">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="cf9ff-110">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="cf9ff-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="cf9ff-111">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="cf9ff-112">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="cf9ff-113">Creare un Azure Cosmos DB usando il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="cf9ff-113">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="cf9ff-114">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-114">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![Portale di Azure][AZ01]

1. <span data-ttu-id="cf9ff-116">Fare clic su **Database** e quindi su **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-116">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Portale di Azure][AZ02]

1. <span data-ttu-id="cf9ff-118">Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-118">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="cf9ff-119">Selezionare la **sottoscrizione** da usare per il database.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-119">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="cf9ff-120">Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-120">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="cf9ff-121">Immettere un **nome account** univoco, che verrà usato come URI per il database,</span><span class="sxs-lookup"><span data-stu-id="cf9ff-121">Enter a unique **Account Name**, which you will use as the URI for your database.</span></span> <span data-ttu-id="cf9ff-122">ad esempio *wingtiptoysdata*.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-122">For example: *wingtiptoysdata*.</span></span>
   * <span data-ttu-id="cf9ff-123">Scegliere **Core (SQL)** come API.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-123">Choose **Core (SQL)** for the API.</span></span>
   * <span data-ttu-id="cf9ff-124">Specificare il **percorso** per il database.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-124">Specify the **Location** for your database.</span></span>

   <span data-ttu-id="cf9ff-125">Dopo aver specificato queste opzioni, fare clic su **Rivedi e crea** per creare il database.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-125">When you have specified these options, click **Review + create** to create your database.</span></span>

   ![Portale di Azure][AZ03]

1. <span data-ttu-id="cf9ff-127">Al termine della creazione, il database viene elencato nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-127">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="cf9ff-128">È possibile fare clic sul database in una di queste posizioni per aprire la pagina delle proprietà per la cache.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-128">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Portale di Azure][AZ04]

1. <span data-ttu-id="cf9ff-130">Quando viene visualizzata la pagina delle proprietà per il database, fare clic su **Chiavi** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati nell'applicazione Spring Boot.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-130">When the properties page for your database is displayed, click **Keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Portale di Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="cf9ff-132">Creare un'applicazione Spring Boot semplice con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="cf9ff-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="cf9ff-133">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="cf9ff-134">Specificare che si vuole generare un progetto **Maven** con **Java**, specificare la versione di **Spring Boot**, immettere i nomi di **Gruppo** (Gruppo) e **Artifact** (Artefatto) per l'applicazione, aggiungere **Azure Support** (Supporto di Azure) nelle dipendenze e quindi fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="cf9ff-134">Specify that you want to generate a **Maven Project** with **Java**, specify your **Spring Boot** version, enter the **Group** and **Artifact** names for your application, add **Azure Support** in the dependencies, and then click the button to **Generate Project**.</span></span>

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="cf9ff-136">Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.example.wintiptoysdata*.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-136">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoysdata*.</span></span>
   >

1. <span data-ttu-id="cf9ff-137">Quando richiesto, scaricare il progetto in un percorso nel computer locale ed estrarre i file.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-137">When prompted, download the project to a path on your local computer and extract the files.</span></span>

   ![Estrarre il progetto Spring Boot personalizzato][SI02]

1. <span data-ttu-id="cf9ff-139">Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-139">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![File del progetto Spring Boot personalizzato][SI03]

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="cf9ff-141">Configurare l'applicazione Spring Boot per l'uso di Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="cf9ff-141">Configure your Spring Boot application to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="cf9ff-142">Individuare il file *pom.xml* nella directory dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-142">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="cf9ff-143">-oppure-</span><span class="sxs-lookup"><span data-stu-id="cf9ff-143">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![Individuare il file pom.xml][PM01]

1. <span data-ttu-id="cf9ff-145">Aprire il file *pom.xml* in un editor di testo e aggiungere le righe seguenti all'elenco di `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-145">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
   </dependency>
   ```

   ![Modifica del file pom.xml][PM02]

1. <span data-ttu-id="cf9ff-147">Verificare che la versione di Spring Boot sia la versione scelta al momento della creazione dell'applicazione con Spring Initializr, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-147">Verify that the Spring Boot version is the version that you chose when you created your application with the Spring Initializr; for example:</span></span>

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. <span data-ttu-id="cf9ff-148">Verificare di usare la versione più recente di [Azure Spring Boot Starter](https://github.com/microsoft/azure-spring-boot), ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-148">Verify that you use the most recent [Azure Spring Boot starters](https://github.com/microsoft/azure-spring-boot) version, for example:</span></span>

   ```xml
   <azure.version>2.1.6</azure.version>
   ```

1. <span data-ttu-id="cf9ff-149">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-149">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a><span data-ttu-id="cf9ff-150">Configurare l'applicazione Spring Boot per l'uso di Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cf9ff-150">Configure your Spring Boot application to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="cf9ff-151">Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-151">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   <span data-ttu-id="cf9ff-152">-oppure-</span><span class="sxs-lookup"><span data-stu-id="cf9ff-152">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![Individuare il file application.properties][RE01]

1. <span data-ttu-id="cf9ff-154">Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-154">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.cosmosdb.database=wingtiptoysdata
   ```

   ![Modifica del file application.properties][RE02]

1. <span data-ttu-id="cf9ff-156">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-156">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="cf9ff-157">Aggiungere il codice di esempio per implementare le funzionalità di base del database</span><span class="sxs-lookup"><span data-stu-id="cf9ff-157">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="cf9ff-158">In questa sezione si creano due classi Java per l'archiviazione dei dati utente, quindi si modifica la classe dell'applicazione main per creare un'istanza della classe *User* e salvarla nel database.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-158">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the *User* class and save it to your database.</span></span>

### <a name="define-a-base-class-for-storing-user-data"></a><span data-ttu-id="cf9ff-159">Definire una classe di base per l'archiviazione dei dati utente</span><span class="sxs-lookup"><span data-stu-id="cf9ff-159">Define a base class for storing user data</span></span>

1. <span data-ttu-id="cf9ff-160">Creare un nuovo file denominato *User.java* nella stessa directory del file dell'applicazione Java main.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-160">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="cf9ff-161">Aprire il file *User.java* in un editor di testo e aggiungere le righe seguenti al file per definire una classe User generica che consenta di archiviare e recuperare i valori nel database:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-161">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   // Define a generic User class.
   public class User {
      private String id;
      private String firstName;
      private String lastName;

      public User() {
      }

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
         return String.format("User: %s %s %s", id, firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="cf9ff-162">Salvare e chiudere il file *User.java*.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-162">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="cf9ff-163">Definire un'interfaccia del repository di dati</span><span class="sxs-lookup"><span data-stu-id="cf9ff-163">Define a data repository interface</span></span>

1. <span data-ttu-id="cf9ff-164">Creare un nuovo file denominato *UserRepository.java* nella stessa directory del file dell'applicazione Java main.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-164">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="cf9ff-165">Aprire il file *UserRepository.java* in un editor di testo e aggiungere le righe seguenti al file per definire l'interfaccia utente del repository che estende l'interfaccia del repository predefinita di DocumentDB:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-165">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   import com.microsoft.azure.spring.data.cosmosdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { }
   ```

1. <span data-ttu-id="cf9ff-166">Salvare e chiudere il file *UserRepository.java*.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-166">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="cf9ff-167">Modificare la classe dell'applicazione main</span><span class="sxs-lookup"><span data-stu-id="cf9ff-167">Modify the main application class</span></span>

1. <span data-ttu-id="cf9ff-168">Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-168">Locate the main application Java file in the package directory of your application, for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="cf9ff-169">-oppure-</span><span class="sxs-lookup"><span data-stu-id="cf9ff-169">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![Individuare il file Java dell'applicazione][JV01]

1. <span data-ttu-id="cf9ff-171">Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-171">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
    package com.example.wingtiptoysdata;

    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    import java.util.Optional;
    import java.util.UUID;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private final UserRepository repository;

        public WingtiptoysdataApplication(UserRepository repository) {
            this.repository = repository;
        }

        public static void main(String[] args) {
            // Execute the command line runner.
            SpringApplication.run(WingtiptoysdataApplication.class, args);
            System.exit(0);
        }

        public void run(String... args) throws Exception {
            // Create a unique identifier.
            String uuid = UUID.randomUUID().toString();

            // Create a new User class.
            final User testUser = new User(uuid, "John", "Doe");

            // For this example, remove all of the existing records.
            repository.deleteAll();

            // Save the User class to the Azure database.
            repository.save(testUser);

            // Retrieve the database record for the User class you just saved by ID.
            Optional<User> result = repository.findById(testUser.getId());

            // Display the results of the database record retrieval.
            System.out.println("\nSaved user is: " + result + "\n")
        }
    }
   ```

1. <span data-ttu-id="cf9ff-172">Salvare e chiudere il file Java dell'applicazione main.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-172">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="cf9ff-173">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="cf9ff-173">Build and test your app</span></span>

1. <span data-ttu-id="cf9ff-174">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-174">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="cf9ff-175">-oppure-</span><span class="sxs-lookup"><span data-stu-id="cf9ff-175">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="cf9ff-176">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-176">Build your Spring Boot application with Maven and run it, for example:</span></span>

   ```shell
   mvnw clean spring-boot:run
   ```

1. <span data-ttu-id="cf9ff-177">L'applicazione visualizzerà diversi messaggi di runtime e un messaggio simile agli esempi seguenti per indicare che i valori sono stati archiviati e recuperati dal database correttamente.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-177">Your application will display several runtime messages, and it will display a message like the following examples to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ```shell
   Saved user is: Optional[User: 24093cb5-55fe-4d2c-b459-cb8bafdd39fe John Doe]
   ```

   ![Output positivo dall'applicazione][JV02]

1. <span data-ttu-id="cf9ff-179">FACOLTATIVO: è possibile usare il portale di Azure per visualizzare il contenuto di Azure Cosmos DB nella pagina delle proprietà del database facendo clic su **Esplora dati** e quindi selezionando un elemento nell'elenco disponibile per visualizzare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-179">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Data Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![Utilizzo di Esplora documenti per visualizzare i dati][JV03]

## <a name="next-steps"></a><span data-ttu-id="cf9ff-181">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="cf9ff-181">Next steps</span></span>

<span data-ttu-id="cf9ff-182">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-182">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cf9ff-183">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="cf9ff-183">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="cf9ff-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cf9ff-184">Additional Resources</span></span>

<span data-ttu-id="cf9ff-185">Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-185">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="cf9ff-186">[Documentazione di Azure Cosmos DB].</span><span class="sxs-lookup"><span data-stu-id="cf9ff-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="cf9ff-187">[Azure Cosmos DB: creare un database di documenti con Java e il portale di Azure][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="cf9ff-187">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="cf9ff-188">[Spring Data per l'API SQL Azure Cosmos DB]</span><span class="sxs-lookup"><span data-stu-id="cf9ff-188">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="cf9ff-189">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf9ff-189">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* <span data-ttu-id="cf9ff-190">[Spring Boot Cosmos DB Starter per Azure]</span><span class="sxs-lookup"><span data-stu-id="cf9ff-190">[Spring Boot Cosmos DB Starter for Azure]</span></span>

* [<span data-ttu-id="cf9ff-191">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="cf9ff-191">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="cf9ff-192">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container</span><span class="sxs-lookup"><span data-stu-id="cf9ff-192">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="cf9ff-193">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="cf9ff-193">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="cf9ff-194">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-194">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="cf9ff-195">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-195">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="cf9ff-196">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-196">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="cf9ff-197">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="cf9ff-197">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Azure per sviluppatori Java]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Cosmos DB Starter per Azure]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[Spring Boot Cosmos DB Starter for Azure]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: https://azure.microsoft.com/services/devops/java/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
