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
ms.openlocfilehash: 4e0138e3cc474b4c47d3bf492e696ec49ea3ef37
ms.sourcegitcommit: c2019ba6da6c7c28b17b5a85f89e49bb5e570ba4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040269"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="30e70-103">Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="30e70-103">How to use the Spring Data Gremlin Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="30e70-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="30e70-104">Overview</span></span>

<span data-ttu-id="30e70-105">Spring Data Gremlin Starter fornisce il supporto Spring Data per il linguaggio di query Gremlin di Apache, che gli sviluppatori possono usare con qualsiasi archivio dati compatibile con Gremlin.</span><span class="sxs-lookup"><span data-stu-id="30e70-105">The Spring Data Gremlin Starter provides Spring Data support for the Gremlin query language from Apache, which developers can use with any Gremlin-compatible data store.</span></span>

<span data-ttu-id="30e70-106">Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure per l'uso con l'API Gremlin, l'uso di **[Spring Initializr]** per creare un'applicazione Java personalizzata e quindi aggiungere la funzionalità di Spring Data Gremlin Starter all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB con Gremlin.</span><span class="sxs-lookup"><span data-stu-id="30e70-106">This article demonstrates creating an Azure Cosmos DB by using the Azure portal for use with Gremlin API, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Data Gremlin Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using Gremlin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30e70-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="30e70-107">Prerequisites</span></span>

<span data-ttu-id="30e70-108">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="30e70-108">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="30e70-109">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="30e70-109">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="30e70-110">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="30e70-110">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="30e70-111">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="30e70-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="30e70-112">Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="30e70-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a><span data-ttu-id="30e70-113">Creare un'istanza di Azure Cosmos DB usando il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="30e70-113">Create an Azure Cosmos DB using the Azure portal</span></span>

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a><span data-ttu-id="30e70-114">Creare il database Azure Cosmos da usare con l'API Gremlin</span><span class="sxs-lookup"><span data-stu-id="30e70-114">Create your Azure Cosmos Database for use with Gremlin API</span></span>

1. <span data-ttu-id="30e70-115">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="30e70-115">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![Creare una risorsa][AZ01]

1. <span data-ttu-id="30e70-117">Fare clic su **Database** e quindi su **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="30e70-117">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Creare l'istanza di Azure Cosmos DB][AZ02]

1. <span data-ttu-id="30e70-119">Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-119">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="30e70-120">Immettere un **ID** univoco che verrà usato come parte dell'URI Gremlin del database.</span><span class="sxs-lookup"><span data-stu-id="30e70-120">Enter a unique **ID**, which you will use as part of the Gremlin URI for your database.</span></span> <span data-ttu-id="30e70-121">Se ad esempio è stato immesso **wingtiptoysdata** per l'**ID**, l'URI Gremlin sarà *wingtiptoysdata.gremlin.cosmosdb.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="30e70-121">For example: if you entered **wingtiptoysdata** for the **ID**, the Gremlin URI would be *wingtiptoysdata.gremlin.cosmosdb.azure.com*.</span></span>
   * <span data-ttu-id="30e70-122">Scegliere **Gremlin (grafo)** per l'API.</span><span class="sxs-lookup"><span data-stu-id="30e70-122">Choose **Gremlin (Graph)** for the API.</span></span>
   * <span data-ttu-id="30e70-123">Selezionare la **sottoscrizione** da usare per il database.</span><span class="sxs-lookup"><span data-stu-id="30e70-123">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="30e70-124">Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="30e70-124">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="30e70-125">Specificare il **percorso** per il database.</span><span class="sxs-lookup"><span data-stu-id="30e70-125">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="30e70-126">Dopo avere specificato queste opzioni, fare clic su **Crea** per creare il database.</span><span class="sxs-lookup"><span data-stu-id="30e70-126">When you have specified these options, click **Create** to create your database.</span></span>

   ![Specificare le opzioni di Azure Cosmos DB][AZ03]

