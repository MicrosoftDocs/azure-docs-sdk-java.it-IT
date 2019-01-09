---
title: Come usare Spring Data JDBC con Azure PostgreSQL
description: Informazioni su come usare Spring Data JDBC con un database PostgreSQL di Azure.
services: postgresql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 371a8c686f7ad045443328d02a32a4e65af55981
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992339"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-postgresql"></a><span data-ttu-id="619d3-103">Come usare Spring Data JDBC con Azure PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="619d3-103">How to use Spring Data JDBC with Azure PostgreSQL</span></span>

## <a name="overview"></a><span data-ttu-id="619d3-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="619d3-104">Overview</span></span>

<span data-ttu-id="619d3-105">Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni in un database [PostgreSQL]https://www.postgresql.org/ di Azure con [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).</span><span class="sxs-lookup"><span data-stu-id="619d3-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [PostgreSQL]https://www.postgresql.org/ database using [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="619d3-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="619d3-106">Prerequisites</span></span>

<span data-ttu-id="619d3-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="619d3-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="619d3-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="619d3-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="619d3-109">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="619d3-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="619d3-110">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="619d3-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="619d3-111">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="619d3-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="619d3-112">[Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="619d3-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="619d3-113">Utilità da riga di comando [psql](https://www.postgresql.org/docs/current/app-psql.html).</span><span class="sxs-lookup"><span data-stu-id="619d3-113">The [psql](https://www.postgresql.org/docs/current/app-psql.html) command-line utility.</span></span>
* <span data-ttu-id="619d3-114">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="619d3-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-postgresql-database-for-azure"></a><span data-ttu-id="619d3-115">Creare un database PostgreSQL per Azure</span><span class="sxs-lookup"><span data-stu-id="619d3-115">Create a PostgreSQL database for Azure</span></span>

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="619d3-116">Creare un server di database PostgreSQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="619d3-116">Create a PostgreSQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="619d3-117">Per informazioni più dettagliate sulla creazione di database PostgreSQL, vedere [Creare un server Database di Azure per PostgreSQL nel portale di Azure](/azure/postgresql/quickstart-create-server-database-portal).</span><span class="sxs-lookup"><span data-stu-id="619d3-117">You can read more detailed information about creating PostgreSQL databases in [Create an Azure Database for PostgreSQL server by using the Azure portal](/azure/postgresql/quickstart-create-server-database-portal).</span></span>

1. <span data-ttu-id="619d3-118">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="619d3-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="619d3-119">Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Database di Azure per PostgreSQL**.</span><span class="sxs-lookup"><span data-stu-id="619d3-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for PostgreSQL**.</span></span>

   ![Creare un database PostgreSQL][POSTGRESQL01]

1. <span data-ttu-id="619d3-121">Immettere le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="619d3-121">Enter the following information:</span></span>

   - <span data-ttu-id="619d3-122">**Nome server**: scegliere un nome univoco per il server PostgreSQL. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoyspostgresql.postgres.database.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="619d3-122">**Server name**: Choose a unique name for your PostgreSQL server; this will be used to create a fully-qualified domain name like *wingtiptoyspostgresql.postgres.database.azure.com*.</span></span>
   - <span data-ttu-id="619d3-123">**Sottoscrizione**: specificare la sottoscrizione di Azure da usare.</span><span class="sxs-lookup"><span data-stu-id="619d3-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="619d3-124">**Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="619d3-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="619d3-125">**Selezionare l'origine**: per questa esercitazione selezionare `Blank` per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="619d3-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="619d3-126">**Account di accesso amministratore server**: specificare il nome dell'amministratore del database.</span><span class="sxs-lookup"><span data-stu-id="619d3-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="619d3-127">**Password** e **Conferma password**: specificare la password dell'amministratore del database.</span><span class="sxs-lookup"><span data-stu-id="619d3-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="619d3-128">**Località**: specificare l'area geografica più vicina per il database.</span><span class="sxs-lookup"><span data-stu-id="619d3-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="619d3-129">**Versione**: specificare la versione del database più aggiornata.</span><span class="sxs-lookup"><span data-stu-id="619d3-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="619d3-130">**Piano tariffario**: per questa esercitazione specificare il piano tariffario meno costoso.</span><span class="sxs-lookup"><span data-stu-id="619d3-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![Creare le proprietà del database PostgreSQL][POSTGRESQL02]

1. <span data-ttu-id="619d3-132">Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="619d3-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="619d3-133">Configurare una regola del firewall per il server di database PostgreSQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="619d3-133">Configure a firewall rule for your PostgreSQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="619d3-134">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="619d3-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="619d3-135">Fare clic su **Tutte le risorse** e quindi sul database PostgreSQL appena creato.</span><span class="sxs-lookup"><span data-stu-id="619d3-135">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![Selezionare il database PostgreSQL][POSTGRESQL03]

1. <span data-ttu-id="619d3-137">Fare clic su **Sicurezza delle connessioni**, creare una nuova regola specificando un nome univoco in **Regole del firewall**, immettere l'intervallo di indirizzi IP che dovrà avere accesso al database e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="619d3-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![Configurare la sicurezza delle connessioni][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a><span data-ttu-id="619d3-139">Recuperare la stringa di connessione per il server PostgreSQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="619d3-139">Retrieve the connection string for your PostgreSQL server using the Azure Portal</span></span>

1. <span data-ttu-id="619d3-140">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="619d3-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="619d3-141">Fare clic su **Tutte le risorse** e quindi sul database PostgreSQL appena creato.</span><span class="sxs-lookup"><span data-stu-id="619d3-141">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![Selezionare il database PostgreSQL][POSTGRESQL03]

1. <span data-ttu-id="619d3-143">Fare clic su **Stringhe di connessione** e copiare il valore del campo di testo **JDBC**.</span><span class="sxs-lookup"><span data-stu-id="619d3-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![Recuperare la stringa di connessione JDBC][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a><span data-ttu-id="619d3-145">Creare un database PostgreSQL con l'utilità da riga di comando `psql`</span><span class="sxs-lookup"><span data-stu-id="619d3-145">Create PostgreSQL database using the `psql` command-line utility</span></span>

1. <span data-ttu-id="619d3-146">Aprire una shell dei comandi e connettersi al server PostgreSQL immettendo un comando `psql` come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="619d3-146">Open a command shell and connect to your PostgreSQL server by entering a `psql` command like the following example:</span></span>

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   <span data-ttu-id="619d3-147">Dove:</span><span class="sxs-lookup"><span data-stu-id="619d3-147">Where:</span></span>

   | <span data-ttu-id="619d3-148">Parametro</span><span class="sxs-lookup"><span data-stu-id="619d3-148">Parameter</span></span> | <span data-ttu-id="619d3-149">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="619d3-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="619d3-150">Specifica il nome completo del server PostgreSQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="619d3-150">Specifies your fully qualified PostgreSQL server name from earlier in this article.</span></span> |
   | `host` | <span data-ttu-id="619d3-151">Specifica la porta del server PostgreSQL, che per impostazione predefinita è `5432`.</span><span class="sxs-lookup"><span data-stu-id="619d3-151">Specifies the PostgreSQL server port, which is `5432` by default.</span></span> |
   | `username` | <span data-ttu-id="619d3-152">Specifica l'amministratore PostgreSQL e il nome abbreviato del server dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="619d3-152">Specifies your PostgreSQL administrator and shortened server name from earlier in this article.</span></span> |
   | `dbname` | <span data-ttu-id="619d3-153">Specifica che per il momento si vuole usare il database `postgres` predefinito.</span><span class="sxs-lookup"><span data-stu-id="619d3-153">Specifies that you want to use the default `postgres` database for now.</span></span> |

   <span data-ttu-id="619d3-154">La risposta visualizzata dal server PostgreSQL sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="619d3-154">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. <span data-ttu-id="619d3-155">Creare un database denominato *mypgsqldb* immettendo un comando `psql` come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="619d3-155">Create a database named *mypgsqldb* by entering a `psql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   <span data-ttu-id="619d3-156">La risposta visualizzata dal server PostgreSQL sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="619d3-156">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   CREATE DATABASE
   ```

1. <span data-ttu-id="619d3-157">FACOLTATIVO: è possibile verificare la creazione del database immettendo `\l` in `psql`. La risposta del server PostgreSQL sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="619d3-157">OPTIONAL: You can verify that your database was created by entering a `\l` at the `psql`; your PostgreSQL server should respond with something like the following example:</span></span>

   ```shell
                   List of databases
          Name        |      Owner      | Encoding
   -------------------+-----------------+----------
    azure_maintenance | azure_superuser | UTF8
    azure_sys         | azure_superuser | UTF8
    mypgsqldb         | wingtiptoysuser | UTF8
    postgres          | azure_superuser | UTF8
    template0         | azure_superuser | UTF8
    template1         | azure_superuser | UTF8
   (6 rows)
   ```

1. <span data-ttu-id="619d3-158">Immettere `\q` per uscire dall'utilità `psql`.</span><span class="sxs-lookup"><span data-stu-id="619d3-158">Enter `\q` to exit the `psql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="619d3-159">Configurare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="619d3-159">Configure the sample application</span></span>

1. <span data-ttu-id="619d3-160">Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="619d3-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="619d3-161">Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="619d3-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="619d3-162">Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:</span><span class="sxs-lookup"><span data-stu-id="619d3-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   <span data-ttu-id="619d3-163">Dove:</span><span class="sxs-lookup"><span data-stu-id="619d3-163">Where:</span></span>

   | <span data-ttu-id="619d3-164">Parametro</span><span class="sxs-lookup"><span data-stu-id="619d3-164">Parameter</span></span> | <span data-ttu-id="619d3-165">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="619d3-165">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="619d3-166">Specifica la stringa JDBC per PostgreSQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="619d3-166">Specifies your PostgreSQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="619d3-167">Specifica il nome dell'amministratore PostgreSQL dei passaggi precedenti di questo articolo, cui viene aggiunto il nome abbreviato del server.</span><span class="sxs-lookup"><span data-stu-id="619d3-167">Specifies your PostgreSQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="619d3-168">Specifica la password dell'amministratore PostgreSQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="619d3-168">Specifies your PostgreSQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="619d3-169">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="619d3-169">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="619d3-170">Creare il pacchetto dell'applicazione di esempio e testarla</span><span class="sxs-lookup"><span data-stu-id="619d3-170">Package and test the sample application</span></span> 

1. <span data-ttu-id="619d3-171">Compilare l'applicazione di esempio con Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="619d3-171">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P postgresql
   ```

1. <span data-ttu-id="619d3-172">Avviare l'applicazione di esempio. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="619d3-172">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="619d3-173">Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="619d3-173">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="619d3-174">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="619d3-174">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="619d3-175">Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="619d3-175">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="619d3-176">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="619d3-176">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="619d3-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="619d3-177">Summary</span></span>

<span data-ttu-id="619d3-178">In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni in un database PostgreSQL di Azure con JDBC.</span><span class="sxs-lookup"><span data-stu-id="619d3-178">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure PostgreSQL database using JDBC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="619d3-179">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="619d3-179">Next steps</span></span>

<span data-ttu-id="619d3-180">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="619d3-180">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="619d3-181">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="619d3-181">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="619d3-182">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="619d3-182">Additional Resources</span></span>

<span data-ttu-id="619d3-183">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="619d3-183">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[Azure per sviluppatori Java]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[POSTGRESQL01]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-05.png
