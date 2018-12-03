---
title: Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot del registro contenitori di Azure nel servizio app di Azure
description: Questa esercitazione illustra in modo dettagliato la procedura per distribuire un'applicazione Spring Boot del registro contenitori di Azure nel servizio app di Azure usando un plug-in Maven.
services: container-registry
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: cc345987085e442978b59a45d6fc3e1b52160515
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338945"
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a><span data-ttu-id="267cd-103">Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot del registro contenitori di Azure nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="267cd-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app in Azure Container Registry to Azure App Service</span></span>

<span data-ttu-id="267cd-104">Questo articolo illustra come distribuire un'applicazione [Spring Boot] di esempio nel registro contenitori di Azure e quindi usare il plug-in Maven per App Web di Azure per distribuire l'applicazione nel servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-104">This article demonstrates how to deploy a sample [Spring Boot] application to Azure Container Registry, and then use the Maven Plugin for Azure Web Apps to deploy your application to Azure App Service.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="267cd-105">Il plug-in Maven per App Web di Azure disponibile per [Apache Maven](http://maven.apache.org/) consente una facile integrazione del servizio app di Azure nei progetti Maven e semplifica il processo con cui gli sviluppatori distribuiscono app Web nel servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-105">The Maven Plugin for Azure Web Apps for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>
> 
> <span data-ttu-id="267cd-106">Il plug-in Maven per App Web di Azure è attualmente disponibile in anteprima.</span><span class="sxs-lookup"><span data-stu-id="267cd-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="267cd-107">Per il momento è supportata solo la pubblicazione FTP, ma sono previste in futuro funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="267cd-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="267cd-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="267cd-108">Prerequisites</span></span>

