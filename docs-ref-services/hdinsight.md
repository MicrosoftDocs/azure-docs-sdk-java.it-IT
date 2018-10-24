---
title: Azure HDInsight SDK per Java
description: Informazioni di riferimento su Azure HDInsight SDK per Java. HDInsight SDK per Java offre classi e metodi che consentono di gestire i cluster HDInsight.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 9/20/2018
ms.openlocfilehash: 1271f70fff876f4d24c8afa81123c54735f2d522
ms.sourcegitcommit: 788b49d0b37909c575c9e5176e484cba627e7921
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49120539"
---
# <a name="hdinsight-java-management-sdk-preview"></a>HDInsight Management SDK per Java (anteprima)

## <a name="overview"></a>Panoramica

HDInsight SDK per Java offre classi e metodi che consentono di gestire i cluster HDInsight. Include operazioni per creare, eliminare, aggiornare, elencare, ridimensionare, eseguire azioni di script, monitorare, ottenere le proprietà dei cluster di HDInsight e altro ancora.

## <a name="prerequisites"></a>Prerequisiti

* Un account Azure. Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Maven](https://maven.apache.org/install.html)

## <a name="sdk-installation"></a>Installazione dell'SDK

HDInsight SDK per Java è disponibile tramite Maven [qui](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight). Aggiungere la dipendenza seguente a pom.xml:

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

A pom.xml sarà anche necessario aggiungere le dipendenze seguenti:

* [Libreria per l'autenticazione client di Azure](https://mvnrepository.com/artifact/com.microsoft.azure/azure-client-authentication/1.6.2)
```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.2</version>
    <scope>test</scope>
</dependency>
```

* [Runtime client Java di Azure per ARM](https://mvnrepository.com/artifact/com.microsoft.azure/azure-arm-client-runtime/1.6.2)
```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.2</version>
</dependency>
```

## <a name="authentication"></a>Authentication

L'SDK deve essere prima autenticato con la sottoscrizione di Azure.  Seguire questo esempio per creare un'entità servizio e usarla per l'autenticazione. Al termine si avrà un'istanza di un `HDInsightManagementClientImpl` che contiene molti metodi, descritti nelle sezioni seguenti, che possono essere usati per operazioni di gestione.

> [!NOTE]
> Oltre all'esempio seguente esistono altre modalità di autenticazione che possono essere più adatte alle proprie esigenze. Tutti i metodi sono descritti nell'articolo su come [eseguire l'autenticazione con le librerie di gestione di Azure per Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable).

### <a name="authentication-example-using-a-service-principal"></a>Esempio di autenticazione con un'entità servizio

Per prima cosa, accedere ad [Azure Cloud Shell](https://shell.azure.com/bash). Verificare che si stia attualmente usando la sottoscrizione in cui si vuole creare l'entità servizio. 

```azurecli-interactive
az account show
```

Le informazioni sulla sottoscrizione vengono visualizzate in formato JSON.

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

Se non si è eseguito l'accesso alla sottoscrizione corretta, selezionare quella corretta eseguendo: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> Se il provider di risorse HDInsight non è già stato registrato con un altro metodo, ad esempio creando un cluster HDInsight tramite il portale di Azure, è necessario eseguire questa operazione una volta prima di poter eseguire l'autenticazione. La registrazione può essere eseguita da [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo questo comando:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

Scegliere quindi un nome per l'entità servizio e crearla con il comando seguente:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

Verranno visualizzate le informazioni relative all'entità servizio in formato JSON.

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Copiare il frammento di codice seguente e compilare `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET` e `SUBSCRIPTION_ID` con le stringhe JSON restituite dopo aver eseguito il comando per creare l'entità servizio.

```java
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.*;
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.HDInsightManagementClientImpl;

public class Main {
    public static void main (String[] args) {

        // Tenant ID for your Azure Subscription
        String TENANT_ID = "";
        // Your Service Principal App Client ID
        String CLIENT_ID = "";
        // Your Service Principal Client Secret
        String CLIENT_SECRET = "";
        // Azure Subscription ID
        String SUBSCRIPTION_ID = "";

        ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(
                CLIENT_ID,
                TENANT_ID,
                CLIENT_SECRET,
                AzureEnvironment.AZURE);

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials);
```


## <a name="cluster-management"></a>Gestione dei cluster

> [!NOTE]
> Questa sezione presuppone che l'utente abbia già eseguito l'autenticazione e abbia creato un'istanza `HDInsightManagementClientImpl` che ha poi archiviato in una variabile chiamata `client`. Le istruzioni per l'autenticazione e l'ottenimento di un `HDInsightManagementClientImpl` sono disponibili nella sezione Autenticazione precedente.

### <a name="create-a-cluster"></a>Creare un cluster

Un nuovo cluster può essere creato chiamando `client.clusters().create()`.

#### <a name="example"></a>Esempio

Questo esempio illustra come creare un cluster Spark con 2 nodi head e 1 nodo del ruolo di lavoro.

> [!NOTE]
> È prima necessario creare un gruppo di risorse e un account di archiviazione, come spiegato di seguito. Se sono già stati creati, è possibile ignorare questi passaggi.

##### <a name="creating-a-resource-group"></a>Creazione di un gruppo di risorse

È possibile creare un gruppo di risorse con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

È possibile creare un account di archiviazione con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo:
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
Eseguire ora questo comando per ottenere la chiave per l'account di archiviazione. Questa chiave sarà necessaria per creare un cluster:
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
Il frammento Java seguente crea un cluster Spark con 2 nodi head e 1 nodo di lavoro. Inserire le variabili vuote come spiegato nei commenti. È possibile modificare altri parametri in base alle proprie esigenze.

```java
// The name for the cluster you are creating
String clusterName = "";
// The name of your existing Resource Group
String resourceGroupName = "";
// Choose a username
String username = "";
// Choose a password
String password = "";
// Replace <> with the name of your storage account
String storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
String storageAccountKey = "";
// Choose a region
String location = "";
String container = "default";

HashMap<String, HashMap<String, String>> configurations = new HashMap<String, HashMap<String, String>>();
        HashMap<String, String> gateway = new HashMap<String, String>();
        gateway.put("restAuthCredential.enabled_credential", "True");
        gateway.put("restAuthCredential.username", username);
        gateway.put("restAuthCredential.password", password);
        configurations.put("gateway", gateway);

        ClusterCreateParametersExtended parameters = new ClusterCreateParametersExtended()
            .withLocation(location)
            .withTags(Collections.EMPTY_MAP)
            .withProperties(
                new ClusterCreateProperties()
                    .withClusterVersion("3.6")
                    .withOsType(OSType.LINUX)
                    .withClusterDefinition(new ClusterDefinition()
                            .withKind("spark")
                            .withConfigurations(configurations)
                    )
                    .withTier(Tier.STANDARD)
                    .withComputeProfile(new ComputeProfile()
                        .withRoles(List.of(
                            new Role()
                                .withName("headnode")
                                .withTargetInstanceCount(2)
                                .withHardwareProfile(new HardwareProfile()
                                    .withVmSize("Large")
                                )
                                .withOsProfile(new OsProfile()
                                    .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                    )
                                ),
                            new Role()
                                    .withName("workernode")
                                    .withTargetInstanceCount(1)
                                    .withHardwareProfile(new HardwareProfile()
                                        .withVmSize("Large")
                                    )
                                    .withOsProfile(new OsProfile()
                                        .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                        )
                                    )
                        ))
                    )
                    .withStorageProfile(new StorageProfile()
                        .withStorageaccounts(List.of(
                                new StorageAccount()
                                    .withName(storageAccount)
                                    .withKey(storageAccountKey)
                                    .withContainer(container)
                                    .withIsDefault(true)
                        ))
                    )
            );
        client.clusters().create(resourceGroupName, clusterName, parameters);
```

### <a name="get-cluster-details"></a>Ottenere i dettagli del cluster

Per ottenere le proprietà di un dato cluster:

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>Esempio

È possibile usare `get` per verificare che il cluster sia stato creato correttamente.

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

L'output sarà simile al seguente:

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>Elencare cluster

#### <a name="list-clusters-under-the-subscription"></a>Elencare i cluster nella sottoscrizione

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a>Elencare i cluster per gruppo di risorse

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> Sia `List()` che `ListByResourceGroup()` restituiscono un oggetto `PagedList<ClusterInner>`. La chiamata di `loadNext()` restituisce un elenco di cluster nella pagina e determina l'avanzamento dell'oggetto `ClusterPaged` alla pagina successiva. Questa operazione può essere ripetuta finché `hasNextPage()` non restituisce `false`, che indica che non sono presenti altre pagine.

#### <a name="example"></a>Esempio

L'esempio seguente mostra le proprietà di tutti i cluster per la sottoscrizione corrente:

```java
PagedList<ClusterInner> clusterPages = client.clusters().list();
while (true) {
    for (ClusterInner cluster : clusterPages.currentPage().items()) {
        System.out.println(cluster.name());
    }
    if (clusterPages.hasNextPage()) {
        clusterPages.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="delete-a-cluster"></a>Eliminare un cluster

Per eliminare un cluster:

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a>Aggiornare i tag del cluster

È possibile aggiornare i tag di un dato cluster nel modo seguente:

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="resize-cluster"></a>Ridimensionare un cluster

È possibile ridimensionare il numero di nodi di ruolo di lavoro di un dato cluster specificando una nuova dimensione nel modo seguente:

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a>Monitoraggio del cluster

HDInsight Management SDK può essere usato anche per gestire il monitoraggio dei cluster tramite Operations Management Suite (OMS).

### <a name="enable-oms-monitoring"></a>Abilitare il monitoraggio di OMS

> [!NOTE]
> Per abilitare il monitoraggio di OMS è necessaria un'area di lavoro di Log Analytics esistente. Se l'area non è stata ancora creata, vedere [Creare un'area di lavoro di Log Analytics nel portale di Azure](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace) per informazioni su come crearla.

Per abilitare il monitoraggio di OMS nel cluster:

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a>Visualizzare lo stato del monitoraggio di OMS

Per ottenere lo stato di OMS nel cluster:

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a>Disabilitare il monitoraggio di OMS

Per disabilitare OMS nel cluster:

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a>Azioni script

HDInsight offre un metodo di configurazione denominato "azioni script" che richiama script personalizzati per il cluster.
> [!NOTE]
> Per altre informazioni sulle azioni script, vedere [Personalizzare i cluster HDInsight basati su Linux tramite azioni script](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>Eseguire azioni script

È possibile eseguire azioni script in un dato cluster nel modo seguente:

```java
RuntimeScriptAction scriptAction1 = new RuntimeScriptAction()
    .withName("<Script Name>")
    .withUri("<URL To Script>")
    .withRoles(<List<String> of roles>);
client.clusters().executeScriptActions(
    resourceGroupName, 
    clusterName, 
    new ExecuteScriptActionParameters().withPersistOnSuccess(false).withScriptActions(new LinkedList<>(Arrays.asList(scriptAction1)))); //add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a>Eliminare un'azione script

Per eliminare una determinata azione script persistente in un dato cluster:

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a>Elencare le azioni script persistenti

> [!NOTE]
> `listByCluster()` restituisce un oggetto `PagedList<RuntimeScriptActionDetailInner>`. La chiamata di `currentPage().items()` restituisce un elenco di `RuntimeScriptActionDetailInner`, con avanzamento di `loadNextPage()` alla pagina successiva. Questa operazione può essere ripetuta finché `hasNextPage()` non restituisce `false`, che indica che non sono presenti altre pagine.

Per elencare tutte le azioni script persistenti per il cluster specificato:
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>Esempio

```java
PagedList<RuntimeScriptActionDetailInner> scriptsPaged = client.scriptActions().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptsPaged.hasNextPage()) {
        scriptsPaged.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="list-all-scripts-execution-history"></a>Elencare la cronologia di esecuzione di tutti gli script

Per elencare la cronologia di esecuzione di tutti gli script per il cluster specificato:

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>Esempio

Questo esempio visualizza tutti i dettagli di tutte le precedenti esecuzioni di script.

```java
PagedList<RuntimeScriptActionDetailInner> scriptExecutionsPaged = client.scriptExecutionHistorys().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptExecutionsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptExecutionsPaged.hasNextPage()) {
        scriptExecutionsPaged.loadNextPage();
    } else {
        break;
    }
}
```
