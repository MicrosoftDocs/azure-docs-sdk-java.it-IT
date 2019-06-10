---
title: Come usare Spring e Cosmos DB con Servizio app in Linux
description: Questo articolo illustra la procedura per compilare, configurare, distribuire e ridimensionare le app Web Java in Servizio app in Linux, oltre che per risolvere i problemi.
documentationcenter: java
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 4/24/2019
ms.devlang: java
ms.service: app-service, cosmos-db
ms.topic: article
ms.openlocfilehash: 5d209c0670d6f4265b1237e7b8cff45faa9bb5d8
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568634"
---
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a>Come usare Spring e Cosmos DB con Servizio app in Linux

## <a name="overview"></a>Panoramica

Questo articolo illustra la procedura per compilare, configurare, distribuire e ridimensionare le app Web Java in Servizio app in Linux, oltre che per risolvere i problemi.

Verrà dimostrato l'utilizzo dei componenti seguenti:

- [Spring Boot Starter con l'API SQL di Azure Cosmos DB](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
- [Servizio app in Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

- Per distribuire un'app Web Java nel cloud, è necessaria una sottoscrizione di Azure. Se non si ha già una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito]((https://azure.microsoft.com/pricing/free-trial/)).
- [Interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Java 8 JDK](https://docs.microsoft.com/java/azure/jdk/java-jdk-install)
- [Maven 3](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a>Clonare il repository di app Web Java di esempio
Per questa esercitazione si userà l'app Spring Todo, un'applicazione Java creata con [Spring Boot](https://spring.io/projects/spring-boot), [Spring Data per Cosmos DB](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable) e [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction).
1. Per inizializzare il progetto, clonare l'app Spring Todo e copiare il contenuto della cartella con estensione **prep**:

    Per Bash:
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    Per Windows:
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. Passare alla cartella seguente nel repository clonato:

   ```bash
   cd initial\spring-todo-app
   ``` 
## <a name="create-an-azure-cosmos-db-from-azure-cli"></a>Creare un'istanza di Azure Cosmos DB dall'interfaccia della riga di comando di Azure

1. Accedere all'interfaccia della riga di comando di Azure e impostare l'ID sottoscrizione.

    ```bash
    az login
    ```

2. Se necessario, impostare l'ID sottoscrizione.
    ```bash
    az account set -s <your-subscription-id>
    ```

3. Creare un gruppo di risorse di Azure e prendere nota del relativo nome per uso futuro.

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. Creare l'istanza di Cosmos DB e specificare GlobalDocumentDB come tipo.
Il nome dell'istanza di Cosmos DB deve contenere solo lettere minuscole. Assicurarsi di prendere nota del contenuto del campo `documentEndpoint` nella risposta, che sarà necessario più avanti.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

4. Ottenere le chiavi di Azure Cosmos DB e prendere nota del valore di `primaryMasterKey` per uso futuro.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a>Compilare ed eseguire l'app in locale

1. All'interno di una console a scelta configurare le variabili di ambiente mostrate nelle sezioni di codice seguenti con le informazioni di connessione per Azure e Cosmos DB raccolte in precedenza in questo articolo. Sarà necessario specificare un nome univoco per **WEBAPP_NAME** e un valore per le variabili **REGION**.

Per Linux (Bash):

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Per Windows (prompt dei comandi):
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> Se si preferisce eseguire il provisioning di queste variabili con uno script, è disponibile un modello per Bash nella directory prep che è possibile copiare e usare come punto iniziale.

2. Passare alla directory seguente:

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. Eseguire l'app Spring Todo in locale con il comando seguente:

    ```bash
    mvn package spring-boot:run
    ```

4. Dopo l'avvio dell'applicazione, è possibile verificare la distribuzione accedendo all'app Spring Todo qui: [http://localhost:8080/](http://localhost:8080/).

 ![App Spring in esecuzione in locale][SCDB01]

## <a name="deploy-to-app-service-linux"></a>Eseguire la distribuzione nel Servizio app in Linux

1. Aprire il file pom.xml copiato in precedenza nella directory **initial/spring-todo-app** del repository. Assicurarsi che il [plug-in Maven per Servizio app di Azure](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) sia incluso, come illustrato nel file pom.xml seguente. Se la versione non è impostata su **1.6.0**, aggiornare il valore.

```xml
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.6.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

2. Eseguire la distribuzione in Java SE in Servizio app in Linux

    ```bash
    mvn azure-webapp:deploy
    ```

```bash
// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.6.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesnt exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlan11111111-1111-1111'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

3. Passare all'app Web in esecuzione in Java SE in Servizio app in Linux:

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![App Spring in esecuzione in Servizio app in Linux][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a>Risolvere i problemi dell'app Spring Todo in Azure visualizzando i log

1. Configurare i log per l'app Web Java distribuita in Servizio app di Azure in Linux:

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. Aprire il flusso di log remoti dell'app Web Java da un computer locale:

    ```bash
    az webapp log tail --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME}
     ```

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: http://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers needs non existent service dev
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======


```

3. Al termine, è possibile confrontare i risultati con il codice incluso in [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete).


## <a name="scale-out-the-spring-todo-app"></a>Aumentare le istanze dell'app Spring Todo

1. Aumentare le istanze dell'app Web Java con l'interfaccia della riga di comando di Azure:

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a>Passaggi successivi

- [Guida dello sviluppatore Java per il servizio app in Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-java)
- [Azure per sviluppatori Java](https://docs.microsoft.com/en-us/java/azure/) Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Azure per sviluppatori Java]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
