---
title: Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializer con l'API SQL di Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.date: 08/20/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 3e7ec1a2f6b15ec9444dc6ee8d8f2d0f779b1f10
ms.sourcegitcommit: e017de4677c5bedd6ef88c8c1b6da279dc973efe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45639794"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB

## <a name="overview"></a>Panoramica

Spring Data Gremlin Starter fornisce il supporto Spring Data per il linguaggio di query Gremlin di Apache, che gli sviluppatori possono usare con qualsiasi archivio dati compatibile con Gremlin.

Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure per l'uso con l'API Gremlin, l'uso di **[Spring Initializr]** per creare un'applicazione Java personalizzata e quindi aggiungere la funzionalità di Spring Data Gremlin Starter all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB con Gremlin.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

> [!IMPORTANT]
>
> Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a>Creare un'istanza di Azure Cosmos DB usando il portale di Azure

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a>Creare il database Azure Cosmos da usare con l'API Gremlin

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Crea una risorsa**.

   ![Creare una risorsa][AZ01]

1. Fare clic su **Database** e quindi su **Azure Cosmos DB**.

   ![Creare l'istanza di Azure Cosmos DB][AZ02]

1. Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:

   * Immettere un **ID** univoco che verrà usato come parte dell'URI Gremlin del database. Se ad esempio è stato immesso **wingtiptoysdata** per l'**ID**, l'URI Gremlin sarà *wingtiptoysdata.gremlin.cosmosdb.azure.com*.
   * Scegliere **Gremlin (grafo)** per l'API.
   * Selezionare la **sottoscrizione** da usare per il database.
   * Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.
   * Specificare il **percorso** per il database.
   
   Dopo avere specificato queste opzioni, fare clic su **Crea** per creare il database.

   ![Specificare le opzioni di Azure Cosmos DB][AZ03]

1. Al termine della creazione, il database viene elencato nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**. È possibile fare clic sul database in una di queste posizioni per aprire la pagina delle proprietà per la cache.

   ![Tutte le risorse][AZ04]

1. Quando viene visualizzata la pagina delle proprietà per il database, fare clic su **Chiavi di accesso** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati all'interno dell'applicazione Spring Boot.

   ![Chiavi di accesso][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>Aggiungere un grafo al database di Azure Cosmos DB

1. Fare clic su **Esplora dati**, quindi su **New Graph** (Nuovo grafo).

   ![Nuovo grafo][AZ06]

1. Quando viene visualizzato **Aggiungi Graph**, immettere le informazioni seguenti:

   * Specificare un **ID database** univoco per il database.
   * Specificare un **ID grafo** univoco per il grafo.
   * È possibile specificare la propria **capacità di archiviazione** oppure accettare l'impostazione predefinita.
   * Specificare la **velocità effettiva**. Per questo esempio è possibile scegliere 400 unità richiesta (UR).
   
   Dopo avere specificato queste opzioni, fare clic su **OK** per creare il grafo.

   ![Aggiungere il grafo][AZ07]

1. Dopo aver creato il grafo, è possibile usare **Esplora dati** per visualizzarlo.

   ![Visualizzazione delle proprietà del grafo][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per **Group** (Gruppo) e **Artifact** (Artefatto) dell'applicazione, specificare la versione **Spring Boot** (2.0 o successiva), quindi fare clic su **Generate Project** (Genera progetto).

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.example.wintiptoysdata.
   >

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

   ![Scaricare il progetto Spring Boot personalizzato][SI02]

1. Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.

   ![File del progetto Spring Boot personalizzato][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a>Configurare l'app Spring Boot per l'uso di Spring Data Gremlin Starter

1. Individuare il file *pom.xml* nella directory dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -oppure-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![Individuare il file pom.xml][PM01]

1. Aprire il file *pom.xml* in un editor di testo e aggiungere Spring Data Gremlin Starter all'elenco di `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![Modifica del file pom.xml][PM02]

1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>Configurare l'app Spring Boot per usare Azure Cosmos DB

1. Individuare la directory *resources* dell'app e creare un nuovo file denominato *application.yml*. Ad esempio: 

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![Creare il file application.yml][RE01]

1.  Aprire il file *application.yml* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   
   Dove:
   
   | Campo | DESCRIZIONE |
   |---|---|
   | `endpoint` | Specifica l'URI Gremlin per il database, derivante dall'**ID** univoco specificato quando è stata creata l'istanza di Azure Cosmos DB in un passaggio precedente dell'esercitazione. |
   | `port` | Specifica la porta TCP/IP, che deve essere **443** per HTTPS. |
   | `username` | Specifica l'**ID database** e l'**ID grafo** univoci usati quando è stato aggiunto il grafo in un passaggio precedente dell'esercitazione. Il valore deve essere immesso con la sintassi "/dbs/**{ID database}**/colls/**{ID grafo}**". |
   | `password` | Specifica la **chiave di accesso** primaria o secondaria copiata in un passaggio precedente dell'esercitazione. |
   | `telemetryAllowed` | Specificare **true** se si vuole abilitare la telemetria, altrimenti **false**.

1. Salvare e chiudere il file *application.yml*.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Aggiungere il codice di esempio per implementare le funzionalità di base del database

In questa sezione si creano le classi Java necessarie per l'archiviazione dei dati nel database.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![Individuare il file Java dell'applicazione][JV01]

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

   ```java
   package com.example.wingtiptoysdata;
   
   // These imports are required for the application.
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.example.wingtiptoysdata.domain.Network;
   import com.example.wingtiptoysdata.domain.Person;
   import com.example.wingtiptoysdata.domain.Relation;
   import com.example.wingtiptoysdata.repository.NetworkRepository;
   import com.example.wingtiptoysdata.repository.PersonRepository;
   import com.example.wingtiptoysdata.repository.RelationRepository;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import javax.annotation.PostConstruct;
   import javax.annotation.PreDestroy;
   
   @SpringBootApplication
   public class WingtiptoysdataApplication {
   
       // Define several person classes to store personal data.
       private final Person person1 = new Person("01", "Nellie Hughes", "23");
       private final Person person2 = new Person("02", "Delmar Alfred", "34");
       private final Person person3 = new Person("03", "Kelley Hunter", "45");
       private final Person person4 = new Person("04", "Roscoe Guerin", "56");
       private final Person person5 = new Person("05", "Gracie Chavez", "67");
   
       // Define relationship classes to define the relationships between some of the persons.
       private final Relation relation1 = new Relation("0102", "siblings", person1, person2);
       private final Relation relation2 = new Relation("0203", "coworkers", person2, person3);
       private final Relation relation3 = new Relation("0301", "parent", person3, person1);
       private final Relation relation4 = new Relation("0302", "parent", person3, person2);
       private final Relation relation5 = new Relation("0501", "grandparent", person5, person1);
       private final Relation relation6 = new Relation("0502", "grandparent", person5, person2);
   
       // Define the network.
       private final Network network = new Network();
   
       // Autowire the repositories and factory.
       @Autowired
       private PersonRepository personRepo;
       @Autowired
       private RelationRepository relationRepo;
       @Autowired
       private NetworkRepository networkRepo;
       @Autowired
       private GremlinFactory factory;
   
       // Run the Spring application and exit.
    public static void main(String[] args) {
           SpringApplication.run(WingtiptoysdataApplication.class, args);
           System.exit(0);
    }
   
       // Perform post-construct operations.    
       @PostConstruct
       public void setup() {
           // Delete any existing data from the database.
           this.networkRepo.deleteAll();
   
           // Add the relationship classes as edges.
           this.network.getEdges().add(this.relation1);
           this.network.getEdges().add(this.relation2);
           this.network.getEdges().add(this.relation3);
           this.network.getEdges().add(this.relation4);
           this.network.getEdges().add(this.relation5);
           this.network.getEdges().add(this.relation6);
   
           // Add the person classes as vertices.
           this.network.getVertexes().add(this.person1);
           this.network.getVertexes().add(this.person2);
           this.network.getVertexes().add(this.person3);
           this.network.getVertexes().add(this.person4);
           this.network.getVertexes().add(this.person5);
   
           // Save the network.
           this.networkRepo.save(this.network);
       }
   }
   ```

1. Salvare e chiudere il file Java dell'applicazione main.

### <a name="define-a-basic-class-for-storing-configuration-information"></a>Definire una classe di base per l'archiviazione delle informazioni di configurazione

1. Creare una cartella denominata *config* nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. Creare un nuovo file Java denominato *UserRepositoryConfiguration.java* nella directory *config*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.config;

   import com.microsoft.spring.data.gremlin.common.GremlinConfiguration;
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.microsoft.spring.data.gremlin.config.AbstractGremlinConfiguration;
   import com.microsoft.spring.data.gremlin.repository.config.EnableGremlinRepositories;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   
   @Configuration
   @EnableGremlinRepositories(basePackages = "com.example.wingtiptoysdata.repository")
   @EnableConfigurationProperties(GremlinConfiguration.class)
   @PropertySource("classpath:application.yml")
   public class UserRepositoryConfiguration extends AbstractGremlinConfiguration {
   
       @Autowired
       private GremlinConfiguration config;
   
       @Override
       public GremlinConfiguration getGremlinConfiguration() {
           return this.config;
       }
   }
   ```

1. Salvare e chiudere il file *UserRepositoryConfiguration.java*.

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a>Definire un insieme di classi che indicano gli elementi del database a grafo

1. Creare una cartella denominata *domain* nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. Creare un nuovo file Java denominato *Person.java* nella directory *domain*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Vertex;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Vertex
   @AllArgsConstructor
   @NoArgsConstructor
   public class Person {
   
       @Id
       private String id;
   
       private String name;
   
       private String age;
   }
   ```

1. Salvare e chiudere il file *Person.java*.

1. Creare un nuovo file Java denominato *Relation.java* nella directory *domain*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Edge;
   import com.microsoft.spring.data.gremlin.annotation.EdgeFrom;
   import com.microsoft.spring.data.gremlin.annotation.EdgeTo;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Edge
   @AllArgsConstructor
   @NoArgsConstructor
   public class Relation {
   
       @Id
       private String id;
   
       private String name;
   
       @EdgeFrom
       private Person personFrom;
   
       @EdgeTo
       private Person personTo;
   }
   ```

1. Salvare e chiudere il file *Relation.java*.

1. Creare un nuovo file Java denominato *Network.java* nella directory *domain*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.EdgeSet;
   import com.microsoft.spring.data.gremlin.annotation.Graph;
   import com.microsoft.spring.data.gremlin.annotation.VertexSet;
   import lombok.Getter;
   import org.springframework.data.annotation.Id;
   
   import java.util.ArrayList;
   import java.util.List;
   
   @Graph
   public class Network {
   
       @Id
       private String id;
   
       public Network() {
           this.edges = new ArrayList<Object>();
           this.vertexes = new ArrayList<Object>();
       }
   
       @EdgeSet
       @Getter
       private List<Object> edges;
   
       @VertexSet
       @Getter
       private List<Object> vertexes;
   }
   ```

1. Salvare e chiudere il file *Network.java*.

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a>Definire un insieme di classi che indicano i repository del database a grafo

1. Creare una cartella denominata *repository* nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   -oppure-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. Creare un nuovo file Java denominato *NetworkRepository.java* nella directory *repository*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. Salvare e chiudere il file *NetworkRepository.java*.

1. Creare un nuovo file Java denominato *PersonRepository.java* nella directory *repository*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. Salvare e chiudere il file *PersonRepository.java*.

1. Creare un nuovo file Java denominato *RelationRepository.java* nella directory *repository*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. Salvare e chiudere il file *RelationRepository.java*.

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:

   `cd C:\SpringBoot\wingtiptoysdata`

   -oppure-

   `cd /users/example/home/wingtiptoysdata`

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. L'applicazione visualizzerà diversi messaggi di runtime e, in assenza di errori, è possibile usare il portale di Azure per visualizzare il contenuto della propria istanza di Azure Cosmos DB. A tale scopo, fare clic su **Esplora dati** nella pagina delle proprietà del database, quindi fare clic su **Esegui query Gremlin** e selezionare un elemento dall'elenco dei risultati per visualizzare i dati.

   ![Utilizzo di Esplora documenti per visualizzare i dati][JV03]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul supporto di Azure per Gremlin e l'API Graph, vedere gli articoli seguenti:

* [Introduzione ad Azure Cosmos DB: API Graph](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)

* [Supporto Gremlin Graph di Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB: Creare un database a grafo con Java e il portale di Azure](https://docs.microsoft.com/azure/cosmos-db/create-graph-java)

* [Esercitazione: Eseguire query nell'API Graph di Azure Cosmos DB con Gremlin](https://docs.microsoft.com/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:

* [Documentazione di Azure Cosmos DB].

* [Azure Cosmos DB: Creare un database di documenti con Java e il portale di Azure][Build a SQL API app with Java]

* [Spring Data per l'API SQL Azure Cosmos DB]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.


<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ05.png
[AZ06]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ06.png
[AZ07]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ07.png
[AZ08]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ08.png

[SI01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV03.png
