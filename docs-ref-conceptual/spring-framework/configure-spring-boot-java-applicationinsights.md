---
title: Configurare un'app Spring Boot Initializer per l'uso di Azure Application Insights Spring Boot Starter
description: Configurare un'applicazione Spring Boot creata con Spring Initializr per l'uso di Application Insights Spring Boot Starter.
services: Application-Insights
documentationcenter: java
author: dhaval24
manager: alexklim
editor: ''
ms.assetid: ''
ms.author: dhdoshi
ms.date: 12/19/2018
ms.devlang: java
ms.service: Azure Monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.workload: na
ms.openlocfilehash: bf4f7e51f3108d684503465050d69461240f17e3
ms.sourcegitcommit: 9df42bd342ef2d25d56a6045f1ab1baf6f2c250e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54237292"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a><span data-ttu-id="4a0cc-103">Configurare un'app Spring Boot Initializer per l'uso di Application Insights</span><span class="sxs-lookup"><span data-stu-id="4a0cc-103">Configure a Spring Boot Initializer app to use Application Insights</span></span>

<span data-ttu-id="4a0cc-104">Questo articolo illustra come creare un'applicazione Spring Boot con **[Spring Initializr]**, che usa Azure Application Insights Spring Boot Starter per il monitoraggio end-to-end delle applicazioni Java nel cloud.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-104">This article walks you through creating a Spring Boot application using **[Spring Initializr]**, that uses Azure Application Insights Spring Boot Starter for end-to-end monitoring of Java applications on cloud.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a0cc-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4a0cc-105">Prerequisites</span></span>