<span data-ttu-id="267cd-109">Per completare la procedura di questa esercitazione, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="267cd-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="267cd-110">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="267cd-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="267cd-111">[Interfaccia della riga di comando di Azure].</span><span class="sxs-lookup"><span data-stu-id="267cd-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="267cd-112">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="267cd-112">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="267cd-113">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="267cd-113">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="267cd-114">Lo strumento di compilazione [Maven] di Apache (versione 3).</span><span class="sxs-lookup"><span data-stu-id="267cd-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="267cd-115">Un client [Git].</span><span class="sxs-lookup"><span data-stu-id="267cd-115">A [Git] client.</span></span>
* <span data-ttu-id="267cd-116">Un client [Docker].</span><span class="sxs-lookup"><span data-stu-id="267cd-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="267cd-117">A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.</span><span class="sxs-lookup"><span data-stu-id="267cd-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="267cd-118">Clonare l'app Web Spring Boot in Docker di esempio</span><span class="sxs-lookup"><span data-stu-id="267cd-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="267cd-119">In questa sezione si clona e si testa in locale un'applicazione Spring Boot in contenitore.</span><span class="sxs-lookup"><span data-stu-id="267cd-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="267cd-120">Aprire un prompt dei comandi o una finestra del terminale e creare una directory locale in cui salvare l'applicazione Spring Boot, quindi passare a tale directory. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="267cd-121">-- o --</span><span class="sxs-lookup"><span data-stu-id="267cd-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="267cd-122">Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory appena creata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone -b https://github.com/spring-guides/gs-spring-boot-docker
   ```

1. <span data-ttu-id="267cd-123">Passare alla directory del progetto completato. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="267cd-124">Compilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="267cd-125">Dopo che è stata creata, avviare l'app Web con Maven, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="267cd-126">Testare l'app Web esplorandola localmente tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="267cd-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="267cd-127">Se è disponibile curl, ad esempio, è possibile usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="267cd-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="267cd-128">Dovrebbe essere visualizzato il messaggio seguente: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="267cd-128">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Esplorare l'app di esempio in locale][SB01]

> [!NOTE]
>
> <span data-ttu-id="267cd-130">Quando si usa Docker localmente, potrebbe essere visualizzato un errore che indica che non è possibile a connettersi all'host locale sulla porta 2375.</span><span class="sxs-lookup"><span data-stu-id="267cd-130">When you are using Docker locally, you may see an error which states that you cannot connect to localhost on port 2375.</span></span> <span data-ttu-id="267cd-131">In questo caso, potrebbe essere necessario abilitare l'uso locale di Docker senza TLS.</span><span class="sxs-lookup"><span data-stu-id="267cd-131">If this happens, you may need to enable using Docker locally without TLS.</span></span> <span data-ttu-id="267cd-132">A questo scopo, aprire le impostazioni di Docker e selezionare l'opzione **Expose daemon on TCP://localhost:2375 without TLS** (Esponi il daemon su TCP://localhost:2375 senza TLS).</span><span class="sxs-lookup"><span data-stu-id="267cd-132">To do so, open your Docker settings and check the option to **Expose daemon on TCP://localhost:2375 without TLS**.</span></span>
>
> ![Esporre il daemon Docker sulla porta TCP locale 2375][TL01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="267cd-134">Creare un'entità servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="267cd-134">Create an Azure service principal</span></span>

<span data-ttu-id="267cd-135">In questa sezione si crea un'entità servizio di Azure che verrà usata dal plug-in Maven durante la distribuzione del contenitore in Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-135">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="267cd-136">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="267cd-136">Open a command prompt.</span></span>

2. <span data-ttu-id="267cd-137">Accedere all'account Azure con l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="267cd-137">Sign into your Azure account by using the Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="267cd-138">Seguire le istruzioni per completare il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="267cd-138">Follow the instructions to complete the sign-in process.</span></span>

3. <span data-ttu-id="267cd-139">Creare un'entità servizio di Azure:</span><span class="sxs-lookup"><span data-stu-id="267cd-139">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="267cd-140">Dove:</span><span class="sxs-lookup"><span data-stu-id="267cd-140">Where:</span></span>

   | <span data-ttu-id="267cd-141">Parametro</span><span class="sxs-lookup"><span data-stu-id="267cd-141">Parameter</span></span>  |                    <span data-ttu-id="267cd-142">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="267cd-142">Description</span></span>                     |
   |------------|----------------------------------------------------|
   | `uuuuuuuu` | <span data-ttu-id="267cd-143">Specifica il nome utente per l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="267cd-143">Specifies the user name for the service principal.</span></span> |
   | `pppppppp` | <span data-ttu-id="267cd-144">Specifica la password per l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="267cd-144">Specifies the password for the service principal.</span></span>  |


4. <span data-ttu-id="267cd-145">Azure restituisce una risposta JSON simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="267cd-145">Azure responds with JSON that resembles the following example:</span></span>
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="267cd-146">I valori di questa risposta JSON verranno usati durante la configurazione del plug-in Maven per la distribuzione del contenitore in Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-146">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="267cd-147">`aaaaaaaa`, `uuuuuuuu`, `pppppppp` e `tttttttt` sono segnaposto che vengono usati in questo esempio per facilitare il mapping di questi valori ai rispettivi elementi durante la configurazione del file `settings.xml` di Maven nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="267cd-147">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="267cd-148">Creare un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="267cd-148">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="267cd-149">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="267cd-149">Open a command prompt.</span></span>

1. <span data-ttu-id="267cd-150">Accedere all'account di Azure:</span><span class="sxs-lookup"><span data-stu-id="267cd-150">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="267cd-151">Creare un gruppo di risorse per le risorse di Azure che verranno usate in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="267cd-151">Create a resource group for the Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="267cd-152">Sostituire il valore `wingtiptoysresources` di questo esempio con un nome univoco per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="267cd-152">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="267cd-153">Creare un registro contenitori privato di Azure nel gruppo di risorse per l'app Spring Boot:</span><span class="sxs-lookup"><span data-stu-id="267cd-153">Create a private Azure container registry in the resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="267cd-154">Sostituire il valore `wingtiptoysregistry` di questo esempio con un nome univoco per il registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="267cd-154">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="267cd-155">Recuperare la password per il registro contenitori:</span><span class="sxs-lookup"><span data-stu-id="267cd-155">Retrieve the password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="267cd-156">Azure restituirà la password, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-156">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a><span data-ttu-id="267cd-157">Aggiungere il registro contenitori e l'entità servizio di Azure alle impostazioni di Maven</span><span class="sxs-lookup"><span data-stu-id="267cd-157">Add your Azure container registry and Azure service principal to your Maven settings</span></span>

1. <span data-ttu-id="267cd-158">Aprire il file `settings.xml` di Maven in un editor di testo. Il file potrebbe trovarsi in un percorso come quelli riportati negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="267cd-158">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

2. <span data-ttu-id="267cd-159">Aggiungere le impostazioni di accesso per il registro contenitori di Azure della sezione precedente di questo articolo alla raccolta `<servers>` nel file *settings.xml*. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-159">Add your Azure Container Registry access settings from the previous section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="267cd-160">Dove:</span><span class="sxs-lookup"><span data-stu-id="267cd-160">Where:</span></span>

   |   <span data-ttu-id="267cd-161">Elemento</span><span class="sxs-lookup"><span data-stu-id="267cd-161">Element</span></span>    |                                 <span data-ttu-id="267cd-162">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="267cd-162">Description</span></span>                                  |
   |--------------|------------------------------------------------------------------------------|
   |    `<id>`    |         <span data-ttu-id="267cd-163">Contiene il nome del registro contenitori privato di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-163">Contains the name of your private Azure container registry.</span></span>          |
   | `<username>` |         <span data-ttu-id="267cd-164">Contiene il nome del registro contenitori privato di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-164">Contains the name of your private Azure container registry.</span></span>          |
   | `<password>` | <span data-ttu-id="267cd-165">Contiene la password recuperata nella sezione precedente di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="267cd-165">Contains the password you retrieved in the previous section of this article.</span></span> |


3. <span data-ttu-id="267cd-166">Aggiungere le impostazioni dell'entità servizio di Azure di una sezione precedente di questo articolo alla raccolta `<servers>` nel file *settings.xml*. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-166">Add your Azure service principal settings from an earlier section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   <span data-ttu-id="267cd-167">Dove:</span><span class="sxs-lookup"><span data-stu-id="267cd-167">Where:</span></span>

   |     <span data-ttu-id="267cd-168">Elemento</span><span class="sxs-lookup"><span data-stu-id="267cd-168">Element</span></span>     |                                                                                   <span data-ttu-id="267cd-169">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="267cd-169">Description</span></span>                                                                                   |
   |-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `<id>`      |                                <span data-ttu-id="267cd-170">Specifica un nome univoco che viene usato da Maven per cercare le impostazioni di sicurezza quando si distribuisce l'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-170">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>                                |
   |   `<client>`    |                                                             <span data-ttu-id="267cd-171">Contiene il valore `appId` dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="267cd-171">Contains the `appId` value from your service principal.</span></span>                                                             |
   |   `<tenant>`    |                                                            <span data-ttu-id="267cd-172">Contiene il valore `tenant` dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="267cd-172">Contains the `tenant` value from your service principal.</span></span>                                                             |
   |     `<key>`     |                                                           <span data-ttu-id="267cd-173">Contiene il valore `password` dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="267cd-173">Contains the `password` value from your service principal.</span></span>                                                            |
   | `<environment>` | <span data-ttu-id="267cd-174">Definisce l'ambiente cloud di Azure di destinazione, che in questo esempio è `AZURE`.</span><span class="sxs-lookup"><span data-stu-id="267cd-174">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="267cd-175">Un elenco completo degli ambienti è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="267cd-175">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span> |


4. <span data-ttu-id="267cd-176">Salvare e chiudere il file *settings.xml*.</span><span class="sxs-lookup"><span data-stu-id="267cd-176">Save and close the *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a><span data-ttu-id="267cd-177">Compilare l'immagine del contenitore Docker ed eseguirne il push nel registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="267cd-177">Build your Docker container image and push it to your Azure container registry</span></span>

1. <span data-ttu-id="267cd-178">Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="267cd-178">Navigate to the completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

2. <span data-ttu-id="267cd-179">Aggiornare la raccolta `<properties>` nel file *pom.xml* con il valore del server di accesso per il Registro contenitori di Azure dalla sezione precedente di questa esercitazione. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-179">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="267cd-180">Dove:</span><span class="sxs-lookup"><span data-stu-id="267cd-180">Where:</span></span>

   |           <span data-ttu-id="267cd-181">Elemento</span><span class="sxs-lookup"><span data-stu-id="267cd-181">Element</span></span>           |                                                                       <span data-ttu-id="267cd-182">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="267cd-182">Description</span></span>                                                                       |
   |-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `<azure.containerRegistry>` |                                              <span data-ttu-id="267cd-183">Specifica il nome del registro contenitori privato di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-183">Specifies the name of your private Azure container registry.</span></span>                                               |
   |   `<docker.image.prefix>`   | <span data-ttu-id="267cd-184">Specifica l'URL del registro contenitori privato di Azure, ottenuto accodando ".azurecr.io" al nome del registro contenitori privato.</span><span class="sxs-lookup"><span data-stu-id="267cd-184">Specifies the URL of your private Azure container registry, which is derived by appending ".azurecr.io" to the name of your private container registry.</span></span> |


3. <span data-ttu-id="267cd-185">Verificare che l'elemento `<plugin>` per il plug-in Docker nel file *pom.xml* contenga le proprietà corrette per l'indirizzo del server di accesso e il nome del registro dal passaggio precedente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="267cd-185">Verify that `<plugin>` for the Docker plugin in your *pom.xml* file contains the correct properties for the login server address and registry name from the previous step in this tutorial.</span></span> <span data-ttu-id="267cd-186">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="267cd-186">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   <span data-ttu-id="267cd-187">Dove:</span><span class="sxs-lookup"><span data-stu-id="267cd-187">Where:</span></span>

   |     <span data-ttu-id="267cd-188">Elemento</span><span class="sxs-lookup"><span data-stu-id="267cd-188">Element</span></span>     |                                       <span data-ttu-id="267cd-189">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="267cd-189">Description</span></span>                                       |
   |-----------------|-----------------------------------------------------------------------------------------|
   |  `<serverId>`   |  <span data-ttu-id="267cd-190">Specifica la proprietà contenente il nome del registro contenitori privato di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-190">Specifies the property which contains name of your private Azure container registry.</span></span>   |
   | `<registryUrl>` | <span data-ttu-id="267cd-191">Specifica la proprietà contenente l'URL del registro contenitori privato di Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-191">Specifies the property which contains the URL of your private Azure container registry.</span></span> |


4. <span data-ttu-id="267cd-192">Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire questo comando per ricompilare l'applicazione ed effettuare il push del contenitore nel registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="267cd-192">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

5. <span data-ttu-id="267cd-193">FACOLTATIVO: passare al [portale di Azure] e verificare che nel registro contenitori sia presente l'immagine del contenitore Docker denominata **gs-spring-boot-docker**.</span><span class="sxs-lookup"><span data-stu-id="267cd-193">OPTIONAL: Browse to the [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Verificare il contenitore nel portale di Azure][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a><span data-ttu-id="267cd-195">Personalizzare pom.xml e quindi compilare e distribuire il contenitore in Azure</span><span class="sxs-lookup"><span data-stu-id="267cd-195">Customize your pom.xml, then build and deploy your container to Azure</span></span>

<span data-ttu-id="267cd-196">Aprire il file `pom.xml` per l'applicazione Spring Boot in un editor di testo e quindi individuare l'elemento `<plugin>` per `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="267cd-196">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="267cd-197">L'elemento dovrebbe essere simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="267cd-197">This element should resemble the following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="267cd-198">È possibile modificare diversi valori per il plug-in Maven. Una descrizione dettagliata di ognuno di questi elementi è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="267cd-198">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="267cd-199">Dopo questa premessa, in questo articolo è opportuno evidenziare diversi valori:</span><span class="sxs-lookup"><span data-stu-id="267cd-199">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="267cd-200">Elemento</span><span class="sxs-lookup"><span data-stu-id="267cd-200">Element</span></span> | <span data-ttu-id="267cd-201">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="267cd-201">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="267cd-202">Specifica la versione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="267cd-202">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="267cd-203">È consigliabile controllare la versione riportata nel [repository centrale Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) per assicurarsi di usare l'ultima versione.</span><span class="sxs-lookup"><span data-stu-id="267cd-203">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<authentication>` | <span data-ttu-id="267cd-204">Specifica le informazioni di autenticazione per Azure, che in questo esempio includono un elemento `<serverId>` contenente `azure-auth`. Maven usa questo valore per cercare i valori dell'entità servizio di Azure nel file *settings.xml* di Maven, in base a quanto definito in una sezione precedente di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="267cd-204">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="267cd-205">Specifica il gruppo di risorse di destinazione, che in questo esempio è `wingtiptoysresources`.</span><span class="sxs-lookup"><span data-stu-id="267cd-205">Specifies the target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="267cd-206">Se non esiste già, questo gruppo di risorse verrà creato durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="267cd-206">The resource group will be created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="267cd-207">Specifica il nome di destinazione dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="267cd-207">Specifies the target name for your web app.</span></span> <span data-ttu-id="267cd-208">In questo esempio, il nome di destinazione è `maven-linux-app-${maven.build.timestamp}` e il suffisso `${maven.build.timestamp}` viene accodato per evitare conflitti.</span><span class="sxs-lookup"><span data-stu-id="267cd-208">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="267cd-209">Il timestamp è facoltativo. Come nome dell'app è possibile specificare qualsiasi stringa univoca.</span><span class="sxs-lookup"><span data-stu-id="267cd-209">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="267cd-210">Specifica l'area di destinazione, che in questo esempio è `westus`.</span><span class="sxs-lookup"><span data-stu-id="267cd-210">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="267cd-211">Un elenco completo è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)].</span><span class="sxs-lookup"><span data-stu-id="267cd-211">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<containerSettings>` | <span data-ttu-id="267cd-212">Specifica le proprietà contenenti il nome e l'URL del contenitore.</span><span class="sxs-lookup"><span data-stu-id="267cd-212">Specifies the properties which contain the name and URL of your container.</span></span> |
| `<appSettings>` | <span data-ttu-id="267cd-213">Specifica qualsiasi impostazione univoca che dovrà essere usata da Maven durante la distribuzione dell'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="267cd-213">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="267cd-214">In questo esempio, un elemento `<property>` contiene una coppia nome-valore di elementi figlio che specificano la porta dell'app.</span><span class="sxs-lookup"><span data-stu-id="267cd-214">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span> |

