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
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB

## <a name="overview"></a>Panoramica

Azure Cosmos DB è un servizio di database distribuito a livello globale che consente agli sviluppatori di usare i dati usando un'ampia gamma di API standard, come SQL, MongoDB, Graph e Table. Microsoft Spring Boot Starter consente agli sviluppatori di usare applicazioni Spring Boot che si integrano facilmente con Azure Cosmos DB usando l'API SQL.

Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure, l'uso di **[Spring Initializr]** per creare un'applicazione Spring Boot personalizzata e quindi l'aggiunta delle funzionalità di [Spring Boot Cosmos DB Starter per Azure] all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB usando Spring Data e l'API SQL di Cosmos DB.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Creare un Azure Cosmos DB usando il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Crea una risorsa**.

   ![Portale di Azure][AZ01]

1. Fare clic su **Database** e quindi su **Azure Cosmos DB**.

   ![Portale di Azure][AZ02]

1. Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:

   * Selezionare la **sottoscrizione** da usare per il database.
   * Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.
   * Immettere un **nome account** univoco, che verrà usato come URI per il database, ad esempio *wingtiptoysdata*.
   * Scegliere **Core (SQL)** come API.
   * Specificare il **percorso** per il database.

   Dopo aver specificato queste opzioni, fare clic su **Rivedi e crea** per creare il database.

   ![Portale di Azure][AZ03]

1. Al termine della creazione, il database viene elencato nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**. È possibile fare clic sul database in una di queste posizioni per aprire la pagina delle proprietà per la cache.

   ![Portale di Azure][AZ04]

1. Quando viene visualizzata la pagina delle proprietà per il database, fare clic su **Chiavi** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati nell'applicazione Spring Boot.

   ![Portale di Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, specificare la versione di **Spring Boot**, immettere i nomi di **Gruppo** (Gruppo) e **Artifact** (Artefatto) per l'applicazione, aggiungere **Azure Support** (Supporto di Azure) nelle dipendenze e quindi fare clic sul pulsante **Generate Project** (Genera progetto).

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.example.wintiptoysdata*.
   >

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale ed estrarre i file.

   ![Estrarre il progetto Spring Boot personalizzato][SI02]

1. Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.

   ![File del progetto Spring Boot personalizzato][SI03]

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>Configurare l'applicazione Spring Boot per l'uso di Azure Spring Boot Starter

1. Individuare il file *pom.xml* nella directory dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -oppure-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![Individuare il file pom.xml][PM01]

1. Aprire il file *pom.xml* in un editor di testo e aggiungere le righe seguenti all'elenco di `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
   </dependency>
   ```

   ![Modifica del file pom.xml][PM02]

1. Verificare che la versione di Spring Boot sia la versione scelta al momento della creazione dell'applicazione con Spring Initializr, ad esempio:

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. Verificare di usare la versione più recente di [Azure Spring Boot Starter](https://github.com/microsoft/azure-spring-boot), ad esempio:

   ```xml
   <azure.version>2.1.6</azure.version>
   ```

1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>Configurare l'applicazione Spring Boot per l'uso di Azure Cosmos DB

1. Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![Individuare il file application.properties][RE01]

1. Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.cosmosdb.database=wingtiptoysdata
   ```

   ![Modifica del file application.properties][RE02]

1. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Aggiungere il codice di esempio per implementare le funzionalità di base del database

In questa sezione si creano due classi Java per l'archiviazione dei dati utente, quindi si modifica la classe dell'applicazione main per creare un'istanza della classe *User* e salvarla nel database.

### <a name="define-a-base-class-for-storing-user-data"></a>Definire una classe di base per l'archiviazione dei dati utente

1. Creare un nuovo file denominato *User.java* nella stessa directory del file dell'applicazione Java main.

1. Aprire il file *User.java* in un editor di testo e aggiungere le righe seguenti al file per definire una classe User generica che consenta di archiviare e recuperare i valori nel database:

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

1. Salvare e chiudere il file *User.java*.

### <a name="define-a-data-repository-interface"></a>Definire un'interfaccia del repository di dati

1. Creare un nuovo file denominato *UserRepository.java* nella stessa directory del file dell'applicazione Java main.

1. Aprire il file *UserRepository.java* in un editor di testo e aggiungere le righe seguenti al file per definire l'interfaccia utente del repository che estende l'interfaccia del repository predefinita di DocumentDB:

   ```java
   package com.example.wingtiptoysdata;

   import com.microsoft.azure.spring.data.cosmosdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { }
   ```

1. Salvare e chiudere il file *UserRepository.java*.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![Individuare il file Java dell'applicazione][JV01]

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

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

1. Salvare e chiudere il file Java dell'applicazione main.

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:

   `cd C:\SpringBoot\wingtiptoysdata`

   -oppure-

   `cd /users/example/home/wingtiptoysdata`

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvnw clean spring-boot:run
   ```

1. L'applicazione visualizzerà diversi messaggi di runtime e un messaggio simile agli esempi seguenti per indicare che i valori sono stati archiviati e recuperati dal database correttamente.

   ```shell
   Saved user is: Optional[User: 24093cb5-55fe-4d2c-b459-cb8bafdd39fe John Doe]
   ```

   ![Output positivo dall'applicazione][JV02]

1. FACOLTATIVO: è possibile usare il portale di Azure per visualizzare il contenuto di Azure Cosmos DB nella pagina delle proprietà del database facendo clic su **Esplora dati** e quindi selezionando un elemento nell'elenco disponibile per visualizzare il contenuto.

   ![Utilizzo di Esplora documenti per visualizzare i dati][JV03]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:

* [Documentazione di Azure Cosmos DB].

* [Azure Cosmos DB: creare un database di documenti con Java e il portale di Azure][Build a SQL API app with Java]

* [Spring Data per l'API SQL Azure Cosmos DB]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Spring Boot Cosmos DB Starter per Azure]

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure per sviluppatori Java]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Cosmos DB Starter per Azure]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: https://azure.microsoft.com/services/devops/java/
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
