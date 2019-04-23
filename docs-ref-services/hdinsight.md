---
title: Azure HDInsight SDK per Java
description: Informazioni di riferimento per Azure HDInsight SDK per Java. HDInsight SDK per Java fornisce classi e metodi che consentono di gestire i cluster HDInsight.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 04/15/2019
ms.openlocfilehash: fe87c9214e2a620230cf2f1f52261fd66a2b8857
ms.sourcegitcommit: f33befab25a66a252b4c91c7aeb1b77cb32821bb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705119"
---
# <a name="hdinsight-sdk-for-java"></a><span data-ttu-id="e8820-104">HDInsight SDK per Java</span><span class="sxs-lookup"><span data-stu-id="e8820-104">HDInsight SDK for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e8820-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e8820-105">Overview</span></span>

<span data-ttu-id="e8820-106">HDInsight SDK per Java fornisce classi e metodi che consentono di gestire i cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e8820-106">The HDInsight SDK for Java provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="e8820-107">Include operazioni per creare, eliminare, aggiornare, elencare, ridimensionare, eseguire azioni di script, monitorare, ottenere le proprietà dei cluster di HDInsight e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="e8820-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8820-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e8820-108">Prerequisites</span></span>

* <span data-ttu-id="e8820-109">Un account Azure.</span><span class="sxs-lookup"><span data-stu-id="e8820-109">An Azure account.</span></span> <span data-ttu-id="e8820-110">Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="e8820-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="e8820-111">Java Development Kit (JDK) supportato.</span><span class="sxs-lookup"><span data-stu-id="e8820-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="e8820-112">Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.</span><span class="sxs-lookup"><span data-stu-id="e8820-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* [<span data-ttu-id="e8820-113">Maven</span><span class="sxs-lookup"><span data-stu-id="e8820-113">Maven</span></span>](https://maven.apache.org/download.cgi)

## <a name="sdk-installation"></a><span data-ttu-id="e8820-114">Installazione dell'SDK</span><span class="sxs-lookup"><span data-stu-id="e8820-114">SDK Installation</span></span>

<span data-ttu-id="e8820-115">HDInsight SDK per Java è disponibile tramite Maven [qui](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight).</span><span class="sxs-lookup"><span data-stu-id="e8820-115">The HDInsight SDK for Java is available through Maven [here](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight).</span></span> <span data-ttu-id="e8820-116">Aggiungere la dipendenza seguente a pom.xml:</span><span class="sxs-lookup"><span data-stu-id="e8820-116">Add the following dependency to your pom.xml:</span></span>

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

<span data-ttu-id="e8820-117">A pom.xml sarà anche necessario aggiungere le dipendenze seguenti:</span><span class="sxs-lookup"><span data-stu-id="e8820-117">You will also need to add the following dependencies to your pom.xml:</span></span>

