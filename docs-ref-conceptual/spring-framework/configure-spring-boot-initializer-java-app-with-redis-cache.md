---
title: Configurare un'app Spring Boot Initializer per l'uso della cache Redis di Azure
description: Configurare un'applicazione Spring Boot creata con Spring Initializr per l'uso di Redis sul cloud con la cache Redis di Azure.
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm;zhijzhao;yidon
ms.date: 02/01/2018
ms.devlang: java
ms.service: cache
ms.tgt_pltfrm: cache-redis
ms.topic: article
ms.workload: na
ms.openlocfilehash: 8bfe7c2ddd238e0e5a259de9078b831a97b1b1a4
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
---
# <a name="configure-a-spring-boot-initializer-app-to-use-redis-in-the-cloud-with-azure-redis-cache"></a><span data-ttu-id="45414-103">Configurare un'app Spring Boot Initializer per l'uso di Redis sul cloud con la cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="45414-103">Configure a Spring Boot Initializer app to use Redis in the cloud with Azure Redis Cache</span></span>

<span data-ttu-id="45414-104">Questo articolo illustra in modo dettagliato come creare una cache Redis sul cloud usando il portale di Azure, quindi usare **[Spring Initializr]** per creare un'applicazione personalizzata e infine creare un'applicazione Web Java che archivia e recupera i dati tramite la cache Redis.</span><span class="sxs-lookup"><span data-stu-id="45414-104">This article walks you through creating a Redis cache in the cloud using the Azure portal, then using the **[Spring Initializr]** to create a custom application, and then creating a Java web application that stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45414-105">prerequisiti</span><span class="sxs-lookup"><span data-stu-id="45414-105">Prerequisites</span></span>

<span data-ttu-id="45414-106">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="45414-106">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="45414-107">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="45414-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="45414-108">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="45414-108">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="45414-109">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="45414-109">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="45414-110">Creare un'applicazione personalizzata tramite Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="45414-110">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="45414-111">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="45414-111">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="45414-112">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi di **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul collegamento relativo a **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="45414-112">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="45414-114">Spring Initializr userà i valori di **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.contoso.myazuredemo*.</span><span class="sxs-lookup"><span data-stu-id="45414-114">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="45414-115">Scorrere verso il basso fino alla sezione **Web** e selezionare la casella relativa a **Web**, quindi scorrere verso il basso fino alla sezione **NoSQL** e selezionare la casella **Redis**. Scorrere infine fino alla parte inferiore della pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="45414-115">Scroll down to the **Web** section and check the box for **Web**, then scroll down to the **NoSQL** section and check the box for **Redis**, then scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Opzioni complete di Spring Initializr][SI02]

1. <span data-ttu-id="45414-117">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="45414-117">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring Boot][SI03]

1. <span data-ttu-id="45414-119">Dopo l'estrazione dei file nel sistema locale, l'applicazione Spring Boot personalizzata sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="45414-119">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![File del progetto Spring Boot personalizzato][SI04]

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="45414-121">Creare una cache Redis in Azure</span><span class="sxs-lookup"><span data-stu-id="45414-121">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="45414-122">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="45414-122">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Portale di Azure][AZ01]

1. <span data-ttu-id="45414-124">Fare clic su **Database** e quindi su **Cache Redis**.</span><span class="sxs-lookup"><span data-stu-id="45414-124">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Portale di Azure][AZ02]

