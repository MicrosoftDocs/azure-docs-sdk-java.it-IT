---
title: Introduzione ad Azure per Java con Eclipse
description: Introduzione all'uso di base delle librerie di Azure per Java con la propria sottoscrizione di Azure.
keywords: Azure, Java, SDK, API, autenticare, introduzione
services: 
documentationcenter: java
author: roygara
manager: timlt
editor: 
ms.author: v-rogara
ms.date: 02/01/2018
ms.devlang: java
ms.prod: azure
ms.technology: azure
ms.topic: get-started-article
ms.service: multiple
ms.openlocfilehash: 740679197981f49d99b8d8251e257963d3030fb1
ms.sourcegitcommit: 720c2eaf66532d277015610ec375c71e934d9ee6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2018
---
# <a name="get-started-with-the-azure-libraries-using-eclipse"></a><span data-ttu-id="453de-104">Introduzione alle librerie di Azure con Eclipse</span><span class="sxs-lookup"><span data-stu-id="453de-104">Get started with the Azure libraries using Eclipse</span></span>

<span data-ttu-id="453de-105">Questa guida illustra la configurazione di un ambiente di sviluppo e l'uso delle librerie di Azure per Java.</span><span class="sxs-lookup"><span data-stu-id="453de-105">This guide walks you through setting up a development environment and using the Azure libraries for Java.</span></span> <span data-ttu-id="453de-106">Si creerà un'entità servizio per l'autenticazione con Azure e verrà eseguito un codice di esempio che crea e usa risorse di Azure nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="453de-106">You'll create a service principal to authenticate with Azure and run some sample code that creates and uses Azure resources in your subscription.</span></span> <span data-ttu-id="453de-107">L'uso di Eclipse è facoltativo per lo sviluppo Java con Azure.</span><span class="sxs-lookup"><span data-stu-id="453de-107">Using Eclipse is optional for Java development with Azure.</span></span> <span data-ttu-id="453de-108">È possibile usare qualsiasi IDE con integrazione Maven.</span><span class="sxs-lookup"><span data-stu-id="453de-108">Any IDE that has Maven integration works.</span></span> <span data-ttu-id="453de-109">In alternativa, è possibile eseguire il codice dalla riga di comando usando Maven, se si preferisce non usare IDE.</span><span class="sxs-lookup"><span data-stu-id="453de-109">Alternatively, you can run your code from the commandline using Maven if you prefer not to use any IDE.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="453de-110">prerequisiti</span><span class="sxs-lookup"><span data-stu-id="453de-110">Prerequisites</span></span>

- <span data-ttu-id="453de-111">Un account Azure.</span><span class="sxs-lookup"><span data-stu-id="453de-111">An Azure account.</span></span> <span data-ttu-id="453de-112">Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="453de-112">If you don't have one, [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="453de-113">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="453de-113">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="453de-114">La versione stabile più recente di [Eclipse](http://www.eclipse.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="453de-114">The latest stable version of [Eclipse](http://www.eclipse.org/downloads/)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="453de-115">Configurare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="453de-115">Set up authentication</span></span>

