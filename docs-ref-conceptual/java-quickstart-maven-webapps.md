---
title: Distribuire un'app Web Java in Azure in cinque minuti con Maven | Microsoft Docs
description: Creare e distribuire un'app Java compilata con Maven in Azure
services: app-service\web
documentationcenter: ''
author: rloutlaw
manager: douge
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: routlaw
ms.openlocfilehash: 70b508118c50b75693e2d746dc1e2919c827cb29
ms.sourcegitcommit: 0f38ef9ad64cffdb7b2e9e966224dfd0af251b0f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42703544"
---
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a><span data-ttu-id="8b992-103">Creare e distribuire un'app Java in Azure con Maven</span><span class="sxs-lookup"><span data-stu-id="8b992-103">Create and deploy a Java app to Azure with Maven</span></span>

<span data-ttu-id="8b992-104">Questa guida introduttiva illustra come creare e distribuire una semplice app Web Java nel [servizio app di Azure](/azure/app-service/app-service-value-prop-what-is) in pochi minuti usando [Apache Maven](http://maven.apache.org) e l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b992-104">This quickstart helps you create and deploy a simple Java web app to [Azure App Service](/azure/app-service/app-service-value-prop-what-is) in just a few minutes using [Apache Maven](http://maven.apache.org) and the Azure CLI.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8b992-105">Prima di iniziare</span><span class="sxs-lookup"><span data-stu-id="8b992-105">Before you begin</span></span>

<span data-ttu-id="8b992-106">Prima di iniziare, installare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="8b992-106">Before you start, set up the following:</span></span>

- [<span data-ttu-id="8b992-107">Git</span><span class="sxs-lookup"><span data-stu-id="8b992-107">Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="8b992-108">Java 8</span><span class="sxs-lookup"><span data-stu-id="8b992-108">Java 8</span></span>](https://www.azul.com/downloads/zulu/)
- [<span data-ttu-id="8b992-109">Maven 3</span><span class="sxs-lookup"><span data-stu-id="8b992-109">Maven 3</span></span>](http://maven.apache.org/download.cgi)
- [<span data-ttu-id="8b992-110">Interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="8b992-110">Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-az-cli2)

<span data-ttu-id="8b992-111">Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="8b992-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="get-the-sample-code"></a><span data-ttu-id="8b992-112">Scaricare il codice di esempio</span><span class="sxs-lookup"><span data-stu-id="8b992-112">Get the sample code</span></span>

<span data-ttu-id="8b992-113">Clonare l'archivio dell'app di esempio nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="8b992-113">Clone the sample app repository to your local machine.</span></span> <span data-ttu-id="8b992-114">L'esempio è una semplice applicazione JSP con configurazione aggiuntiva di Maven nel progetto `pom.xml` per testare l'app in locale e distribuire l'esempio nel servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b992-114">The sample is a simple JSP application with additional Maven configuration in the project `pom.xml` to test the app locally and help deploy the sample to Azure App Service.</span></span>

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a><span data-ttu-id="8b992-115">Eseguire l'app in locale</span><span class="sxs-lookup"><span data-stu-id="8b992-115">Run the app locally</span></span>

<span data-ttu-id="8b992-116">Aprire un prompt dei comandi e usare Maven per compilare l'app ed eseguirla in un contenitore Web [Tomcat](https://tomcat.apache.org) locale.</span><span class="sxs-lookup"><span data-stu-id="8b992-116">Open a command prompt and use Maven to build the app and run it in a local [Tomcat](https://tomcat.apache.org) web container.</span></span> 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

<span data-ttu-id="8b992-117">Aprire un Web browser e passare a http://localhost:8080 per visualizzare in anteprima l'app:</span><span class="sxs-lookup"><span data-stu-id="8b992-117">Open a web browser and navigate to http://localhost:8080 to preview the app:</span></span>

  ![Output di Hello World dall'app Java di esempio](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="8b992-119">Accedere ad Azure</span><span class="sxs-lookup"><span data-stu-id="8b992-119">Log in to Azure</span></span>

<span data-ttu-id="8b992-120">Accedere all'interfaccia della riga di comando di Azure con `az login`.</span><span class="sxs-lookup"><span data-stu-id="8b992-120">Log in to the Azure CLI with `az login`.</span></span> <span data-ttu-id="8b992-121">L'interfaccia della riga di comando viene usata in questa guida introduttiva per eseguire query e creare le risorse di Azure necessarie per ospitare l'app.</span><span class="sxs-lookup"><span data-stu-id="8b992-121">This quickstart uses the CLI to query and create the Azure resources needed to host the app.</span></span>
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="8b992-122">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="8b992-122">Create a resource group</span></span>   

<span data-ttu-id="8b992-123">Come prima cosa creare un gruppo di risorse con [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="8b992-123">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="8b992-124">Un gruppo di risorse di Azure è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite.</span><span class="sxs-lookup"><span data-stu-id="8b992-124">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

```azurecli
az group create --location "East US" --name myResourceGroup
```

<span data-ttu-id="8b992-125">Per visualizzare gli altri possibili valori disponibili per `---location`, usare [az appservice list-locations](/cli/azure/appservice#list-locations).</span><span class="sxs-lookup"><span data-stu-id="8b992-125">To see other possible values you can use for `---location`, use [az appservice list-locations](/cli/azure/appservice#list-locations)</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="8b992-126">Creare un piano di servizio app</span><span class="sxs-lookup"><span data-stu-id="8b992-126">Create an App Service plan</span></span>

<span data-ttu-id="8b992-127">Creare un [piano di servizio app](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) **GRATUITO** con [az appservice plan create](/cli/azure/appservice/plan#create).</span><span class="sxs-lookup"><span data-stu-id="8b992-127">Create a **FREE** [App Service plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) using [az appservice plan create](/cli/azure/appservice/plan#create).</span></span> <span data-ttu-id="8b992-128">I piani di servizio app allocano le risorse condivise tra tutte le app Web eseguite nel piano.</span><span class="sxs-lookup"><span data-stu-id="8b992-128">App Service plans allocate resources shared across all web apps running in the plan.</span></span>


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="8b992-129">Quando il piano di servizio app è pronto, l'interfaccia della riga di comando di Azure visualizza informazioni simili all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="8b992-129">When the App Service Plan is ready, the Azure CLI shows information similar to the following example:</span></span>

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "location": "East US",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```


## <a name="create-a-web-app"></a><span data-ttu-id="8b992-130">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="8b992-130">Create a web app</span></span>

<span data-ttu-id="8b992-131">Creare un'app Web che usa le risorse del piano con il comando [az webapp create](/cli/azure/appservice/web#create) dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b992-131">Create a web app that uses the plan's resources using the Azure CLI [az webapp create](/cli/azure/appservice/web#create) command.</span></span> <span data-ttu-id="8b992-132">La definizione dell'app Web offre uno spazio per la distribuzione del codice e un URL per l'accesso all'app quando è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="8b992-132">The web app definition provides a space to deploy the code to and a URL to access the app once it's running.</span></span>

<span data-ttu-id="8b992-133">Nel comando seguente sostituire il segnaposto <appname> con il nome univoco della propria app.</span><span class="sxs-lookup"><span data-stu-id="8b992-133">In the command below substitute your own unique app name where you see the <appname> placeholder.</span></span> <span data-ttu-id="8b992-134"><appname> viene usato nel nome host predefinito per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="8b992-134">The <appname> is used in the default hostname for the web app.</span></span> <span data-ttu-id="8b992-135">Se <appname> non è univoco, verrà visualizzato un messaggio di errore descrittivo che indica che il sito Web con il nome specificato esiste già.</span><span class="sxs-lookup"><span data-stu-id="8b992-135">If <appname> is not unique, you get the friendly error message "Website with given name already exists."</span></span>

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

<span data-ttu-id="8b992-136">Al termine della creazione dell'app Web, l'interfaccia della riga di comando di Azure visualizza informazioni simili all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="8b992-136">When the Web App has been created, the Azure CLI shows information similar to the following example.1</span></span>

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<appname>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<appname>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "East US",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

## <a name="configure-java-and-tomcat"></a><span data-ttu-id="8b992-137">Configurare Java e Tomcat</span><span class="sxs-lookup"><span data-stu-id="8b992-137">Configure Java and Tomcat</span></span>

<span data-ttu-id="8b992-138">Configurare l'app Web per l'uso di Java e Tomcat con il comando [az webapp config](/cli/azure/appservice/web#config) dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b992-138">Configure the web app to use Java and Tomcat using the Azure CLI's [az webapp config](/cli/azure/appservice/web#config) command.</span></span> <span data-ttu-id="8b992-139">L'esempio seguente configura il servizio app per eseguire Java 8 e [Apache Tomcat 8.5](http://tomcat.apache.org/) per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="8b992-139">The example below configures App Service to run Java 8 and [Apache Tomcat 8.5](http://tomcat.apache.org/) to run the app.</span></span>

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a><span data-ttu-id="8b992-140">Configurare Maven</span><span class="sxs-lookup"><span data-stu-id="8b992-140">Configure Maven</span></span> 

<span data-ttu-id="8b992-141">Il file `pom.xml` di Maven di esempio include la configurazione per la distribuzione FTP dell'esempio in Azure, ma deve essere personalizzato per la distribuzione nella propria app Web.</span><span class="sxs-lookup"><span data-stu-id="8b992-141">The sample's Maven `pom.xml` includes configuration to FTP the sample into Azure, but you'll need to customize it to deploy to your own web app.</span></span> <span data-ttu-id="8b992-142">Recuperare le credenziali del servizio app con [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles):</span><span class="sxs-lookup"><span data-stu-id="8b992-142">Retreive your App Service credentials with [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles):</span></span>

```azurecli
az webapp deployment list-publishing-profiles  \
--name appname \ 
--resource-group myResourceGroup  \
--query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" 
```

```json
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "appname\\$appname"
  }
]
```

<span data-ttu-id="8b992-143">Sostituire i segnaposto nel file `az-settings.xml` con la password, il nome utente e il nome host FTP dell'output.</span><span class="sxs-lookup"><span data-stu-id="8b992-143">Replace the placeholders in the `az-settings.xml` file with password, username, and FTP hostname from the output.</span></span> <span data-ttu-id="8b992-144">Assicurarsi di usare un solo carattere `\` nel nome utente invece del valore preceduto da un carattere di escape dell'output dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="8b992-144">Make sure to only use one `\` character in the username instead of the escaped value from the CLI output.</span></span>
   
```XML
...
    <server>
      <id>azure-hello-world</id>
      <username>appname\$appname</username>
      <password>aBcDeFgHiJkLmNoPqRsTuVwXyZ</password>
    </server>
...
   <configuration>
     <fromFile>${basedir}/target/AzureAppDemo-0.0.1-SNAPSHOT.war</fromFile>
     <url>ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot</url>
     <toFile>webapps/ROOT.war</toFile>
     <serverId>azure-hello-world</serverId>
   </configuration>
...
```

## <a name="deploy-the-sample-application"></a><span data-ttu-id="8b992-145">Distribuire l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="8b992-145">Deploy the sample application</span></span>

<span data-ttu-id="8b992-146">Distribuire l'esempio in Azure, includendo il parametro `-s` di Maven per usare la configurazione nel file `az-settings.xml`.</span><span class="sxs-lookup"><span data-stu-id="8b992-146">Deploy with sample to Azure, using Maven's `-s` parameter to use the configuration in the `az-settings.xml` file.</span></span>

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a><span data-ttu-id="8b992-147">Passare all'app Web</span><span class="sxs-lookup"><span data-stu-id="8b992-147">Browse to web app</span></span>

<span data-ttu-id="8b992-148">Visualizzare l'esempio in esecuzione in Azure:</span><span class="sxs-lookup"><span data-stu-id="8b992-148">View the sample running in Azure:</span></span>

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a><span data-ttu-id="8b992-149">Aggiornare l'app</span><span class="sxs-lookup"><span data-stu-id="8b992-149">Update the app</span></span>

<span data-ttu-id="8b992-150">Usando un editor di testo, aprire `src/main/webapp/index.jsp` e sostituirne il contenuto JSP esistente con il seguente:</span><span class="sxs-lookup"><span data-stu-id="8b992-150">Using a text editor, open `src/main/webapp/index.jsp`, and replace the existing JSP with the one below.</span></span>

```html
<%--
  Copyright (c) Microsoft Corporation. All rights reserved.
  Licensed under the MIT License. See License.txt in the project root for
  license information.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java"  import="java.util.Date" %>
<html>
<head>
  <title>Azure Samples Hello World</title>
</head>
<body>
  <H1>Hello Azure!</H1>
   Current time is: <%= new Date() %>
</body>
</html>
```

<span data-ttu-id="8b992-151">Distribuire l'aggiornamento con Maven:</span><span class="sxs-lookup"><span data-stu-id="8b992-151">Deploy the update with Maven:</span></span>

```
mvn clean package
mvn install -s az-settings.xml
```

<span data-ttu-id="8b992-152">Aggiornare il browser dopo la ridistribuzione dell'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="8b992-152">Refresh your browser after the app redeploys to view your changes.</span></span>

## <a name="manage-your-new-azure-app"></a><span data-ttu-id="8b992-153">Gestire la nuova app Azure</span><span class="sxs-lookup"><span data-stu-id="8b992-153">Manage your new Azure app</span></span>

<span data-ttu-id="8b992-154">Passare al portale di Azure per esaminare l'app Web appena creata.</span><span class="sxs-lookup"><span data-stu-id="8b992-154">Go to the Azure portal to take a look at the web app you just created.</span></span>

<span data-ttu-id="8b992-155">A tale scopo, accedere a [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8b992-155">To do this, sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="8b992-156">Nel menu a sinistra fare clic su **Servizi app** e quindi sul nome dell'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b992-156">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

 ![Selezionare l'app nell'elenco delle app Web nel portale](media/azure-app-service-portal.png)

<span data-ttu-id="8b992-158">Testare il monitoraggio eseguendo `curl` sull'app per inviare traffico.</span><span class="sxs-lookup"><span data-stu-id="8b992-158">Test the monitoring by running `curl` against the app to send some traffic.</span></span>

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

<span data-ttu-id="8b992-159">L'attività delle richieste verrà visualizzata nel monitoraggio dopo qualche minuto.</span><span class="sxs-lookup"><span data-stu-id="8b992-159">You'll see the request activity in the monitoring after a couple of minutes.</span></span>

 ![Monitoraggio nel portale di Azure](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a><span data-ttu-id="8b992-161">Pulire le risorse</span><span class="sxs-lookup"><span data-stu-id="8b992-161">Clean up resources</span></span>

<span data-ttu-id="8b992-162">Per rimuovere tutte le risorse create in questa guida, eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="8b992-162">To remove all the resources created in this guide, run the following command:</span></span>

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a><span data-ttu-id="8b992-163">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="8b992-163">Next steps</span></span>

<span data-ttu-id="8b992-164">Esplorare l'elenco completo degli [esempi Java per Azure](https://azure.microsoft.com/resources/samples/?term=java).</span><span class="sxs-lookup"><span data-stu-id="8b992-164">Browse the full list of [Azure Java samples](https://azure.microsoft.com/resources/samples/?term=java).</span></span>
