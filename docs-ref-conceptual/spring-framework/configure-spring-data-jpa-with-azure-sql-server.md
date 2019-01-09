---
title: Come usare Spring Data JPA con un database SQL di Azure
description: Informazioni su come usare Spring Data JPA con un database SQL di Azure.
services: sql-database
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 7119283bec250a4ab0854ba2c29b0906624448e9
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992334"
---
# <a name="how-to-use-spring-data-jpa-with-azure-sql-database"></a><span data-ttu-id="3026e-103">Come usare Spring Data JPA con un database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="3026e-103">How to use Spring Data JPA with Azure SQL Database</span></span>

## <a name="overview"></a><span data-ttu-id="3026e-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3026e-104">Overview</span></span>

<span data-ttu-id="3026e-105">Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni in un [database SQL di Azure](https://azure.microsoft.com/services/sql-database/) con [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span><span class="sxs-lookup"><span data-stu-id="3026e-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3026e-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3026e-106">Prerequisites</span></span>

<span data-ttu-id="3026e-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="3026e-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="3026e-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="3026e-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="3026e-109">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="3026e-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="3026e-110">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="3026e-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="3026e-111">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="3026e-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="3026e-112">[Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="3026e-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="3026e-113">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="3026e-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-sql-satabase"></a><span data-ttu-id="3026e-114">Creare un database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="3026e-114">Create an Azure SQL Satabase</span></span>

### <a name="create-a-sql-database-server-using-the-azure-portal"></a><span data-ttu-id="3026e-115">Creare un server di database SQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="3026e-115">Create a SQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="3026e-116">Per informazioni più dettagliate sulla creazione di database SQL di Azure, vedere [Creare un database SQL di Azure nel portale di Azure](/azure/sql-database/sql-database-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="3026e-116">You can read more detailed information about creating Azure SQL databases in [Create an Azure SQL database in the Azure portal](/azure/sql-database/sql-database-get-started-portal).</span></span>

1. <span data-ttu-id="3026e-117">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3026e-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="3026e-118">Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Database SQL**.</span><span class="sxs-lookup"><span data-stu-id="3026e-118">Click **+Create a resource**, then **Databases**, and then click **SQL Database**.</span></span>

   ![Creazione di un database SQL][SQL01]

1. <span data-ttu-id="3026e-120">Specificare le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="3026e-120">Specify the following information:</span></span>

   - <span data-ttu-id="3026e-121">**Nome database**: scegliere un nome univoco per il database SQL, che verrà creato nel server SQL specificato in seguito.</span><span class="sxs-lookup"><span data-stu-id="3026e-121">**Database name**: Choose a unique name for your SQL database; this will be created in the SQL server that you will specify later.</span></span>
   - <span data-ttu-id="3026e-122">**Sottoscrizione**: specificare la sottoscrizione di Azure da usare.</span><span class="sxs-lookup"><span data-stu-id="3026e-122">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="3026e-123">**Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="3026e-123">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="3026e-124">**Selezionare l'origine**: per questa esercitazione selezionare `Blank database` per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="3026e-124">**Select source**: For this tutorial, select `Blank database` to create a new database.</span></span>

   ![Specificare le proprietà del database SQL][SQL02]
   
1. <span data-ttu-id="3026e-126">Fare clic su **Server** e su **Crea un nuovo server** e quindi specificare le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="3026e-126">Click **Server**, then **Create a new server**, and then specify the following information:</span></span>

   - <span data-ttu-id="3026e-127">**Nome server**: scegliere un nome univoco per il server SQL. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoyssql.database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="3026e-127">**Server name**: Choose a unique name for your SQL server; this will be used to create a fully-qualified domain name like *wingtiptoyssql.database.windows.net*.</span></span>
   - <span data-ttu-id="3026e-128">**Account di accesso amministratore server**: specificare il nome dell'amministratore del database.</span><span class="sxs-lookup"><span data-stu-id="3026e-128">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="3026e-129">**Password** e **Conferma password**: specificare la password dell'amministratore del database.</span><span class="sxs-lookup"><span data-stu-id="3026e-129">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="3026e-130">**Località**: specificare l'area geografica più vicina per il database.</span><span class="sxs-lookup"><span data-stu-id="3026e-130">**Location**: Specify the closest geographic region for your database.</span></span>

   ![Specificare il server SQL][SQL03]

1. <span data-ttu-id="3026e-132">Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="3026e-132">When you have entered all of the above information, click **Select**.</span></span>

1. <span data-ttu-id="3026e-133">Per questa esercitazione specificare il **piano tariffario** meno costoso e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3026e-133">For this tutorial, specify the least-expensive **Pricing tier**, and then click **Create**.</span></span>

   ![Creare il database SQL][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="3026e-135">Configurare una regola del firewall per il server SQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="3026e-135">Configure a firewall rule for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="3026e-136">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3026e-136">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="3026e-137">Fare clic su **Tutte le risorse** e quindi sul server SQL appena creato.</span><span class="sxs-lookup"><span data-stu-id="3026e-137">Click **All Resources**, then click the SQL server you just created.</span></span>

   ![Selezionare il server SQL][SQL05]

1. <span data-ttu-id="3026e-139">Nella sezione **Panoramica** fare clic su **Mostra impostazioni firewall**.</span><span class="sxs-lookup"><span data-stu-id="3026e-139">In the **Overview** section, click **Show firewall settings**</span></span>

   ![Mostra impostazioni firewall][SQL06]

1. <span data-ttu-id="3026e-141">Nella sezione **Firewall e reti virtuali** creare una nuova regola specificando un nome univoco, immettere l'intervallo di indirizzi IP che dovrà avere accesso al database e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="3026e-141">In the **Firewalls and virtual networks** section, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![Configurare le impostazioni del firewall][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="3026e-143">Recuperare la stringa di connessione per il server SQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="3026e-143">Retrieve the connection string for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="3026e-144">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3026e-144">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="3026e-145">Fare clic su **Tutte le risorse** e quindi sul database SQL appena creato.</span><span class="sxs-lookup"><span data-stu-id="3026e-145">Click **All Resources**, then click the SQL database you just created.</span></span>

   ![Selezionare il database SQL][SQL08]

1. <span data-ttu-id="3026e-147">Fare clic su **Stringhe di connessione** e quindi su **JDBC** e copiare il valore del campo di testo JDBC.</span><span class="sxs-lookup"><span data-stu-id="3026e-147">Click **Connection strings**, then click **JDBC**, and copy the value in the JDBC text field.</span></span>

   ![Recuperare la stringa di connessione JDBC][SQL09]

## <a name="configure-the-sample-application"></a><span data-ttu-id="3026e-149">Configurare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="3026e-149">Configure the sample application</span></span>

1. <span data-ttu-id="3026e-150">Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3026e-150">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="3026e-151">Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="3026e-151">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="3026e-152">Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:</span><span class="sxs-lookup"><span data-stu-id="3026e-152">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   <span data-ttu-id="3026e-153">Dove:</span><span class="sxs-lookup"><span data-stu-id="3026e-153">Where:</span></span>

   | <span data-ttu-id="3026e-154">Parametro</span><span class="sxs-lookup"><span data-stu-id="3026e-154">Parameter</span></span> | <span data-ttu-id="3026e-155">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="3026e-155">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="3026e-156">Specifica una versione modificata della stringa JDBC per SQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="3026e-156">Specifies an edited version of your SQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="3026e-157">Specifica il nome dell'amministratore SQL dei passaggi precedenti di questo articolo, cui viene aggiunto il nome abbreviato del server.</span><span class="sxs-lookup"><span data-stu-id="3026e-157">Specifies your SQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="3026e-158">Specifica la password dell'amministratore SQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="3026e-158">Specifies your SQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="3026e-159">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="3026e-159">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="3026e-160">Creare il pacchetto dell'applicazione di esempio e testarla</span><span class="sxs-lookup"><span data-stu-id="3026e-160">Package and test the sample application</span></span> 

1. <span data-ttu-id="3026e-161">Compilare l'applicazione di esempio con Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3026e-161">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P sql
   ```

1. <span data-ttu-id="3026e-162">Avviare l'applicazione di esempio. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3026e-162">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="3026e-163">Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3026e-163">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="3026e-164">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="3026e-164">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="3026e-165">Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3026e-165">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="3026e-166">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="3026e-166">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="3026e-167">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="3026e-167">Summary</span></span>

<span data-ttu-id="3026e-168">In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni in un database SQL di Azure con JPA.</span><span class="sxs-lookup"><span data-stu-id="3026e-168">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure SQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3026e-169">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3026e-169">Next steps</span></span>

<span data-ttu-id="3026e-170">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="3026e-170">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3026e-171">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="3026e-171">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="3026e-172">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3026e-172">Additional Resources</span></span>

<span data-ttu-id="3026e-173">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="3026e-173">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[SQL01]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-09.png
