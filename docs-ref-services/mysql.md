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
ms.locfileid: "21931017"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a>Librerie di Database di Azure per MySQL per Java

## <a name="overview"></a>Panoramica

[Database di Azure per MySQL](/azure/sql-database/sql-database-technical-overview) è un servizio di database relazionale basato sul motore del server open source [MySQL](https://www.mysql.com/). 

Per iniziare a usare il Database di Azure per MySQL, vedere [Usare Java per connettersi ai dati ed eseguire query](/azure/mysql/connect-java).

## <a name="client-jbdc-driver"></a>Driver JBDC client

Connettersi al Database di Azure per MySQL dalle applicazioni usando il [driver JDBC per MySQL](https://dev.mysql.com/downloads/connector/j/) open source. È possibile usare l'[API Java JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) per connettersi direttamente al database o usare i framework di accesso ai dati che interagiscono con il database tramite JDBC, ad esempio [Hibernate](http://hibernate.org/).

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare il driver JDBC client nel progetto.  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a>Esempio

Connettersi al Database di Azure per MySQL usando JDBC e selezionare tutti i record nella tabella relativa alle vendite. È possibile ottenere la stringa di connessione JDBC per il database dal portale di Azure.

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a>Esempi

[Compilare un'app Web Java e MySQL](/azure/app-service-web/app-service-web-tutorial-java-mysql)   
[Progettare un database MySQL tramite l'interfaccia della riga di comando di Azure](/azure/mysql/tutorial-design-database-using-cli)   

Esplorare altri [esempi di codice Java per il Database di Azure per MySQL](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql) disponibili per l'uso nelle app.
