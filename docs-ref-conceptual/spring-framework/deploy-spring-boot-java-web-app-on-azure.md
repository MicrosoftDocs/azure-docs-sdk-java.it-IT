---
title: Distribuire un'applicazione Spring Boot nel servizio app di Azure
description: "Questa esercitazione fornirà agli sviluppatori i passaggi per distribuire l'app Web Introduzione a Spring Boot nel servizio app di Azure."
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: b520cc80360f8162c929bb2cc88c24311a7e20f8
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a><span data-ttu-id="73405-103">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="73405-103">Deploy a Spring Boot Application to the Azure App Service</span></span>

<span data-ttu-id="73405-104">Questa esercitazione illustra la creazione di un esempio di app Web introduttiva di [Spring Boot] e la sua distribuzione nel [servizio app di Azure].</span><span class="sxs-lookup"><span data-stu-id="73405-104">This tutorial will walk you though creating a sample [Spring Boot] Getting Started web app and deploying it to [Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="73405-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="73405-105">Prerequisites</span></span>

<span data-ttu-id="73405-106">Per completare i passaggi di questa esercitazione, sono necessari gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73405-106">In order to complete the steps in this tutorial, you need to have the following:</span></span>

* <span data-ttu-id="73405-107">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="73405-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="73405-108">Un [Java Developer Kit (JDK)] aggiornato.</span><span class="sxs-lookup"><span data-stu-id="73405-108">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="73405-109">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="73405-109">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="73405-110">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="73405-110">A [Git] client.</span></span>

## <a name="create-the-spring-boot-getting-started-web-app"></a><span data-ttu-id="73405-111">Creare l'app Web Introduzione a Spring Boot</span><span class="sxs-lookup"><span data-stu-id="73405-111">Create the Spring Boot Getting Started web app</span></span>

<span data-ttu-id="73405-112">I passaggi seguenti illustrano i passaggi necessari per creare una semplice applicazione Web Spring Boot e testarla localmente.</span><span class="sxs-lookup"><span data-stu-id="73405-112">The following steps will walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="73405-113">Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-113">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="73405-114">- o -</span><span class="sxs-lookup"><span data-stu-id="73405-114">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="73405-115">Clonare il progetto di esempio [Introduzione a Spring Boot] nella directory appena creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-115">Clone the [Spring Boot Getting Started] sample project into the directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="73405-116">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-116">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="73405-117">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-117">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="73405-118">Dopo aver creato l'app Web, passare alla directory del file JAR e avviare l'app Web. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-118">Once the web app has been created, change directory to the JAR file and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="73405-119">Testare l'app Web passando a http://localhost:8080 tramite un Web browser o usare una sintassi simile all'esempio seguente, se si hanno curl disponibili:</span><span class="sxs-lookup"><span data-stu-id="73405-119">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="73405-120">Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!)</span><span class="sxs-lookup"><span data-stu-id="73405-120">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Esplora la app di esempio][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="73405-122">Creare un'app web di Azure con Java</span><span class="sxs-lookup"><span data-stu-id="73405-122">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="73405-123">I passaggi seguenti illustrano la creazione di un'app Web di Azure, la configurazione delle impostazioni necessarie per Java e delle proprie credenziali FTP.</span><span class="sxs-lookup"><span data-stu-id="73405-123">The following steps will walk you through the steps to create an Azure Web App, configure the required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="73405-124">Aprire il [portale di Azure] ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="73405-124">Browse to the [Azure portal] and log in.</span></span>

1. <span data-ttu-id="73405-125">Dopo aver effettuato l'accesso all'account nel portale di Azure, fare clic sull'icona di menu per i **servizi App**:</span><span class="sxs-lookup"><span data-stu-id="73405-125">Once you have logged into your account on the Azure portal, click the menu icon for **App Services**:</span></span>
   
   ![Portale di Azure][AZ01]

1. <span data-ttu-id="73405-127">Quando la pagina **servizi App** viene visualizzata, fare clic su **+ Aggiungi** per creare un nuovo servizio App.</span><span class="sxs-lookup"><span data-stu-id="73405-127">When the **App Services** page is displayed, click **+ Add** to create a new App Service.</span></span>

   ![Creare un servizio app][AZ02]

1. <span data-ttu-id="73405-129">Quando viene visualizzato l'elenco di modelli di app Web, fare clic sul collegamento per l'applicazione Web Microsoft base.</span><span class="sxs-lookup"><span data-stu-id="73405-129">When the list of web app templates is displayed, click the link for the basic Microsoft Web App.</span></span>

   ![Modelli di app Web][AZ03]

1. <span data-ttu-id="73405-131">Quando la pagina delle informazioni per il modello di app Web viene visualizzata, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="73405-131">When the information page for the Web App template is displayed, click **Create**.</span></span>

   ![Crea app Web][AZ04]

1. <span data-ttu-id="73405-133">Specificare un nome univoco per l'app Web, quindi specificare eventuali impostazioni aggiuntive e scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="73405-133">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![Creare delle impostazioni app Web][AZ05]

1. <span data-ttu-id="73405-135">Dopo aver creato un'app Web, fare clic sull'icona di menu **servizi App**, quindi fare clic sulla app Web appena creata:</span><span class="sxs-lookup"><span data-stu-id="73405-135">Once your web app has been created, click the menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Elenco delle app Web][AZ06]

