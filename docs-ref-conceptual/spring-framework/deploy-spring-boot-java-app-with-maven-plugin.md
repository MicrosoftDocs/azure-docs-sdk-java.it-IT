---
title: Distribuire un'app Spring Boot sul cloud con Maven e Azure
description: Informazioni su come distribuire un'app Spring Boot sul cloud con il plug-in Maven per le app Web di Azure.
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: robmcm;kevinzha;brborges
ms.date: 06/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ca788354d26964bd9f1e21a0d3a8005ff65ce4bc
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324347"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a><span data-ttu-id="bee27-103">Distribuire un'app Spring Boot nel cloud con il plug-in Maven per il servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="bee27-103">Deploy a Spring Boot app to the cloud using the Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="bee27-104">Questo articolo illustra l'uso del plug-in Maven per App Web del servizio app di Azure per distribuire un'applicazione Spring Boot di esempio.</span><span class="sxs-lookup"><span data-stu-id="bee27-104">This article demonstrates using the Maven Plugin for Azure App Service Web Apps to deploy a sample Spring Boot application.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="bee27-105">Il [plug-in Maven per App Web del servizio app di Azure](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) disponibile per [Apache Maven](http://maven.apache.org/) consente una facile integrazione del servizio app di Azure nei progetti Maven e semplifica il processo con cui gli sviluppatori distribuiscono app Web nel servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="bee27-105">The [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="bee27-106">Prima di usare il plug-in Maven, verificarne l'ultima versione disponibile nel repository centrale Maven: [![Repository centrale Maven](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span><span class="sxs-lookup"><span data-stu-id="bee27-106">Before using the Maven plugin, check on Maven Central for the latest available version of the plugin: [![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="bee27-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bee27-107">Prerequisites</span></span>

<span data-ttu-id="bee27-108">Per completare la procedura di questa esercitazione, saranno necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="bee27-108">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="bee27-109">Sottoscrizione di Azure. Se non se ne ha già una, è possibile iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="bee27-109">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="bee27-110">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="bee27-110">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="bee27-111">Java Development Kit (JDK) aggiornato, versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="bee27-111">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="bee27-112">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="bee27-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="bee27-113">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="bee27-113">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="bee27-114">Clonare l'app Web Spring Boot di esempio</span><span class="sxs-lookup"><span data-stu-id="bee27-114">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="bee27-115">In questa sezione sarà clonata e testata in locale un'applicazione Spring Boot completata.</span><span class="sxs-lookup"><span data-stu-id="bee27-115">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="bee27-116">Aprire un prompt dei comandi o una finestra del terminale e creare una directory locale in cui salvare l'applicazione Spring Boot, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-116">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="bee27-117">-- o --</span><span class="sxs-lookup"><span data-stu-id="bee27-117">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="bee27-118">Clonare il progetto di esempio di [introduzione a Spring Boot] nella directory creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-118">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="bee27-119">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-119">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="bee27-120">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-120">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="bee27-121">Dopo che è stata creata, avviare l'app Web con Maven, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-121">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="bee27-122">Testare l'app Web esplorandola localmente tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="bee27-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="bee27-123">Se è disponibile curl, ad esempio, è possibile usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bee27-123">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="bee27-124">Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!)</span><span class="sxs-lookup"><span data-stu-id="bee27-124">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a><span data-ttu-id="bee27-125">Modificare il progetto per la distribuzione basata su WAR nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="bee27-125">Adjust project for WAR-based deployment on Azure App Service</span></span>

<span data-ttu-id="bee27-126">In questa sezione si modificherà rapidamente il progetto Spring Boot per distribuirlo come file WAR nel servizio app di Azure, che per impostazione predefinita offre Tomcat come runtime.</span><span class="sxs-lookup"><span data-stu-id="bee27-126">In this section we will quickly adjust the Spring Boot project to be deployed as a WAR file on Azure App Service, which provides Tomcat as the runtime by default.</span></span> <span data-ttu-id="bee27-127">Per poter eseguire questa operazione, è necessario modificare due file:</span><span class="sxs-lookup"><span data-stu-id="bee27-127">For this to work, there are two files to be modified:</span></span>

- <span data-ttu-id="bee27-128">File `pom.xml` di Maven</span><span class="sxs-lookup"><span data-stu-id="bee27-128">The Maven `pom.xml` file</span></span>
- <span data-ttu-id="bee27-129">Classe Java `Application`</span><span class="sxs-lookup"><span data-stu-id="bee27-129">The `Application` Java class</span></span>

<span data-ttu-id="bee27-130">Iniziare con le impostazioni di Maven:</span><span class="sxs-lookup"><span data-stu-id="bee27-130">Let's start with the Maven settings:</span></span>