1. <span data-ttu-id="30e70-128">Al termine della creazione, il database viene elencato nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="30e70-128">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="30e70-129">È possibile fare clic sul database in una di queste posizioni per aprire la pagina delle proprietà per la cache.</span><span class="sxs-lookup"><span data-stu-id="30e70-129">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Tutte le risorse][AZ04]

1. <span data-ttu-id="30e70-131">Quando viene visualizzata la pagina delle proprietà per il database, fare clic su **Chiavi di accesso** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati all'interno dell'applicazione Spring Boot.</span><span class="sxs-lookup"><span data-stu-id="30e70-131">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Chiavi di accesso][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a><span data-ttu-id="30e70-133">Aggiungere un grafo al database di Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="30e70-133">Add a graph to your Azure Cosmos Database</span></span>

1. <span data-ttu-id="30e70-134">Fare clic su **Esplora dati**, quindi su **New Graph** (Nuovo grafo).</span><span class="sxs-lookup"><span data-stu-id="30e70-134">Click **Data Explorer**, and then click **New Graph**.</span></span>

   ![Nuovo grafo][AZ06]

1. <span data-ttu-id="30e70-136">Quando viene visualizzato **Aggiungi Graph**, immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-136">When the **Add Graph** is displayed, enter the following information:</span></span>

   * <span data-ttu-id="30e70-137">Specificare un **ID database** univoco per il database.</span><span class="sxs-lookup"><span data-stu-id="30e70-137">Specify a unique **Database id** for your database.</span></span>
   * <span data-ttu-id="30e70-138">Specificare un **ID grafo** univoco per il grafo.</span><span class="sxs-lookup"><span data-stu-id="30e70-138">Specify a unique **Graph id** for your graph.</span></span>
   * <span data-ttu-id="30e70-139">È possibile specificare la propria **capacità di archiviazione** oppure accettare l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="30e70-139">You can choose to specify your **Storage capacity**, or you can accept the default.</span></span>
   * <span data-ttu-id="30e70-140">Specificare la **velocità effettiva**. Per questo esempio è possibile scegliere 400 unità richiesta (UR).</span><span class="sxs-lookup"><span data-stu-id="30e70-140">Specify your **Throughput**, and for this example you can choose 400 Request Units (RUs).</span></span>
   
   <span data-ttu-id="30e70-141">Dopo avere specificato queste opzioni, fare clic su **OK** per creare il grafo.</span><span class="sxs-lookup"><span data-stu-id="30e70-141">When you have specified these options, click **OK** to create your graph.</span></span>

   ![Aggiungere il grafo][AZ07]

1. <span data-ttu-id="30e70-143">Dopo aver creato il grafo, è possibile usare **Esplora dati** per visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="30e70-143">After your graph has been created, you can use the **Data Explorer** to view it.</span></span>

   ![Visualizzazione delle proprietà del grafo][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="30e70-145">Creare un'applicazione Spring Boot semplice con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="30e70-145">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="30e70-146">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="30e70-146">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="30e70-147">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per **Group** (Gruppo) e **Artifact** (Artefatto) dell'applicazione, specificare la versione **Spring Boot** (2.0 o successiva), quindi fare clic su **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="30e70-147">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version with a version that is equal to or greater than 2.0, and then click **Generate Project**.</span></span>

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="30e70-149">Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio \*com.example.wintiptoysdata.</span><span class="sxs-lookup"><span data-stu-id="30e70-149">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: \*com.example.wintiptoysdata.</span></span>
   >

1. <span data-ttu-id="30e70-150">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="30e70-150">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring Boot personalizzato][SI02]

1. <span data-ttu-id="30e70-152">Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="30e70-152">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![File del progetto Spring Boot personalizzato][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a><span data-ttu-id="30e70-154">Configurare l'app Spring Boot per l'uso di Spring Data Gremlin Starter</span><span class="sxs-lookup"><span data-stu-id="30e70-154">Configure your Spring Boot app to use the Spring Data Gremlin Starter</span></span>

1. <span data-ttu-id="30e70-155">Individuare il file *pom.xml* nella directory dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-155">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="30e70-156">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-156">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![Individuare il file pom.xml][PM01]

1. <span data-ttu-id="30e70-158">Aprire il file *pom.xml* in un editor di testo e aggiungere Spring Data Gremlin Starter all'elenco di `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="30e70-158">Open the *pom.xml* file in a text editor, and add the Spring Data Gremlin Starter to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![Modifica del file pom.xml][PM02]

1. <span data-ttu-id="30e70-160">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="30e70-160">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="30e70-161">Configurare l'app Spring Boot per usare Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="30e70-161">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="30e70-162">Individuare la directory *resources* dell'app e creare un nuovo file denominato *application.yml*.</span><span class="sxs-lookup"><span data-stu-id="30e70-162">Locate the *resources* directory of your app, and create a new file named *application.yml*.</span></span> <span data-ttu-id="30e70-163">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="30e70-163">For example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   <span data-ttu-id="30e70-164">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-164">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![Creare il file application.yml][RE01]

1.  <span data-ttu-id="30e70-166">Aprire il file *application.yml* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:</span><span class="sxs-lookup"><span data-stu-id="30e70-166">Open the *application.yml* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   <span data-ttu-id="30e70-167">Dove:</span><span class="sxs-lookup"><span data-stu-id="30e70-167">Where:</span></span>
   | <span data-ttu-id="30e70-168">Campo</span><span class="sxs-lookup"><span data-stu-id="30e70-168">Field</span></span> | <span data-ttu-id="30e70-169">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="30e70-169">Description</span></span> |
   | ---|---|
   | `endpoint` | <span data-ttu-id="30e70-170">Specifica l'URI Gremlin per il database, derivante dall'**ID** univoco specificato quando è stata creata l'istanza di Azure Cosmos DB in un passaggio precedente dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="30e70-170">Specifies the Gremlin URI for your database, which is derived from the unique **ID** that you specified when you created your Azure Cosmos DB earlier in this tutorial.</span></span> |
   | `port` | <span data-ttu-id="30e70-171">Specifica la porta TCP/IP, che deve essere **443** per HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30e70-171">Specifies the TCP/IP port, which should be **443** for HTTPS.</span></span> |
   | `username` | <span data-ttu-id="30e70-172">Specifica l'**ID database** e l'**ID grafo** univoci usati quando è stato aggiunto il grafo in un passaggio precedente dell'esercitazione. Il valore deve essere immesso con la sintassi "/dbs/**{ID database}**/colls/**{ID grafo}**".</span><span class="sxs-lookup"><span data-stu-id="30e70-172">Specifies the unique **Database id** and **Graph id** that you used when you added your graph earlier in this tutorial; this must be entered using the following syntax: "/dbs/**{Database id}**/colls/**{Graph id}**".</span></span> |
   | `password` | <span data-ttu-id="30e70-173">Specifica la **chiave di accesso** primaria o secondaria copiata in un passaggio precedente dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="30e70-173">Specifies either the primary or secondary **Access key** that you copied earlier in this tutorial.</span></span> |
   | `telemetryAllowed` | <span data-ttu-id="30e70-174">Specificare **true** se si vuole abilitare la telemetria, altrimenti **false**.</span><span class="sxs-lookup"><span data-stu-id="30e70-174">Specify **true** if you want to enable telemetry; otherwise, **false**.</span></span>

1. <span data-ttu-id="30e70-175">Salvare e chiudere il file *application.yml*.</span><span class="sxs-lookup"><span data-stu-id="30e70-175">Save and close the *application.yml* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="30e70-176">Aggiungere il codice di esempio per implementare le funzionalità di base del database</span><span class="sxs-lookup"><span data-stu-id="30e70-176">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="30e70-177">In questa sezione si creano le classi Java necessarie per l'archiviazione dei dati nel database.</span><span class="sxs-lookup"><span data-stu-id="30e70-177">In this section, you create the necessary Java classes for storing data in your database.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="30e70-178">Modificare la classe dell'applicazione main</span><span class="sxs-lookup"><span data-stu-id="30e70-178">Modify the main application class</span></span>

1. <span data-ttu-id="30e70-179">Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-179">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="30e70-180">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-180">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![Individuare il file Java dell'applicazione][JV01]

1. <span data-ttu-id="30e70-182">Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="30e70-182">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

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

1. <span data-ttu-id="30e70-183">Salvare e chiudere il file Java dell'applicazione main.</span><span class="sxs-lookup"><span data-stu-id="30e70-183">Save and close the main application Java file.</span></span>

### <a name="define-a-basic-class-for-storing-configuration-information"></a><span data-ttu-id="30e70-184">Definire una classe di base per l'archiviazione delle informazioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="30e70-184">Define a basic class for storing configuration information</span></span>

1. <span data-ttu-id="30e70-185">Creare una cartella denominata *config* nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-185">Create a folder named *config* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   <span data-ttu-id="30e70-186">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-186">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. <span data-ttu-id="30e70-187">Creare un nuovo file Java denominato *UserRepositoryConfiguration.java* nella directory *config*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-187">Create a new Java file named *UserRepositoryConfiguration.java* in the *config* directory, then open the file in a text editor, and add the following lines:</span></span>

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

1. <span data-ttu-id="30e70-188">Salvare e chiudere il file *UserRepositoryConfiguration.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-188">Save and close the *UserRepositoryConfiguration.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a><span data-ttu-id="30e70-189">Definire un insieme di classi che indicano gli elementi del database a grafo</span><span class="sxs-lookup"><span data-stu-id="30e70-189">Define a set of classes that define the elements of your graph database</span></span>

1. <span data-ttu-id="30e70-190">Creare una cartella denominata *domain* nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-190">Create a folder named *domain* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   <span data-ttu-id="30e70-191">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-191">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. <span data-ttu-id="30e70-192">Creare un nuovo file Java denominato *Person.java* nella directory *domain*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-192">Create a new Java file named *Person.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

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

1. <span data-ttu-id="30e70-193">Salvare e chiudere il file *Person.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-193">Save and close the *Person.java* file.</span></span>

1. <span data-ttu-id="30e70-194">Creare un nuovo file Java denominato *Relation.java* nella directory *domain*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-194">Create a new Java file named *Relation.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

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

1. <span data-ttu-id="30e70-195">Salvare e chiudere il file *Relation.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-195">Save and close the *Relation.java* file.</span></span>

1. <span data-ttu-id="30e70-196">Creare un nuovo file Java denominato *Network.java* nella directory *domain*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-196">Create a new Java file named *Network.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

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

1. <span data-ttu-id="30e70-197">Salvare e chiudere il file *Network.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-197">Save and close the *Network.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a><span data-ttu-id="30e70-198">Definire un insieme di classi che indicano i repository del database a grafo</span><span class="sxs-lookup"><span data-stu-id="30e70-198">Define a set of classes that define the repositories for your graph database</span></span>

1. <span data-ttu-id="30e70-199">Creare una cartella denominata *repository* nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-199">Create a folder named *repository* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   <span data-ttu-id="30e70-200">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-200">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. <span data-ttu-id="30e70-201">Creare un nuovo file Java denominato *NetworkRepository.java* nella directory *repository*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-201">Create a new Java file named *NetworkRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. <span data-ttu-id="30e70-202">Salvare e chiudere il file *NetworkRepository.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-202">Save and close the *NetworkRepository.java* file.</span></span>

1. <span data-ttu-id="30e70-203">Creare un nuovo file Java denominato *PersonRepository.java* nella directory *repository*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-203">Create a new Java file named *PersonRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. <span data-ttu-id="30e70-204">Salvare e chiudere il file *PersonRepository.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-204">Save and close the *PersonRepository.java* file.</span></span>

1. <span data-ttu-id="30e70-205">Creare un nuovo file Java denominato *RelationRepository.java* nella directory *repository*, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-205">Create a new Java file named *RelationRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. <span data-ttu-id="30e70-206">Salvare e chiudere il file *RelationRepository.java*.</span><span class="sxs-lookup"><span data-stu-id="30e70-206">Save and close the *RelationRepository.java* file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="30e70-207">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="30e70-207">Build and test your app</span></span>

1. <span data-ttu-id="30e70-208">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-208">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="30e70-209">-oppure-</span><span class="sxs-lookup"><span data-stu-id="30e70-209">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="30e70-210">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30e70-210">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="30e70-211">L'applicazione visualizzerà diversi messaggi di runtime e, in assenza di errori, è possibile usare il portale di Azure per visualizzare il contenuto della propria istanza di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="30e70-211">Your application will display several runtime messages, and if there were no errors, you can use the Azure portal to view the contents of your Azure Cosmos DB.</span></span> <span data-ttu-id="30e70-212">A tale scopo, fare clic su **Esplora dati** nella pagina delle proprietà del database, quindi fare clic su **Esegui query Gremlin** e selezionare un elemento dall'elenco dei risultati per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="30e70-212">To do so, click **Data Explorer** on the properties page for your database, then click **Execute Gremlin Query**, and then select an item from the list of results to view the data.</span></span>

   ![Utilizzo di Esplora documenti per visualizzare i dati][JV03]

## <a name="next-steps"></a><span data-ttu-id="30e70-214">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="30e70-214">Next steps</span></span>

<span data-ttu-id="30e70-215">Per altre informazioni sul supporto di Azure per Gremlin e l'API Graph, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-215">For more information about Azure support for Gremlin and Graph API, see the following articles:</span></span>

* [<span data-ttu-id="30e70-216">Introduzione ad Azure Cosmos DB: API Graph</span><span class="sxs-lookup"><span data-stu-id="30e70-216">Introduction to Azure Cosmos DB: Graph API</span></span>](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)

* [<span data-ttu-id="30e70-217">Supporto Gremlin Graph di Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="30e70-217">Azure Cosmos DB Gremlin graph support</span></span>](https://docs.microsoft.com/azure/cosmos-db/gremlin-support)

* [<span data-ttu-id="30e70-218">Azure Cosmos DB: Creare un database a grafo con Java e il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="30e70-218">Azure Cosmos DB: Create a graph database using Java and the Azure portal</span></span>](https://docs.microsoft.com/azure/cosmos-db/create-graph-java)

* [<span data-ttu-id="30e70-219">Esercitazione: Eseguire query nell'API Graph di Azure Cosmos DB con Gremlin</span><span class="sxs-lookup"><span data-stu-id="30e70-219">Tutorial: Query Azure Cosmos DB Graph API by using Gremlin</span></span>](https://docs.microsoft.com/azure/cosmos-db/tutorial-query-graph)

* <span data-ttu-id="30e70-220">[Spring Data Gremlin Starter]</span><span class="sxs-lookup"><span data-stu-id="30e70-220">[Spring Data Gremlin Starter]</span></span>

<span data-ttu-id="30e70-221">Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-221">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="30e70-222">[Documentazione di Azure Cosmos DB].</span><span class="sxs-lookup"><span data-stu-id="30e70-222">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="30e70-223">[Azure Cosmos DB: Creare un database di documenti con Java e il portale di Azure][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="30e70-223">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="30e70-224">[Spring Data per l'API SQL Azure Cosmos DB]</span><span class="sxs-lookup"><span data-stu-id="30e70-224">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="30e70-225">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="30e70-225">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="30e70-226">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="30e70-226">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="30e70-227">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="30e70-227">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="30e70-228">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="30e70-228">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="30e70-229">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="30e70-229">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="30e70-230">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="30e70-230">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="30e70-231">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="30e70-231">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="30e70-232">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="30e70-232">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>


<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
