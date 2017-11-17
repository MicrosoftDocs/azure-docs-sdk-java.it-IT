---
title: Configurare le distribuzioni di app Web usando Java | Microsoft Docs
description: Codice di esempio Java per configurare le distribuzioni di Servizio app di Azure Git o FTP usando Azure SDK per Java
author: rloutlaw
manager: douge
ms.assetid: 833e9c78-1e50-4c23-a611-f73a2f0c2983
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 910d1e43c9942d6402aeccb8757ba819b7453dab
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a><span data-ttu-id="c8e97-103">Configurare le origini distribuzione di Servizio app di Azure dalle applicazioni Java</span><span class="sxs-lookup"><span data-stu-id="c8e97-103">Configure Azure App Service deployment sources from your Java applications</span></span>

<span data-ttu-id="c8e97-104">[Questo esempio](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) distribuisce il codice a quattro applicazioni in un singolo piano di [Servizio app di Azure](https://docs.microsoft.com/azure/app-service/), ognuna delle quali usa un'origine distribuzione diversa.</span><span class="sxs-lookup"><span data-stu-id="c8e97-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) deploys code to four applications in a single [Azure App Service](https://docs.microsoft.com/azure/app-service/) plan, each using a different deployment source.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="c8e97-105">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="c8e97-105">Run the sample</span></span>

<span data-ttu-id="c8e97-106">Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer.</span><span class="sxs-lookup"><span data-stu-id="c8e97-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="c8e97-107">Quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="c8e97-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

<span data-ttu-id="c8e97-108">Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java).</span><span class="sxs-lookup"><span data-stu-id="c8e97-108">View the [complete sample code on GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="c8e97-109">Eseguire l'autenticazione con Azure</span><span class="sxs-lookup"><span data-stu-id="c8e97-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a><span data-ttu-id="c8e97-110">Creare un'app del servizio app che esegue Apache Tomcat</span><span class="sxs-lookup"><span data-stu-id="c8e97-110">Create a App Service app running Apache Tomcat</span></span>

```java
// create a new Standard app service plan and create a single Java 8/Tomcat 8 app in it
WebApp app1 = azure.webApps().define(app1Name)
             .withNewResourceGroup(rgName)
             .withNewAppServicePlan(planName)
             .withRegion(Region.US_WEST)
             .withPricingTier(AppServicePricingTier.STANDARD_S1)
             .withJavaVersion(JavaVersion.JAVA_8_NEWEST)
             .withWebContainer(WebContainer.TOMCAT_8_0_NEWEST)
             .create();
```

<span data-ttu-id="c8e97-111">`withJavaVersion()` e `withWebContainer()` configurano il servizio app per rispondere alle richieste HTTP usando Tomcat 8.</span><span class="sxs-lookup"><span data-stu-id="c8e97-111">`withJavaVersion()` and `withWebContainer()` configure App Service to serve HTTP requests using Tomcat 8.</span></span>

## <a name="deploy-a-java-application-using-ftp"></a><span data-ttu-id="c8e97-112">Distribuire un'applicazione Java tramite FTP</span><span class="sxs-lookup"><span data-stu-id="c8e97-112">Deploy a Java application using FTP</span></span>
```java
// pass the PublishingProfile that contains FTP information to a helper method 
uploadFileToFtp(app1.getPublishingProfile(), "helloworld.war", 
      ManageWebAppSourceControl.class.getResourceAsStream("/helloworld.war"));

// Use the FTP classes in the Apache Commons library to connect to Azure using 
// the information from the PublishingProfile
private static void uploadFileToFtp(PublishingProfile profile, String fileName, InputStream file) throws Exception {
        FTPClient ftpClient = new FTPClient();
        String[] ftpUrlSegments = profile.ftpUrl().split("/", 2);
        String server = ftpUrlSegments[0];
        // Tomcat will deploy WAR files uploaded to this directory.
        String path = "./site/wwwroot/webapps"; 

        // FTP the build WAR to Azure
        ftpClient.connect(server);
        ftpClient.login(profile.ftpUsername(), profile.ftpPassword());
        ftpClient.setFileType(FTP.BINARY_FILE_TYPE);
        ftpClient.changeWorkingDirectory(path);
        ftpClient.storeFile(fileName, file);
        ftpClient.disconnect();
}
```

<span data-ttu-id="c8e97-113">Questo codice carica un file WAR nella directory `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-113">This code uploads a WAR file to the `/site/wwwroot/webapps` directory.</span></span> <span data-ttu-id="c8e97-114">Tomcat distribuisce per impostazione predefinita i file WAR inseriti in questa directory nel servizio app.</span><span class="sxs-lookup"><span data-stu-id="c8e97-114">Tomcat deploys WAR files placed in this directory by default in App Service.</span></span>

## <a name="deploy-a-java-application-from-a-local-git-repo"></a><span data-ttu-id="c8e97-115">Distribuire un'applicazione Java da un repository Git locale</span><span class="sxs-lookup"><span data-stu-id="c8e97-115">Deploy a Java application from a local Git repo</span></span>

```java
// get the publishing profile from the App Service webapp
PublishingProfile profile = app2.getPublishingProfile();

// create a new Git repo in the sample directory under src/main/resources 
Git git = Git
    .init()
    .setDirectory(new File(ManageWebAppSourceControl.class.getResource("/azure-samples-appservice-helloworld/").getPath()))
    .call();
git.add().addFilepattern(".").call();
// add the files in the sample app to an initial commit
git.commit().setMessage("Initial commit").call(); 

// push the commit using the Azure Git remote URL and credentials in the publishing profile
PushCommand command = git.push();
command.setRemote(profile.gitUrl()); 
command.setCredentialsProvider(new UsernamePasswordCredentialsProvider(profile.gitUsername(), profile.gitPassword()));
command.setRefSpecs(new RefSpec("master:master")); 
command.setForce(true);
command.call();
```      

<span data-ttu-id="c8e97-116">Questo codice usa le librerie [JGit](https://eclipse.org/jgit/) per creare un nuovo repository Git nella cartella `src/main/resources/azure-samples-appservice-helloworld`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-116">This code uses the [JGit](https://eclipse.org/jgit/) libraries to create a new Git repo in the `src/main/resources/azure-samples-appservice-helloworld` folder.</span></span> <span data-ttu-id="c8e97-117">L'esempio aggiunge quindi tutti i file nella cartella a un commit iniziale ed esegue il push del commit in Azure usando le informazioni di distribuzione Git dell'oggetto `PublishingProfile` dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="c8e97-117">The sample then adds all files in the folder to an initial commit and pushes the commit to Azure using Git deployment information from the webapp's `PublishingProfile`.</span></span> 

>[!NOTE]
> <span data-ttu-id="c8e97-118">Il layout dei file nel repository deve corrispondere esattamente al modo in cui si vuole che i file vengano distribuiti nella directory `/site/wwwroot/` in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8e97-118">The layout of the files in the repo must match exactly how you want the files deployed under the `/site/wwwroot/` directory in Azure App Service.</span></span>

## <a name="deploy-an-application-from-a-public-git-repo"></a><span data-ttu-id="c8e97-119">Distribuire un'applicazione da un repository Git pubblico</span><span class="sxs-lookup"><span data-stu-id="c8e97-119">Deploy an application from a public Git repo</span></span>

```java
// deploy a .NET sample app from a public GitHub repo into a new webapp
WebApp app3 = azure.webApps().define(app3Name)
                .withNewResourceGroup(rgName)
                .withExistingAppServicePlan(plan)
                .defineSourceControl()
                .withPublicGitRepository(
                   "https://github.com/Azure-Samples/app-service-web-dotnet-get-started")
                .withBranch("master")
                .attach()
                .create();
 ```

 <span data-ttu-id="c8e97-120">Il runtime del servizio app compila e distribuisce automaticamente il progetto .NET usando il codice più recente nel ramo `master` del repository.</span><span class="sxs-lookup"><span data-stu-id="c8e97-120">The App Service runtime automatically builds and deploys the .NET project using the latest code on the `master` branch of the repo.</span></span>

## <a name="continuous-deployment-from-a-github-repo"></a><span data-ttu-id="c8e97-121">Distribuzione continua da un repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c8e97-121">Continuous deployment from a GitHub repo</span></span>

```java
// deploy the application whenever you push a new commit or merge a pull request into your master branch
WebApp app4 = azure.webApps()
                    .define(app4Name)
                    .withExistingResourceGroup(rgName)
                    .withExistingAppServicePlan(plan)
                    // Uncomment the following lines to turn on continuous deployment scenario
                    //.defineSourceControl()
                    //    .withContinuouslyIntegratedGitHubRepository("username", "reponame")
                    //    .withBranch("master")
                    //    .withGitHubAccessToken("YOUR GITHUB PERSONAL TOKEN")
                    //    .attach()
                    .create();
```  

<span data-ttu-id="c8e97-122">I valori `username` e `reponame` sono quelli usati in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c8e97-122">The `username` and `reponame` values are the ones used in GitHub.</span></span> <span data-ttu-id="c8e97-123">[Creare un token di accesso personale GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) con autorizzazioni di lettura per il repository e passarlo a `withGitHubAccessToken`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-123">[Create a GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) with repo read permissions and pass it to `withGitHubAccessToken`.</span></span> 


## <a name="sample-explanation"></a><span data-ttu-id="c8e97-124">Spiegazione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="c8e97-124">Sample explanation</span></span>

<span data-ttu-id="c8e97-125">L'esempio crea la prima applicazione usando Java 8 e Tomcat 8 in esecuzione in un piano di servizio app [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) appena creato.</span><span class="sxs-lookup"><span data-stu-id="c8e97-125">The sample creates the first application using Java 8 and Tomcat 8 running in a newly created [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service plan.</span></span> <span data-ttu-id="c8e97-126">Il codice quindi trasmette tramite FTP un file WAR usando le informazioni nell'oggetto `PublishingProfile` e Tomcat lo distribuisce.</span><span class="sxs-lookup"><span data-stu-id="c8e97-126">The code then FTPs a WAR file using the information in the `PublishingProfile` object and Tomcat deploys it.</span></span>

<span data-ttu-id="c8e97-127">La seconda applicazione usa lo stesso piano della prima ed è anch'essa configurata come applicazione Java 8/Tomcat 8.</span><span class="sxs-lookup"><span data-stu-id="c8e97-127">The second application uses in the same plan as the first and is also configured as a Java 8/Tomcat 8 application.</span></span> <span data-ttu-id="c8e97-128">Le librerie JGit creano un nuovo repository Git in una cartella che contiene un'applicazione Web Java decompressa in una struttura di directory mappata al servizio app.</span><span class="sxs-lookup"><span data-stu-id="c8e97-128">The JGit libraries create a new Git repository in a folder that contains an unpacked Java web application in a directory structure that maps to App Service.</span></span> <span data-ttu-id="c8e97-129">Un nuovo commit aggiunge i file nella cartella al nuovo repository Git e Git esegue il push del commit in Azure con un URL remoto e con nome utente e password forniti dall'oggetto `PublishingProfile` dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="c8e97-129">A new commit adds the files in the folder to the new Git repo, and Git pushes the commit to Azure with a remote URL and username/password provided by the webapp's `PublishingProfile`.</span></span>

<span data-ttu-id="c8e97-130">La terza applicazione non è configurata per Java e Tomcat.</span><span class="sxs-lookup"><span data-stu-id="c8e97-130">The third application is not configured for Java and Tomcat.</span></span> <span data-ttu-id="c8e97-131">Viene invece distribuito un esempio .NET in un repository GitHub pubblico direttamente dall'origine.</span><span class="sxs-lookup"><span data-stu-id="c8e97-131">Instead, a .NET sample in a public GitHub repo is deployed directly from source.</span></span>

<span data-ttu-id="c8e97-132">La quarta applicazione distribuisce il codice nel ramo principale ogni volta che si esegue il push delle modifiche o si unisce una richiesta di pull nel ramo principale del repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="c8e97-132">The fourth application deploys the code in your master branch every time you push changes or merge a pull request into the GitHub repo's master branch.</span></span>

| <span data-ttu-id="c8e97-133">Classe usata nell'esempio</span><span class="sxs-lookup"><span data-stu-id="c8e97-133">Class used in sample</span></span> | <span data-ttu-id="c8e97-134">Note</span><span class="sxs-lookup"><span data-stu-id="c8e97-134">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="c8e97-135">WebApp</span><span class="sxs-lookup"><span data-stu-id="c8e97-135">WebApp</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | <span data-ttu-id="c8e97-136">Creata dalla catena Fluent `azure.webApps().define()....create()`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-136">Created from the `azure.webApps().define()....create()` fluent chain.</span></span> <span data-ttu-id="c8e97-137">Crea un'app Web del servizio app e tutte le risorse necessarie per l'app.</span><span class="sxs-lookup"><span data-stu-id="c8e97-137">Creates a App Service web app and any resources needed for the app.</span></span> <span data-ttu-id="c8e97-138">La maggior parte dei metodi esegue una query sull'oggetto per i dettagli di configurazione, ma i metodi verbo come `restart()` modificano lo stato dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="c8e97-138">Most methods query the object for configuration details, but verb methods like `restart()` change the state of the webapp.</span></span>
| [<span data-ttu-id="c8e97-139">WebContainer</span><span class="sxs-lookup"><span data-stu-id="c8e97-139">WebContainer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | <span data-ttu-id="c8e97-140">Classe con campi pubblici statici usati come parametri di `withWebContainer()` quando si definisce un'app Web che esegue un contenitore Web Java.</span><span class="sxs-lookup"><span data-stu-id="c8e97-140">Class with static public fields used as paramters to `withWebContainer()` when defining a WebApp running a Java webcontainer.</span></span> <span data-ttu-id="c8e97-141">Offre opzioni per entrambe le versioni, Jetty e Tomcat.</span><span class="sxs-lookup"><span data-stu-id="c8e97-141">Has choices for both Jetty and Tomcat versions.</span></span>
| [<span data-ttu-id="c8e97-142">PublishingProfile</span><span class="sxs-lookup"><span data-stu-id="c8e97-142">PublishingProfile</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | <span data-ttu-id="c8e97-143">Classe ottenuta tramite un oggetto WebApp usando il metodo `getPublishingProfile()`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-143">Obtained through a WebApp object using the `getPublishingProfile()` method.</span></span> <span data-ttu-id="c8e97-144">Contiene informazioni di distribuzione FTP e Git, inclusi nome utente e password della distribuzione (distinti dall'account di Azure o dalle credenziali dell'entità servizio).</span><span class="sxs-lookup"><span data-stu-id="c8e97-144">Contains FTP and Git deployment information, including deployment username and password (which is separate from Azure account or service principal credentials).</span></span>
| [<span data-ttu-id="c8e97-145">AppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c8e97-145">AppServicePlan</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | <span data-ttu-id="c8e97-146">Classe restituita da `azure.appServices().appServicePlans().getByResourceGroup()`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-146">Returned by `azure.appServices().appServicePlans().getByResourceGroup()`.</span></span> <span data-ttu-id="c8e97-147">Sono disponibili metodi per verificare la capacità, il livello e il numero di app Web in esecuzione nel piano.</span><span class="sxs-lookup"><span data-stu-id="c8e97-147">Methods are availble to check the capacity, tier, and number of web apps running in the plan.</span></span>
| [<span data-ttu-id="c8e97-148">AppServicePricingTier</span><span class="sxs-lookup"><span data-stu-id="c8e97-148">AppServicePricingTier</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | <span data-ttu-id="c8e97-149">Classe con campi pubblici statici che rappresentano i livelli del servizio app.</span><span class="sxs-lookup"><span data-stu-id="c8e97-149">Class with static public fields representing App Service tiers.</span></span> <span data-ttu-id="c8e97-150">Usata per definire una livello di piano inline durante la creazione dell'app con `withPricingTier()` o direttamente quando si definisce un piano tramite `azure.appServices().appServicePlans().define()`.</span><span class="sxs-lookup"><span data-stu-id="c8e97-150">Used to define a plan tier in-line during app creation with `withPricingTier()` or directly when defining a plan via `azure.appServices().appServicePlans().define()`</span></span>
| [<span data-ttu-id="c8e97-151">JavaVersion</span><span class="sxs-lookup"><span data-stu-id="c8e97-151">JavaVersion</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | <span data-ttu-id="c8e97-152">Classe con campi pubblici statici che rappresentano le versioni di Java supportate dal servizio app.</span><span class="sxs-lookup"><span data-stu-id="c8e97-152">Class with static public fields representing Java versions supported by App Service.</span></span> <span data-ttu-id="c8e97-153">Usata con `withJavaVersion()` nella catena `define()...create()` quando si crea una nuova app Web.</span><span class="sxs-lookup"><span data-stu-id="c8e97-153">Used with `withJavaVersion()` during the `define()...create()` chain when creating a new webapp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8e97-154">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c8e97-154">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]