<span data-ttu-id="453de-116">L'applicazione Java deve leggere e creare le autorizzazioni nella sottoscrizione di Azure per eseguire il codice di esempio in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="453de-116">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="453de-117">Creare un'entità servizio e configurare l'applicazione per l'esecuzione con le rispettive credenziali.</span><span class="sxs-lookup"><span data-stu-id="453de-117">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="453de-118">Le entità servizio consentono di creare un account non interattivo associato all'identità a cui vengono concessi solo i privilegi necessari per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="453de-118">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="453de-119">[Creare un'entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli) per autorizzare il codice alla creazione e all'aggiornamento delle risorse nella sottoscrizione senza usare direttamente le credenziali dell'account.</span><span class="sxs-lookup"><span data-stu-id="453de-119">[Create a service principal](/cli/azure/create-an-azure-service-principal-azure-cli) to grant your code permission to create and update resources in your subscription without using your account credentials directly.</span></span> <span data-ttu-id="453de-120">Assicurarsi di acquisire l'output.</span><span class="sxs-lookup"><span data-stu-id="453de-120">Make sure to capture the output.</span></span> <span data-ttu-id="453de-121">Specificare una [password di protezione](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) nell'argomento password invece di `MY_SECURE_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="453de-121">Provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span> <span data-ttu-id="453de-122">La password deve contenere da 8 a 16 caratteri e soddisfare almeno 3 dei 4 criteri seguenti:</span><span class="sxs-lookup"><span data-stu-id="453de-122">Your password must be 8 to 16 characters and match at least 3 out of the 4 following criteria:</span></span>

* <span data-ttu-id="453de-123">Includere caratteri minuscoli</span><span class="sxs-lookup"><span data-stu-id="453de-123">Include lowercase characters</span></span>
* <span data-ttu-id="453de-124">Includere caratteri maiuscoli</span><span class="sxs-lookup"><span data-stu-id="453de-124">Include uppercase characters</span></span>
* <span data-ttu-id="453de-125">Includere numeri</span><span class="sxs-lookup"><span data-stu-id="453de-125">Include numbers</span></span>
* <span data-ttu-id="453de-126">Includere uno dei simboli seguenti: @ # $ % ^ & \* - _ !</span><span class="sxs-lookup"><span data-stu-id="453de-126">Include one of the following symbols: @ # $ % ^ & \* - _ !</span></span> <span data-ttu-id="453de-127">+ = [ ] { } | \ : ‘ , .</span><span class="sxs-lookup"><span data-stu-id="453de-127">+ = [ ] { } | \ : ‘ , .</span></span> <span data-ttu-id="453de-128">?</span><span class="sxs-lookup"><span data-stu-id="453de-128">?</span></span> <span data-ttu-id="453de-129">/ \` ~ “ ( ) ;</span><span class="sxs-lookup"><span data-stu-id="453de-129">/ \` ~ “ ( ) ;</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

<span data-ttu-id="453de-130">Restituisce una risposta nel formato seguente:</span><span class="sxs-lookup"><span data-stu-id="453de-130">Which gives you a reply in the following format:</span></span>

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

<span data-ttu-id="453de-131">Copiare quindi il codice seguente in un file di testo nel sistema:</span><span class="sxs-lookup"><span data-stu-id="453de-131">Next, copy the following into a text file on your system:</span></span>

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

<span data-ttu-id="453de-132">Sostituire i primi quattro valori con i seguenti:</span><span class="sxs-lookup"><span data-stu-id="453de-132">Replace the top four values with the following:</span></span>

- <span data-ttu-id="453de-133">subscription: usare il valore *id* da `az account show` nell'interfaccia della riga di comando di Azure 2.0.</span><span class="sxs-lookup"><span data-stu-id="453de-133">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="453de-134">client: usare il valore *appId* dell'output di un'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="453de-134">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="453de-135">key: usare il valore *password* dell'output dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="453de-135">key: use the *password* value from the service principal output.</span></span>
- <span data-ttu-id="453de-136">tenant: usare il valore *tenant* dell'output dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="453de-136">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="453de-137">Salvare questo file nel sistema in una posizione sicura e leggibile dal codice.</span><span class="sxs-lookup"><span data-stu-id="453de-137">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="453de-138">È possibile usare questo file per il codice futuro, quindi è consigliabile archiviarlo in una posizione esterna rispetto all'applicazione descritta in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="453de-138">You may use this file for future code so it's recommended to store it somewhere external to the application in this article.</span></span>

<span data-ttu-id="453de-139">Impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file di autenticazione nella shell.</span><span class="sxs-lookup"><span data-stu-id="453de-139">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="453de-140">Se si lavora in ambiente Windows, aggiungere la variabile alle proprietà del sistema.</span><span class="sxs-lookup"><span data-stu-id="453de-140">If you're working in a windows environment, add the variable to your system properties.</span></span> <span data-ttu-id="453de-141">Aprire una finestra di PowerShell con privilegi di amministratore, sostituire la seconda variabile con il percorso del file e quindi immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="453de-141">Open a PowerShell window with administrator privledges and, after replacing the second variable with the path to your file, enter the following command:</span></span>

```powershell
setx AZURE_AUTH_LOCATION "C:\<fullpath>\azureauth.properties" /m
```

## <a name="create-a-new-maven-project"></a><span data-ttu-id="453de-142">Creare un nuovo progetto Maven</span><span class="sxs-lookup"><span data-stu-id="453de-142">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="453de-143">Questa guida usa lo strumento di compilazione Maven per compilare ed eseguire il codice di esempio, ma con le librerie di Azure per Java si possono usare anche altri strumenti di compilazione, ad esempio Gradle.</span><span class="sxs-lookup"><span data-stu-id="453de-143">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="453de-144">Aprire Eclipse e selezionare **File** -> **New** (Nuovo).</span><span class="sxs-lookup"><span data-stu-id="453de-144">Open Eclipse, select **File** -> **New**.</span></span> <span data-ttu-id="453de-145">Nella nuova finestra visualizzata aprire la cartella Maven, quindi selezionare Maven Project (Progetto Maven).</span><span class="sxs-lookup"><span data-stu-id="453de-145">In the new window that appears open the Maven folder then select Maven Project.</span></span> 

<span data-ttu-id="453de-146">Lasciare le impostazioni predefinite nella schermata successiva, quindi selezionare **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="453de-146">Leave the selections on the next screen defaults, and select **Next**.</span></span> <span data-ttu-id="453de-147">Lasciare le impostazioni predefinite anche in questa schermata relativa agli archetipi.</span><span class="sxs-lookup"><span data-stu-id="453de-147">Do the same for this screen regarding archetypes.</span></span>

<span data-ttu-id="453de-148">Nella schermata che chiede groupID, ArtifactID e così via immettere "com.fabrikam" per groupID e "AzureApp" per artifactID.</span><span class="sxs-lookup"><span data-stu-id="453de-148">When you come to the screen asking for groupID, ArtifactID, etc. Enter "com.fabrikam" for the groupID and enter "AzureApp" for the artifactID.</span></span>

<span data-ttu-id="453de-149">Aprire il file pom.xml.</span><span class="sxs-lookup"><span data-stu-id="453de-149">Now, open the pom.xml file.</span></span> <span data-ttu-id="453de-150">Aggiungere il codice seguente all'interno del tag `dependencies`:</span><span class="sxs-lookup"><span data-stu-id="453de-150">Inside the `dependencies` tag add the following code:</span></span>

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

<span data-ttu-id="453de-151">Salvare il file pom.xml.</span><span class="sxs-lookup"><span data-stu-id="453de-151">Now, save the pom.xml.</span></span> <span data-ttu-id="453de-152">Eclipse scaricherà tutte le dipendenze specificate.</span><span class="sxs-lookup"><span data-stu-id="453de-152">This prompts Eclipse to download all the specified dependencies.</span></span> <span data-ttu-id="453de-153">L'operazione potrebbe richiedere alcuni istanti.</span><span class="sxs-lookup"><span data-stu-id="453de-153">This may take a moment.</span></span>
   
## <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="453de-154">Installare Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="453de-154">Install the azure toolkit for Eclipse</span></span>

<span data-ttu-id="453de-155">[Azure Toolkit](azure-toolkit-for-eclipse.md) è necessario se si prevede di distribuire app Web o API a livello di codice, ma non viene attualmente usato per altri tipi di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="453de-155">The [Azure toolkit](azure-toolkit-for-eclipse.md) is necessary if you're going to be deploying web apps or APIs programmatically but is not currently used for any other kinds of development.</span></span> <span data-ttu-id="453de-156">Di seguito è riportato un riepilogo della procedura di installazione.</span><span class="sxs-lookup"><span data-stu-id="453de-156">The following is a summary of the installation process.</span></span> <span data-ttu-id="453de-157">Per i passaggi dettagliati, vedere [Installazione di Azure Toolkit for Eclipse](azure-toolkit-for-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="453de-157">For detailed stpes, visit [Installing the Azure Toolkit for Eclipse](azure-toolkit-for-eclipse.md).</span></span>

<span data-ttu-id="453de-158">Dal menu **Help** (?) scegliere **Install New Software** (Installa nuovo software).</span><span class="sxs-lookup"><span data-stu-id="453de-158">Select the **Help** menu and then select **Install New software**.</span></span>

<span data-ttu-id="453de-159">Nel campo **Usa:** immettere `http://dl.microsoft.com/eclipse` e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="453de-159">In the **Work with:** field enter `http://dl.microsoft.com/eclipse` and press enter.</span></span>

