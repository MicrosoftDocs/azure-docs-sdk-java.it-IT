---
title: Come creare un'applicazione Spring Cloud Stream Binder con Hub eventi di Azure
description: Informazioni su come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializr con Hub eventi di Azure.
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: 3f7eeffe8bd36196f9b79edd60830b5d202ea285
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506429"
---
# <a name="how-to-create-a-spring-cloud-stream-binder-application-with-azure-event-hubs"></a><span data-ttu-id="01830-103">Come creare un'applicazione Spring Cloud Stream Binder con Hub eventi di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-103">How to create a Spring Cloud Stream Binder application with Azure Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="01830-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="01830-104">Overview</span></span>

<span data-ttu-id="01830-105">Questo articolo illustra come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializer con Hub eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="01830-105">This article demonstrates how to configure a Java-based Spring Cloud Stream Binder application created with the Spring Boot Initializer with Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01830-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="01830-106">Prerequisites</span></span>

<span data-ttu-id="01830-107">I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="01830-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="01830-108">Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].</span><span class="sxs-lookup"><span data-stu-id="01830-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="01830-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="01830-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="01830-110">[Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="01830-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="01830-111">Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="01830-111">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a><span data-ttu-id="01830-112">Creare un hub eventi di Azure con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-112">Create an Azure Event Hub using the Azure portal</span></span>

### <a name="create-an-azure-event-hub-namespace"></a><span data-ttu-id="01830-113">Creare uno spazio dei nomi dell'hub eventi di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-113">Create an Azure Event Hub Namespace</span></span>

1. <span data-ttu-id="01830-114">Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="01830-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="01830-115">Fare clic su **+Crea una risorsa**, quindi su **Internet delle cose** e infine su **Hub eventi**.</span><span class="sxs-lookup"><span data-stu-id="01830-115">Click **+Create a resource**, then **Internet of Things**, and then click **Event Hubs**.</span></span>

   ![Creare uno spazio dei nomi dell'hub eventi di Azure][IMG01]

1. <span data-ttu-id="01830-117">Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="01830-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="01830-118">Immettere un **nome** univoco, che diventerà parte dell'URI dello spazio dei nomi dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="01830-118">Enter a unique **Name**, which will become part of the URI for your event hub namespace.</span></span> <span data-ttu-id="01830-119">Se si immette **wingtiptoys** in **Nome**, ad esempio, l'URI sarà *wingtiptoys.servicebus.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="01830-119">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.servicebus.windows.net*.</span></span>
   * <span data-ttu-id="01830-120">Scegliere un **piano tariffario** per lo spazio dei nomi dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="01830-120">Choose a **Pricing tier** for your event hub namespace.</span></span>
   * <span data-ttu-id="01830-121">Scegliere la **sottoscrizione** da usare per lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="01830-121">Choose the **Subscription** you want to use for your namespace.</span></span>
   * <span data-ttu-id="01830-122">Specificare se creare un nuovo **gruppo di risorse** per lo spazio dei nomi o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="01830-122">Specify whether to create a new **Resource group** for your namespace, or choose an existing resource group.</span></span>
   * <span data-ttu-id="01830-123">Specificare la **località** per lo spazio dei nomi dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="01830-123">Specify the **Location** for your event hub namespace.</span></span>
   
   ![Specificare le opzioni per lo spazio dei nomi dell'hub eventi di Azure][IMG02]

1. <span data-ttu-id="01830-125">Dopo aver specificato le opzioni elencate sopra, fare clic su **Crea** per creare lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="01830-125">When you have specified the options listed above, click **Create** to create your namespace.</span></span>

### <a name="create-an-azure-event-hub-in-your-namespace"></a><span data-ttu-id="01830-126">Creare un hub eventi di Azure nello spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="01830-126">Create an Azure Event Hub in your namespace</span></span>

1. <span data-ttu-id="01830-127">Passare al portale di Azure all'indirizzo <https://portal.azure.com/>.</span><span class="sxs-lookup"><span data-stu-id="01830-127">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="01830-128">Fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi creato.</span><span class="sxs-lookup"><span data-stu-id="01830-128">Click **All resources**, and then click the namespace that you created.</span></span>

   ![Selezionare lo spazio dei nomi dell'hub eventi di Azure][IMG03]

1. <span data-ttu-id="01830-130">Fare clic su **Hub eventi** e quindi su **+Hub eventi**.</span><span class="sxs-lookup"><span data-stu-id="01830-130">Click **Event Hubs**, and then click **+Event Hub**.</span></span>

   ![Aggiungere un nuovo hub eventi di Azure][IMG04]

1. <span data-ttu-id="01830-132">Nella pagina **Crea hub eventi** immettere un **nome** univoco per l'hub eventi e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="01830-132">On the **Create Event Hub** page, enter a unique **Name** for your Event Hub, and then click **Create**.</span></span>

   ![Creare Hub eventi di Azure][IMG05]

1. <span data-ttu-id="01830-134">Al termine della creazione, l'hub eventi sarà incluso nell'elenco nella pagina **Hub eventi**.</span><span class="sxs-lookup"><span data-stu-id="01830-134">When your Event Hub has been created, it will be listed on the **Event Hubs** page.</span></span>

   ![Creare Hub eventi di Azure][IMG06]

### <a name="create-an-azure-storage-account-for-your-event-hub-checkpoints"></a><span data-ttu-id="01830-136">Creare un account di archiviazione di Azure per i checkpoint dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="01830-136">Create an Azure Storage Account for your Event Hub checkpoints</span></span>

1. <span data-ttu-id="01830-137">Passare al portale di Azure all'indirizzo <https://portal.azure.com/>.</span><span class="sxs-lookup"><span data-stu-id="01830-137">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="01830-138">Fare clic su **+Crea una risorsa**, quindi su **Archiviazione** e infine su **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="01830-138">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![Creare un account di archiviazione di Azure][IMG07]

1. <span data-ttu-id="01830-140">Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="01830-140">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="01830-141">Immettere un **nome** univoco, che diventerà parte dell'URI dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="01830-141">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="01830-142">Se si immette **wingtiptoys** in **Nome**, ad esempio, l'URI sarà *wingtiptoys.core.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="01830-142">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.core.windows.net*.</span></span>
   * <span data-ttu-id="01830-143">Scegliere **Archivio BLOB** in **Tipologia account**.</span><span class="sxs-lookup"><span data-stu-id="01830-143">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="01830-144">Specificare la **località** per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="01830-144">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="01830-145">Scegliere la **sottoscrizione** da usare per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="01830-145">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="01830-146">Specificare se creare un nuovo **gruppo di risorse** per l'account di archiviazione o sceglierne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="01830-146">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>
   
   ![Specificare le opzioni per l'account di archiviazione di Azure][IMG08]

1. <span data-ttu-id="01830-148">Dopo aver specificato le opzioni elencate sopra, fare clic su **Crea** per creare l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="01830-148">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="01830-149">Creare un'applicazione Spring Boot semplice con Spring Initializr</span><span class="sxs-lookup"><span data-stu-id="01830-149">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="01830-150">Passare a <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="01830-150">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="01830-151">Specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="01830-151">Specify the following options:</span></span>

   * <span data-ttu-id="01830-152">Generare un progetto **Maven** con **Java**.</span><span class="sxs-lookup"><span data-stu-id="01830-152">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="01830-153">Specificare **Spring Boot** versione 2.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="01830-153">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="01830-154">Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="01830-154">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="01830-155">Aggiungere la dipendenza **Web**.</span><span class="sxs-lookup"><span data-stu-id="01830-155">Add the **Web** dependency.</span></span>

      ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="01830-157">Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.wingtiptoys.eventhub*.</span><span class="sxs-lookup"><span data-stu-id="01830-157">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.eventhub*.</span></span>
   >

1. <span data-ttu-id="01830-158">Dopo aver specificato le opzioni elencate sopra, fare clic su **Generate Project** (Genera progetto).</span><span class="sxs-lookup"><span data-stu-id="01830-158">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="01830-159">Quando richiesto, scaricare il progetto in un percorso nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="01830-159">When prompted, download the project to a path on your local computer.</span></span>

   ![Scaricare il progetto Spring][SI02]

1. <span data-ttu-id="01830-161">Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.</span><span class="sxs-lookup"><span data-stu-id="01830-161">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-event-hub-starter"></a><span data-ttu-id="01830-162">Configurare l'app Spring Boot per l'uso dell'utilità di avvio per Hub eventi di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-162">Configure your Spring Boot app to use the Azure Event Hub starter</span></span>

1. <span data-ttu-id="01830-163">Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-163">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\pom.xml`

   <span data-ttu-id="01830-164">-oppure-</span><span class="sxs-lookup"><span data-stu-id="01830-164">-or-</span></span>

   `/users/example/home/eventhub/pom.xml`

1. <span data-ttu-id="01830-165">Aprire il file *pom.xml* in un editor di testo e aggiungere l'utilità di avvio Spring Cloud Stream Binder per Hub eventi di Azure all'elenco in `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="01830-165">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Event Hub Stream Binder starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-eventhub-stream-binder</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![Modificare il file pom.xml][SI03]

1. <span data-ttu-id="01830-167">Salvare e chiudere il file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="01830-167">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="01830-168">Creare un file di credenziali di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-168">Create an Azure Credential File</span></span>

1. <span data-ttu-id="01830-169">Aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="01830-169">Open a command prompt.</span></span>

1. <span data-ttu-id="01830-170">Passare alla directory *resources* dell'app Spring Boot, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-170">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   <span data-ttu-id="01830-171">-oppure-</span><span class="sxs-lookup"><span data-stu-id="01830-171">-or-</span></span>

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. <span data-ttu-id="01830-172">Accedere all'account Azure:</span><span class="sxs-lookup"><span data-stu-id="01830-172">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="01830-173">Elencare le sottoscrizioni:</span><span class="sxs-lookup"><span data-stu-id="01830-173">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="01830-174">Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-174">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the subscription you want to use with Azure; for example:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. <span data-ttu-id="01830-175">Creare il file di credenziali di Azure:</span><span class="sxs-lookup"><span data-stu-id="01830-175">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="01830-176">Questo comando creerà un file *my.azureauth* nella directory *resources* con contenuto simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="01830-176">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a><span data-ttu-id="01830-177">Configurare l'app Spring Boot per l'uso dell'hub eventi di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-177">Configure your Spring Boot app to use your Azure Event Hub</span></span>

1. <span data-ttu-id="01830-178">Individuare *application.properties* nella directory *resources* dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-178">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   <span data-ttu-id="01830-179">-oppure-</span><span class="sxs-lookup"><span data-stu-id="01830-179">-or-</span></span>

   `/users/example/home/eventhub/src/main/resources/application.properties`

1.  <span data-ttu-id="01830-180">Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'hub eventi:</span><span class="sxs-lookup"><span data-stu-id="01830-180">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your event hub:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace
   spring.cloud.azure.eventhub.checkpoint-storage-account=wingtiptoysstorage
   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   spring.cloud.stream.eventhub.bindings.input.consumer.checkpoint-mode=MANUAL
   ```
   <span data-ttu-id="01830-181">Dove:</span><span class="sxs-lookup"><span data-stu-id="01830-181">Where:</span></span>
   | <span data-ttu-id="01830-182">Campo</span><span class="sxs-lookup"><span data-stu-id="01830-182">Field</span></span> | <span data-ttu-id="01830-183">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="01830-183">Description</span></span> |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | <span data-ttu-id="01830-184">Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="01830-184">Specifies Azure credential file that you created earlier in this tutorial.</span></span> |
   | `spring.cloud.azure.resource-group` | <span data-ttu-id="01830-185">Specifica il gruppo di risorse di Azure contenente l'hub eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="01830-185">Specifies the Azure Resource Group that contains your Azure Event Hub.</span></span> |
   | `spring.cloud.azure.region` | <span data-ttu-id="01830-186">Specifica l'area geografica indicata al momento della creazione dell'hub eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="01830-186">Specifies the geographical region that you specified when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.azure.eventhub.namespace` | <span data-ttu-id="01830-187">Specifica il nome univoco fornito al momento della creazione dello spazio dei nomi dell'hub eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="01830-187">Specifies the unique name that you specified when you created your Azure Event Hub Namespace.</span></span> |
   | `spring.cloud.azure.eventhub.checkpoint-storage-account` | <span data-ttu-id="01830-188">Specifica l'account di archiviazione di Azure creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="01830-188">Specifies Azure Storage Account that you created earlier in this tutorial.</span></span>
   | `spring.cloud.stream.bindings.input.destination` | <span data-ttu-id="01830-189">Specifica l'hub eventi di Azure destinazione di input, che in questo caso è l'hub creato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="01830-189">Specifies the input destination Azure Event Hub, which for this tutorial is the  hub you created earlier in this tutorial.</span></span> |
   | `spring.cloud.stream.bindings.input.group `| <span data-ttu-id="01830-190">Specifica un gruppo di consumer dell'hub eventi di Azure, che è possibile impostare su "$Default" per usare il gruppo di consumer di base creato al momento della creazione dell'hub eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="01830-190">Specifies a Consumer Group from Azure Event Hub, which can be set to '$Default' in order to use the basic consumer group that was created when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.stream.bindings.output.destination` | <span data-ttu-id="01830-191">Specifica l'hub eventi di Azure destinazione di output, che per questa esercitazione è uguale alla destinazione di input.</span><span class="sxs-lookup"><span data-stu-id="01830-191">Specifies the output destination Azure Event Hub, which for this tutorial will be the same as the input destination.</span></span> |

1. <span data-ttu-id="01830-192">Salvare e chiudere il file *application.properties*.</span><span class="sxs-lookup"><span data-stu-id="01830-192">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a><span data-ttu-id="01830-193">Aggiungere codice di esempio per implementare le funzionalità di base dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="01830-193">Add sample code to implement basic event hub functionality</span></span>

<span data-ttu-id="01830-194">In questa sezione si creano le classi Java necessarie per inviare eventi all'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="01830-194">In this section, you create the necessary Java classes for sending events to your event hub.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="01830-195">Modificare la classe dell'applicazione main</span><span class="sxs-lookup"><span data-stu-id="01830-195">Modify the main application class</span></span>

1. <span data-ttu-id="01830-196">Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-196">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\java\com\wingtiptoys\eventhub\EventhubApplication.java`

   <span data-ttu-id="01830-197">-oppure-</span><span class="sxs-lookup"><span data-stu-id="01830-197">-or-</span></span>

   `/users/example/home/eventhub/src/main/java/com/wingtiptoys/eventhub/EventhubApplication.java`

1. <span data-ttu-id="01830-198">Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:</span><span class="sxs-lookup"><span data-stu-id="01830-198">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.eventhub;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class EventhubApplication {
      public static void main(String[] args) {
         SpringApplication.run(EventhubApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="01830-199">Salvare e chiudere il file Java dell'applicazione main.</span><span class="sxs-lookup"><span data-stu-id="01830-199">Save and close the main application Java file.</span></span>

### <a name="create-a-new-class-for-the-source-connector"></a><span data-ttu-id="01830-200">Creare una nuova classe per il connettore di origine</span><span class="sxs-lookup"><span data-stu-id="01830-200">Create a new class for the source connector</span></span>

1. <span data-ttu-id="01830-201">Creare un nuovo file Java denominato *EventhubSource.java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="01830-201">Create a new Java file named *EventhubSource.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.eventhub;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   
   @EnableBinding(Source.class)
   @RestController
   public class EventhubSource {
   
      @Autowired
      private Source source;
   
      @PostMapping("/messages")
      public String postMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```
1. <span data-ttu-id="01830-202">Salvare e chiudere il file *EventhubSource.java*.</span><span class="sxs-lookup"><span data-stu-id="01830-202">Save and close the *EventhubSource.java* file.</span></span>

### <a name="create-a-new-class-for-the-sink-connector"></a><span data-ttu-id="01830-203">Creare una nuova classe per il connettore sink</span><span class="sxs-lookup"><span data-stu-id="01830-203">Create a new class for the sink connector</span></span>

1. <span data-ttu-id="01830-204">Creare un nuovo file Java denominato *EventhubSink.java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="01830-204">Create a new Java file named *EventhubSink.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.eventhub;
   
   import com.microsoft.azure.spring.integration.core.AzureHeaders;
   import com.microsoft.azure.spring.integration.core.api.Checkpointer;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   import org.springframework.messaging.handler.annotation.Header;
   
   @EnableBinding(Sink.class)
   public class EventhubSink {
   
      private static final Logger LOGGER = LoggerFactory.getLogger(EventhubSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
         LOGGER.info("New message received: '{}'", message);
         checkpointer.success().handle((r, ex) -> {
            if (ex == null) {
               LOGGER.info("Message '{}' successfully checkpointed", message);
            }
            return null;
         });
      }
   }
   ```

1. <span data-ttu-id="01830-205">Salvare e chiudere il file *EventhubSink.java*.</span><span class="sxs-lookup"><span data-stu-id="01830-205">Save and close the *EventhubSink.java* file.</span></span>

## <a name="build-and-test-your-application"></a><span data-ttu-id="01830-206">Compilare e testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="01830-206">Build and test your application</span></span>

1. <span data-ttu-id="01830-207">Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-207">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\eventhub`

   <span data-ttu-id="01830-208">-oppure-</span><span class="sxs-lookup"><span data-stu-id="01830-208">-or-</span></span>

   `cd /users/example/home/eventhub`

1. <span data-ttu-id="01830-209">Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-209">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="01830-210">Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01830-210">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   <span data-ttu-id="01830-211">Verrà visualizzato l'inserimento di "hello" nei log dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="01830-211">You should see "hello" posted to your application's logs.</span></span> <span data-ttu-id="01830-212">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="01830-212">For example:</span></span>

   ```shell
   [Thread-13] INFO com.wingtiptoys.eventhub.EventhubSink - New message received: 'hello'
   [pool-10-thread-7] INFO com.wingtiptoys.eventhub.EventhubSink - Message 'hello' successfully checkpointed
   ```

## <a name="next-steps"></a><span data-ttu-id="01830-213">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="01830-213">Next steps</span></span>

<span data-ttu-id="01830-214">Per altre informazioni sul supporto di Azure per Stream Binder per Hub eventi, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="01830-214">For more information about Azure support for Event Hub Stream Binder, see the following articles:</span></span>

* [<span data-ttu-id="01830-215">Che cos'è l'hub di eventi di Azure?</span><span class="sxs-lookup"><span data-stu-id="01830-215">What is Azure Event Hubs?</span></span>](/azure/event-hubs/event-hubs-about)

* [<span data-ttu-id="01830-216">Creare uno spazio dei nomi di Hub eventi e un hub eventi usando il Portale di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-216">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>](/azure/event-hubs/event-hubs-create)

* [<span data-ttu-id="01830-217">Come usare Spring Boot Starter per Apache Kafka con Hub eventi di Azure</span><span class="sxs-lookup"><span data-stu-id="01830-217">How to use the Spring Boot Starter for Apache Kafka with Azure Event Hubs</span></span>](configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub.md)

<span data-ttu-id="01830-218">Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e gli [strumenti Java per Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="01830-218">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="01830-219">**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise.</span><span class="sxs-lookup"><span data-stu-id="01830-219">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="01830-220">Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome.</span><span class="sxs-lookup"><span data-stu-id="01830-220">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="01830-221">Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="01830-221">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="01830-222">Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.</span><span class="sxs-lookup"><span data-stu-id="01830-222">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

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

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-03.png