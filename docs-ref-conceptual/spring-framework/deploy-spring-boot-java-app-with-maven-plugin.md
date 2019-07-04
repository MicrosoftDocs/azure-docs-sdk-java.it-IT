---
title: Distribuire un'app Spring Boot basata su file JAR nel cloud con Maven e Azure
description: Informazioni su come distribuire un'app Spring Boot nel cloud con il plug-in Maven per app Web di Azure per Linux.
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: brborges
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: b133290d1f14429cbf36d6ed5a67d27e1a637593
ms.sourcegitcommit: 599405a9ce892d75073ef0776befa2fa22407b4c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/19/2019
ms.locfileid: "67237604"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="186b1-103">Distribuire un'app Web Spring Boot basata su file JAR nel servizio app di Azure in Linux</span><span class="sxs-lookup"><span data-stu-id="186b1-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="186b1-104">Questo articolo illustra l'uso del [plug-in Maven per app Web del servizio app di Azure](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) per distribuire un'applicazione Spring Boot inserita in un pacchetto JAR Java SE nel [servizio app di Azure in Linux](/azure/app-service/containers/).</span><span class="sxs-lookup"><span data-stu-id="186b1-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](/azure/app-service/containers/).</span></span> <span data-ttu-id="186b1-105">Preferire la distribuzione con Java SE a [Tomcat e file WAR](/azure/app-service/containers/quickstart-java) quando si vogliono consolidare le dipendenze, il runtime e la configurazione dell'app in un singolo artefatto distribuibile.</span><span class="sxs-lookup"><span data-stu-id="186b1-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="186b1-106">Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="186b1-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="186b1-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="186b1-107">Prerequisites</span></span>

