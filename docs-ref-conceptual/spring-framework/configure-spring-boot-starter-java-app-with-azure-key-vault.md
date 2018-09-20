---
title: Come usare l'utilità di avvio Spring Boot per Azure Key Vault
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Key Vault.
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 78b7a9a2e26168b19dc8a1d12e47456752b57ffc
ms.sourcegitcommit: e017de4677c5bedd6ef88c8c1b6da279dc973efe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45639774"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a><span data-ttu-id="10c52-103">Come usare l'utilità di avvio Spring Boot per Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="10c52-103">How to use the Spring Boot Starter for Azure Key Vault</span></span>

## <a name="overview"></a><span data-ttu-id="10c52-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="10c52-104">Overview</span></span>

<span data-ttu-id="10c52-105">Questo articolo illustra la creazione di un'app con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Key Vault per recuperare una stringa di connessione archiviata come segreto in un insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="10c52-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Key Vault to retrieve a connection string that is stored as a secret in a key vault.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10c52-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="10c52-106">Prerequisites</span></span>

<span data-ttu-id="10c52-107">I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="10c52-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="10c52-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="10c52-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="10c52-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="10c52-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="10c52-110">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="10c52-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-the-spring-initialzr"></a><span data-ttu-id="10c52-111">Creare un'app usando Spring Initialzr</span><span class="sxs-lookup"><span data-stu-id="10c52-111">Create an app using the Spring Initialzr</span></span>

1. <span data-ttu-id="10c52-112">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="10c52-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="10c52-113">Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi di **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul collegamento relativo a **Switch to the full version** (Passa alla versione completa) di Spring Initializr.</span><span class="sxs-lookup"><span data-stu-id="10c52-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Specificare il nome del gruppo e dell'elemento][secrets-01]

1. <span data-ttu-id="10c52-115">Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="10c52-115">Scroll down to the **Azure** section and check the box for **Azure Key Vault**.</span></span>

   ![Selezionare l'utilità di avvio Azure Key Vault][secrets-02]

1. <span data-ttu-id="10c52-117">Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="10c52-117">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Generare il progetto Spring Boot][secrets-03]

1. <span data-ttu-id="10c52-119">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="10c52-119">When prompted, download the project to a path on your local computer.</span></span>

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="10c52-120">Accedere ad Azure e selezionare la sottoscrizione da usare</span><span class="sxs-lookup"><span data-stu-id="10c52-120">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="10c52-121">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="10c52-121">Open a command prompt.</span></span>

1. <span data-ttu-id="10c52-122">Accedere all'account Azure con l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="10c52-122">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="10c52-123">Seguire le istruzioni per completare il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="10c52-123">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="10c52-124">Elencare le sottoscrizioni:</span><span class="sxs-lookup"><span data-stu-id="10c52-124">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="10c52-125">Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-125">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="10c52-126">Specificare il GUID per l'account che si vuole usare con Azure, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-126">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a><span data-ttu-id="10c52-127">Creare e configurare un nuovo insieme di credenziali delle chiavi di Azure usando l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="10c52-127">Create and configure a new Azure Key Vault using the Azure CLI</span></span>