1. <span data-ttu-id="73405-137">Quando viene visualizzata l'app web, specificare la versione di Java usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="73405-137">When your web app is displayed, specify the Java version by using the following steps:</span></span>

   <span data-ttu-id="73405-138">a.</span><span class="sxs-lookup"><span data-stu-id="73405-138">a.</span></span> <span data-ttu-id="73405-139">Fare clic sulla voce di menu **Impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="73405-139">Click the **Application Settings** menu item.</span></span>

   <span data-ttu-id="73405-140">b.</span><span class="sxs-lookup"><span data-stu-id="73405-140">b.</span></span> <span data-ttu-id="73405-141">Scegliere **Java 8** per la versione di Java.</span><span class="sxs-lookup"><span data-stu-id="73405-141">Choose **Java 8** for the Java version.</span></span>

   <span data-ttu-id="73405-142">c.</span><span class="sxs-lookup"><span data-stu-id="73405-142">c.</span></span> <span data-ttu-id="73405-143">Scegliere **Più recenti** per la versione di Java.</span><span class="sxs-lookup"><span data-stu-id="73405-143">Choose **Newest** for the minor Java version.</span></span>

   <span data-ttu-id="73405-144">d.</span><span class="sxs-lookup"><span data-stu-id="73405-144">d.</span></span> <span data-ttu-id="73405-145">Scegliere **Newest Tomcat 8.5** [Tomcat 8.5 più recente] per il contenitore Web.</span><span class="sxs-lookup"><span data-stu-id="73405-145">Choose **Newest Tomcat 8.5** for the web container.</span></span> <span data-ttu-id="73405-146">(Questo contenitore non verrà effettivamente usato; Azure userà il contenitore dall'app Spring Boot.)</span><span class="sxs-lookup"><span data-stu-id="73405-146">(This container will not actually be used; Azure will use the container from your Spring Boot application.)</span></span>

   <span data-ttu-id="73405-147">e.</span><span class="sxs-lookup"><span data-stu-id="73405-147">e.</span></span> <span data-ttu-id="73405-148">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="73405-148">Click **Save**.</span></span>

   ![Impostazioni dell'applicazione][AZ07]

1. <span data-ttu-id="73405-150">Specificare le credenziali di distribuzione FTP usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="73405-150">Specify your FTP deployment credentials by using the following steps:</span></span>

   <span data-ttu-id="73405-151">a.</span><span class="sxs-lookup"><span data-stu-id="73405-151">a.</span></span> <span data-ttu-id="73405-152">Fare clic sulla voce di menu **Credenziali distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="73405-152">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="73405-153">b.</span><span class="sxs-lookup"><span data-stu-id="73405-153">b.</span></span> <span data-ttu-id="73405-154">Specificare il proprio nome utente e la password.</span><span class="sxs-lookup"><span data-stu-id="73405-154">Specify your username and password.</span></span>

   <span data-ttu-id="73405-155">c.</span><span class="sxs-lookup"><span data-stu-id="73405-155">c.</span></span> <span data-ttu-id="73405-156">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="73405-156">Click **Save**.</span></span>

   ![Specificare le credenziali di distribuzione][AZ08]

1. <span data-ttu-id="73405-158">Recuperare le informazioni di connessione FTP usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="73405-158">Retrieve your FTP connection information by using the following steps:</span></span>

   <span data-ttu-id="73405-159">a.</span><span class="sxs-lookup"><span data-stu-id="73405-159">a.</span></span> <span data-ttu-id="73405-160">Fare clic sulla voce di menu **Credenziali distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="73405-160">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="73405-161">b.</span><span class="sxs-lookup"><span data-stu-id="73405-161">b.</span></span> <span data-ttu-id="73405-162">Copiare il nome utente FTP completo e l'URL e salvarli per la sezione successiva di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="73405-162">Copy your full FTP username and URL and save them for the next section of this tutorial.</span></span>

   ![URL FTP e credenziali][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a><span data-ttu-id="73405-164">Distribuire l'applicazione Web Spring Boot in Azure</span><span class="sxs-lookup"><span data-stu-id="73405-164">Deploy your Spring Boot web app to Azure</span></span>

<span data-ttu-id="73405-165">La procedura seguente illustra i passaggi per distribuire l'app Web Spring Boot in Azure.</span><span class="sxs-lookup"><span data-stu-id="73405-165">The following steps will walk you through the steps to deploy your Spring Boot web app to Azure.</span></span>

1. <span data-ttu-id="73405-166">Aprire un editor di testo come Blocco note e incollare il testo seguente in un nuovo documento, quindi salvare il file come *web.config*:</span><span class="sxs-lookup"><span data-stu-id="73405-166">Open a text editor such as Windows Notepad and paste the following text into a new document, then save the file as *web.config*:</span></span>
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. <span data-ttu-id="73405-167">Dopo aver salvato il file *web.config* nel sistema, effettuare la connessione all'app Web tramite FTP usando l'URL, il nome utente e la password della sezione precedente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="73405-167">After you have saved the *web.config* file to your system, connect to your web app via FTP using the URL, username, and password from the preceding section of this tutorial.</span></span> <span data-ttu-id="73405-168">ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-168">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="73405-169">Modificare la directory remota nella cartella radice dell'app Web (ovvero */sito/wwwroot*), quindi copiare il file JAR dall'applicazione Spring Boot e il file *web.config* salvato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="73405-169">Change the remote directory to the root folder of your web app, (which is at */site/wwwroot*), then copy the JAR file from your Spring Boot application and the *web.config* from earlier.</span></span> <span data-ttu-id="73405-170">ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73405-170">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="73405-171">Dopo aver distribuito i file JAR e *web.config* nell'app Web, è necessario riavviare l'app Web tramite il portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="73405-171">After you have deployed your JAR and *web.config* files to your web app, you need to restart your web app using the Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="73405-172">Testare l'app Web passando all'URL relativo tramite un browser Web o usare una sintassi simile all'esempio seguente, se si hanno curl disponibili:</span><span class="sxs-lookup"><span data-stu-id="73405-172">Test the web app by browsing to your web app's URL using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="73405-173">Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!)</span><span class="sxs-lookup"><span data-stu-id="73405-173">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Esplora la app di esempio][SB02]

## <a name="next-steps"></a><span data-ttu-id="73405-175">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="73405-175">Next steps</span></span>

<span data-ttu-id="73405-176">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="73405-176">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="73405-177">Distribuire un'applicazione Spring Boot in Linux nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="73405-177">Deploy a Spring Boot Application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

* [<span data-ttu-id="73405-178">Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="73405-178">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="73405-179">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].</span><span class="sxs-lookup"><span data-stu-id="73405-179">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="73405-180">Per altre informazioni sulla distribuzione di app Web in Azure tramite FTP, vedere [Distribuire l'app nel servizio app di Azure usando FTP/S].</span><span class="sxs-lookup"><span data-stu-id="73405-180">For additional information about depoying web apps to Azure using FTP, see [Deploy your app to Azure App Service using FTP/S].</span></span>

<span data-ttu-id="73405-181">Per altre informazioni sul progetto di esempio Spring Boot, vedere [Introduzione a Spring Boot].</span><span class="sxs-lookup"><span data-stu-id="73405-181">For further details about the Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="73405-182">Per informazioni sulla Guida introduttiva con le proprie applicazioni Spring Boot, vedere **Spring Initializr** (Inizializzazione di SpringBoot) all'indirizzo https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="73405-182">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="73405-183">Per altre informazioni sulla configurazione delle impostazioni aggiuntive per l'app Web, vedere [Configurare app Web in Servizio app di Azure].</span><span class="sxs-lookup"><span data-stu-id="73405-183">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

[servizio app di Azure]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[portale di Azure]: https://portal.azure.com/
[Configurare app Web in Servizio app di Azure]: /azure/app-service/web-sites-configure
[Distribuire l'app nel servizio app di Azure usando FTP/S]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Introduzione a Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-web-app-on-azure/SB01.png
[SB02]: ./media/deploy-spring-boot-java-web-app-on-azure/SB02.png

[AZ01]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ01.png
[AZ02]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ02.png
[AZ03]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ03.png
[AZ04]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ04.png
[AZ05]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ05.png
[AZ06]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ06.png
[AZ07]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ07.png
[AZ08]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ08.png
[AZ09]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ09.png
[AZ10]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ10.png