<span data-ttu-id="186b1-108">Per completare i passaggi di questa esercitazione, devono essere installati e configurati gli strumenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="186b1-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="186b1-109">[Interfaccia della riga di comando di Azure](/cli/azure/), in locale o tramite [Azure Cloud Shell](https://shell.azure.com).</span><span class="sxs-lookup"><span data-stu-id="186b1-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="186b1-110">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="186b1-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="186b1-111">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="186b1-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="186b1-112">Apache [Maven](https://maven.apache.org/), versione 3.</span><span class="sxs-lookup"><span data-stu-id="186b1-112">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="186b1-113">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="186b1-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="186b1-114">Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso</span><span class="sxs-lookup"><span data-stu-id="186b1-114">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="186b1-115">Il modo più semplice e facile per distribuire l'applicazione Spring Boot tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="186b1-115">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](/cli/azure/).</span></span>

<span data-ttu-id="186b1-116">Accedere all'account Azure con l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="186b1-116">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="186b1-117">Seguire le istruzioni per completare il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="186b1-117">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="186b1-118">Clonare l'app di esempio</span><span class="sxs-lookup"><span data-stu-id="186b1-118">Clone the sample app</span></span>

<span data-ttu-id="186b1-119">In questa sezione sarà clonata e testata in locale un'applicazione Spring Boot completata.</span><span class="sxs-lookup"><span data-stu-id="186b1-119">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="186b1-120">Aprire un prompt dei comandi o una finestra del terminale e creare una directory locale in cui salvare l'applicazione Spring Boot, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="186b1-121">-- o --</span><span class="sxs-lookup"><span data-stu-id="186b1-121">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="186b1-122">Clonare il progetto di esempio di [introduzione a Spring Boot] nella directory creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-122">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="186b1-123">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="186b1-124">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="186b1-125">Dopo che è stata creata, avviare l'app Web con Maven, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="186b1-126">Testare l'app Web esplorandola localmente tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="186b1-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="186b1-127">Se è disponibile curl, ad esempio, è possibile usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="186b1-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="186b1-128">Dovrebbe essere visualizzato il messaggio **Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="186b1-128">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="186b1-129">Configurare il plug-in Maven per il servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="186b1-129">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="186b1-130">In questa sezione si configurerà il progetto Spring Boot `pom.xml` in modo che Maven possa distribuire l'app nel servizio app di Azure in Linux.</span><span class="sxs-lookup"><span data-stu-id="186b1-130">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="186b1-131">Aprire `pom.xml` in un editor di codice.</span><span class="sxs-lookup"><span data-stu-id="186b1-131">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="186b1-132">Nella sezione `<build>` di pom.xml aggiungere la voce `<plugin>` seguente all'interno del tag `<plugins>`.</span><span class="sxs-lookup"><span data-stu-id="186b1-132">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.6.0</version>
   </plugin>
   ```

3. <span data-ttu-id="186b1-133">È quindi possibile configurare la distribuzione, eseguire il comando maven `mvn azure-webapp:config` nel prompt dei comandi e usare il **numero** scegliere queste opzioni nel prompt:</span><span class="sxs-lookup"><span data-stu-id="186b1-133">Then you can configure the deployment, run the maven command `mvn azure-webapp:config` in the Command Prompt and use the **number** to choose these options in the prompt:</span></span>
    * <span data-ttu-id="186b1-134">**OS**: linux</span><span class="sxs-lookup"><span data-stu-id="186b1-134">**OS**: linux</span></span>  
    * <span data-ttu-id="186b1-135">**javaVersion**: jre8</span><span class="sxs-lookup"><span data-stu-id="186b1-135">**javaVersion**: jre8</span></span>
    * <span data-ttu-id="186b1-136">**runtimeStack**: jre8</span><span class="sxs-lookup"><span data-stu-id="186b1-136">**runtimeStack**: jre8</span></span>

<span data-ttu-id="186b1-137">Quando viene visualizzato il prompt **Confirm (Y/N)** , premere **'y'** per completare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="186b1-137">When you get the **Confirm (Y/N)** prompt, press **'y'** and the configuration is done.</span></span>

```cmd
~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< org.springframework:gs-spring-boot >-----------------
[INFO] Building gs-spring-boot 0.1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.6.0:config (default-cli) @ gs-spring-boot ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. jre8 [*]
2. java11
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. jre8
3. TOMCAT 8.5 [*]
4. WILDFLY 14
Enter index to use: 2
Please confirm webapp properties
AppName : gs-spring-boot-1559091271202
ResourceGroup : gs-spring-boot-1559091271202-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : JAVA 8-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

4. <span data-ttu-id="186b1-138">Aggiungere la sezione `<appSettings>` alla sezione `<configuration>` di `<azure-webapp-maven-plugin>` per essere in ascolto sulla porta *80*.</span><span class="sxs-lookup"><span data-stu-id="186b1-138">Add the `<appSettings>` section to the `<configuration>` section of `<azure-webapp-maven-plugin>` to listen on the *80* port.</span></span>

    ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.6.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>

          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          ...
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="186b1-139">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="186b1-139">Deploy the app to Azure</span></span>

<span data-ttu-id="186b1-140">Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="186b1-140">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="186b1-141">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="186b1-141">To do so, use the following steps:</span></span>

1. <span data-ttu-id="186b1-142">Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-142">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="186b1-143">Distribuire l'app Web in Azure usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="186b1-143">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="186b1-144">Maven distribuirà l'app Web in Azure. Se l'app Web o il piano dell'app Web non esiste già, verrà creato.</span><span class="sxs-lookup"><span data-stu-id="186b1-144">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="186b1-145">Dopo che è stata distribuita, l'app Web potrà essere gestita tramite il [portale di Azure].</span><span class="sxs-lookup"><span data-stu-id="186b1-145">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="186b1-146">L'app Web sarà elencata in **Servizi app**:</span><span class="sxs-lookup"><span data-stu-id="186b1-146">Your web app will be listed in **App Services**:</span></span>

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* <span data-ttu-id="186b1-148">L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:</span><span class="sxs-lookup"><span data-stu-id="186b1-148">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Individuazione dell'URL dell'app Web][AP02]

<span data-ttu-id="186b1-150">Verificare il completamento della distribuzione con lo stesso comando curl eseguito in precedenza, usando l'URL dell'app Web riportato nel portale invece di `localhost`.</span><span class="sxs-lookup"><span data-stu-id="186b1-150">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="186b1-151">Dovrebbe essere visualizzato il messaggio **Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="186b1-151">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="186b1-152">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="186b1-152">Next steps</span></span>

<span data-ttu-id="186b1-153">Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.</span><span class="sxs-lookup"><span data-stu-id="186b1-153">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="186b1-154">Spring in Azure</span><span class="sxs-lookup"><span data-stu-id="186b1-154">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="186b1-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="186b1-155">Additional Resources</span></span>

<span data-ttu-id="186b1-156">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="186b1-156">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="186b1-157">[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]</span><span class="sxs-lookup"><span data-stu-id="186b1-157">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="186b1-158">Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot in contenitore in Azure</span><span class="sxs-lookup"><span data-stu-id="186b1-158">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="186b1-159">Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="186b1-159">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="186b1-160">Informazioni di riferimento sulle impostazioni di Maven</span><span class="sxs-lookup"><span data-stu-id="186b1-160">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Portale di Azure]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Introduzione a Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