<span data-ttu-id="453de-160">Selezionare quindi la casella di controllo accanto ad **Azure toolkit for Java** e deselezionare la casella di controllo **Contact all update sites during install to find required software** (Contatta tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto).</span><span class="sxs-lookup"><span data-stu-id="453de-160">Then, select the checkbox next to **Azure toolkit for Java** and uncheck the checkbox for **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="453de-161">Selezionare Next (Avanti).</span><span class="sxs-lookup"><span data-stu-id="453de-161">Then select next.</span></span>

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="453de-162">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="453de-162">Create a Linux virtual machine</span></span>

<span data-ttu-id="453de-163">Creare un nuovo file denominato `AzureApp.java` nella directory `src/main/java` del progetto e incollare il blocco di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="453de-163">Create a new file named `AzureApp.java` in the project's `src/main/java` directory and paste in the following block of code.</span></span> <span data-ttu-id="453de-164">Aggiornare le variabili `userName` e `sshKey` con i valori reali del computer in uso.</span><span class="sxs-lookup"><span data-stu-id="453de-164">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="453de-165">Questo codice crea una nuova VM Linux denominata `testLinuxVM` in un gruppo di risorse `sampleResourceGroup` in esecuzione nell'area di Azure Stati Uniti orientali.</span><span class="sxs-lookup"><span data-stu-id="453de-165">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

