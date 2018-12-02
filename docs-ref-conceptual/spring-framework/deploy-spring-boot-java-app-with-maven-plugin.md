---
title: Distribuire un'app Spring Boot basata su file JAR nel cloud con Maven e Azure
description: Informazioni su come distribuire un'app Spring Boot nel cloud con il plug-in Maven per app Web di Azure per Linux.
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: brborges
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: 066ac30697c6adccc0c6a7b9d57205de488bdc53
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339005"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="49933-103">Distribuire un'app Web Spring Boot basata su file JAR nel servizio app di Azure in Linux</span><span class="sxs-lookup"><span data-stu-id="49933-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="49933-104">Questo articolo illustra l'uso del [plug-in Maven per app Web del servizio app di Azure](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) per distribuire un'applicazione Spring Boot inserita in un pacchetto JAR Java SE nel [servizio app di Azure in Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/).</span><span class="sxs-lookup"><span data-stu-id="49933-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/).</span></span> <span data-ttu-id="49933-105">Preferire la distribuzione con Java SE a [Tomcat e file WAR](/azure/app-service/containers/quickstart-java) quando si vogliono consolidare le dipendenze, il runtime e la configurazione dell'app in un singolo artefatto distribuibile.</span><span class="sxs-lookup"><span data-stu-id="49933-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="49933-106">Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="49933-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49933-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="49933-107">Prerequisites</span></span>

