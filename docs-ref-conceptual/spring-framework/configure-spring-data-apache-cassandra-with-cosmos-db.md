---
title: Come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB
description: Informazioni su come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB.
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
ms.openlocfilehash: 1f3f7a55b49d64077df9e7d4d6acc3af4495b424
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992326"
---
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a><span data-ttu-id="74460-103">Come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="74460-103">How to use Spring Data Apache Cassandra API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="74460-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="74460-104">Overview</span></span>

<span data-ttu-id="74460-105">Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni con l'[API Cassandra di Azure Cosmos DB](/azure/cosmos-db/cassandra-introduction).</span><span class="sxs-lookup"><span data-stu-id="74460-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74460-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="74460-106">Prerequisites</span></span>

<span data-ttu-id="74460-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="74460-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="74460-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="74460-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="74460-109">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="74460-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="74460-110">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="74460-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="74460-111">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="74460-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="74460-112">[Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="74460-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="74460-113">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="74460-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="74460-114">Creare un account Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="74460-114">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="74460-115">Creare un account Cosmos DB con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="74460-115">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="74460-116">Per informazioni più dettagliate sulla creazione di account Azure Cosmos DB, vedere [Documentazione di Azure Cosmos DB](/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="74460-116">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="74460-117">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="74460-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="74460-118">Fare clic su **+Crea una risorsa**, quindi su **Database** e infine su **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="74460-118">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Creare un account Azure Cosmos DB][COSMOSDB01]

1. <span data-ttu-id="74460-120">Specificare le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="74460-120">Specify the following information:</span></span>

   - <span data-ttu-id="74460-121">**Sottoscrizione**: specificare la sottoscrizione di Azure da usare.</span><span class="sxs-lookup"><span data-stu-id="74460-121">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="74460-122">**Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="74460-122">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="74460-123">**Account name** (Nome dell'account): scegliere un nome univoco per l'account Cosmos DB. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoyscassandra.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="74460-123">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoyscassandra.documents.azure.com*.</span></span>
   - <span data-ttu-id="74460-124">**API**: per questa esercitazione specificare `Cassandra`.</span><span class="sxs-lookup"><span data-stu-id="74460-124">**API**: Specify `Cassandra` for this tutorial.</span></span>
   - <span data-ttu-id="74460-125">**Località**: specificare l'area geografica più vicina per il database.</span><span class="sxs-lookup"><span data-stu-id="74460-125">**Location**: Specify the closest geographic region for your database.</span></span>

   ![Specificare le impostazioni dell'account Cosmos DB][COSMOSDB02]
   
1. <span data-ttu-id="74460-127">Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Rivedi e crea**.</span><span class="sxs-lookup"><span data-stu-id="74460-127">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="74460-128">Se tutte le informazioni contenute nella pagina visualizzata risultano corrette, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="74460-128">If everything looks correct on the review page, click **Create**.</span></span>

   ![Rivedere le impostazioni dell'account Cosmos DB][COSMOSDB03]

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a><span data-ttu-id="74460-130">Aggiungere un keyspace all'account Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="74460-130">Add a keyspace to your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="74460-131">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="74460-131">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="74460-132">Fare clic su **Tutte le risorse** e quindi sull'account Azure Cosmos DB appena creato.</span><span class="sxs-lookup"><span data-stu-id="74460-132">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![Selezionare l'account Azure Cosmos DB][COSMOSDB04]

1. <span data-ttu-id="74460-134">Fare clic su **Esplora dati** e quindi su **New Keyspace** (Nuovo keyspace).</span><span class="sxs-lookup"><span data-stu-id="74460-134">Click **Data Explorer**, then click **New Keyspace**.</span></span> <span data-ttu-id="74460-135">Immettere un identificatore univoco in **Keyspace id** (ID keyspace) e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="74460-135">Enter a unique identifier for your **Keyspace id**, then click **OK**.</span></span>

   ![Creare un keyspace Cosmos DB][COSMOSDB05]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a><span data-ttu-id="74460-137">Recuperare le impostazioni di connessione per l'account Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="74460-137">Retrieve the connection settings for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="74460-138">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="74460-138">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="74460-139">Fare clic su **Tutte le risorse** e quindi sull'account Azure Cosmos DB appena creato.</span><span class="sxs-lookup"><span data-stu-id="74460-139">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![Selezionare l'account Azure Cosmos DB][COSMOSDB04]

1. <span data-ttu-id="74460-141">Fare clic su **Stringhe di connessione** e copiare i valori dei campi **Punto di contatto**, **Porta**, **Nome utente** e **Password primaria**. Questi valori verranno usati più avanti per configurare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="74460-141">Click **Connection strings**, and copy the values for the **Contact Point**, **Port**, **Username**, and **Primary Password** fields; you will use those values to configure your application later.</span></span>

   ![Recuperare le impostazioni di connessione di Cosmos DB][COSMOSDB05]

## <a name="configure-the-sample-application"></a><span data-ttu-id="74460-143">Configurare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="74460-143">Configure the sample application</span></span>

1. <span data-ttu-id="74460-144">Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="74460-144">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. <span data-ttu-id="74460-145">Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="74460-145">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="74460-146">Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:</span><span class="sxs-lookup"><span data-stu-id="74460-146">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmosdb.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   <span data-ttu-id="74460-147">Dove:</span><span class="sxs-lookup"><span data-stu-id="74460-147">Where:</span></span>

   | <span data-ttu-id="74460-148">Parametro</span><span class="sxs-lookup"><span data-stu-id="74460-148">Parameter</span></span> | <span data-ttu-id="74460-149">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="74460-149">Description</span></span> |
   |---|---|
   | `spring.data.cassandra.contact-points` | <span data-ttu-id="74460-150">Specifica il valore di **Punto di contatto** copiato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="74460-150">Specifies the **Contact Point** from earlier in this article.</span></span> |
   | `spring.data.cassandra.port` | <span data-ttu-id="74460-151">Specifica il valore di **Porta** copiato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="74460-151">Specifies the **Port** from earlier in this article.</span></span> |
   | `spring.data.cassandra.username` | <span data-ttu-id="74460-152">Specifica il valore di **Nome utente** copiato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="74460-152">Specifies your **Username** from earlier in this article.</span></span> |
   | `spring.data.cassandra.password` | <span data-ttu-id="74460-153">Specifica il valore di **Password primaria** copiato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="74460-153">Specifies your **Primary Password** from earlier in this article.</span></span> |

1. <span data-ttu-id="74460-154">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="74460-154">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="74460-155">Creare il pacchetto dell'applicazione di esempio e testarla</span><span class="sxs-lookup"><span data-stu-id="74460-155">Package and test the sample application</span></span> 

1. <span data-ttu-id="74460-156">Compilare l'applicazione di esempio con Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="74460-156">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="74460-157">Avviare l'applicazione di esempio. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="74460-157">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="74460-158">Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="74460-158">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="74460-159">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="74460-159">Your application should return values like the following:</span></span>

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. <span data-ttu-id="74460-160">Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="74460-160">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="74460-161">L'applicazione dovrebbe restituire valori come i seguenti:</span><span class="sxs-lookup"><span data-stu-id="74460-161">Your application should return values like the following:</span></span>

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="74460-162">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="74460-162">Summary</span></span>

<span data-ttu-id="74460-163">In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni con l'API Cassandra di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="74460-163">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB Cassandra API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74460-164">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="74460-164">Next steps</span></span>

<span data-ttu-id="74460-165">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="74460-165">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="74460-166">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="74460-166">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="74460-167">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="74460-167">Additional Resources</span></span>

<span data-ttu-id="74460-168">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="74460-168">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