1. <span data-ttu-id="10c52-128">Creare un gruppo di risorse per le risorse di Azure che verranno usate per l'insieme di credenziali delle chiavi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-128">Create a resource group for the Azure resources you will use for your key vault; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="10c52-129">Dove:</span><span class="sxs-lookup"><span data-stu-id="10c52-129">Where:</span></span>

   | <span data-ttu-id="10c52-130">Parametro</span><span class="sxs-lookup"><span data-stu-id="10c52-130">Parameter</span></span> | <span data-ttu-id="10c52-131">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="10c52-131">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="10c52-132">Specifica un nome univoco per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="10c52-132">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="10c52-133">Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="10c52-133">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="10c52-134">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del gruppo di risorse, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-134">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. <span data-ttu-id="10c52-135">Creare un'entità servizio di Azure dalla registrazione per l'applicazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-135">Create an Azure service principal from your application registration; for example:</span></span>
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   <span data-ttu-id="10c52-136">Dove:</span><span class="sxs-lookup"><span data-stu-id="10c52-136">Where:</span></span>

   | <span data-ttu-id="10c52-137">Parametro</span><span class="sxs-lookup"><span data-stu-id="10c52-137">Parameter</span></span> | <span data-ttu-id="10c52-138">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="10c52-138">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="10c52-139">Specifica il nome dell'entità servizio di Azure.</span><span class="sxs-lookup"><span data-stu-id="10c52-139">Specifies the name for your Azure service principal.</span></span> |

   <span data-ttu-id="10c52-140">L'interfaccia della riga di comando di Azure restituirà un messaggio di stato JSON contenente *appId* e *password*, che si useranno in seguito come ID client e password client, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-140">The Azure CLI will return a JSON status message that contains the *appId* and *password*, which you will use later as the client id and client password; for example:</span></span>

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. <span data-ttu-id="10c52-141">Creare un nuovo insieme di credenziali delle chiavi nel gruppo di risorse, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-141">Create a new key vault in the resource group; for example:</span></span>
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   <span data-ttu-id="10c52-142">Dove:</span><span class="sxs-lookup"><span data-stu-id="10c52-142">Where:</span></span>

   | <span data-ttu-id="10c52-143">Parametro</span><span class="sxs-lookup"><span data-stu-id="10c52-143">Parameter</span></span> | <span data-ttu-id="10c52-144">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="10c52-144">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="10c52-145">Specifica un nome univoco per l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="10c52-145">Specifies a unique name for your key vault.</span></span> |
   | `location` | <span data-ttu-id="10c52-146">Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="10c52-146">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |
   | `enabled-for-deployment` | <span data-ttu-id="10c52-147">Specifica l'[opzione di distribuzione dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="10c52-147">Specifies the [key vault deployment option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `enabled-for-disk-encryption` | <span data-ttu-id="10c52-148">Specifica l'[opzione di crittografia dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="10c52-148">Specifies the [key vault encryption option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `enabled-for-template-deployment` | <span data-ttu-id="10c52-149">Specifica l'[opzione di crittografia dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="10c52-149">Specifies the [key vault encryption option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `sku` | <span data-ttu-id="10c52-150">Specifica l'[opzione SKU dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="10c52-150">Specifies the [key vault SKU option](https://docs.microsoft.com/cli/azure/keyvault).</span></span> |
   | `query` | <span data-ttu-id="10c52-151">Specifica un valore da recuperare dalla risposta, corrispondente all'URI dell'insieme di credenziali delle chiavi che sarà necessario per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="10c52-151">Specifies a value to retrieve from the response, which is the key vault URI that you will need to complete this tutorial.</span></span> |

   <span data-ttu-id="10c52-152">L'interfaccia della riga di comando di Azure visualizzerà l'URI per l'insieme di credenziali delle chiavi, che verrà usato in un secondo momento, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-152">The Azure CLI will display the URI for key vault, which you will use later; for example:</span></span>  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

4. <span data-ttu-id="10c52-153">Impostare il criterio di accesso per l'entità servizio di Azure creata prima, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-153">Set the access policy for the Azure service principal you created earlier; for example:</span></span>
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   <span data-ttu-id="10c52-154">Dove:</span><span class="sxs-lookup"><span data-stu-id="10c52-154">Where:</span></span>

   | <span data-ttu-id="10c52-155">Parametro</span><span class="sxs-lookup"><span data-stu-id="10c52-155">Parameter</span></span> | <span data-ttu-id="10c52-156">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="10c52-156">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="10c52-157">Specifica il nome dell'insieme di credenziali delle chiavi precedente.</span><span class="sxs-lookup"><span data-stu-id="10c52-157">Specifies your key vault name from earlier.</span></span> |
   | `secret-permission` | <span data-ttu-id="10c52-158">Specifica i [criteri di sicurezza](https://docs.microsoft.com/cli/azure/keyvault) per l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="10c52-158">Specifies the [security policies](https://docs.microsoft.com/cli/azure/keyvault) for your key vault.</span></span> |
   | `spn` | <span data-ttu-id="10c52-159">Specifica il GUID della registrazione per l'applicazione precedente.</span><span class="sxs-lookup"><span data-stu-id="10c52-159">Specifies the GUID for your application registration from earlier.</span></span> |

   <span data-ttu-id="10c52-160">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione dei criteri di sicurezza, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-160">The Azure CLI will display the results of your security policy creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "wingtiptoyskeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "wingtiptoysresources",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

5. <span data-ttu-id="10c52-161">Archiviare un segreto nel nuovo insieme di credenziali delle chiavi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-161">Store a secret in your new key vault; for example:</span></span>
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   <span data-ttu-id="10c52-162">Dove:</span><span class="sxs-lookup"><span data-stu-id="10c52-162">Where:</span></span>

   | <span data-ttu-id="10c52-163">Parametro</span><span class="sxs-lookup"><span data-stu-id="10c52-163">Parameter</span></span> | <span data-ttu-id="10c52-164">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="10c52-164">Description</span></span> |
   |---|---|
   | `vault-name` | <span data-ttu-id="10c52-165">Specifica il nome dell'insieme di credenziali delle chiavi precedente.</span><span class="sxs-lookup"><span data-stu-id="10c52-165">Specifies your key vault name from earlier.</span></span> |
   | `name` | <span data-ttu-id="10c52-166">Specifica il nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="10c52-166">Specifies the name of your secret.</span></span> |
   | `value` | <span data-ttu-id="10c52-167">Specifica il valore del segreto.</span><span class="sxs-lookup"><span data-stu-id="10c52-167">Specifies the value of your secret.</span></span> |

   <span data-ttu-id="10c52-168">Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del segreto, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-168">The Azure CLI will display the results of your secret creation; for example:</span></span>  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://wingtiptoyskeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://wingtiptoys.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="10c52-169">Configurare e compilare l'applicazione Spring Boot</span><span class="sxs-lookup"><span data-stu-id="10c52-169">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="10c52-170">Estrarre i file dai file di archivio del progetto Spring Boot scaricati prima in una directory.</span><span class="sxs-lookup"><span data-stu-id="10c52-170">Extract the files from the Spring Boot project archive files that you downloaded earlier into a directory.</span></span>

2. <span data-ttu-id="10c52-171">Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="10c52-171">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

3. <span data-ttu-id="10c52-172">Aggiungere i valori per l'insieme di credenziali delle chiavi usando i valori dei passaggi completati prima in questa esercitazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-172">Add the values for your key vault using values from the steps that you completed earlier in this tutorial; for example:</span></span>
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   <span data-ttu-id="10c52-173">Dove:</span><span class="sxs-lookup"><span data-stu-id="10c52-173">Where:</span></span>

   |          <span data-ttu-id="10c52-174">Parametro</span><span class="sxs-lookup"><span data-stu-id="10c52-174">Parameter</span></span>          |                                 <span data-ttu-id="10c52-175">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="10c52-175">Description</span></span>                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           <span data-ttu-id="10c52-176">Specifica l'URI di quando è stato creato l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="10c52-176">Specifies the URI from when you created your key vault.</span></span>           |
   | `azure.keyvault.client-id`  |  <span data-ttu-id="10c52-177">Specifica il GUID *appId* di quando è stata creata l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="10c52-177">Specifies the *appId* GUID from when you created your service principal.</span></span>   |
   | `azure.keyvault.client-key` | <span data-ttu-id="10c52-178">Specifica il GUID *password* di quando è stata creata l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="10c52-178">Specifies the *password* GUID from when you created your service principal.</span></span> |


4. <span data-ttu-id="10c52-179">Passare al file di codice sorgente principale del progetto, ad esempio: */src/main/java/com/wingtiptoys/secrets*.</span><span class="sxs-lookup"><span data-stu-id="10c52-179">Navigate to the main source code file of your project; for example: */src/main/java/com/wingtiptoys/secrets*.</span></span>

5. <span data-ttu-id="10c52-180">Aprire il file Java principale dell'applicazione in un file in un editor di testo, ad esempio: *SecretsApplication.java* e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="10c52-180">Open the application's main Java file in a file in a text editor; for example: *SecretsApplication.java*, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   <span data-ttu-id="10c52-181">Questo esempio di codice recupera la stringa di connessione dall'insieme di credenziali delle chiavi e lo visualizza nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="10c52-181">This code example retrieves the connection string from the key vault and displays it to the command line.</span></span>

6. <span data-ttu-id="10c52-182">Salvare e chiudere il file Java.</span><span class="sxs-lookup"><span data-stu-id="10c52-182">Save and close the Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="10c52-183">Compilare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="10c52-183">Build and test your app</span></span>

1. <span data-ttu-id="10c52-184">Passare alla directory in cui si trova il file *pom.xml* per l'app Spring Boot:</span><span class="sxs-lookup"><span data-stu-id="10c52-184">Navigate to the directory where the *pom.xml* file for your Spring Boot app is located:</span></span>

1. <span data-ttu-id="10c52-185">Compilare l'applicazione Spring Boot con Maven, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="10c52-185">Build your Spring Boot application with Maven; for example:</span></span>

   ```bash
   mvn clean package
   ```

   <span data-ttu-id="10c52-186">Maven visualizzerà i risultati della compilazione.</span><span class="sxs-lookup"><span data-stu-id="10c52-186">Maven will display the results of your build.</span></span>

   ![Stato di compilazione dell'applicazione Spring Boot][build-application-01]

1. <span data-ttu-id="10c52-188">Eseguire l'applicazione Spring Boot con Maven. L'applicazione visualizzerà la stringa di connessione dall'insieme di credenziali delle chiavi,</span><span class="sxs-lookup"><span data-stu-id="10c52-188">Run your Spring Boot application with Maven; the application will display the connection string from your key vault.</span></span> <span data-ttu-id="10c52-189">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="10c52-189">For example:</span></span>

   ```bash
   mvn spring-boot:run
   ```

   ![Messaggio in fase di esecuzione di Spring Boot][build-application-02]

## <a name="next-steps"></a><span data-ttu-id="10c52-191">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="10c52-191">Next steps</span></span>

<span data-ttu-id="10c52-192">Per altre informazioni sull'uso di Azure Key Vault, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="10c52-192">For more information about using Azure Key Vaults, see the following articles:</span></span>

* <span data-ttu-id="10c52-193">[Documentazione su Key Vault].</span><span class="sxs-lookup"><span data-stu-id="10c52-193">[Key Vault Documentation].</span></span>

* <span data-ttu-id="10c52-194">[Introduzione all'insieme di credenziali delle chiavi di Azure]</span><span class="sxs-lookup"><span data-stu-id="10c52-194">[Get started with Azure Key Vault]</span></span>

<span data-ttu-id="10c52-195">Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="10c52-195">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="10c52-196">Distribuire un'applicazione Spring Boot nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="10c52-196">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="10c52-197">Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure</span><span class="sxs-lookup"><span data-stu-id="10c52-197">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="10c52-198">Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="10c52-198">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Documentazione su Key Vault]: /azure/key-vault/
[Key Vault Documentation]: /azure/key-vault/
[Introduzione all'insieme di credenziali delle chiavi di Azure]: /azure/key-vault/key-vault-get-started
[Get started with Azure Key Vault]: /azure/key-vault/key-vault-get-started
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