<span data-ttu-id="453de-166">Per creare un elemento `sshkey`, aprire Azure Cloud Shell e immettere `ssh-keygen -t rsa -b 2048`.</span><span class="sxs-lookup"><span data-stu-id="453de-166">In order to create an `sshkey`, open the azure cloud shell and enter `ssh-keygen -t rsa -b 2048`.</span></span> <span data-ttu-id="453de-167">Assegnare un nome al file e quindi accedere al file con estensione .public per ottenere la chiave che verrà usata nel codice seguente. Copiare e incollare per intero nella variabile `sshKey`.</span><span class="sxs-lookup"><span data-stu-id="453de-167">Name the file and then access the .public file to get the key, which you use in the following code, copy and paste it all into your variable `sshKey`.</span></span>

```java
package com.fabrikam.AzureApp;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.PricingTier;
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
import java.util.List;

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


<span data-ttu-id="453de-168">Verranno visualizzate alcune richieste e risposte REST nella console perché l'SDK esegue le chiamate sottostanti all'API REST di Azure per configurare la macchina virtuale e le risorse.</span><span class="sxs-lookup"><span data-stu-id="453de-168">You see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="453de-169">Al termine del programma, verificare la macchina virtuale nella sottoscrizione con l'interfaccia della riga di comando di Azure 2.0:</span><span class="sxs-lookup"><span data-stu-id="453de-169">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="453de-170">Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare la VM e le risorse.</span><span class="sxs-lookup"><span data-stu-id="453de-170">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="453de-171">Distribuire un'app Web da un repository di GitHub</span><span class="sxs-lookup"><span data-stu-id="453de-171">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="453de-172">Sostituire il metodo principale in `AzureApp.java` con quello seguente, aggiornando la variabile `appName` a un valore univoco prima di eseguire il codice.</span><span class="sxs-lookup"><span data-stu-id="453de-172">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="453de-173">Questo codice sviluppa un'applicazione Web dal ramo `master` di un repository di GitHub pubblico in una nuova [app Web del servizio app di Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) in esecuzione nel piano tariffario gratuito.</span><span class="sxs-lookup"><span data-stu-id="453de-173">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

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

<span data-ttu-id="453de-174">Eseguire il codice come prima usando Maven:</span><span class="sxs-lookup"><span data-stu-id="453de-174">Run the code as before using Maven:</span></span>

<span data-ttu-id="453de-175">Aprire un browser che punta all'applicazione usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="453de-175">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```
<span data-ttu-id="453de-176">Rimuovere l'app Web e il piano dalla sottoscrizione dopo avere verificato la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="453de-176">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a><span data-ttu-id="453de-177">Connettersi a un database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="453de-177">Connect to an Azure SQL database</span></span>