<span data-ttu-id="49933-108">Per completare i passaggi di questa esercitazione, devono essere installati e configurati gli strumenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="49933-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="49933-109">[Interfaccia della riga di comando di Azure](/cli/azure/), in locale o tramite [Azure Cloud Shell](https://shell.azure.com).</span><span class="sxs-lookup"><span data-stu-id="49933-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="49933-110">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="49933-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="49933-111">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="49933-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="49933-112">Apache [Maven](https://maven.apache.org/), versione 3.</span><span class="sxs-lookup"><span data-stu-id="49933-112">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="49933-113">Un client [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="49933-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="49933-114">Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso</span><span class="sxs-lookup"><span data-stu-id="49933-114">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="49933-115">Il modo più semplice e facile per distribuire l'applicazione Spring Boot tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="49933-115">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span>

<span data-ttu-id="49933-116">Accedere all'account Azure con l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="49933-116">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="49933-117">Seguire le istruzioni per completare il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="49933-117">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="49933-118">Clonare l'app di esempio</span><span class="sxs-lookup"><span data-stu-id="49933-118">Clone the sample app</span></span>

<span data-ttu-id="49933-119">In questa sezione sarà clonata e testata in locale un'applicazione Spring Boot completata.</span><span class="sxs-lookup"><span data-stu-id="49933-119">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="49933-120">Aprire un prompt dei comandi o una finestra del terminale e creare una directory locale in cui salvare l'applicazione Spring Boot, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="49933-121">-- o --</span><span class="sxs-lookup"><span data-stu-id="49933-121">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="49933-122">Clonare il progetto di esempio di [introduzione a Spring Boot] nella directory creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-122">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="49933-123">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="49933-124">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="49933-125">Dopo che è stata creata, avviare l'app Web con Maven, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="49933-126">Testare l'app Web esplorandola localmente tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="49933-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="49933-127">Se è disponibile curl, ad esempio, è possibile usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="49933-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="49933-128">Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!)</span><span class="sxs-lookup"><span data-stu-id="49933-128">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="49933-129">Configurare il plug-in Maven per il servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="49933-129">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="49933-130">In questa sezione si configurerà il progetto Spring Boot `pom.xml` in modo che Maven possa distribuire l'app nel servizio app di Azure in Linux.</span><span class="sxs-lookup"><span data-stu-id="49933-130">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="49933-131">Aprire `pom.xml` in un editor di codice.</span><span class="sxs-lookup"><span data-stu-id="49933-131">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="49933-132">Nella sezione `<build>` di pom.xml aggiungere la voce `<plugin>` seguente all'interno del tag `<plugins>`.</span><span class="sxs-lookup"><span data-stu-id="49933-132">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
    <configuration>
      <deploymentType>jar</deploymentType>

      <!-- configure app to run on port 80, required by App Service -->
      <appSettings>
        <property> 
          <name>JAVA_OPTS</name> 
          <value>-Dserver.port=80</value> 
        </property> 
      </appSettings>

      <!-- Web App information -->
      <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
      <appName>${WEBAPP_NAME}</appName>
      <region>${REGION}</region>  

      <!-- Java Runtime Stack for Web App on Linux-->
      <linuxRuntime>jre8</linuxRuntime>
    </configuration>
   </plugin>
   ```

3. <span data-ttu-id="49933-133">Aggiornare i segnaposto seguenti nella configurazione del plug-in:</span><span class="sxs-lookup"><span data-stu-id="49933-133">Update the following placeholders in the plugin configuration:</span></span>

| <span data-ttu-id="49933-134">Placeholder</span><span class="sxs-lookup"><span data-stu-id="49933-134">Placeholder</span></span> | <span data-ttu-id="49933-135">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="49933-135">Description</span></span> |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | <span data-ttu-id="49933-136">Nome del nuovo gruppo di risorse in cui creare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="49933-136">Name for the new resource group in which to create your web app.</span></span> <span data-ttu-id="49933-137">Inserendo tutte le risorse per un'app in un gruppo è possibile gestirle insieme.</span><span class="sxs-lookup"><span data-stu-id="49933-137">By putting all the resources for an app in a group, you can manage them together.</span></span> <span data-ttu-id="49933-138">Ad esempio, eliminando il gruppo di risorse si eliminano tutte le risorse associate all'app.</span><span class="sxs-lookup"><span data-stu-id="49933-138">For example, deleting the resource group would delete all resources associated with the app.</span></span> <span data-ttu-id="49933-139">Aggiornare questo valore con un nuovo nome univoco di gruppo di risorse, ad esempio *TestResources*.</span><span class="sxs-lookup"><span data-stu-id="49933-139">Update this value with a unique new resource group name, for example, *TestResources*.</span></span> <span data-ttu-id="49933-140">Questo nome di gruppo di risorse verrà usato per pulire tutte le risorse di Azure in una sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="49933-140">You will use this resource group name to clean up all Azure resources in a later section.</span></span> |
| `WEBAPP_NAME` | <span data-ttu-id="49933-141">Il nome dell'app sarà incluso nel nome host dell'app Web durante la distribuzione in Azure (WEBAPP_NAME.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="49933-141">The app name will be part the host name for the web app when deployed to Azure (WEBAPP_NAME.azurewebsites.net).</span></span> <span data-ttu-id="49933-142">Aggiornare questo valore con un nome univoco per l'app Web di Azure, che ospiterà l'app Java, ad esempio *contoso*.</span><span class="sxs-lookup"><span data-stu-id="49933-142">Update this value with a unique name for the new Azure web app, which will host your Java app, for example *contoso*.</span></span> |
| `REGION` | <span data-ttu-id="49933-143">Un'area di Azure in cui l'app web è ospitata, ad esempio `westus2`.</span><span class="sxs-lookup"><span data-stu-id="49933-143">An Azure region where the web app is hosted, for example `westus2`.</span></span> <span data-ttu-id="49933-144">È possibile ottenere un elenco di aree da Cloud Shell o dall’interfaccia della riga di comando utilizzando il comando `az account list-locations`.</span><span class="sxs-lookup"><span data-stu-id="49933-144">You can get a list of regions from the Cloud Shell or CLI using the `az account list-locations` command.</span></span> |

<span data-ttu-id="49933-145">Un elenco completo delle opzioni di configurazione è disponibile nelle [informazioni di riferimento sul plug-in Maven in GitHub](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin).</span><span class="sxs-lookup"><span data-stu-id="49933-145">A full list of configuration options can be found in the [Maven plugin reference on GitHub](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin).</span></span>

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="49933-146">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="49933-146">Deploy the app to Azure</span></span>

<span data-ttu-id="49933-147">Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="49933-147">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="49933-148">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="49933-148">To do so, use the following steps:</span></span>

1. <span data-ttu-id="49933-149">Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-149">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="49933-150">Distribuire l'app Web in Azure usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49933-150">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="49933-151">Maven distribuirà l'app Web in Azure. Se l'app Web o il piano dell'app Web non esiste già, verrà creato.</span><span class="sxs-lookup"><span data-stu-id="49933-151">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="49933-152">Dopo che è stata distribuita, l'app Web potrà essere gestita tramite il [portale di Azure].</span><span class="sxs-lookup"><span data-stu-id="49933-152">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="49933-153">L'app Web sarà elencata in **Servizi app**:</span><span class="sxs-lookup"><span data-stu-id="49933-153">Your web app will be listed in **App Services**:</span></span>

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* <span data-ttu-id="49933-155">L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:</span><span class="sxs-lookup"><span data-stu-id="49933-155">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Individuazione dell'URL dell'app Web][AP02]

<span data-ttu-id="49933-157">Verificare il completamento della distribuzione con lo stesso comando curl eseguito in precedenza, usando l'URL dell'app Web riportato nel portale invece di `localhost`.</span><span class="sxs-lookup"><span data-stu-id="49933-157">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="49933-158">Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!)</span><span class="sxs-lookup"><span data-stu-id="49933-158">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="49933-159">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="49933-159">Next steps</span></span>

<span data-ttu-id="49933-160">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="49933-160">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="49933-161">[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]</span><span class="sxs-lookup"><span data-stu-id="49933-161">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="49933-162">Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot in contenitore in Azure</span><span class="sxs-lookup"><span data-stu-id="49933-162">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="49933-163">Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="49933-163">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="49933-164">Informazioni di riferimento sulle impostazioni di Maven</span><span class="sxs-lookup"><span data-stu-id="49933-164">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Portale di Azure]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Introduzione a Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
