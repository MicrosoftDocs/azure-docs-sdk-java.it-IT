---
title: Come usare Spring Data JDBC con un database SQL di Azure
description: Informazioni su come usare Spring Data JDBC con un database SQL di Azure.
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
ms.openlocfilehash: 92f489b513eeb05efaacdf07af2b976de8f84868
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992323"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-sql-database"></a>Come usare Spring Data JDBC con un database SQL di Azure

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni in un [database SQL di Azure](https://azure.microsoft.com/services/sql-database/) con [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.
* [Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.
* Un client [Git](https://git-scm.com/downloads).

## <a name="create-an-azure-sql-satabase"></a>Creare un database SQL di Azure

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>Creare un server di database SQL con il portale di Azure

> [!NOTE]
> 
> Per informazioni più dettagliate sulla creazione di database SQL di Azure, vedere [Creare un database SQL di Azure nel portale di Azure](/azure/sql-database/sql-database-get-started-portal).

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Database SQL**.

   ![Creazione di un database SQL][SQL01]

1. Specificare le informazioni seguenti.

   - **Nome database**: scegliere un nome univoco per il database SQL, che verrà creato nel server SQL specificato in seguito.
   - **Sottoscrizione**: specificare la sottoscrizione di Azure da usare.
   - **Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.
   - **Selezionare l'origine**: per questa esercitazione selezionare `Blank database` per creare un nuovo database.

   ![Specificare le proprietà del database SQL][SQL02]
   
1. Fare clic su **Server** e su **Crea un nuovo server** e quindi specificare le informazioni seguenti.

   - **Nome server**: scegliere un nome univoco per il server SQL. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoyssql.database.windows.net*.
   - **Account di accesso amministratore server**: specificare il nome dell'amministratore del database.
   - **Password** e **Conferma password**: specificare la password dell'amministratore del database.
   - **Località**: specificare l'area geografica più vicina per il database.

   ![Specificare il server SQL][SQL03]

1. Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Seleziona**.

1. Per questa esercitazione specificare il **piano tariffario** meno costoso e quindi fare clic su **Crea**.

   ![Creare il database SQL][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>Configurare una regola del firewall per il server SQL con il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sul server SQL appena creato.

   ![Selezionare il server SQL][SQL05]

1. Nella sezione **Panoramica** fare clic su **Mostra impostazioni firewall**.

   ![Mostra impostazioni firewall][SQL06]

1. Nella sezione **Firewall e reti virtuali** creare una nuova regola specificando un nome univoco, immettere l'intervallo di indirizzi IP che dovrà avere accesso al database e quindi fare clic su **Salva**.

   ![Configurare le impostazioni del firewall][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>Recuperare la stringa di connessione per il server SQL con il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sul database SQL appena creato.

   ![Selezionare il database SQL][SQL08]

1. Fare clic su **Stringhe di connessione** e quindi su **JDBC** e copiare il valore del campo di testo JDBC.

   ![Recuperare la stringa di connessione JDBC][SQL09]

## <a name="configure-the-sample-application"></a>Configurare l'applicazione di esempio

1. Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.

1. Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   Dove:

   | Parametro | DESCRIZIONE |
   |---|---|
   | `spring.datasource.url` | Specifica una versione modificata della stringa JDBC per SQL dei passaggi precedenti di questo articolo. |
   | `spring.datasource.username` | Specifica il nome dell'amministratore SQL dei passaggi precedenti di questo articolo, cui viene aggiunto il nome abbreviato del server. |
   | `spring.datasource.password` | Specifica la password dell'amministratore SQL dei passaggi precedenti di questo articolo. |

1. Salvare e chiudere il file *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Creare il pacchetto dell'applicazione di esempio e testarla 

1. Compilare l'applicazione di esempio con Maven. Ad esempio:

   ```shell
   mvn clean package -P sql
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

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni in un database SQL di Azure con JDBC.

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

[SQL01]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-09.png
