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
ms.openlocfilehash: fa93788f9b600d963f35ea599a58d309d3a3ee52
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240824"
---
# <a name="configure-microprofile-with-azure-key-vault"></a><span data-ttu-id="f7ab3-103">Configurare MicroProfile con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f7ab3-103">Configure MicroProfile with Azure Key Vault</span></span>

<span data-ttu-id="f7ab3-104">Questa esercitazione illustra come configurare un'applicazione [MicroProfile](http://microprofile.io) per il recupero di segreti da [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) usando le [API MicroProfile Config](https://microprofile.io/project/eclipse/microprofile-config) per creare una connessione diretta ad Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-104">This tutorial will demonstrate how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) using the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to Azure Key Vault.</span></span> <span data-ttu-id="f7ab3-105">Con le API MicroProfile Config, gli sviluppatori hanno a disposizione un'API standard per il recupero e l'inserimento dei dati di configurazione nei propri microservizi.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-105">By using the MicroProfile Config APIs, developers benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="f7ab3-106">Prima di passare ai dettagli, si esaminerà rapidamente la combinazione di Azure Key Vault e API MicroProfile Config che consente di scrivere nel proprio codice.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-106">Before we dive in, lets quickly take a look at what a combination of Azure Key Vault and the MicroProfile Config API enables us to write in our code.</span></span> <span data-ttu-id="f7ab3-107">Di seguito è riportato un frammento di codice di un campo in una classe annotato con `@Inject` e `@ConfigProperty`.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-107">Here's a code snippet of a field in a class that has been annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="f7ab3-108">Il valore `name` specificato nell'annotazione è il nome della proprietà da cercare in Azure Key Vault, mentre `defaultValue` è il valore che verrà impostato se la chiave non viene rilevata.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-108">The `name` specified in the annotation is the name of the property to look up in Azure Key Vault, and the `defaultValue` is what will be set if the key is not discovered.</span></span> <span data-ttu-id="f7ab3-109">Il risultato finale è che il valore archiviato in Azure Key Vault, o il valore predefinito, verrà inserito automaticamente nel campo in fase di esecuzione, semplificando il lavoro degli sviluppatori che non dovranno più passare i valori tra costruttori e metodi setter, ma lasceranno la gestione di queste attività a MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-109">The end result is that the value stored in Azure Key Vault, or the default value, will be injected automatically into the field at runtime, simplifying the life of developers as they no longer need to pass values around in constuctors and setter methods, instead leaving it to MicroProfile to handle.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="f7ab3-110">È anche possibile accedere direttamente a MicroProfile Config per richiedere i segreti necessari, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f7ab3-110">It is also possible to access the MicroProfile config directly, to request secrets as necessary, e.g.:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="f7ab3-111">Questo esempio usa [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile](https://microprofile.io/) per creare un piccolo file WAR Java che può essere eseguito localmente nel computer.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-111">This sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java war file that can be run locally on your machine.</span></span> <span data-ttu-id="f7ab3-112">Non dimostra come inserire il codice in contenitori Docker o eseguirne il push in Azure, ma la sezione dei collegamenti al termine di questa esercitazione contiene rimandi ad altre utili esercitazioni che illustrano queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-112">It does not demonstrate how to dockerise or push the code to Azure, but the links section at the end of this tutorial has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="f7ab3-113">Questo esempio usa una libreria gratuita e open source che crea un'origine di configurazione con l'API MicroProfile Config per Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-113">This sample makes use of a free and open source library that creats a config source (using the MicroProfile Config API) for Azure Key Vault.</span></span> <span data-ttu-id="f7ab3-114">Per altre informazioni su questa libreria e per recuperare il codice, vedere la [pagina GitHub del progetto](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span><span class="sxs-lookup"><span data-stu-id="f7ab3-114">You can learn more about this library, and review the code, on the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="f7ab3-115">Usando questa libreria, il codice riportato in questa esercitazione si concentra solo sulla configurazione della libreria stessa seguita dall'inserimento delle chiavi nel codice. Non è necessario scrivere codice specifico per Azure.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-115">By using this library, the code in this tutorial can simply focus on configuration of the library, followed by injecting keys into your code, and we don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="f7ab3-116">Di seguito sono indicati i passaggi necessari per eseguire questo codice nel computer locale, iniziando con la creazione di una risorsa di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-116">Here are the steps required to run this code on your local machine, starting with creating an Azure Key Vault resource.</span></span>

## <a name="creating-an-azure-key-vault-resource"></a><span data-ttu-id="f7ab3-117">Creazione di una risorsa di Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f7ab3-117">Creating an Azure Key Vault resource</span></span>

<span data-ttu-id="f7ab3-118">Si userà l'interfaccia della riga di comando di Azure per creare la risorsa di Azure Key Vault e popolarla con un solo segreto.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-118">We will use the Azure CLI to create the Azure Key Vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="f7ab3-119">Verrà prima creata un'entità servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="f7ab3-119">Firstly, lets create an Azure service principal.</span></span> <span data-ttu-id="f7ab3-120">per ottenere l'ID client e la chiave per l'accesso a Key Vault:</span><span class="sxs-lookup"><span data-stu-id="f7ab3-120">This will provide us with the client ID and key we need to access Key Vault:</span></span>

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

