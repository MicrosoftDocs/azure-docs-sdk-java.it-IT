---
title: Librerie del Database di Azure per PostgreSQL per Java
description: Documentazione di riferimento per le librerie client Java per il Database di Azure per PostgreSQL
keywords: Azure, Java, SDK, API, SQL, database, PostGres, PostgreSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.devlang: java
ms.service: postgresql
ms.openlocfilehash: 0f93d26a07de9acf72a46792793b573dba878fba
ms.sourcegitcommit: 1b22376e4ceb3d2f2734c8fc80823a44cc5fe8fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42703320"
---
# <a name="azure-database-for-postgresql-libraries-for-java"></a><span data-ttu-id="6d735-104">Librerie del Database di Azure per PostgreSQL per Java</span><span class="sxs-lookup"><span data-stu-id="6d735-104">Azure Database for PostgreSQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="6d735-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6d735-105">Overview</span></span>

<span data-ttu-id="6d735-106">[Database di Azure per PostgreSQL](/azure/sql-database/sql-database-technical-overview) è un servizio di database relazionale in Azure creato per gli sviluppatori e basato sulla versione della community del motore di database [PostgreSQL](https://www.postgresql.org/) open source.</span><span class="sxs-lookup"><span data-stu-id="6d735-106">[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) is a relational database service in Azure built for developers based on the community version of open source [PostgreSQL](https://www.postgresql.org/) database engine.</span></span>

<span data-ttu-id="6d735-107">Per iniziare a usare il Database di Azure per PostgreSQL, vedere [Usare Java per connettersi ai dati ed eseguire query](/azure/postgresql/connect-java).</span><span class="sxs-lookup"><span data-stu-id="6d735-107">To get started with Azure Database for PostgreSQL, see [Use Java to connect and query data](/azure/postgresql/connect-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="6d735-108">Driver JDBC client</span><span class="sxs-lookup"><span data-stu-id="6d735-108">Client JDBC driver</span></span>

<span data-ttu-id="6d735-109">Connettersi al Database di Azure per PostgreSQL dalle applicazioni usando il [driver JDBC per PostgreSQL](https://jdbc.postgresql.org/) open source.</span><span class="sxs-lookup"><span data-stu-id="6d735-109">Connect to Azure Database for PostgreSQL from your applications using the open-source [PostgreSQL JDBC driver](https://jdbc.postgresql.org/).</span></span> <span data-ttu-id="6d735-110">È possibile usare l'[API Java JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) per connettersi direttamente al database o usare i framework di accesso ai dati che interagiscono con il database tramite JDBC, ad esempio [Hibernate](http://hibernate.org/).</span><span class="sxs-lookup"><span data-stu-id="6d735-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="6d735-111">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare il driver JDBC client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="6d735-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="6d735-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="6d735-112">Example</span></span>

<span data-ttu-id="6d735-113">Connettersi al Database di Azure per PostgreSQL usando JDBC e selezionare tutti i record nella tabella relativa alle vendite.</span><span class="sxs-lookup"><span data-stu-id="6d735-113">Connect to Azure Database for PostgreSQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="6d735-114">È possibile ottenere la stringa di connessione JDBC per il database dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="6d735-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:postgresql://mypostgresdb.postgres.database.azure.com:5432/mydb?user=frank@mypostgresdb&password=AbCdEfGhIjK&ssl=true");
Connection connection = null;
try {
    connection = DriverManager.getConnection(url);
    String selectSql = "SELECT * FROM SALES";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="6d735-115">Esempi</span><span class="sxs-lookup"><span data-stu-id="6d735-115">Samples</span></span>

[<span data-ttu-id="6d735-116">Progettare un database PostgreSQL tramite l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="6d735-116">Design a PostgreSQL database using the Azure CLI</span></span>](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

<span data-ttu-id="6d735-117">Esplorare altri [esempi di codice Java per il Database di Azure per PostgreSQL](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="6d735-117">Explore more [sample Java code for Azure Database for PostgreSQL](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres) you can use in your apps.</span></span>