1. <span data-ttu-id="bee27-131">Aprire `pom.xml`</span><span class="sxs-lookup"><span data-stu-id="bee27-131">Open `pom.xml`</span></span>

1. <span data-ttu-id="bee27-132">Aggiungere `<packaging>war</packaging>` subito dopo la definizione `<artifactId>` all'inizio:</span><span class="sxs-lookup"><span data-stu-id="bee27-132">Add `<packaging>war</packaging>` right after the `<artifactId>` definition at the top:</span></span>
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. <span data-ttu-id="bee27-133">Aggiungere la dipendenza seguente:</span><span class="sxs-lookup"><span data-stu-id="bee27-133">Add the following dependency:</span></span>
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

<span data-ttu-id="bee27-134">Aprire quindi la classe `Application`, possibilmente dopo che l'ambiente di sviluppo integrato ha rilevato le nuove dipendenze, e procedere con le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="bee27-134">Now open the class `Application`, hopefully after your IDE has already picked up the new dependencies, and proceed with the following modifications:</span></span>

1. <span data-ttu-id="bee27-135">Rendere la classe Application una sottoclasse di `SpringBootServletInitializer`:</span><span class="sxs-lookup"><span data-stu-id="bee27-135">Make class Application a subclass of `SpringBootServletInitializer`:</span></span>
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. <span data-ttu-id="bee27-136">Aggiungere il metodo seguente alla classe Application:</span><span class="sxs-lookup"><span data-stu-id="bee27-136">Add the following method to the Application class:</span></span>
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```
1. <span data-ttu-id="bee27-137">Organizzare le importazioni per assicurarsi che `SpringApplicationBuilder` e `SpringBootServletInitializer` vengano importati correttamente.</span><span class="sxs-lookup"><span data-stu-id="bee27-137">Organize your imports to ensure `SpringApplicationBuilder` and `SpringBootServletInitializer` are properly imported.</span></span>

<span data-ttu-id="bee27-138">L'applicazione è ora pronta per essere distribuita in Tomcat e qualsiasi altro runtime servlet (ad esempio, Jetty).</span><span class="sxs-lookup"><span data-stu-id="bee27-138">Your application is now ready to be deployed to Tomcat and any other Servlet runtime (e.g. Jetty).</span></span>

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a><span data-ttu-id="bee27-139">Aggiungere il plug-in Maven per App Web del servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="bee27-139">Add the Maven Plugin for Azure App Service Web Apps</span></span>

<span data-ttu-id="bee27-140">In questa sezione si aggiungerà un plug-in Maven che automatizzerà l'intera distribuzione dell'applicazione in App Web del servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="bee27-140">In this section, we will add a Maven plugin that will automate the entire deployment of this application into Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="bee27-141">Aprire di nuovo `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="bee27-141">Open `pom.xml` once again.</span></span>

1. <span data-ttu-id="bee27-142">All'interno di `<properties>` impostare un formato di timestamp personalizzato con la proprietà `maven.build.timestamp.format`.</span><span class="sxs-lookup"><span data-stu-id="bee27-142">Inside `<properties>`, set a custom timestamp format with the property `maven.build.timestamp.format`.</span></span> <span data-ttu-id="bee27-143">Dato che il servizio app di Azure crea un URL pubblico per l'applicazione, questa impostazione verrà usata per generare il nome della distribuzione ed evitare conflitti con le distribuzioni live di altri utenti.</span><span class="sxs-lookup"><span data-stu-id="bee27-143">Because Azure App Service creates a public URL for your application, this setting will be used to generate the name of your deployment, and avoid conflict with other users' live deployments.</span></span>
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. <span data-ttu-id="bee27-144">Nell'elemento `<plugins>` aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="bee27-144">In the `<plugins>` element, add the following:</span></span>
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

<span data-ttu-id="bee27-145">Con queste impostazioni, il progetto Maven è ora pronto per la distribuzione live in App Web del servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="bee27-145">With these settings, your Maven project is now ready for live deployment to Azure App Service Web App.</span></span>

## <a name="install-and-log-in-to-azure-cli"></a><span data-ttu-id="bee27-146">Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso</span><span class="sxs-lookup"><span data-stu-id="bee27-146">Install and log in to Azure CLI</span></span>

<span data-ttu-id="bee27-147">Il modo più semplice e facile per distribuire l'applicazione Spring Boot tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="bee27-147">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span> <span data-ttu-id="bee27-148">Verificare che sia installata.</span><span class="sxs-lookup"><span data-stu-id="bee27-148">Make sure you have it installed.</span></span>

1. <span data-ttu-id="bee27-149">Accedere all'account Azure con l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="bee27-149">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
   <span data-ttu-id="bee27-150">Seguire le istruzioni per completare il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="bee27-150">Follow the instructions to complete the sign-in process.</span></span>