<span data-ttu-id="4a0cc-106">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-106">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="4a0cc-107">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="4a0cc-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="4a0cc-108">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-108">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="4a0cc-109">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-109">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="4a0cc-110">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="4a0cc-111">Le API Web Flux e Netty **non sono attualmente supportate** con Application Insights Spring Boot Starter.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-111">Web Flux and Netty APIs are **not currently supported** with the Application Insights Spring Boot starter.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="4a0cc-112">Creare un'applicazione personalizzata tramite Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="4a0cc-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="4a0cc-113">Passare a [https://start.spring.io/](https://start.spring.io/).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-113">Browse to [https://start.spring.io/](https://start.spring.io/).</span></span>

1. <span data-ttu-id="4a0cc-114">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi selezionare la dipendenza Web nella sezione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select web dependency in the dependenies section.</span></span>

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="4a0cc-116">Spring Initializr userà i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.example.demo*.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-116">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.example.demo*.</span></span>
   >

1. <span data-ttu-id="4a0cc-117">Fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-117">Click the button to **Generate Project**.</span></span>

1. <span data-ttu-id="4a0cc-118">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-118">When prompted, download the project to a path on your local computer.</span></span>

1. <span data-ttu-id="4a0cc-119">Dopo l'estrazione dei file nel sistema locale, l'applicazione Spring Boot personalizzata sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-119">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![File del progetto Spring Boot personalizzato][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a><span data-ttu-id="4a0cc-121">Creare una risorsa di Application Insights in Azure</span><span class="sxs-lookup"><span data-stu-id="4a0cc-121">Create an Application Insights Resource on Azure</span></span>

1. <span data-ttu-id="4a0cc-122">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-122">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Portale di Azure][AZ01]

1. <span data-ttu-id="4a0cc-124">Fare clic su **Strumenti di gestione** e quindi su **Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-124">Click **Management Tools**, and then click **Application Insights**.</span></span>

   ![Portale di Azure][AZ02]

1. <span data-ttu-id="4a0cc-126">Nella pagina per la **nuova risorsa di Application Insights** specificare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-126">On the **New Application Insights Resource** page, specify the following information:</span></span>

   * <span data-ttu-id="4a0cc-127">Immettere il **nome** della risorsa di Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-127">Enter the **Name** for your Application Insights resource.</span></span>
   * <span data-ttu-id="4a0cc-128">Scegliere Applicazione Web Java in **Tipo di applicazione**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-128">Choose the **Application Type** to Java Web Application.</span></span>
   * <span data-ttu-id="4a0cc-129">Specificare **Sottoscrizione**, **Gruppo di risorse** e **Località**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-129">Specify your **Subscription**, **Resource group** and **Location**.</span></span>
   * <span data-ttu-id="4a0cc-130">Selezionare l'opzione Aggiungi al dashboard, se si vuole aggiungere la risorsa al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-130">Select Pin to dashboard option, if you would like to pin the resource on your Azure portal.</span></span>

   <span data-ttu-id="4a0cc-131">Dopo aver specificato queste opzioni, fare clic su **Crea** per creare la risorsa di Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-131">When you have specified these options, click **Create** to create your Application Insights resource.</span></span>

   ![Portale di Azure][AZ03]

1. <span data-ttu-id="4a0cc-133">Al termine della creazione, la risorsa verrà elencata nel **dashboard** di Azure, nonché nelle pagine **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-133">Once your resource has been created, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources** pages.</span></span> <span data-ttu-id="4a0cc-134">È possibile fare clic sulla risorsa di Application Insights in una di queste posizioni per aprirne la pagina di panoramica.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-134">You can click on your resource on any of those locations to open the overview page of the Application Insights resource.</span></span> <span data-ttu-id="4a0cc-135">Nella pagina di panoramica copiare la **chiave di strumentazione**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-135">From this overview page please copy the **instrumentation key**.</span></span>

   ![Portale di Azure][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a><span data-ttu-id="4a0cc-137">Configurare l'applicazione Spring Boot scaricata per l'uso di Application Insights</span><span class="sxs-lookup"><span data-stu-id="4a0cc-137">Configure your downloaded Spring Boot Application to use Application Insights</span></span>

1. <span data-ttu-id="4a0cc-138">Individuare il file *POM.xml* nella directory radice dell'app e aggiungere la dipendenza seguente nella sezione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-138">Locate the *POM.xml* file in the root directory of your app, and add the following dependency in its dependencies section.</span></span> 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

1. <span data-ttu-id="4a0cc-139">Individuare il file *application.properties* nella directory *resources* dell'app o creare il file se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-139">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![Individuare il file application.properties][RE01]

1. <span data-ttu-id="4a0cc-141">Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate con le credenziali corrette:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-141">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties with appropriate credentials:</span></span>

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   <span data-ttu-id="4a0cc-142">Per altri modi di ottimizzare Application Insights, vedere il [file leggimi di Application Insights Spring Boot Starter](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-142">For more ways to fine tune Application Insights please refer to [Application Insights Springboot starter readme](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="4a0cc-143">È possibile usare chiavi di strumentazione di Application Insights diverse (ossia</span><span class="sxs-lookup"><span data-stu-id="4a0cc-143">You can use different Application Insights instrumentation keys (i.e</span></span> <span data-ttu-id="4a0cc-144">diverse risorse) per profili diversi, come PROD, DEV e così via. Per altre informazioni, vedere le [proprietà specifiche dei profili di Spring Boot].</span><span class="sxs-lookup"><span data-stu-id="4a0cc-144">different resources) for different profiles like PROD, DEV etc. Please refer to [Spring Boot Profile Specific Properties] for additional information.</span></span> 

1. <span data-ttu-id="4a0cc-145">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-145">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="4a0cc-146">Creare una cartella denominata *controller* nella cartella di origine del pacchetto, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-146">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   <span data-ttu-id="4a0cc-147">-oppure-</span><span class="sxs-lookup"><span data-stu-id="4a0cc-147">-or-</span></span>

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. <span data-ttu-id="4a0cc-148">Creare un nuovo file denominato *TestController.java* nella cartella *controller*.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-148">Create a new file named *TestController.java* in the *controller* folder.</span></span> <span data-ttu-id="4a0cc-149">Aprire il file in un editor di testo e aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-149">Open the file in a text editor and add the following code to it:</span></span>

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   <span data-ttu-id="4a0cc-150">Sarà necessario sostituire `com.example.demo` con il nome del pacchetto per il progetto.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-150">Where you will need to replace `com.example.demo` with the package name for your project.</span></span>

1. <span data-ttu-id="4a0cc-151">Salvare e chiudere il file *TestController.java*.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-151">Save and close the *TestController.java* file.</span></span>

1. <span data-ttu-id="4a0cc-152">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="4a0cc-153">Testare l'app Web passando a http://localhost:8080/sample/hello con un Web browser oppure usare una sintassi simile all'esempio seguente, se è disponibile curl:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-153">Test the web app by browsing to http://localhost:8080/sample/hello using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   <span data-ttu-id="4a0cc-154">Verrà visualizzato il messaggio "hello!"</span><span class="sxs-lookup"><span data-stu-id="4a0cc-154">You should see the "hello!"</span></span> <span data-ttu-id="4a0cc-155">dal controller di esempio.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-155">message from your sample controller displayed.</span></span> <span data-ttu-id="4a0cc-156">Application Insights raccoglierà automaticamente la richiesta e la invierà come elemento di telemetria con l'evento, le metriche, la dipendenza e la traccia personalizzati associati, come specificato nella logica del controller.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-156">Application Insights will automatically collect this request and send it as a telemetry item with it's associated custom event, custom metric, custom dependency and custom trace as specified in the controller logic.</span></span> 

   <span data-ttu-id="4a0cc-157">Dopo alcuni secondi verranno visualizzati i dati nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-157">After a few seconds you should see the data on Azure portal.</span></span> 

   ![Portale di Azure][AZ05]

   <span data-ttu-id="4a0cc-159">È possibile fare clic sul riquadro Mappa delle applicazioni per visualizzare i componenti di alto livello e la reciproca interazione.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-159">You can click on Application Map tile to view high level components and their interaction with each other.</span></span> <span data-ttu-id="4a0cc-160">È consigliabile usare questo riquadro per ottenere una panoramica generale dell'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-160">This is a recommended place to get a high level overview of entire application.</span></span> <span data-ttu-id="4a0cc-161">Ogni microservizio Spring Boot viene riconosciuto in base al nome dell'applicazione Spring.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-161">Each Spring Boot Microservice is recognized by the spring application name.</span></span> <span data-ttu-id="4a0cc-162">Ricordare di impostarlo.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-162">Please remember to set it.</span></span>

   ![Portale di Azure][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a><span data-ttu-id="4a0cc-164">Configurare l'applicazione Spring Boot per l'invio di log log4j ad Application Insights</span><span class="sxs-lookup"><span data-stu-id="4a0cc-164">Configure Springboot Application to send log4j logs to Application Insights</span></span>

1. <span data-ttu-id="4a0cc-165">Modificare il file POM.xml del progetto e aggiungere/modificare la sezione delle dipendenze come segue.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-165">Modify the POM.xml file of the project and add/modify the dependencies section with following.</span></span> 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. <span data-ttu-id="4a0cc-166">Salvare e chiudere il file *POM.xml*.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-166">Save and close the *POM.xml* file.</span></span>

3. <span data-ttu-id="4a0cc-167">Nella cartella \src\main\resources creare un nuovo file *log4j2.xml* e configurarlo.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-167">In \src\main\resources folder, create a new file *log4j2.xml* and configure it.</span></span> <span data-ttu-id="4a0cc-168">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="4a0cc-168">For example:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```
4. <span data-ttu-id="4a0cc-169">Compilare ed eseguire di nuovo l'applicazione Spring Boot come illustrato sopra.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-169">Build and run the Spring Boot application again as shown above.</span></span> 

<span data-ttu-id="4a0cc-170">In pochi secondi, nel portale di Azure verranno visualizzati tutti i log Spring disponibili.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-170">Within few seconds, you should see all the spring logs being available on Azure Portal.</span></span> 

![Portale di Azure][AZ06]

<span data-ttu-id="4a0cc-172">È anche possibile esaminare i messaggi di log dettagliati ed eseguire analisi nel portale Analisi.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-172">You can even look at the detailed log messages and do analysis on Analytics Portal.</span></span> 

![Portale di Azure][AZ07]

## <a name="next-steps"></a><span data-ttu-id="4a0cc-174">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4a0cc-174">Next steps</span></span>

<span data-ttu-id="4a0cc-175">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-175">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4a0cc-176">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="4a0cc-176">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="4a0cc-177">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4a0cc-177">Additional Resources</span></span>

<span data-ttu-id="4a0cc-178">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a0cc-178">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="4a0cc-179">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="4a0cc-179">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="4a0cc-180">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="4a0cc-180">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="4a0cc-181">Application Insights supporta la raccolta automatica delle dipendenze esterne e la correlazione con le richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-181">Application Insights supports automatic collection of external dependencies and its correlation with incoming requests.</span></span> <span data-ttu-id="4a0cc-182">La raccolta automatica è attualmente supportata per Oracle, MsSQL, MySQL e Redis.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-182">Currently we support autocollection of Oracle, MsSQL, MySQL and Redis.</span></span> <span data-ttu-id="4a0cc-183">Per altri dettagli sull'abilitazione della raccolta automatica, vedere l'articolo su [come usare l'agente Java di Application Insights](/azure/application-insights/app-insights-java-agent).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-183">For more details on enabling autocollection please follow the article [how to use Application Insights Java agent](/azure/application-insights/app-insights-java-agent).</span></span>

<span data-ttu-id="4a0cc-184">Per altre informazioni su Azure Application Insights e sulle relative funzionalità di monitoraggio, vedere la home page di **[Application Insights]**.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-184">For more information about Azure Application Insights, and it's monitoring capabilities, see the **[Application Insights]** home page.</span></span>

<span data-ttu-id="4a0cc-185">Per altre informazioni su dettagli aggiuntivi della configurazione di Application Insights Spring Boot Starter, vedere questo [collegamento](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-185">For more information about additional configuration details of Application Insights Spring Boot Starter, please refer to this [link](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

<span data-ttu-id="4a0cc-186">Per richieste di funzionalità e potenziali bug, aprire problemi nel repository [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-186">For feature requests and potential bugs, please open issues on our [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) repository.</span></span>

<span data-ttu-id="4a0cc-187">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].</span><span class="sxs-lookup"><span data-stu-id="4a0cc-187">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="4a0cc-188">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-188">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="4a0cc-189">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-189">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="4a0cc-190">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, diversi pacchetti Spring Boot di esempio sono disponibili all'indirizzo [https://github.com/spring-guides/](https://github.com/spring-guides/).</span><span class="sxs-lookup"><span data-stu-id="4a0cc-190">To help developers get started with Spring Boot, several sample Spring Boot packages are available at [https://github.com/spring-guides/](https://github.com/spring-guides/).</span></span> <span data-ttu-id="4a0cc-191">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="4a0cc-191">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

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
[Proprietà specifiche dei profili di Spring Boot]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Boot Profile Specific Properties]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)
[Application Insights]: /azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
