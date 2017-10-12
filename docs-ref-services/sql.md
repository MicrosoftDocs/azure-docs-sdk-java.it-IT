---
title: Librerie del database SQL di Azure per Java
description: Connettersi al database SQL di Azure tramite il driver JDBC le istanze di gestione del database SQL di Azure con l'API di gestione.
keywords: Azure, Java, SDK, API, SQL, database, JDBC
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/05/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: sql-database
ms.openlocfilehash: 3ae4015ae57e5eac4dafb30f4a42881986501853
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2017
---
# <a name="azure-sql-database-libraries-for-java"></a><span data-ttu-id="e2ef3-104">Librerie del database SQL di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="e2ef3-104">Azure SQL Database libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e2ef3-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e2ef3-105">Overview</span></span>

<span data-ttu-id="e2ef3-106">Il [database SQL di Azure](/azure/sql-database/sql-database-technical-overview) è un database come servizio relazionale che usa il motore Microsoft SQL Server che supporta le tabelle, JSON, i dati spaziali e i dati XML.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-106">[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) is a relational database service using the Microsoft SQL Server engine that supports table, JSON, spatial, and XML data.</span></span> 

<span data-ttu-id="e2ef3-107">Per iniziare a usare il database SQL di Azure, vedere [Database SQL di Azure: Usare Java per connettersi ai dati ed eseguire query](/azure/sql-database/sql-database-connect-query-java).</span><span class="sxs-lookup"><span data-stu-id="e2ef3-107">To get started with Azure SQL Database, see [Azure SQL Database: Use Java to connect and query data](/azure/sql-database/sql-database-connect-query-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="e2ef3-108">Driver JDBC client</span><span class="sxs-lookup"><span data-stu-id="e2ef3-108">Client JDBC driver</span></span>

<span data-ttu-id="e2ef3-109">Connettersi al database SQL di Azure SQL dalle applicazioni usando il [driver JDBC per il database SQL](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server).</span><span class="sxs-lookup"><span data-stu-id="e2ef3-109">Connect to Azure SQL Database from your applications using the [SQL Database JDBC driver](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server).</span></span> <span data-ttu-id="e2ef3-110">È possibile usare l'[API Java JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) per connettersi direttamente al database o usare i framework di accesso ai dati che interagiscono con il database tramite JDBC, ad esempio [Hibernate](http://hibernate.org/).</span><span class="sxs-lookup"><span data-stu-id="e2ef3-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect with the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="e2ef3-111">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare il driver JDBC client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="e2ef3-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="e2ef3-112">Example</span></span>

<span data-ttu-id="e2ef3-113">Connettersi al database SQL e selezionare tutti i record di una tabella usando JDBC.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-113">Connect to SQL database and select all records in a table using JDBC.</span></span>

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a><span data-ttu-id="e2ef3-114">API di gestione</span><span class="sxs-lookup"><span data-stu-id="e2ef3-114">Management API</span></span>

<span data-ttu-id="e2ef3-115">Creare e gestire le risorse del database SQL di Azure nella sottoscrizione con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-115">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span>   

<span data-ttu-id="e2ef3-116">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e2ef3-117">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="e2ef3-117">Explore the Management APIs</span></span>](/java/api/overview/azure/sql/managementapi)

### <a name="example"></a><span data-ttu-id="e2ef3-118">Esempio</span><span class="sxs-lookup"><span data-stu-id="e2ef3-118">Example</span></span>

<span data-ttu-id="e2ef3-119">Creare una risorsa del database SQL e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-119">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a><span data-ttu-id="e2ef3-120">Esempi</span><span class="sxs-lookup"><span data-stu-id="e2ef3-120">Samples</span></span>

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

<span data-ttu-id="e2ef3-121">Esplorare altri [esempi di codice Java per il database SQL di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="e2ef3-121">Explore more [sample Java code for Azure SQL Database](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL) you can use in your apps.</span></span>