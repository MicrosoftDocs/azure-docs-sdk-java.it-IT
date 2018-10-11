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
ms.openlocfilehash: 37f7d3caf10e6b709cee2452c63a543d49e0ead8
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893312"
---
# <a name="azure-sql-database-libraries-for-java"></a>Librerie del database SQL di Azure per Java

## <a name="overview"></a>Panoramica

Il [database SQL di Azure](/azure/sql-database/sql-database-technical-overview) è un database come servizio relazionale che usa il motore Microsoft SQL Server che supporta le tabelle, JSON, i dati spaziali e i dati XML. 

Per iniziare a usare il database SQL di Azure, vedere [Database SQL di Azure: Usare Java per connettersi ai dati ed eseguire query](/azure/sql-database/sql-database-connect-query-java).

## <a name="client-jdbc-driver"></a>Driver JDBC client

Connettersi al database SQL di Azure SQL dalle applicazioni usando il [driver JDBC per il database SQL](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server). È possibile usare l'[API Java JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) per connettersi direttamente al database o usare i framework di accesso ai dati che interagiscono con il database tramite JDBC, ad esempio [Hibernate](http://hibernate.org/).

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare il driver JDBC client nel progetto.


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a>Esempio

Connettersi al database SQL e selezionare tutti i record di una tabella usando JDBC.

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a>API di gestione

Creare e gestire le risorse del database SQL di Azure nella sottoscrizione con l'API di gestione.   

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/sql/management)

### <a name="example"></a>Esempio

Creare una risorsa del database SQL e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a>Esempi

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

Esplorare altri [esempi di codice Java per il database SQL di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL) disponibili per l'uso nelle app.