<span data-ttu-id="f7ab3-121">Si userà `microprofile-keyvault-service-principal` come nome dell'entità servizio creata nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-121">Lets say we use `microprofile-keyvault-service-principal` for the service principal name in the previous step.</span></span> <span data-ttu-id="f7ab3-122">La risposta di Azure per questa operazione sarà nella forma seguente, qui leggermente offuscata:</span><span class="sxs-lookup"><span data-stu-id="f7ab3-122">The response from Azure for doing this will be in the following, slightly censored, form:</span></span>

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

<span data-ttu-id="f7ab3-123">Di particolare importanza sono i valori `appID` e `password`, che in seguito verranno usati rispettivamente come `client ID` e `key`.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-123">Of particular note here is the `appID` and `password` values - these are what we will use later on as `client ID` and `key`, respectively.</span></span>

<span data-ttu-id="f7ab3-124">Ora che è stata creata l'entità servizio, è facoltativamente possibile creare un gruppo di risorse. Se è già disponibile un gruppo di risorse, è possibile omettere questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-124">Now that we have created a service principal, we can optionally create a resource group (if you already have one you want to use, you can skip this step).</span></span> <span data-ttu-id="f7ab3-125">Si noti che per ottenere un elenco di posizioni di gruppi di risorse è possibile chiamare `az account list-locations` e usare il valore `name` di tale elenco per specificare la posizione in cui creare il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-125">Note that to get a list of resource group locations, you can call `az account list-locations` and use the `name` value from that list to specify where the resource group should be created.</span></span>

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

<span data-ttu-id="f7ab3-126">Verrà ora creata una risorsa di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-126">We now create an Azure Key Vault resource.</span></span> <span data-ttu-id="f7ab3-127">Si noti che il nome dell'istanza di Key Vault verrà usato per fare riferimento all'istanza in seguito, quindi scegliere un nome facile da ricordare.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-127">Note that the Key Vault name is what you will use to reference the key vault later, so choose something memorable.</span></span>

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

<span data-ttu-id="f7ab3-128">È anche necessario concedere le autorizzazioni appropriate all'entità servizio creata in precedenza, affinché possa accedere ai segreti di Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-128">We also need to grant the appropriate permissions to the service principal we created earlier, so that it may access the Key Vault secrets.</span></span> <span data-ttu-id="f7ab3-129">Si noti che il valore appID è il valore `appId` del passaggio in cui è stata creata l'entità servizio, ovvero `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79`. Usare tuttavia l'output del terminale.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-129">Note that the appID value is the `appId` value from above where we created the service principal (that is, `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79` - but use the value from your terminal output).</span></span>

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

<span data-ttu-id="f7ab3-130">Ora è possibile eseguire il push di un segreto in Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-130">We are now at the point where we can push a secret into Key Vault.</span></span> <span data-ttu-id="f7ab3-131">Si userà il nome chiave `demo-key` e il valore della chiave sarà `demo-value`:</span><span class="sxs-lookup"><span data-stu-id="f7ab3-131">Lets use the key name `demo-key`, and set the value of the key to be `demo-value`:</span></span>

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

<span data-ttu-id="f7ab3-132">L'operazione è terminata.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-132">That's it!</span></span> <span data-ttu-id="f7ab3-133">Key Vault è ora in esecuzione in Azure con un solo segreto.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-133">We now have Key Vault running in Azure with a single secret.</span></span> <span data-ttu-id="f7ab3-134">A questo punto è possibile clonare questo repository e configurarlo per l'uso di questa risorsa nell'app.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-134">We can now clone this repo and configure it to use this resource in our app.</span></span>

## <a name="getting-up-and-running-locally"></a><span data-ttu-id="f7ab3-135">Esecuzione in locale</span><span class="sxs-lookup"><span data-stu-id="f7ab3-135">Getting up and running locally</span></span>

<span data-ttu-id="f7ab3-136">Questo esempio si basa su un'applicazione di esempio disponibile in GitHub, che verrà clonata per poi esaminarne il codice.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-136">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="f7ab3-137">Seguire questa procedura per clonare il codice nel computer:</span><span class="sxs-lookup"><span data-stu-id="f7ab3-137">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="f7ab3-138">Passare a `src/main/resources/META-INF/microprofile-config.properties` e modificare le proprietà nel file microprofile-config.properties con i dettagli precedenti.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-138">Navigate to `src/main/resources/META-INF/microprofile-config.properties` and change the properties in microprofile-config.properties file with details from above.</span></span>

1. <span data-ttu-id="f7ab3-139">Provare a eseguire il server usando `mvn clean package payara-micro:start`.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-139">Try running the server using `mvn clean package payara-micro:start`</span></span>

1. <span data-ttu-id="f7ab3-140">Provare ad accedere a [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) nel Web browser. Verrà visualizzata una risposta semplice che indica la lettura dei valori da Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-140">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser - you should see a simple response that demonstrates values being read from Azure Key Vault.</span></span>

## <a name="summary"></a><span data-ttu-id="f7ab3-141">Summary</span><span class="sxs-lookup"><span data-stu-id="f7ab3-141">Summary</span></span>

<span data-ttu-id="f7ab3-142">Questa applicazione di esempio usa l'API MicroProfile Config, Azure Key Vault e la libreria [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) gratuita e open source per consentire un facile inserimento di dati di configurazione e segreti nei propri servizi Web MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="f7ab3-142">This sample application bakes together the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into our MicroProfile web services.</span></span>