* [<span data-ttu-id="e8820-118">Libreria per l'autenticazione client di Azure</span><span class="sxs-lookup"><span data-stu-id="e8820-118">Azure Client Authentication Library:</span></span>](https://search.maven.org/artifact/com.microsoft.azure/azure-client-authentication)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.5</version>
  </dependency>
  ```

* [<span data-ttu-id="e8820-119">Runtime client Java di Azure per ARM</span><span class="sxs-lookup"><span data-stu-id="e8820-119">Azure Java Client Runtime For ARM:</span></span>](https://search.maven.org/artifact/com.microsoft.azure/azure-arm-client-runtime)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.5</version>
  </dependency>
  ```

## <a name="authentication"></a><span data-ttu-id="e8820-120">Authentication</span><span class="sxs-lookup"><span data-stu-id="e8820-120">Authentication</span></span>

<span data-ttu-id="e8820-121">L'SDK deve essere prima autenticato con la sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="e8820-121">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="e8820-122">Seguire questo esempio per creare un'entità servizio e usarla per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e8820-122">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="e8820-123">Al termine si avrà un'istanza di un `HDInsightManagementClientImpl` che contiene molti metodi, descritti nelle sezioni seguenti, che possono essere usati per operazioni di gestione.</span><span class="sxs-lookup"><span data-stu-id="e8820-123">After this is done, you will have an instance of an `HDInsightManagementClientImpl`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="e8820-124">Oltre all'esempio seguente esistono altre modalità di autenticazione che possono essere più adatte alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="e8820-124">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="e8820-125">Tutti i metodi sono descritti di seguito: [Eseguire l'autenticazione con le librerie di gestione di Azure per Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span><span class="sxs-lookup"><span data-stu-id="e8820-125">All methods are outlined here: [Authenticate with the Azure management libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="e8820-126">Esempio di autenticazione con un'entità servizio</span><span class="sxs-lookup"><span data-stu-id="e8820-126">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="e8820-127">Per prima cosa, accedere ad [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="e8820-127">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="e8820-128">Verificare che si stia attualmente usando la sottoscrizione in cui si vuole creare l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="e8820-128">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="e8820-129">Le informazioni sulla sottoscrizione vengono visualizzate in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="e8820-129">Your subscription information is displayed as JSON.</span></span>

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

<span data-ttu-id="e8820-130">Se non si è eseguito l'accesso alla sottoscrizione corretta, selezionare quella corretta eseguendo:</span><span class="sxs-lookup"><span data-stu-id="e8820-130">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="e8820-131">Se il provider di risorse HDInsight non è già stato registrato con un altro metodo, ad esempio creando un cluster HDInsight tramite il portale di Azure, è necessario eseguire questa operazione una volta prima di poter eseguire l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e8820-131">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="e8820-132">La registrazione può essere eseguita da [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo questo comando:</span><span class="sxs-lookup"><span data-stu-id="e8820-132">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="e8820-133">Scegliere quindi un nome per l'entità servizio e crearla con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e8820-133">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="e8820-134">Verranno visualizzate le informazioni relative all'entità servizio in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="e8820-134">The service principal information is displayed as JSON.</span></span>

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
<span data-ttu-id="e8820-135">Copiare il frammento di codice seguente e compilare `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET` e `SUBSCRIPTION_ID` con le stringhe JSON restituite dopo aver eseguito il comando per creare l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="e8820-135">Copy the below snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

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

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials)
                .withSubscriptionId(SUBSCRIPTION_ID);
