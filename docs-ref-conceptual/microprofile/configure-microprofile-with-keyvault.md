---
title: Configurare MicroProfile con Azure Key Vault
description: Informazioni su come inserire i segreti in un servizio web MicroProfile con Azure Key Vault
services: key-vault
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c405711813176823f2ddee6b4002f75c2b25fdb5
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533618"
---
# <a name="configure-microprofile-by-using-azure-key-vault"></a><span data-ttu-id="00b95-103">Configurare MicroProfile con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="00b95-103">Configure MicroProfile by using Azure Key Vault</span></span>

<span data-ttu-id="00b95-104">Questo articolo illustra come configurare un'applicazione [MicroProfile](http://microprofile.io) per il recupero di segreti da un [insieme di credenziali delle chiavi di Azure](https://azure.microsoft.com/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="00b95-104">This article demonstrates how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from an [Azure key vault](https://azure.microsoft.com/services/key-vault/).</span></span> <span data-ttu-id="00b95-105">In questo processo si usano le [API MicroProfile Config](https://microprofile.io/project/eclipse/microprofile-config) per creare una connessione diretta a un insieme di credenziali delle chiavi di Azure.</span><span class="sxs-lookup"><span data-stu-id="00b95-105">In this process, you use the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to an Azure key vault.</span></span> <span data-ttu-id="00b95-106">Con le API MicroProfile Config, gli sviluppatori hanno a disposizione un'API standard per il recupero e l'inserimento dei dati di configurazione nei propri microservizi.</span><span class="sxs-lookup"><span data-stu-id="00b95-106">As a developer, by using the MicroProfile Config APIs, you benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="00b95-107">Prima di iniziare, esaminare rapidamente la combinazione di Azure Key Vault e API MicroProfile Config che consente di scrivere nel proprio codice.</span><span class="sxs-lookup"><span data-stu-id="00b95-107">Before you start, take a quick look at what a combination of Azure Key Vault and the MicroProfile Config API enables you to write in your code.</span></span> <span data-ttu-id="00b95-108">Il frammento di codice seguente si riferisce a un campo in una classe annotato con `@Inject` e `@ConfigProperty`.</span><span class="sxs-lookup"><span data-stu-id="00b95-108">The following code snippet is of a field in a class that's annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="00b95-109">Il valore *name* specificato nell'annotazione corrisponde al nome della proprietà da cercare nell'insieme di credenziali delle chiavi, mentre *defaultValue* è il valore che verrà impostato se la chiave non viene rilevata.</span><span class="sxs-lookup"><span data-stu-id="00b95-109">The *name* that's specified in the annotation is the name of the property to look up in your key vault, and the *defaultValue* is what will be set if the key is not discovered.</span></span> <span data-ttu-id="00b95-110">Il risultato è che il valore archiviato nell'insieme di credenziali delle chiavi, ovvero il valore predefinito, viene inserito automaticamente nel campo in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="00b95-110">The result is that the value that's stored in the key vault, or the default value, is injected automatically into the field at runtime.</span></span> <span data-ttu-id="00b95-111">Questa azione può semplificare le operazioni, in quanto non è più necessario passare valori in costruttori e metodi setter.</span><span class="sxs-lookup"><span data-stu-id="00b95-111">This action can simplify your life, because you no longer need to pass values around in constructors and setter methods.</span></span> <span data-ttu-id="00b95-112">Questa attività viene infatti gestita da MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="00b95-112">Instead, MicroProfile handles this task.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="00b95-113">Per richiedere i segreti, è anche possibile accedere direttamente a MicroProfile Config.</span><span class="sxs-lookup"><span data-stu-id="00b95-113">To request secrets, as necessary, you can also access the MicroProfile config directly.</span></span> <span data-ttu-id="00b95-114">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="00b95-114">For example:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="00b95-115">In questo esempio di codice si usa [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile](https://microprofile.io/) per creare un piccolo file WAR (Web Application Requirement) Java che è possibile eseguire localmente nel computer.</span><span class="sxs-lookup"><span data-stu-id="00b95-115">This sample code uses [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java web application requirement (WAR) file that you can run locally on your machine.</span></span> <span data-ttu-id="00b95-116">Non viene illustrato come inserire il codice in contenitori Docker o eseguirne il push in Azure, ma la sezione al termine di questo articolo contiene collegamenti ad altre utili esercitazioni che illustrano queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="00b95-116">It doesn't demonstrate how to Dockerize or push the code to Azure, but the section at the end of this article has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="00b95-117">Nell'esempio si usa una libreria gratuita e open source che crea un'origine di configurazione con l'API MicroProfile Config nell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="00b95-117">The sample uses a free and open source library that creates a config source (using the MicroProfile Config API) in your key vault.</span></span> <span data-ttu-id="00b95-118">Per altre informazioni su questa libreria e per esaminare il codice, vedere la [pagina GitHub del progetto](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span><span class="sxs-lookup"><span data-stu-id="00b95-118">To learn more about this library and review the code, see the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="00b95-119">Se si usa la libreria, il codice usato in questa esercitazione si concentra solo sulla configurazione della libreria stessa seguita dall'inserimento delle chiavi nel codice.</span><span class="sxs-lookup"><span data-stu-id="00b95-119">If you use the library, the code in this tutorial can simply focus on the configuration of the library and then inject keys into your code.</span></span> <span data-ttu-id="00b95-120">Non è necessario scrivere codice specifico per Azure.</span><span class="sxs-lookup"><span data-stu-id="00b95-120">You don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="00b95-121">Per eseguire questo codice nel computer locale, iniziando con la creazione di una risorsa dell'insieme di credenziali delle chiavi, seguire le istruzioni fornite nelle sezioni successive.</span><span class="sxs-lookup"><span data-stu-id="00b95-121">To run this code on your local machine, starting with creating a key vault resource, follow the instructions in the next sections.</span></span>