<span data-ttu-id="453de-178">Sostituire il metodo principale corrente in `AzureApp.java` con il codice seguente, impostando un valore reale per la variabile `dbPassword`.</span><span class="sxs-lookup"><span data-stu-id="453de-178">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="453de-179">Questo codice crea un nuovo database SQL con una regola del firewall che consente l'accesso remoto e quindi vi si connette usando il driver JBDC del database SQL.</span><span class="sxs-lookup"><span data-stu-id="453de-179">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

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
<span data-ttu-id="453de-180">Eseguire l'esempio dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="453de-180">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="453de-181">Pulire quindi le risorse usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="453de-181">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="453de-182">Scrivere un BLOB in un nuovo account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="453de-182">Write a blob into a new storage account</span></span>

<span data-ttu-id="453de-183">Sostituire il metodo principale corrente in `AzureApp.java` con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="453de-183">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="453de-184">Questo codice crea un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e quindi usa le librerie di archiviazione di Azure per Java per creare un nuovo file di testo nel cloud.</span><span class="sxs-lookup"><span data-stu-id="453de-184">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

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

            // create a storage container to hold the files
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

<span data-ttu-id="453de-185">Eseguire l'esempio dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="453de-185">Run the sample from the command line:</span></span>

<span data-ttu-id="453de-186">È possibile cercare il file `helloazure.txt` nell'account di archiviazione tramite il portale di Azure o con [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span><span class="sxs-lookup"><span data-stu-id="453de-186">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="453de-187">Pulire l'account di archiviazione usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="453de-187">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="453de-188">Esplorare altri esempi</span><span class="sxs-lookup"><span data-stu-id="453de-188">Explore more samples</span></span>

<span data-ttu-id="453de-189">Per altre informazioni su come usare le librerie di gestione di Azure per Java per gestire le risorse e l'automazione delle attività, vedere il codice di esempio per [macchine virtuali](../java-sdk-azure-virtual-machine-samples.md), [app Web](../java-sdk-azure-web-apps-samples.md) e [database SQL](../java-sdk-azure-sql-database-samples.md).</span><span class="sxs-lookup"><span data-stu-id="453de-189">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](../java-sdk-azure-virtual-machine-samples.md), [web apps](../java-sdk-azure-web-apps-samples.md) and [SQL database](../java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="453de-190">Informazioni di riferimento e note sulla versione</span><span class="sxs-lookup"><span data-stu-id="453de-190">Reference and release notes</span></span>

<span data-ttu-id="453de-191">Le [informazioni di riferimento](http://docs.microsoft.com/java/api) sono disponibili per tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="453de-191">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="453de-192">Ottenere supporto e inviare commenti</span><span class="sxs-lookup"><span data-stu-id="453de-192">Get help and give feedback</span></span>

<span data-ttu-id="453de-193">Pubblicare le domande per la community in [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span><span class="sxs-lookup"><span data-stu-id="453de-193">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="453de-194">Segnalare bug e problemi in sospeso relativi alle librerie di Azure per Java nel [progetto GitHub](https://github.com/Azure/azure-sdk-for-java).</span><span class="sxs-lookup"><span data-stu-id="453de-194">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>