```

## <a name="cluster-management"></a><span data-ttu-id="e8820-136">Gestione dei cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-136">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="e8820-137">Questa sezione presuppone che l'utente abbia già eseguito l'autenticazione e abbia creato un'istanza `HDInsightManagementClientImpl` che ha poi archiviato in una variabile chiamata `client`.</span><span class="sxs-lookup"><span data-stu-id="e8820-137">This section assumes you have already authenticated and constructed an `HDInsightManagementClientImpl` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="e8820-138">Le istruzioni per l'autenticazione e l'ottenimento di un `HDInsightManagementClientImpl` sono disponibili nella sezione Autenticazione precedente.</span><span class="sxs-lookup"><span data-stu-id="e8820-138">Instructions for authenticating and obtaining an `HDInsightManagementClientImpl` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="e8820-139">Creare un cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-139">Create a Cluster</span></span>

<span data-ttu-id="e8820-140">Un nuovo cluster può essere creato chiamando `client.clusters().create()`.</span><span class="sxs-lookup"><span data-stu-id="e8820-140">A new cluster can be created by calling `client.clusters().create()`.</span></span>

#### <a name="samples"></a><span data-ttu-id="e8820-141">Esempi</span><span class="sxs-lookup"><span data-stu-id="e8820-141">Samples</span></span>

<span data-ttu-id="e8820-142">Sono disponibili esempi di codice per la creazione di diversi tipi comuni di cluster HDInsight: [Esempi HDInsight Java](https://github.com/Azure-Samples/hdinsight-java-sdk-samples).</span><span class="sxs-lookup"><span data-stu-id="e8820-142">Code samples for creating several common types of HDInsight clusters are available: [HDInsight Java Samples](https://github.com/Azure-Samples/hdinsight-java-sdk-samples).</span></span>

#### <a name="example"></a><span data-ttu-id="e8820-143">Esempio</span><span class="sxs-lookup"><span data-stu-id="e8820-143">Example</span></span>

<span data-ttu-id="e8820-144">Questo esempio illustra come creare un cluster Spark con 2 nodi head e 1 nodo del ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="e8820-144">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="e8820-145">È prima necessario creare un gruppo di risorse e un account di archiviazione, come spiegato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e8820-145">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="e8820-146">Se sono già stati creati, è possibile ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="e8820-146">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="e8820-147">Creazione di un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="e8820-147">Creating a Resource Group</span></span>

<span data-ttu-id="e8820-148">È possibile creare un gruppo di risorse con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo</span><span class="sxs-lookup"><span data-stu-id="e8820-148">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="e8820-149">Creazione di un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="e8820-149">Creating a Storage Account</span></span>

<span data-ttu-id="e8820-150">È possibile creare un account di archiviazione con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo:</span><span class="sxs-lookup"><span data-stu-id="e8820-150">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="e8820-151">Eseguire ora questo comando per ottenere la chiave per l'account di archiviazione. Questa chiave sarà necessaria per creare un cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-151">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="e8820-152">Il frammento Java seguente crea un cluster Spark con 2 nodi head e 1 nodo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="e8820-152">The below Java snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="e8820-153">Inserire le variabili vuote come spiegato nei commenti. È possibile modificare altri parametri in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="e8820-153">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

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

### <a name="get-cluster-details"></a><span data-ttu-id="e8820-154">Ottenere i dettagli del cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-154">Get Cluster Details</span></span>

<span data-ttu-id="e8820-155">Per ottenere le proprietà di un dato cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-155">To get properties for a given cluster:</span></span>

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="e8820-156">Esempio</span><span class="sxs-lookup"><span data-stu-id="e8820-156">Example</span></span>

<span data-ttu-id="e8820-157">È possibile usare `get` per verificare che il cluster sia stato creato correttamente.</span><span class="sxs-lookup"><span data-stu-id="e8820-157">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

<span data-ttu-id="e8820-158">L'output sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e8820-158">The output should look like:</span></span>

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a><span data-ttu-id="e8820-159">Elencare cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-159">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="e8820-160">Elencare i cluster nella sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="e8820-160">List Clusters Under The Subscription</span></span>

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="e8820-161">Elencare i cluster per gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="e8820-161">List Clusters By Resource Group</span></span>

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> <span data-ttu-id="e8820-162">Sia `List()` che `ListByResourceGroup()` restituiscono un oggetto `PagedList<ClusterInner>`.</span><span class="sxs-lookup"><span data-stu-id="e8820-162">Both `List()` and `ListByResourceGroup()` return a `PagedList<ClusterInner>` object.</span></span> <span data-ttu-id="e8820-163">La chiamata di `loadNext()` restituisce un elenco di cluster nella pagina e determina l'avanzamento dell'oggetto `ClusterPaged` alla pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="e8820-163">Calling `loadNext()` returns a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="e8820-164">Questa operazione può essere ripetuta finché `hasNextPage()` non restituisce `false`, che indica che non sono presenti altre pagine.</span><span class="sxs-lookup"><span data-stu-id="e8820-164">This can be repeated until `hasNextPage()` return `false`, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="e8820-165">Esempio</span><span class="sxs-lookup"><span data-stu-id="e8820-165">Example</span></span>

<span data-ttu-id="e8820-166">L'esempio seguente mostra le proprietà di tutti i cluster per la sottoscrizione corrente:</span><span class="sxs-lookup"><span data-stu-id="e8820-166">The following example prints the properties of all clusters for the current subscription:</span></span>

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

### <a name="delete-a-cluster"></a><span data-ttu-id="e8820-167">Eliminare un cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-167">Delete a Cluster</span></span>

<span data-ttu-id="e8820-168">Per eliminare un cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-168">To delete a cluster:</span></span>

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a><span data-ttu-id="e8820-169">Aggiornare i tag del cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-169">Update Cluster Tags</span></span>

<span data-ttu-id="e8820-170">È possibile aggiornare i tag di un dato cluster nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e8820-170">You can update the tags of a given cluster like so:</span></span>

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="resize-cluster"></a><span data-ttu-id="e8820-171">Ridimensionare un cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-171">Resize Cluster</span></span>

<span data-ttu-id="e8820-172">È possibile ridimensionare il numero di nodi di ruolo di lavoro di un dato cluster specificando una nuova dimensione nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e8820-172">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="e8820-173">Monitoraggio del cluster</span><span class="sxs-lookup"><span data-stu-id="e8820-173">Cluster Monitoring</span></span>

<span data-ttu-id="e8820-174">HDInsight Management SDK può essere usato anche per gestire il monitoraggio dei cluster tramite Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="e8820-174">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="e8820-175">Abilitare il monitoraggio di OMS</span><span class="sxs-lookup"><span data-stu-id="e8820-175">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="e8820-176">Per abilitare il monitoraggio di OMS è necessaria un'area di lavoro Log Analytics esistente.</span><span class="sxs-lookup"><span data-stu-id="e8820-176">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="e8820-177">Se non è già stata creata una, è possibile ottenere informazioni su come farlo qui: [Creare un'area di lavoro Log Analytics nel portale di Azure](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span><span class="sxs-lookup"><span data-stu-id="e8820-177">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="e8820-178">Per abilitare il monitoraggio di OMS nel cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-178">To enable OMS Monitoring on your cluster:</span></span>

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="e8820-179">Visualizzare lo stato del monitoraggio di OMS</span><span class="sxs-lookup"><span data-stu-id="e8820-179">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="e8820-180">Per ottenere lo stato di OMS nel cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-180">To get the status of OMS on your cluster:</span></span>

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="e8820-181">Disabilitare il monitoraggio di OMS</span><span class="sxs-lookup"><span data-stu-id="e8820-181">Disable OMS Monitoring</span></span>

<span data-ttu-id="e8820-182">Per disabilitare OMS nel cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-182">To disable OMS on your cluster:</span></span>

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a><span data-ttu-id="e8820-183">Azioni script</span><span class="sxs-lookup"><span data-stu-id="e8820-183">Script Actions</span></span>

<span data-ttu-id="e8820-184">HDInsight offre un metodo di configurazione denominato "azioni script" che richiama script personalizzati per il cluster.</span><span class="sxs-lookup"><span data-stu-id="e8820-184">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="e8820-185">Altre informazioni su come usare le azioni dello script sono disponibili qui: [Personalizzare i cluster HDInsight basati su Linux tramite azioni script](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="e8820-185">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="e8820-186">Eseguire azioni script</span><span class="sxs-lookup"><span data-stu-id="e8820-186">Execute Script Actions</span></span>

<span data-ttu-id="e8820-187">È possibile eseguire azioni script in un dato cluster nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e8820-187">You can execute script actions on a given cluster like so:</span></span>

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

### <a name="delete-script-action"></a><span data-ttu-id="e8820-188">Eliminare un'azione script</span><span class="sxs-lookup"><span data-stu-id="e8820-188">Delete Script Action</span></span>

<span data-ttu-id="e8820-189">Per eliminare una determinata azione script persistente in un dato cluster:</span><span class="sxs-lookup"><span data-stu-id="e8820-189">To delete a specified persisted script action on a given cluster:</span></span>

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="e8820-190">Elencare le azioni script persistenti</span><span class="sxs-lookup"><span data-stu-id="e8820-190">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="e8820-191">`listByCluster()` restituisce un oggetto `PagedList<RuntimeScriptActionDetailInner>`.</span><span class="sxs-lookup"><span data-stu-id="e8820-191">Both `listByCluster()` returns a `PagedList<RuntimeScriptActionDetailInner>` object.</span></span> <span data-ttu-id="e8820-192">La chiamata di `currentPage().items()` restituisce un elenco di `RuntimeScriptActionDetailInner`, con avanzamento di `loadNextPage()` alla pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="e8820-192">Calling `currentPage().items()` returns a list of `RuntimeScriptActionDetailInner`, and `loadNextPage()` advances to the next page.</span></span> <span data-ttu-id="e8820-193">Questa operazione può essere ripetuta finché `hasNextPage()` non restituisce `false`, che indica che non sono presenti altre pagine.</span><span class="sxs-lookup"><span data-stu-id="e8820-193">This can be repeated until `hasNextPage()` returns `false`, indicating that there are no more pages.</span></span>

<span data-ttu-id="e8820-194">Per elencare tutte le azioni script persistenti per il cluster specificato:</span><span class="sxs-lookup"><span data-stu-id="e8820-194">To list all persisted script actions for the specified cluster:</span></span>
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="e8820-195">Esempio</span><span class="sxs-lookup"><span data-stu-id="e8820-195">Example</span></span>

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

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="e8820-196">Elencare la cronologia di esecuzione di tutti gli script</span><span class="sxs-lookup"><span data-stu-id="e8820-196">List All Scripts' Execution History</span></span>

<span data-ttu-id="e8820-197">Per elencare la cronologia di esecuzione di tutti gli script per il cluster specificato:</span><span class="sxs-lookup"><span data-stu-id="e8820-197">To list all scripts' execution history for the specified cluster:</span></span>

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="e8820-198">Esempio</span><span class="sxs-lookup"><span data-stu-id="e8820-198">Example</span></span>

<span data-ttu-id="e8820-199">Questo esempio visualizza tutti i dettagli di tutte le precedenti esecuzioni di script.</span><span class="sxs-lookup"><span data-stu-id="e8820-199">This example prints all the details for all past script executions.</span></span>

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
