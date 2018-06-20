---
title: Introduzione alle librerie di Azure per Java
description: Informazioni su come creare risorse cloud di Azure e come connetterle e usarle nelle applicazioni Java.
keywords: Azure, Java, SDK, API, autenticare, introduzione
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: get-started-article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.openlocfilehash: f069183c96cdc42d590d2e58a5a6a500be5ab69a
ms.sourcegitcommit: 720c2eaf66532d277015610ec375c71e934d9ee6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2018
ms.locfileid: "29065528"
---
# <a name="get-started-with-cloud-development-using-the-azure-libraries-for-java"></a>Introduzione allo sviluppo per il cloud con le librerie di Azure per Java

Questa guida illustra la configurazione di un ambiente di sviluppo per lo sviluppo di Azure in Java. Verranno quindi create alcune risorse di Azure che verranno connesse per eseguire alcune attività di base, come caricare un file o distribuire un'applicazione Web. Al termine sarà possibile iniziare a usare i servizi di Azure nelle proprie applicazioni Java.

## <a name="prerequisites"></a>prerequisiti

- Un account Azure. Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).
- [Java 8](https://www.azul.com/downloads/zulu/) (incluso in Azure Cloud Shell)
- [Maven 3](http://maven.apache.org/download.cgi) (incluso in Azure Cloud Shell)

## <a name="set-up-authentication"></a>Configurare l'autenticazione

L'applicazione Java deve leggere e creare le autorizzazioni nella sottoscrizione di Azure per eseguire il codice di esempio in questa esercitazione. Creare un'entità servizio e configurare l'applicazione per l'esecuzione con le rispettive credenziali. Le entità servizio consentono di creare un account non interattivo associato all'identità a cui vengono concessi solo i privilegi necessari per l'esecuzione dell'app.

[Crea un'entità servizio usando l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) e acquisire l'output. Specificare una [password di protezione](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) nell'argomento password invece di `MY_SECURE_PASSWORD`. La password deve contenere da 8 a 16 caratteri e soddisfare almeno 3 dei 4 criteri seguenti:

* Includere caratteri minuscoli
* Includere caratteri maiuscoli
* Includere numeri
* Includere uno dei simboli seguenti: @ # $ % ^ & * - _ ! + = [ ] { } | \ : ‘ , . ? / ` ~ “ ( ) ;


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

Restituisce una risposta nel formato seguente:

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

Copiare quindi il codice seguente in un file di testo nel sistema:

```text
# sample management library properties file
subscription=ssssssss-ssss-ssss-ssss-ssssssssssss
client=cccccccc-cccc-cccc-cccc-cccccccccccc
key=kkkkkkkkkkkkkkkk
tenant=tttttttt-tttt-tttt-tttt-tttttttttttt
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

Sostituire i primi quattro valori con i seguenti:

- subscription: usare il valore *id* da `az account show` nell'interfaccia della riga di comando di Azure 2.0.
- client: usare il valore *appId* dell'output di un'entità servizio.
- key: usare il valore *password* dell'output dell'entità servizio.
- tenant: usare il valore *tenant* dell'output dell'entità servizio.

Salvare questo file nel sistema in una posizione sicura e leggibile dal codice. È possibile usare questo file per il codice futuro, quindi è consigliabile archiviarlo in una posizione esterna rispetto all'applicazione descritta in questo articolo.

Impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file di autenticazione nella shell.   

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

Se si lavora in ambiente Windows, aggiungere la variabile alle proprietà del sistema. Aprire una finestra di PowerShell con privilegi di amministratore, sostituire la seconda variabile con il percorso del file e quindi immettere il comando seguente:

```powershell
setx AZURE_AUTH_LOCATION "C:\<fullpath>\azureauth.properties" /m
```

## <a name="create-a-new-maven-project"></a>Creare un nuovo progetto Maven

> [!NOTE]
> Questa guida usa lo strumento di compilazione Maven per compilare ed eseguire il codice di esempio, ma con le librerie di Azure per Java si possono usare anche altri strumenti di compilazione, ad esempio Gradle. 

Creare un progetto Maven dalla riga di comando in una nuova directory del sistema:

```
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp  \ 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Viene creato un progetto Maven di base nella cartella `testAzureApp`. Aggiungere le voci seguenti nel progetto `pom.xml` per importare le librerie usate nel codice di esempio in questa esercitazione.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

Aggiungere una voce `build` sotto l'elemento `project` di primo livello per usare [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) per eseguire gli esempi:

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```
   
## <a name="create-a-linux-virtual-machine"></a>Creare una macchina virtuale Linux

Creare un nuovo file denominato `AzureApp.java` nella directory `src/main/java/com/fabirkam` del progetto e incollare il blocco di codice seguente. Aggiornare le variabili `userName` e `sshKey` con i valori reali del computer in uso. Questo codice crea una nuova VM Linux denominata `testLinuxVM` in un gruppo di risorse `sampleResourceGroup` in esecuzione nell'area di Azure Stati Uniti orientali.

```java
package com.fabrikam;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.WebApp;
import com.microsoft.azure.management.storage.StorageAccount;
import com.microsoft.azure.management.storage.SkuName;
import com.microsoft.azure.management.storage.StorageAccountKey;
import com.microsoft.azure.management.sql.SqlDatabase;
import com.microsoft.azure.management.sql.SqlServer;
import com.microsoft.azure.management.resources.fluentcore.arm.Region;
import com.microsoft.azure.management.resources.fluentcore.utils.SdkContext;

import com.microsoft.rest.LogLevel;

import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));    
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();
           
            // create a Ubuntu virtual machine in a new resource group 
            VirtualMachine linuxVM = azure.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();   

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

Eseguire l'esempio dalla riga di comando:

```
mvn compile exec:java
```

Verranno visualizzate alcune richieste e risposte REST nella console perché l'SDK esegue le chiamate sottostanti all'API REST di Azure per configurare la macchina virtuale e le risorse. Al termine del programma, verificare la macchina virtuale nella sottoscrizione con l'interfaccia della riga di comando di Azure 2.0:

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare la VM e le risorse.

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>Distribuire un'app Web da un repository di GitHub

Sostituire il metodo principale in `AzureApp.java` con quello seguente, aggiornando la variabile `appName` a un valore univoco prima di eseguire il codice. Questo codice sviluppa un'applicazione Web dal ramo `master` di un repository di GitHub pubblico in una nuova [app Web del servizio app di Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) in esecuzione nel piano tariffario gratuito.

```java
    public static void main(String[] args) {
        try {

            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            final String appName = "YOUR_APP_NAME";

            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            WebApp app = azure.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

Eseguire il codice come prima usando Maven:

```
mvn clean compile exec:java
```

Aprire un browser che punta all'applicazione usando l'interfaccia della riga di comando:

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

Rimuovere l'app Web e il piano dalla sottoscrizione dopo avere verificato la distribuzione.

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a>Connettersi a un database SQL di Azure

Sostituire il metodo principale corrente in `AzureApp.java` con il codice seguente, impostando un valore reale per la variabile `dbPassword`.
Questo codice crea un nuovo database SQL con una regola del firewall che consente l'accesso remoto e quindi vi si connette usando il driver JBDC del database SQL. 

```java

    public static void main(String args[])
    {
        // create the db using the management libraries
        try {
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            final String adminUser = SdkContext.randomResourceName("db",8);
            final String sqlServerName = SdkContext.randomResourceName("sql",10);
            final String sqlDbName = SdkContext.randomResourceName("dbname",8);
            final String dbPassword = "YOUR_PASSWORD_HERE";


            SqlServer sampleSQLServer = azure.sqlServers().define(sqlServerName)
                            .withRegion(Region.US_EAST)
                            .withNewResourceGroup("sampleSqlResourceGroup")
                            .withAdministratorLogin(adminUser)
                            .withAdministratorPassword(dbPassword)
                            .withNewFirewallRule("0.0.0.0","255.255.255.255")
                            .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // assemble the connection string to the database
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // connect to the database, create a table and insert a entry into it
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```
Eseguire l'esempio dalla riga di comando:

```
mvn clean compile exec:java
```

Pulire quindi le risorse usando l'interfaccia della riga di comando:

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a>Scrivere un BLOB in un nuovo account di archiviazione

Sostituire il metodo principale corrente in `AzureApp.java` con il codice seguente. Questo codice crea un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e quindi usa le librerie di archiviazione di Azure per Java per creare un nuovo file di testo nel cloud.

```java
public static void main(String[] args) {

    try {

        // use the properties file with the service principal information to authenticate
        // change the name of the environment variable if you used a different name in the previous step
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
                .withLogLevel(LogLevel.BASIC)
                .authenticate(credFile)
                .withDefaultSubscription();

        // create a new storage account
        String storageAccountName = SdkContext.randomResourceName("st",8);
        StorageAccount storage = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleStorageResourceGroup")
                    .create();

        // create a storage container to hold the file
        List<StorageAccountKey> keys = storage.getKeys();
        final String storageConnection = "DefaultEndpointsProtocol=https;"
                + "AccountName=" + storage.name()
                + ";AccountKey=" + keys.get(0).value()
                + ";EndpointSuffix=core.windows.net";

        CloudStorageAccount account = CloudStorageAccount.parse(storageConnection);
        CloudBlobClient serviceClient = account.createCloudBlobClient();

        // Container name must be lower case.
        CloudBlobContainer container = serviceClient.getContainerReference("helloazure");
        container.createIfNotExists();

        // Make the container public
        BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
        containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
        container.uploadPermissions(containerPermissions);

        // write a blob to the container
        CloudBlockBlob blob = container.getBlockBlobReference("helloazure.txt");
        blob.uploadText("hello Azure");

    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }
    }
}
```

Eseguire l'esempio dalla riga di comando:

```
mvn clean compile exec:java
```

È possibile cercare il file `helloazure.txt` nell'account di archiviazione tramite il portale di Azure o con [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).

Pulire l'account di archiviazione usando l'interfaccia della riga di comando:

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>Esplorare altri esempi

Per altre informazioni su come usare le librerie di gestione di Azure per Java per gestire le risorse e l'automazione delle attività, vedere il codice di esempio per [macchine virtuali](java-sdk-azure-virtual-machine-samples.md), [app Web](java-sdk-azure-web-apps-samples.md) e [database SQL](java-sdk-azure-sql-database-samples.md).

## <a name="reference-and-release-notes"></a>Informazioni di riferimento e note sulla versione

Le [informazioni di riferimento](http://docs.microsoft.com/java/api) sono disponibili per tutti i pacchetti.

## <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

Pubblicare le domande per la community in [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java). Segnalare bug e problemi in sospeso relativi alle librerie di Azure per Java nel [progetto GitHub](https://github.com/Azure/azure-sdk-for-java).
