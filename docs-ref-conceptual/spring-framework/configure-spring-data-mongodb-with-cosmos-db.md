---
title: Come usare l'API MongoDB per Spring Data con Azure Cosmos DB
description: Informazioni su come usare l'API MongoDB per Spring Data con Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 55fb356820e2cc5eb8d1fe4030053d9e580583af
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992403"
---
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a>Come usare l'API MongoDB per Spring Data con Azure Cosmos DB

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni con l'[API MongoDB di Azure Cosmos DB](/azure/cosmos-db/mongodb-introduction).

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.
* Un client [Git](https://git-scm.com/downloads).

## <a name="create-an-azure-cosmos-db-account"></a>Creare un account Azure Cosmos DB

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>Creare un account Cosmos DB con il portale di Azure

> [!NOTE]
> 
> Per informazioni più dettagliate sulla creazione di account Azure Cosmos DB, vedere [Documentazione di Azure Cosmos DB](/azure/cosmos-db/).

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Azure Cosmos DB**.

   ![Creare un account Azure Cosmos DB][COSMOSDB01]

1. Specificare le informazioni seguenti.

   - **Sottoscrizione**: specificare la sottoscrizione di Azure da usare.
   - **Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.
   - **Account name** (Nome dell'account): scegliere un nome univoco per l'account Cosmos DB. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoysmongodb.documents.azure.com*.
   - **API**: per questa esercitazione specificare `Azure Cosmos DB for MongoDB API`.
   - **Località**: specificare l'area geografica più vicina per il database.

   ![Specificare le impostazioni dell'account Cosmos DB][COSMOSDB02]
   
1. Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Rivedi e crea**.

1. Se tutte le informazioni contenute nella pagina visualizzata risultano corrette, fare clic su **Crea**.

   ![Rivedere le impostazioni dell'account Cosmos DB][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a>Recuperare la stringa di connessione per l'account Azure Cosmos DB

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sull'account Azure Cosmos DB appena creato.

   ![Selezionare l'account Azure Cosmos DB][COSMOSDB04]

1. Fare clic su **Stringhe di connessione** e copiare il valore del campo **Stringa di connessione primaria**. Questo valore verrà usato più avanti per configurare l'applicazione.

   ![Recuperare la stringa di connessione di Cosmos DB][COSMOSDB06]

## <a name="configure-the-sample-application"></a>Configurare l'applicazione di esempio

1. Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. Creare una directory *resources* nella directory *&lt;radice progetto&gt;/complete/src/main* del progetto di esempio e quindi un file *application.properties* nella directory *resources*.

1. Aprire il file *application.properties* in un editor di testo e aggiungere le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   Dove:

   | Parametro | DESCRIZIONE |
   |---|---|
   | `spring.data.mongodb.database` | Specifica il nome dell'account Cosmos DB dei passaggi precedenti di questo articolo. |
   | `spring.data.mongodb.uri` | Specifica il valore di **Stringa di connessione primaria** copiato in precedenza in questo articolo. |

1. Salvare e chiudere il file *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Creare il pacchetto dell'applicazione di esempio e testarla 

1. Compilare l'applicazione di esempio con Maven e configurare Maven per ignorare i test. Ad esempio:

   ```shell
   mvn clean package -DskipTests
   ```

1. Avviare l'applicazione di esempio. Ad esempio:

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   L'applicazione dovrebbe restituire valori come i seguenti:

   ```json
   Customers found with findAll():
   -------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   
   Customer found with findByFirstName('Alice'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customers found with findByLastName('Smith'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   ```

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni con l'API MongoDB di Azure Cosmos DB.

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

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png