## <a name="create-a-key-vault-resource"></a><span data-ttu-id="00b95-122">Creare una risorsa dell'insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="00b95-122">Create a key vault resource</span></span>

<span data-ttu-id="00b95-123">In questa sezione si usa l'interfaccia della riga di comando di Azure per creare una risorsa dell'insieme di credenziali delle chiavi e popolarla con un solo segreto.</span><span class="sxs-lookup"><span data-stu-id="00b95-123">In this section, you use the Azure CLI to create a key vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="00b95-124">Creare un'entità servizio di Azure.</span><span class="sxs-lookup"><span data-stu-id="00b95-124">Create an Azure service principal.</span></span> <span data-ttu-id="00b95-125">Questo passaggio consente di ottenere l'ID client e la chiave necessarie per accedere all'insieme di credenziali delle chiavi:</span><span class="sxs-lookup"><span data-stu-id="00b95-125">This step gives you the client ID and key that you need to access your key vault:</span></span>

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    <span data-ttu-id="00b95-126">Si userà *microprofile-keyvault-service-principal* per sostituire *\<service_principal_name>* nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="00b95-126">Let's use *microprofile-keyvault-service-principal* to replace *\<service_principal_name>* in the preceding step.</span></span> <span data-ttu-id="00b95-127">La risposta restituita da Azure sarà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="00b95-127">The response from Azure would be similar to the following:</span></span>

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    <span data-ttu-id="00b95-128">Di particolare interesse sono i valori di *appId* e *password*.</span><span class="sxs-lookup"><span data-stu-id="00b95-128">Of particular note here are the *appId* and *password* values.</span></span> <span data-ttu-id="00b95-129">Tali valori verranno usati più avanti in questo articolo come *ID client* e *chiave*.</span><span class="sxs-lookup"><span data-stu-id="00b95-129">You'll use them later in this article as *client ID* and *key*.</span></span>

