---
title: Librerie di Database di Azure per MySQL per Java
description: Documentazione di riferimento per le librerie client Java per Database di Azure per MySQL
keywords: Azure, Java, SDK, API, SQL, database, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: 72c94ef4bdad7adeae63da2efda93b52a9afef53
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="azure-database-for-mysql-libraries-for-java"></a><span data-ttu-id="586e7-104">Librerie di Database di Azure per MySQL per Java</span><span class="sxs-lookup"><span data-stu-id="586e7-104">Azure Database for MySQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="586e7-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="586e7-105">Overview</span></span>

<span data-ttu-id="586e7-106">[Database di Azure per MySQL](/azure/sql-database/sql-database-technical-overview) è un servizio di database relazionale basato sul motore del server open source [MySQL](https://www.mysql.com/).</span><span class="sxs-lookup"><span data-stu-id="586e7-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) is a relational database service based on the open source [MySQL](https://www.mysql.com/) Server engine.</span></span> 

<span data-ttu-id="586e7-107">Per iniziare a usare il Database di Azure per MySQL, vedere [Usare Java per connettersi ai dati ed eseguire query](/azure/mysql/connect-java).</span><span class="sxs-lookup"><span data-stu-id="586e7-107">To get started with Azure Database for MySQL, see [Use Java to connect and query data](/azure/mysql/connect-java).</span></span>

## <a name="client-jbdc-driver"></a><span data-ttu-id="586e7-108">Driver JBDC client</span><span class="sxs-lookup"><span data-stu-id="586e7-108">Client JBDC driver</span></span>

<span data-ttu-id="586e7-109">Connettersi al Database di Azure per MySQL dalle applicazioni usando il [driver JDBC per MySQL](https://dev.mysql.com/downloads/connector/j/) open source.</span><span class="sxs-lookup"><span data-stu-id="586e7-109">Connect to Azure Database for MySQL from your applications using the open-source [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).</span></span> <span data-ttu-id="586e7-110">È possibile usare l'[API Java JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) per connettersi direttamente al database o usare i framework di accesso ai dati che interagiscono con il database tramite JDBC, ad esempio [Hibernate](http://hibernate.org/).</span><span class="sxs-lookup"><span data-stu-id="586e7-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="586e7-111">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare il driver JDBC client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="586e7-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="586e7-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="586e7-112">Example</span></span>

<span data-ttu-id="586e7-113">Connettersi al Database di Azure per MySQL usando JDBC e selezionare tutti i record nella tabella relativa alle vendite.</span><span class="sxs-lookup"><span data-stu-id="586e7-113">Connect to Azure Database for MySQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="586e7-114">È possibile ottenere la stringa di connessione JDBC per il database dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="586e7-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="586e7-115">Esempi</span><span class="sxs-lookup"><span data-stu-id="586e7-115">Samples</span></span>

<span data-ttu-id="586e7-116">[Compilare un'app Web Java e MySQL](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span><span class="sxs-lookup"><span data-stu-id="586e7-116">[Build a Java and MySQL web app](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span></span>  
[<span data-ttu-id="586e7-117">Progettare un database MySQL tramite l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="586e7-117">Design a MySQL database using the Azure CLI</span></span>](/azure/mysql/tutorial-design-database-using-cli)   

<span data-ttu-id="586e7-118">Esplorare altri [esempi di codice Java per il Database di Azure per MySQL](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="586e7-118">Explore more [sample Java code for Azure Database for MySQL](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql) you can use in your apps.</span></span>