> [!NOTE]
>
> <span data-ttu-id="267cd-215">Le impostazioni per modificare il numero di porta in questo esempio sono necessarie solo in caso di modifica della porta rispetto all'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="267cd-215">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

1. <span data-ttu-id="267cd-216">Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-216">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="267cd-217">Distribuire l'app Web in Azure usando Maven. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="267cd-217">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="267cd-218">Maven distribuirà l'app Web in Azure. Se l'app Web non esiste già, verrà creata.</span><span class="sxs-lookup"><span data-stu-id="267cd-218">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="267cd-219">Se nell'area specificata nell'elemento `<region>` del file *pom.xml* non sono disponibili server sufficienti quando si avvia la distribuzione, potrebbe essere visualizzato un errore simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="267cd-219">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="267cd-220">In questo caso, è possibile specificare un'altra area e rieseguire il comando Maven per distribuire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="267cd-220">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="267cd-221">Dopo che è stata distribuita, l'app Web potrà essere gestita usando il [portale di Azure].</span><span class="sxs-lookup"><span data-stu-id="267cd-221">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="267cd-222">L'app Web sarà elencata in **Servizi app**:</span><span class="sxs-lookup"><span data-stu-id="267cd-222">Your web app will be listed in **App Services**:</span></span>

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* <span data-ttu-id="267cd-224">L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:</span><span class="sxs-lookup"><span data-stu-id="267cd-224">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Individuazione dell'URL dell'app Web][AP02]

## <a name="next-steps"></a><span data-ttu-id="267cd-226">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="267cd-226">Next steps</span></span>

<span data-ttu-id="267cd-227">Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="267cd-227">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="267cd-228">[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]</span><span class="sxs-lookup"><span data-stu-id="267cd-228">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="267cd-229">Accedere ad Azure dall'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="267cd-229">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="267cd-230">Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="267cd-230">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="267cd-231">Informazioni di riferimento sulle impostazioni di Maven</span><span class="sxs-lookup"><span data-stu-id="267cd-231">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="267cd-232">[Plug-in Docker per Maven]</span><span class="sxs-lookup"><span data-stu-id="267cd-232">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Portale di Azure]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service/containers/tutorial-custom-docker-image
[Docker]: https://www.docker.com/
[Plug-in Docker per Maven]: https://github.com/spotify/docker-maven-plugin
[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP02.png
[TL01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/TL01.png
