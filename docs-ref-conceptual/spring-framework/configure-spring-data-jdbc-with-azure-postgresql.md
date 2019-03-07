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
ms.openlocfilehash: 29f3c957dd0ccd754eedef12e3fc01c3484dddf3
ms.sourcegitcommit: 1c1412ad5d8960975c3fc7fd3d1948152ef651ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57335394"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-postgresql"></a>Come usare Spring Data JDBC con Azure PostgreSQL

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni in un database [PostgreSQL](https://www.postgresql.org/) di Azure con [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.
* [Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.
* Utilità da riga di comando [psql](https://www.postgresql.org/docs/current/app-psql.html).
* Un client [Git](https://git-scm.com/downloads).

## <a name="create-a-postgresql-database-for-azure"></a>Creare un database PostgreSQL per Azure

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a>Creare un server di database PostgreSQL con il portale di Azure

> [!NOTE]
> 
> Per informazioni più dettagliate sulla creazione di database PostgreSQL, vedere [Creare un server Database di Azure per PostgreSQL nel portale di Azure](/azure/postgresql/quickstart-create-server-database-portal).

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Database di Azure per PostgreSQL**.

   ![Creare un database PostgreSQL][POSTGRESQL01]

1. Immettere le seguenti informazioni:

   - **Nome server**: scegliere un nome univoco per il server PostgreSQL. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoyspostgresql.postgres.database.azure.com*.
   - **Sottoscrizione** specificare la sottoscrizione di Azure da usare.
   - **Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.
   - **Selezionare l'origine**: per questa esercitazione selezionare `Blank` per creare un nuovo database.
   - **Account di accesso amministratore server**: specificare il nome dell'amministratore del database.
   - **Password** e **Conferma password**: specificare la password dell'amministratore del database.
   - **Località**: specificare l'area geografica più vicina per il database.
   - **Versione**: specificare la versione del database più aggiornata.
   - **Piano tariffario**: per questa esercitazione specificare il piano tariffario meno costoso.

   ![Creare le proprietà del database PostgreSQL][POSTGRESQL02]

1. Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Crea**.

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a>Configurare una regola del firewall per il server di database PostgreSQL con il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sul database PostgreSQL appena creato.

   ![Selezionare il database PostgreSQL][POSTGRESQL03]

1. Fare clic su **Sicurezza delle connessioni**, creare una nuova regola specificando un nome univoco in **Regole del firewall**, immettere l'intervallo di indirizzi IP che dovrà avere accesso al database e quindi fare clic su **Salva**.

   ![Configurare la sicurezza delle connessioni][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a>Recuperare la stringa di connessione per il server PostgreSQL con il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sul database PostgreSQL appena creato.

   ![Selezionare il database PostgreSQL][POSTGRESQL03]

1. Fare clic su **Stringhe di connessione** e copiare il valore del campo di testo **JDBC**.

   ![Recuperare la stringa di connessione JDBC][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a>Creare un database PostgreSQL con l'utilità da riga di comando `psql`

1. Aprire una shell dei comandi e connettersi al server PostgreSQL immettendo un comando `psql` come quello riportato nell'esempio seguente:

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   Dove:

   | Parametro | DESCRIZIONE |
   |---|---|
   | `host` | Specifica il nome completo del server PostgreSQL dei passaggi precedenti di questo articolo. |
   | `host` | Specifica la porta del server PostgreSQL, che per impostazione predefinita è `5432`. |
   | `username` | Specifica l'amministratore PostgreSQL e il nome abbreviato del server dei passaggi precedenti di questo articolo. |
   | `dbname` | Specifica che per il momento si vuole usare il database `postgres` predefinito. |

   La risposta visualizzata dal server PostgreSQL sarà simile all'esempio seguente:

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. Creare un database denominato *mypgsqldb* immettendo un comando `psql` come quello riportato nell'esempio seguente:

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   La risposta visualizzata dal server PostgreSQL sarà simile all'esempio seguente:

   ```shell
   CREATE DATABASE
   ```

1. FACOLTATIVO: è possibile verificare la creazione del database immettendo `\l` in `psql`. La risposta del server PostgreSQL sarà simile all'esempio seguente:

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

1. Immettere `\q` per uscire dall'utilità `psql`.

## <a name="configure-the-sample-application"></a>Configurare l'applicazione di esempio

1. Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.

1. Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   Dove:

   | Parametro | DESCRIZIONE |
   |---|---|
   | `spring.datasource.url` | Specifica la stringa JDBC per PostgreSQL dei passaggi precedenti di questo articolo. |
   | `spring.datasource.username` | Specifica il nome dell'amministratore PostgreSQL dei passaggi precedenti di questo articolo, cui viene aggiunto il nome abbreviato del server. |
   | `spring.datasource.password` | Specifica la password dell'amministratore PostgreSQL dei passaggi precedenti di questo articolo. |

1. Salvare e chiudere il file *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Creare il pacchetto dell'applicazione di esempio e testarla 

1. Compilare l'applicazione di esempio con Maven. Ad esempio:

   ```shell
   mvn clean package -P postgresql
   ```

1. Avviare l'applicazione di esempio. Ad esempio:

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   L'applicazione dovrebbe restituire valori come i seguenti:

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   L'applicazione dovrebbe restituire valori come i seguenti:

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>Summary

In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni in un database PostgreSQL di Azure con JDBC.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

<!-- URL List -->

[Azure per sviluppatori Java]: /java/azure/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
