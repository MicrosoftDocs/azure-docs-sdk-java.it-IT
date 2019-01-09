---
title: Come usare Spring Data JPA con Azure MySQL
description: Informazioni su come usare Spring Data JPA con un database MySQL di Azure.
services: mysql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 95dfc292d8f6d7e03d396afd7da4cfff0bd05b1b
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992411"
---
# <a name="how-to-use-spring-data-jpa-with-azure-mysql"></a><span data-ttu-id="f2d62-103">Come usare Spring Data JPA con Azure MySQL</span><span class="sxs-lookup"><span data-stu-id="f2d62-103">How to use Spring Data JPA with Azure MySQL</span></span>

## <a name="overview"></a><span data-ttu-id="f2d62-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f2d62-104">Overview</span></span>

<span data-ttu-id="f2d62-105">Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni in un database [MySQL](https://www.mysql.com/) di Azure con [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span><span class="sxs-lookup"><span data-stu-id="f2d62-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [MySQL](https://www.mysql.com/) database using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2d62-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f2d62-106">Prerequisites</span></span>

<span data-ttu-id="f2d62-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="f2d62-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="f2d62-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="f2d62-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="f2d62-109">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="f2d62-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="f2d62-110">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="f2d62-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="f2d62-111">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="f2d62-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="f2d62-112">[Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="f2d62-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="f2d62-113">Utilità da riga di comando [mysql](https://dev.mysql.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f2d62-113">The [mysql](https://dev.mysql.com/downloads/) command-line utility.</span></span>
* <span data-ttu-id="f2d62-114">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="f2d62-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-mysql-database-for-azure"></a><span data-ttu-id="f2d62-115">Crea un database MySQL per Azure</span><span class="sxs-lookup"><span data-stu-id="f2d62-115">Create a MySQL database for Azure</span></span>

### <a name="create-a-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="f2d62-116">Creare un server di database MySQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="f2d62-116">Create a MySQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="f2d62-117">Per informazioni più dettagliate sulla creazione di database MySQL, vedere [Creare un server Database di Azure per MySQL nel portale di Azure](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="f2d62-117">You can read more detailed information about creating MySQL databases in [Create an Azure Database for MySQL server by using the Azure portal](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).</span></span>

1. <span data-ttu-id="f2d62-118">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="f2d62-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="f2d62-119">Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Database di Azure per MySQL**.</span><span class="sxs-lookup"><span data-stu-id="f2d62-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for MySQL**.</span></span>

   ![Creare un database MySQL][MYSQL01]

1. <span data-ttu-id="f2d62-121">Immettere le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="f2d62-121">Enter the following information:</span></span>

   - <span data-ttu-id="f2d62-122">**Nome server**: scegliere un nome univoco per il server MySQL. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoysmysql.mysql.database.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="f2d62-122">**Server name**: Choose a unique name for your MySQL server; this will be used to create a fully-qualified domain name like *wingtiptoysmysql.mysql.database.azure.com*.</span></span>
   - <span data-ttu-id="f2d62-123">**Sottoscrizione**: specificare la sottoscrizione di Azure da usare.</span><span class="sxs-lookup"><span data-stu-id="f2d62-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="f2d62-124">**Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="f2d62-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="f2d62-125">**Selezionare l'origine**: per questa esercitazione selezionare `Blank` per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="f2d62-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="f2d62-126">**Account di accesso amministratore server**: specificare il nome dell'amministratore del database.</span><span class="sxs-lookup"><span data-stu-id="f2d62-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="f2d62-127">**Password** e **Conferma password**: specificare la password dell'amministratore del database.</span><span class="sxs-lookup"><span data-stu-id="f2d62-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="f2d62-128">**Località**: specificare l'area geografica più vicina per il database.</span><span class="sxs-lookup"><span data-stu-id="f2d62-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="f2d62-129">**Versione**: specificare la versione del database più aggiornata.</span><span class="sxs-lookup"><span data-stu-id="f2d62-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="f2d62-130">**Piano tariffario**: per questa esercitazione specificare il piano tariffario meno costoso.</span><span class="sxs-lookup"><span data-stu-id="f2d62-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![Creare le proprietà del database MySQL][MYSQL02]

1. <span data-ttu-id="f2d62-132">Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f2d62-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="f2d62-133">Configurare una regola del firewall per il server di database MySQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="f2d62-133">Configure a firewall rule for your MySQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="f2d62-134">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="f2d62-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="f2d62-135">Fare clic su **Tutte le risorse** e quindi sul database MySQL appena creato.</span><span class="sxs-lookup"><span data-stu-id="f2d62-135">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![Selezionare il database MySQL][MYSQL03]

1. <span data-ttu-id="f2d62-137">Fare clic su **Sicurezza delle connessioni**, creare una nuova regola specificando un nome univoco in **Regole del firewall**, immettere l'intervallo di indirizzi IP che dovrà avere accesso al database e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="f2d62-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![Configurare la sicurezza delle connessioni][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-mysql-server-using-the-azure-portal"></a><span data-ttu-id="f2d62-139">Recuperare la stringa di connessione per il server MySQL con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="f2d62-139">Retrieve the connection string for your MySQL server using the Azure Portal</span></span>

1. <span data-ttu-id="f2d62-140">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="f2d62-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="f2d62-141">Fare clic su **Tutte le risorse** e quindi sul database MySQL appena creato.</span><span class="sxs-lookup"><span data-stu-id="f2d62-141">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![Selezionare il database MySQL][MYSQL03]

1. <span data-ttu-id="f2d62-143">Fare clic su **Stringhe di connessione** e copiare il valore del campo di testo **JDBC**.</span><span class="sxs-lookup"><span data-stu-id="f2d62-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![Recuperare la stringa di connessione JDBC][MYSQL05]

### <a name="create-mysql-database-using-the-mysql-command-line-utility"></a><span data-ttu-id="f2d62-145">Creare un database MySQL con l'utilità da riga di comando `mysql`</span><span class="sxs-lookup"><span data-stu-id="f2d62-145">Create MySQL database using the `mysql` command-line utility</span></span>

1. <span data-ttu-id="f2d62-146">Aprire una shell dei comandi e connettersi al server MySQL immettendo un comando `mysql` come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2d62-146">Open a command shell and connect to your MySQL server by entering a `mysql` command like the following example:</span></span>

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   <span data-ttu-id="f2d62-147">Dove:</span><span class="sxs-lookup"><span data-stu-id="f2d62-147">Where:</span></span>

   | <span data-ttu-id="f2d62-148">Parametro</span><span class="sxs-lookup"><span data-stu-id="f2d62-148">Parameter</span></span> | <span data-ttu-id="f2d62-149">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="f2d62-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="f2d62-150">Specifica il nome completo del server MySQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="f2d62-150">Specifies your fully qualified MySQL server name from earlier in this article.</span></span> |
   | `user` | <span data-ttu-id="f2d62-151">Specifica l'amministratore MySQL e il nome abbreviato del server dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="f2d62-151">Specifies your MySQL administrator and shortened server name from earlier in this article.</span></span> |
   | `p` | <span data-ttu-id="f2d62-152">Specifica di attendere la richiesta di una password.</span><span class="sxs-lookup"><span data-stu-id="f2d62-152">Specifies to wait until prompted for a password.</span></span> |


   <span data-ttu-id="f2d62-153">La risposta visualizzata dal server MySQL sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2d62-153">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

1. <span data-ttu-id="f2d62-154">Creare un database denominato *mysqldb* immettendo un comando `mysql` come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2d62-154">Create a database named *mysqldb* by entering a `mysql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   <span data-ttu-id="f2d62-155">La risposta visualizzata dal server MySQL sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2d62-155">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. <span data-ttu-id="f2d62-156">FACOLTATIVO: è possibile verificare la creazione del database immettendo un comando `mysql` come quello riportato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="f2d62-156">OPTIONAL: You can verify that your database was created by entering a `mysql` command like the following example:</span></span>

   ```SQL
   SHOW DATABASES;
   ```

   <span data-ttu-id="f2d62-157">La risposta visualizzata dal server MySQL sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2d62-157">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. <span data-ttu-id="f2d62-158">Immettere `\q` per uscire dall'utilità `mysql`.</span><span class="sxs-lookup"><span data-stu-id="f2d62-158">Enter `\q` to exit the `mysql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="f2d62-159">Configurare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="f2d62-159">Configure the sample application</span></span>

1. <span data-ttu-id="f2d62-160">Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2d62-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. <span data-ttu-id="f2d62-161">Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="f2d62-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="f2d62-162">Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:</span><span class="sxs-lookup"><span data-stu-id="f2d62-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   <span data-ttu-id="f2d62-163">Dove:</span><span class="sxs-lookup"><span data-stu-id="f2d62-163">Where:</span></span>

   | <span data-ttu-id="f2d62-164">Parametro</span><span class="sxs-lookup"><span data-stu-id="f2d62-164">Parameter</span></span> | <span data-ttu-id="f2d62-165">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="f2d62-165">Description</span></span> |
   |---|---|
   | `spring.jpa.database-platform` | <span data-ttu-id="f2d62-166">Specifica la piattaforma di database JPA.</span><span class="sxs-lookup"><span data-stu-id="f2d62-166">Specifies the JPA database platform.</span></span> |
   | `spring.datasource.url` | <span data-ttu-id="f2d62-167">Specifica la stringa JDBC per MySQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="f2d62-167">Specifies your MySQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="f2d62-168">Specifica il nome dell'amministratore MySQL dei passaggi precedenti di questo articolo, cui viene aggiunto il nome abbreviato del server.</span><span class="sxs-lookup"><span data-stu-id="f2d62-168">Specifies your MySQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="f2d62-169">Specifica la password dell'amministratore MySQL dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="f2d62-169">Specifies your MySQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="f2d62-170">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="f2d62-170">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="f2d62-171">Creare il pacchetto dell'applicazione di esempio e testarla</span><span class="sxs-lookup"><span data-stu-id="f2d62-171">Package and test the sample application</span></span> 

1. <span data-ttu-id="f2d62-172">Compilare l'applicazione di esempio con Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2d62-172">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P mysql
   ```

1. <span data-ttu-id="f2d62-173">Avviare l'applicazione di esempio. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2d62-173">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="f2d62-174">Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2d62-174">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="f2d62-175">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2d62-175">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="f2d62-176">Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2d62-176">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="f2d62-177">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2d62-177">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="f2d62-178">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f2d62-178">Summary</span></span>

<span data-ttu-id="f2d62-179">In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni in un database MySQL di Azure con JPA.</span><span class="sxs-lookup"><span data-stu-id="f2d62-179">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure MySQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2d62-180">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="f2d62-180">Next steps</span></span>

<span data-ttu-id="f2d62-181">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="f2d62-181">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f2d62-182">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="f2d62-182">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="f2d62-183">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f2d62-183">Additional Resources</span></span>

<span data-ttu-id="f2d62-184">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="f2d62-184">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[MYSQL01]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png
[MYSQL03]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-03.png
[MYSQL04]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-05.png
