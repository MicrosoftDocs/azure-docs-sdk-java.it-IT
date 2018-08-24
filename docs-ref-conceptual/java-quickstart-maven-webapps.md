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
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a>Creare e distribuire un'app Java in Azure con Maven

Questa guida introduttiva illustra come creare e distribuire una semplice app Web Java nel [servizio app di Azure](/azure/app-service/app-service-value-prop-what-is) in pochi minuti usando [Apache Maven](http://maven.apache.org) e l'interfaccia della riga di comando di Azure.

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare, installare quanto segue:

- [Git](https://git-scm.com/)
- [Java 8](https://www.azul.com/downloads/zulu/)
- [Maven 3](http://maven.apache.org/download.cgi)
- [Interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="get-the-sample-code"></a>Scaricare il codice di esempio

Clonare l'archivio dell'app di esempio nel computer locale. L'esempio è una semplice applicazione JSP con configurazione aggiuntiva di Maven nel progetto `pom.xml` per testare l'app in locale e distribuire l'esempio nel servizio app di Azure.

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a>Eseguire l'app in locale

Aprire un prompt dei comandi e usare Maven per compilare l'app ed eseguirla in un contenitore Web [Tomcat](https://tomcat.apache.org) locale. 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

Aprire un Web browser e passare a http://localhost:8080 per visualizzare in anteprima l'app:

  ![Output di Hello World dall'app Java di esempio](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a>Accedere ad Azure

Accedere all'interfaccia della riga di comando di Azure con `az login`. L'interfaccia della riga di comando viene usata in questa guida introduttiva per eseguire query e creare le risorse di Azure necessarie per ospitare l'app.
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse   

Come prima cosa creare un gruppo di risorse con [az group create](/cli/azure/group#create). Un gruppo di risorse di Azure è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite.

```azurecli
az group create --location "East US" --name myResourceGroup
```

Per visualizzare gli altri possibili valori disponibili per `---location`, usare [az appservice list-locations](/cli/azure/appservice#list-locations).

## <a name="create-an-app-service-plan"></a>Creare un piano di servizio app

Creare un [piano di servizio app](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) **GRATUITO** con [az appservice plan create](/cli/azure/appservice/plan#create). I piani di servizio app allocano le risorse condivise tra tutte le app Web eseguite nel piano.


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

Quando il piano di servizio app è pronto, l'interfaccia della riga di comando di Azure visualizza informazioni simili all'esempio seguente:

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


## <a name="create-a-web-app"></a>Creare un'app Web

Creare un'app Web che usa le risorse del piano con il comando [az webapp create](/cli/azure/appservice/web#create) dell'interfaccia della riga di comando di Azure. La definizione dell'app Web offre uno spazio per la distribuzione del codice e un URL per l'accesso all'app quando è in esecuzione.

Nel comando seguente sostituire il segnaposto <appname> con il nome univoco della propria app. <appname> viene usato nel nome host predefinito per l'app Web. Se <appname> non è univoco, verrà visualizzato un messaggio di errore descrittivo che indica che il sito Web con il nome specificato esiste già.

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

Al termine della creazione dell'app Web, l'interfaccia della riga di comando di Azure visualizza informazioni simili all'esempio seguente:

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

## <a name="configure-java-and-tomcat"></a>Configurare Java e Tomcat

Configurare l'app Web per l'uso di Java e Tomcat con il comando [az webapp config](/cli/azure/appservice/web#config) dell'interfaccia della riga di comando di Azure. L'esempio seguente configura il servizio app per eseguire Java 8 e [Apache Tomcat 8.5](http://tomcat.apache.org/) per l'esecuzione dell'app.

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a>Configurare Maven 

Il file `pom.xml` di Maven di esempio include la configurazione per la distribuzione FTP dell'esempio in Azure, ma deve essere personalizzato per la distribuzione nella propria app Web. Recuperare le credenziali del servizio app con [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles):

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

Sostituire i segnaposto nel file `az-settings.xml` con la password, il nome utente e il nome host FTP dell'output. Assicurarsi di usare un solo carattere `\` nel nome utente invece del valore preceduto da un carattere di escape dell'output dell'interfaccia della riga di comando.
   
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

## <a name="deploy-the-sample-application"></a>Distribuire l'applicazione di esempio

Distribuire l'esempio in Azure, includendo il parametro `-s` di Maven per usare la configurazione nel file `az-settings.xml`.

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a>Passare all'app Web

Visualizzare l'esempio in esecuzione in Azure:

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a>Aggiornare l'app

Usando un editor di testo, aprire `src/main/webapp/index.jsp` e sostituirne il contenuto JSP esistente con il seguente:

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

Distribuire l'aggiornamento con Maven:

```
mvn clean package
mvn install -s az-settings.xml
```

Aggiornare il browser dopo la ridistribuzione dell'app per visualizzare le modifiche.

## <a name="manage-your-new-azure-app"></a>Gestire la nuova app Azure

Passare al portale di Azure per esaminare l'app Web appena creata.

A tale scopo, accedere a [https://portal.azure.com](https://portal.azure.com).

Nel menu a sinistra fare clic su **Servizi app** e quindi sul nome dell'app Web di Azure.

 ![Selezionare l'app nell'elenco delle app Web nel portale](media/azure-app-service-portal.png)

Testare il monitoraggio eseguendo `curl` sull'app per inviare traffico.

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

L'attività delle richieste verrà visualizzata nel monitoraggio dopo qualche minuto.

 ![Monitoraggio nel portale di Azure](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a>Pulire le risorse

Per rimuovere tutte le risorse create in questa guida, eseguire questo comando:

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a>Passaggi successivi

Esplorare l'elenco completo degli [esempi Java per Azure](https://azure.microsoft.com/resources/samples/?term=java).
