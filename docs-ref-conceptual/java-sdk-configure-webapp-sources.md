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
ms.locfileid: "21931187"
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a>Configurare le origini distribuzione di Servizio app di Azure dalle applicazioni Java

[Questo esempio](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) distribuisce il codice a quattro applicazioni in un singolo piano di [Servizio app di Azure](https://docs.microsoft.com/azure/app-service/), ognuna delle quali usa un'origine distribuzione diversa.

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Quindi eseguire:

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

Visualizzare l'[esempio di codice completo in GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java).

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a>Creare un'app del servizio app che esegue Apache Tomcat

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

`withJavaVersion()` e `withWebContainer()` configurano il servizio app per rispondere alle richieste HTTP usando Tomcat 8.

## <a name="deploy-a-java-application-using-ftp"></a>Distribuire un'applicazione Java tramite FTP
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

Questo codice carica un file WAR nella directory `/site/wwwroot/webapps`. Tomcat distribuisce per impostazione predefinita i file WAR inseriti in questa directory nel servizio app.

## <a name="deploy-a-java-application-from-a-local-git-repo"></a>Distribuire un'applicazione Java da un repository Git locale

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

Questo codice usa le librerie [JGit](https://eclipse.org/jgit/) per creare un nuovo repository Git nella cartella `src/main/resources/azure-samples-appservice-helloworld`. L'esempio aggiunge quindi tutti i file nella cartella a un commit iniziale ed esegue il push del commit in Azure usando le informazioni di distribuzione Git dell'oggetto `PublishingProfile` dell'app Web. 

>[!NOTE]
> Il layout dei file nel repository deve corrispondere esattamente al modo in cui si vuole che i file vengano distribuiti nella directory `/site/wwwroot/` in Servizio app di Azure.

## <a name="deploy-an-application-from-a-public-git-repo"></a>Distribuire un'applicazione da un repository Git pubblico

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

 Il runtime del servizio app compila e distribuisce automaticamente il progetto .NET usando il codice più recente nel ramo `master` del repository.

## <a name="continuous-deployment-from-a-github-repo"></a>Distribuzione continua da un repository GitHub

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

I valori `username` e `reponame` sono quelli usati in GitHub. [Creare un token di accesso personale GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) con autorizzazioni di lettura per il repository e passarlo a `withGitHubAccessToken`. 


## <a name="sample-explanation"></a>Spiegazione dell'esempio

L'esempio crea la prima applicazione usando Java 8 e Tomcat 8 in esecuzione in un piano di servizio app [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) appena creato. Il codice quindi trasmette tramite FTP un file WAR usando le informazioni nell'oggetto `PublishingProfile` e Tomcat lo distribuisce.

La seconda applicazione usa lo stesso piano della prima ed è anch'essa configurata come applicazione Java 8/Tomcat 8. Le librerie JGit creano un nuovo repository Git in una cartella che contiene un'applicazione Web Java decompressa in una struttura di directory mappata al servizio app. Un nuovo commit aggiunge i file nella cartella al nuovo repository Git e Git esegue il push del commit in Azure con un URL remoto e con nome utente e password forniti dall'oggetto `PublishingProfile` dell'app Web.

La terza applicazione non è configurata per Java e Tomcat. Viene invece distribuito un esempio .NET in un repository GitHub pubblico direttamente dall'origine.

La quarta applicazione distribuisce il codice nel ramo principale ogni volta che si esegue il push delle modifiche o si unisce una richiesta di pull nel ramo principale del repository GitHub.

| Classe usata nell'esempio | Note
|-------|-------|
| [WebApp](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | Creata dalla catena Fluent `azure.webApps().define()....create()`. Crea un'app Web del servizio app e tutte le risorse necessarie per l'app. La maggior parte dei metodi esegue una query sull'oggetto per i dettagli di configurazione, ma i metodi verbo come `restart()` modificano lo stato dell'app Web.
| [WebContainer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | Classe con campi pubblici statici usati come parametri di `withWebContainer()` quando si definisce un'app Web che esegue un contenitore Web Java. Offre opzioni per entrambe le versioni, Jetty e Tomcat.
| [PublishingProfile](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | Classe ottenuta tramite un oggetto WebApp usando il metodo `getPublishingProfile()`. Contiene informazioni di distribuzione FTP e Git, inclusi nome utente e password della distribuzione (distinti dall'account di Azure o dalle credenziali dell'entità servizio).
| [AppServicePlan](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | Classe restituita da `azure.appServices().appServicePlans().getByResourceGroup()`. Sono disponibili metodi per verificare la capacità, il livello e il numero di app Web in esecuzione nel piano.
| [AppServicePricingTier](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | Classe con campi pubblici statici che rappresentano i livelli del servizio app. Usata per definire una livello di piano inline durante la creazione dell'app con `withPricingTier()` o direttamente quando si definisce un piano tramite `azure.appServices().appServicePlans().define()`.
| [JavaVersion](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | Classe con campi pubblici statici che rappresentano le versioni di Java supportate dal servizio app. Usata con `withJavaVersion()` nella catena `define()...create()` quando si crea una nuova app Web.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]