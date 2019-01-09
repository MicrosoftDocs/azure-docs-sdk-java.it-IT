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
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a><span data-ttu-id="95058-103">Come usare l'API MongoDB per Spring Data con Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="95058-103">How to use Spring Data MongoDB API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="95058-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="95058-104">Overview</span></span>

<span data-ttu-id="95058-105">Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni con l'[API MongoDB di Azure Cosmos DB](/azure/cosmos-db/mongodb-introduction).</span><span class="sxs-lookup"><span data-stu-id="95058-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95058-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="95058-106">Prerequisites</span></span>

<span data-ttu-id="95058-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="95058-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="95058-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="95058-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="95058-109">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="95058-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="95058-110">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="95058-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="95058-111">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="95058-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="95058-112">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="95058-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="95058-113">Creare un account Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="95058-113">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="95058-114">Creare un account Cosmos DB con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="95058-114">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="95058-115">Per informazioni più dettagliate sulla creazione di account Azure Cosmos DB, vedere [Documentazione di Azure Cosmos DB](/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="95058-115">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="95058-116">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="95058-116">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="95058-117">Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="95058-117">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Creare un account Azure Cosmos DB][COSMOSDB01]

1. <span data-ttu-id="95058-119">Specificare le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="95058-119">Specify the following information:</span></span>

   - <span data-ttu-id="95058-120">**Sottoscrizione**: specificare la sottoscrizione di Azure da usare.</span><span class="sxs-lookup"><span data-stu-id="95058-120">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="95058-121">**Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="95058-121">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="95058-122">**Account name** (Nome dell'account): scegliere un nome univoco per l'account Cosmos DB. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoysmongodb.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="95058-122">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoysmongodb.documents.azure.com*.</span></span>
   - <span data-ttu-id="95058-123">**API**: per questa esercitazione specificare `Azure Cosmos DB for MongoDB API`.</span><span class="sxs-lookup"><span data-stu-id="95058-123">**API**: Specify `Azure Cosmos DB for MongoDB API` for this tutorial.</span></span>
   - <span data-ttu-id="95058-124">**Località**: specificare l'area geografica più vicina per il database.</span><span class="sxs-lookup"><span data-stu-id="95058-124">**Location**: Specify the closest geographic region for your database.</span></span>

   ![Specificare le impostazioni dell'account Cosmos DB][COSMOSDB02]
   
1. <span data-ttu-id="95058-126">Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Rivedi e crea**.</span><span class="sxs-lookup"><span data-stu-id="95058-126">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="95058-127">Se tutte le informazioni contenute nella pagina visualizzata risultano corrette, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="95058-127">If everything looks correct on the review page, click **Create**.</span></span>

   ![Rivedere le impostazioni dell'account Cosmos DB][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a><span data-ttu-id="95058-129">Recuperare la stringa di connessione per l'account Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="95058-129">Retrieve the connection string for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="95058-130">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="95058-130">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="95058-131">Fare clic su **Tutte le risorse** e quindi sull'account Azure Cosmos DB appena creato.</span><span class="sxs-lookup"><span data-stu-id="95058-131">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![Selezionare l'account Azure Cosmos DB][COSMOSDB04]

1. <span data-ttu-id="95058-133">Fare clic su **Stringhe di connessione** e copiare il valore del campo **Stringa di connessione primaria**. Questo valore verrà usato più avanti per configurare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="95058-133">Click **Connection strings**, and copy the value for the **Primary Connection String** field; you will use that value to configure your application later.</span></span>

   ![Recuperare la stringa di connessione di Cosmos DB][COSMOSDB06]

## <a name="configure-the-sample-application"></a><span data-ttu-id="95058-135">Configurare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="95058-135">Configure the sample application</span></span>

1. <span data-ttu-id="95058-136">Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="95058-136">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. <span data-ttu-id="95058-137">Creare una directory *resources* nella directory *&lt;radice progetto&gt;/complete/src/main* del progetto di esempio e quindi un file *application.properties* nella directory *resources*.</span><span class="sxs-lookup"><span data-stu-id="95058-137">Create a *resources* directory in the *&lt;project root&gt;/complete/src/main* directory of the sample project, and create an *application.properties* file in the *resources* directory.</span></span>

1. <span data-ttu-id="95058-138">Aprire il file *application.properties* in un editor di testo e aggiungere le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:</span><span class="sxs-lookup"><span data-stu-id="95058-138">Open the *application.properties* file in a text editor, and add the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   <span data-ttu-id="95058-139">Dove:</span><span class="sxs-lookup"><span data-stu-id="95058-139">Where:</span></span>

   | <span data-ttu-id="95058-140">Parametro</span><span class="sxs-lookup"><span data-stu-id="95058-140">Parameter</span></span> | <span data-ttu-id="95058-141">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="95058-141">Description</span></span> |
   |---|---|
   | `spring.data.mongodb.database` | <span data-ttu-id="95058-142">Specifica il nome dell'account Cosmos DB dei passaggi precedenti di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="95058-142">Specifies the name of your Cosmos DB account from earlier in this article.</span></span> |
   | `spring.data.mongodb.uri` | <span data-ttu-id="95058-143">Specifica il valore di **Stringa di connessione primaria** copiato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="95058-143">Specifies the **Primary Connection String** from earlier in this article.</span></span> |

1. <span data-ttu-id="95058-144">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="95058-144">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="95058-145">Creare il pacchetto dell'applicazione di esempio e testarla</span><span class="sxs-lookup"><span data-stu-id="95058-145">Package and test the sample application</span></span> 

1. <span data-ttu-id="95058-146">Compilare l'applicazione di esempio con Maven e configurare Maven per ignorare i test. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95058-146">Build the sample application with Maven, and configure Maven to skip tests; for example:</span></span>

   ```shell
   mvn clean package -DskipTests
   ```

1. <span data-ttu-id="95058-147">Avviare l'applicazione di esempio. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95058-147">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   <span data-ttu-id="95058-148">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="95058-148">Your application should return values like the following:</span></span>

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

## <a name="summary"></a><span data-ttu-id="95058-149">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="95058-149">Summary</span></span>

<span data-ttu-id="95058-150">In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni con l'API MongoDB di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="95058-150">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB MongoDB API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95058-151">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="95058-151">Next steps</span></span>

<span data-ttu-id="95058-152">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="95058-152">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="95058-153">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="95058-153">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="95058-154">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="95058-154">Additional Resources</span></span>

<span data-ttu-id="95058-155">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="95058-155">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png
