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
# <a name="azure-database-for-postgresql-libraries-for-java"></a>Librerie del Database di Azure per PostgreSQL per Java

## <a name="overview"></a>Panoramica

[Database di Azure per PostgreSQL](/azure/sql-database/sql-database-technical-overview) è un servizio di database relazionale in Azure creato per gli sviluppatori e basato sulla versione della community del motore di database [PostgreSQL](https://www.postgresql.org/) open source.

Per iniziare a usare il Database di Azure per PostgreSQL, vedere [Usare Java per connettersi ai dati ed eseguire query](/azure/postgresql/connect-java).

## <a name="client-jdbc-driver"></a>Driver JDBC client

Connettersi al Database di Azure per PostgreSQL dalle applicazioni usando il [driver JDBC per PostgreSQL](https://jdbc.postgresql.org/) open source. È possibile usare l'[API Java JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) per connettersi direttamente al database o usare i framework di accesso ai dati che interagiscono con il database tramite JDBC, ad esempio [Hibernate](http://hibernate.org/).

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare il driver JDBC client nel progetto.  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a>Esempio

Connettersi al Database di Azure per PostgreSQL usando JDBC e selezionare tutti i record nella tabella relativa alle vendite. È possibile ottenere la stringa di connessione JDBC per il database dal portale di Azure.

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

## <a name="samples"></a>Esempi

[Progettare un database PostgreSQL tramite l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

Esplorare altri [esempi di codice Java per il Database di Azure per PostgreSQL](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres) disponibili per l'uso nelle app.