1. <span data-ttu-id="45414-126">Nella pagina **Nuova cache Redis** specificare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="45414-126">On the **New Redis Cache** page, specify the following information:</span></span>

   * <span data-ttu-id="45414-127">Immettere **Nome DNS** per la cache.</span><span class="sxs-lookup"><span data-stu-id="45414-127">Enter the **DNS name** for your cache.</span></span>
   * <span data-ttu-id="45414-128">Specificare **Sottoscrizione**, **Gruppo di risorse**, **Posizione** e **Piano tariffario**.</span><span class="sxs-lookup"><span data-stu-id="45414-128">Specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span>
   * <span data-ttu-id="45414-129">Per questa esercitazione, scegliere **Sblocca la porta 6379**.</span><span class="sxs-lookup"><span data-stu-id="45414-129">For this tutorial, choose **Unblock port 6379**.</span></span>

   > [!NOTE]
   >
   > <span data-ttu-id="45414-130">È possibile usare SSL con le cache Redis, ma è necessario usare un client Redis diverso, come Jedis.</span><span class="sxs-lookup"><span data-stu-id="45414-130">You can use SSL with Redis caches, but you would need to use a different Redis client like Jedis.</span></span> <span data-ttu-id="45414-131">Per altre informazioni, vedere [Come usare Cache Redis di Azure con Java][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="45414-131">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>
   >

   <span data-ttu-id="45414-132">Dopo avere specificato queste opzioni, fare clic su **Crea** per creare la cache.</span><span class="sxs-lookup"><span data-stu-id="45414-132">When you have specified these options, click **Create** to create your cache.</span></span>

   ![Portale di Azure][AZ03]

1. <span data-ttu-id="45414-134">Al termine, la cache verrà elencata nel **dashboard** di Azure, nonché nelle pagine **Tutte le risorse** e **Caches Redis**.</span><span class="sxs-lookup"><span data-stu-id="45414-134">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="45414-135">È possibile fare clic sulla cache in una di queste posizioni per aprire la pagina delle proprietà per la cache.</span><span class="sxs-lookup"><span data-stu-id="45414-135">You can click on your cache on any of those locations to open the properties page for your cache.</span></span>

   ![Portale di Azure][AZ04]

1. <span data-ttu-id="45414-137">Quando viene visualizzata la pagina contenente l'elenco delle proprietà della cache, fare clic su **Chiavi di accesso** e copiare le chiavi di accesso per la cache.</span><span class="sxs-lookup"><span data-stu-id="45414-137">When the page that contains the list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Portale di Azure][AZ05]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a><span data-ttu-id="45414-139">Configurare il progetto Spring Boot personalizzato per l'uso della cache Redis</span><span class="sxs-lookup"><span data-stu-id="45414-139">Configure your custom Spring Boot to use your Redis Cache</span></span>

1. <span data-ttu-id="45414-140">Individuare il file *application.properties* nella directory *resources* dell'app o creare il file se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="45414-140">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![Individuare il file application.properties][RE01]

1. <span data-ttu-id="45414-142">Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate dalla cache:</span><span class="sxs-lookup"><span data-stu-id="45414-142">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties from your cache:</span></span>

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Modifica del file application.properties][RE02]

   > [!NOTE] 
   > 
   > <span data-ttu-id="45414-144">Se si usa un client Redis diverso, come Jedis che consente l'uso di SSL, specificare che si intende usare SSL e la porta 6380 nel file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="45414-144">If you were using a different Redis client like Jedis that enables SSL, you would specify that you want to use SSL in your *application.properties* file and use port 6380.</span></span> <span data-ttu-id="45414-145">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="45414-145">For example:</span></span>
   > 
   > ```yaml
   > # Specify the DNS URI of your Redis cache.
   > spring.redis.host=myspringbootcache.redis.cache.windows.net
   > # Specify the access key for your Redis cache.
   > spring.redis.password=57686f6120447564652c2049495320526f636b73=
   > # Specify that you want to use SSL.
   > spring.redis.ssl=true
   > # Specify the SSL port for your Redis cache.
   > spring.redis.port=6380
   > ```
   > 
   > <span data-ttu-id="45414-146">Per altre informazioni, vedere [Come usare Cache Redis di Azure con Java][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="45414-146">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span> 
   > 

1. <span data-ttu-id="45414-147">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="45414-147">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="45414-148">Creare una cartella denominata *controller* nella cartella di origine del pacchetto, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="45414-148">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="45414-149">-oppure-</span><span class="sxs-lookup"><span data-stu-id="45414-149">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="45414-150">Creare un nuovo file denominato *HelloController.java* nella cartella *controller*.</span><span class="sxs-lookup"><span data-stu-id="45414-150">Create a new file named *HelloController.java* in the *controller* folder.</span></span> <span data-ttu-id="45414-151">Aprire il file in un editor di testo e aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="45414-151">Open the file in a text editor and add the following code to it:</span></span>

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   <span data-ttu-id="45414-152">Sarà necessario sostituire `com.contoso.myazuredemo` con il nome del pacchetto per il progetto.</span><span class="sxs-lookup"><span data-stu-id="45414-152">Where you will need to replace `com.contoso.myazuredemo` with the package name for your project.</span></span>

1. <span data-ttu-id="45414-153">Salvare e chiudere il file *HelloController.java*.</span><span class="sxs-lookup"><span data-stu-id="45414-153">Save and close the *HelloController.java* file.</span></span>

1. <span data-ttu-id="45414-154">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="45414-154">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="45414-155">Testare l'app Web passando a http://localhost:8080 tramite un Web browser o usare una sintassi simile all'esempio seguente, se si hanno curl disponibili:</span><span class="sxs-lookup"><span data-stu-id="45414-155">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="45414-156">Dovrebbe essere visualizzato il messaggio "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="45414-156">You should see the "Hello World!"</span></span> <span data-ttu-id="45414-157">dal controller di esempio, recuperato dinamicamente dalla cache Redis.</span><span class="sxs-lookup"><span data-stu-id="45414-157">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45414-158">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="45414-158">Next steps</span></span>

<span data-ttu-id="45414-159">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="45414-159">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="45414-160">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="45414-160">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="45414-161">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="45414-161">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="45414-162">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].</span><span class="sxs-lookup"><span data-stu-id="45414-162">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="45414-163">Per altre informazioni sulle operazioni iniziali nella cache Redis con Java in Azure, vedere [Come usare Cache Redis di Azure con Java][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="45414-163">For more information about getting started using Redis Cache with Java on Azure, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>

<span data-ttu-id="45414-164">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="45414-164">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="45414-165">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="45414-165">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="45414-166">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="45414-166">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="45414-167">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="45414-167">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: /azure/redis-cache/cache-java-get-started

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ01.png
[AZ02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ02.png
[AZ03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ03.png
[AZ04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ04.png
[AZ05]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ05.png

[SI01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI01.png
[SI02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI02.png
[SI03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI03.png
[SI04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI04.png

[RE01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE01.png
[RE02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE02.png