1. <span data-ttu-id="00b95-130">(Facoltativo) Dopo aver creato un'entità servizio, è possibile creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="00b95-130">(Optional) Now that you've created a service principal, you can create a resource group.</span></span> <span data-ttu-id="00b95-131">Se il gruppo di risorse che si vuole usare è già disponibile, è possibile ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="00b95-131">If you already have a resource group that you want to use, you can skip this step.</span></span> <span data-ttu-id="00b95-132">Per ottenere un elenco di località dei gruppi di risorse è possibile chiamare `az account list-locations` e usare il valore *name* di tale elenco per specificare la località in cui creare il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="00b95-132">To get a list of resource group locations, you can call `az account list-locations` and use the *name* value from that list to specify where the resource group should be created.</span></span>

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. <span data-ttu-id="00b95-133">Creare una risorsa dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="00b95-133">Create a key vault resource.</span></span> <span data-ttu-id="00b95-134">Il nome dell'insieme di credenziali delle chiavi verrà usato per fare riferimento all'insieme, quindi scegliere un nome facile da ricordare.</span><span class="sxs-lookup"><span data-stu-id="00b95-134">You'll use your key vault name to refer to the key vault later, so be sure to choose a memorable name.</span></span>

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. <span data-ttu-id="00b95-135">Concedere le autorizzazioni appropriate all'entità servizio creata in precedenza, affinché possa accedere ai segreti dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="00b95-135">Grant the appropriate permissions to the service principal that you created earlier, so that it can access the key vault secrets.</span></span> <span data-ttu-id="00b95-136">Il valore di appId nel codice seguente corrisponde al valore di *appId* del passaggio 1, in cui è stata creata l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="00b95-136">The appId value in the following code is the *appId* value from step 1, where you created the service principal.</span></span> <span data-ttu-id="00b95-137">Il valore di *appId* nel passaggio 1 era *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*, ma è necessario usare il valore visualizzato nell'output del proprio terminale.</span><span class="sxs-lookup"><span data-stu-id="00b95-137">That is, the *appId* in step 1 was *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*, but you should use the value from your own terminal output.</span></span>

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. <span data-ttu-id="00b95-138">A questo punto è possibile eseguire il push di un segreto nell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="00b95-138">Now you can push a secret into your key vault.</span></span> <span data-ttu-id="00b95-139">Usare *demo-key* come nome della chiave e impostarne il valore su *demo-value*:</span><span class="sxs-lookup"><span data-stu-id="00b95-139">Use the key name *demo-key*, and set the value of the key to *demo-value*:</span></span>

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

<span data-ttu-id="00b95-140">L'operazione è terminata.</span><span class="sxs-lookup"><span data-stu-id="00b95-140">That's it!</span></span> <span data-ttu-id="00b95-141">È ora disponibile un insieme di credenziali delle chiavi in esecuzione in Azure con un singolo segreto.</span><span class="sxs-lookup"><span data-stu-id="00b95-141">You now have a key vault running in Azure with a single secret.</span></span> <span data-ttu-id="00b95-142">A questo punto è possibile clonare questo repository e configurarlo per l'uso di questa risorsa nell'app.</span><span class="sxs-lookup"><span data-stu-id="00b95-142">You can now clone this repo and configure it to use this resource in your app.</span></span>

## <a name="get-up-and-running-locally"></a><span data-ttu-id="00b95-143">Esecuzione in locale</span><span class="sxs-lookup"><span data-stu-id="00b95-143">Get up and running locally</span></span>

<span data-ttu-id="00b95-144">Questo esempio si basa su un'applicazione di esempio disponibile in GitHub, che verrà clonata per poi esaminarne il codice.</span><span class="sxs-lookup"><span data-stu-id="00b95-144">This example is based on a sample application that's available on GitHub, so you'll clone that application and then step through the code.</span></span> 

1. <span data-ttu-id="00b95-145">Clonare il file nel computer immettendo i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="00b95-145">Clone the code onto your machine by entering the following commands:</span></span>

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="00b95-146">Passare a *src/main/resources/META-INF/microprofile-config.properties* e quindi modificare le proprietà nel file *microprofile-config.properties* usando i valori dei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="00b95-146">Go to *src/main/resources/META-INF/microprofile-config.properties*, and then change the properties in the *microprofile-config.properties* file by using the values from the previous steps.</span></span>

1. <span data-ttu-id="00b95-147">Provare a eseguire il server usando `mvn clean package payara-micro:start`.</span><span class="sxs-lookup"><span data-stu-id="00b95-147">Try running the server by using `mvn clean package payara-micro:start`.</span></span>

1. <span data-ttu-id="00b95-148">Provare ad accedere a [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) nel Web browser.</span><span class="sxs-lookup"><span data-stu-id="00b95-148">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser.</span></span> <span data-ttu-id="00b95-149">Verrà visualizzata una risposta semplice che indica la lettura dei valori dall'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="00b95-149">You should see a simple response that demonstrates values that are being read from your key vault.</span></span>

## <a name="summary"></a><span data-ttu-id="00b95-150">Summary</span><span class="sxs-lookup"><span data-stu-id="00b95-150">Summary</span></span>

<span data-ttu-id="00b95-151">Questa applicazione di esempio combina l'API MicroProfile Config, Azure Key Vault e la libreria gratuita e open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) per consentire un facile inserimento di segreti e dati di configurazione nei propri servizi Web MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="00b95-151">This sample application combines the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into your MicroProfile web services.</span></span>