## <a name="optionally-customize-pomxml-before-deploying"></a><span data-ttu-id="bee27-151">Personalizzare pom.xml prima della distribuzione (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="bee27-151">Optionally, customize pom.xml before deploying</span></span>

<span data-ttu-id="bee27-152">Aprire il file `pom.xml` per l'applicazione Spring Boot in un editor di testo e quindi individuare l'elemento `<plugin>` per `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="bee27-152">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="bee27-153">L'elemento dovrebbe essere simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="bee27-153">This element should resemble the following example:</span></span>

   ```xml
  <plugins>
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
      <configuration>
         <resourceGroup>maven-projects</resourceGroup>
         <appName>${project.artifactId}-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>war</deploymentType>
      </configuration>
    </plugin>
  </plugins>
   ```

<span data-ttu-id="bee27-154">È possibile modificare diversi valori per il plug-in Maven. Una descrizione dettagliata di ognuno di questi elementi è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="bee27-154">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="bee27-155">Dopo questa premessa, in questo articolo è opportuno evidenziare diversi valori:</span><span class="sxs-lookup"><span data-stu-id="bee27-155">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="bee27-156">Elemento</span><span class="sxs-lookup"><span data-stu-id="bee27-156">Element</span></span> | <span data-ttu-id="bee27-157">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="bee27-157">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="bee27-158">Specifica la versione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="bee27-158">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="bee27-159">Controllare la versione riportata nel [repository centrale Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) per assicurarsi di usare l'ultima versione.</span><span class="sxs-lookup"><span data-stu-id="bee27-159">Verify the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="bee27-160">Specifica il gruppo di risorse di destinazione, che in questo esempio è `maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="bee27-160">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="bee27-161">Se non esiste già, questo gruppo di risorse viene creato durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="bee27-161">The resource group is created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="bee27-162">Specifica il nome di destinazione dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="bee27-162">Specifies the target name for your web app.</span></span> <span data-ttu-id="bee27-163">In questo esempio, il nome di destinazione è `maven-web-app-${maven.build.timestamp}` e il suffisso `${maven.build.timestamp}` viene accodato per evitare conflitti.</span><span class="sxs-lookup"><span data-stu-id="bee27-163">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="bee27-164">Il timestamp è facoltativo. Come nome dell'app è possibile specificare qualsiasi stringa univoca.</span><span class="sxs-lookup"><span data-stu-id="bee27-164">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="bee27-165">Specifica l'area di destinazione, che in questo esempio è `westus`.</span><span class="sxs-lookup"><span data-stu-id="bee27-165">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="bee27-166">Un elenco completo è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="bee27-166">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<javaVersion>` | <span data-ttu-id="bee27-167">Specifica la versione del runtime Java per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="bee27-167">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="bee27-168">Un elenco completo è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="bee27-168">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<deploymentType>` | <span data-ttu-id="bee27-169">Specifica il tipo di distribuzione per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="bee27-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="bee27-170">Il valore predefinito è `war`.</span><span class="sxs-lookup"><span data-stu-id="bee27-170">Default is `war`.</span></span> |

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="bee27-171">Compilare e distribuire l'app Web in Azure</span><span class="sxs-lookup"><span data-stu-id="bee27-171">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="bee27-172">Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="bee27-172">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="bee27-173">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="bee27-173">To do so, use the following steps:</span></span>

1. <span data-ttu-id="bee27-174">Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-174">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="bee27-175">Distribuire l'app Web in Azure usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee27-175">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="bee27-176">Maven distribuirà l'app Web in Azure. Se l'app Web non esiste già, verrà creata.</span><span class="sxs-lookup"><span data-stu-id="bee27-176">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="bee27-177">Dopo che è stata distribuita, l'app Web potrà essere gestita usando il [portale di Azure].</span><span class="sxs-lookup"><span data-stu-id="bee27-177">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="bee27-178">L'app Web sarà elencata in **Servizi app**:</span><span class="sxs-lookup"><span data-stu-id="bee27-178">Your web app will be listed in **App Services**:</span></span>

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* <span data-ttu-id="bee27-180">L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:</span><span class="sxs-lookup"><span data-stu-id="bee27-180">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Individuazione dell'URL dell'app Web][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="bee27-182">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bee27-182">Next steps</span></span>

<span data-ttu-id="bee27-183">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="bee27-183">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="bee27-184">[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]</span><span class="sxs-lookup"><span data-stu-id="bee27-184">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="bee27-185">Accedere ad Azure dall'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="bee27-185">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="bee27-186">Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot in contenitore in Azure</span><span class="sxs-lookup"><span data-stu-id="bee27-186">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="bee27-187">Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="bee27-187">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="bee27-188">Informazioni di riferimento sulle impostazioni di Maven</span><span class="sxs-lookup"><span data-stu-id="bee27-188">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Portale di Azure]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Introduzione a Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
