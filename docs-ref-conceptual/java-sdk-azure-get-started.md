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
ms.openlocfilehash: dee41f08dc303fc95fcfa6a585a5d1a06726f2ef
ms.sourcegitcommit: dad28b332346dfa9af249b5a64e042cbb1eb90d7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39625037"
---
# <a name="get-started-with-cloud-development-using-the-azure-libraries-for-java"></a><span data-ttu-id="9708d-104">Introduzione allo sviluppo per il cloud con le librerie di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="9708d-104">Get started with cloud development using the Azure libraries for Java</span></span>

<span data-ttu-id="9708d-105">Questa guida illustra la configurazione di un ambiente di sviluppo per lo sviluppo di Azure in Java.</span><span class="sxs-lookup"><span data-stu-id="9708d-105">This guide walks you through setting up a development environment for Azure development in Java.</span></span> <span data-ttu-id="9708d-106">Verranno quindi create alcune risorse di Azure che verranno connesse per eseguire alcune attività di base, come caricare un file o distribuire un'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="9708d-106">You'll then create some Azure resources and connect them to to perform some basic tasks, like uploading a file or deploying a web application.</span></span> <span data-ttu-id="9708d-107">Al termine sarà possibile iniziare a usare i servizi di Azure nelle proprie applicazioni Java.</span><span class="sxs-lookup"><span data-stu-id="9708d-107">When you're done, you'll be ready to start using Azure services in your own Java applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9708d-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9708d-108">Prerequisites</span></span>

- <span data-ttu-id="9708d-109">Un account Azure.</span><span class="sxs-lookup"><span data-stu-id="9708d-109">An Azure account.</span></span> <span data-ttu-id="9708d-110">Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="9708d-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="9708d-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="9708d-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="9708d-112">[Java 8](https://www.azul.com/downloads/zulu/) (incluso in Azure Cloud Shell)</span><span class="sxs-lookup"><span data-stu-id="9708d-112">[Java 8](https://www.azul.com/downloads/zulu/) (included in Azure Cloud Shell)</span></span>
- <span data-ttu-id="9708d-113">[Maven 3](http://maven.apache.org/download.cgi) (incluso in Azure Cloud Shell)</span><span class="sxs-lookup"><span data-stu-id="9708d-113">[Maven 3](http://maven.apache.org/download.cgi) (included in Azure Cloud Shell)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="9708d-114">Configurare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="9708d-114">Set up authentication</span></span>

<span data-ttu-id="9708d-115">L'applicazione Java deve leggere e creare le autorizzazioni nella sottoscrizione di Azure per eseguire il codice di esempio in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9708d-115">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="9708d-116">Creare un'entità servizio e configurare l'applicazione per l'esecuzione con le rispettive credenziali.</span><span class="sxs-lookup"><span data-stu-id="9708d-116">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="9708d-117">Le entità servizio consentono di creare un account non interattivo associato all'identità a cui vengono concessi solo i privilegi necessari per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="9708d-117">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="9708d-118">[Crea un'entità servizio usando l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) e acquisire l'output.</span><span class="sxs-lookup"><span data-stu-id="9708d-118">[Create a service principal using the Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) and capture the output.</span></span> <span data-ttu-id="9708d-119">Specificare una [password di protezione](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) nell'argomento password invece di `MY_SECURE_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="9708d-119">Provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span> <span data-ttu-id="9708d-120">La password deve contenere da 8 a 16 caratteri e soddisfare almeno 3 dei 4 criteri seguenti:</span><span class="sxs-lookup"><span data-stu-id="9708d-120">Your password must be 8 to 16 characters and match at least 3 out of the 4 following criteria:</span></span>

* <span data-ttu-id="9708d-121">Includere caratteri minuscoli</span><span class="sxs-lookup"><span data-stu-id="9708d-121">Include lowercase characters</span></span>
* <span data-ttu-id="9708d-122">Includere caratteri maiuscoli</span><span class="sxs-lookup"><span data-stu-id="9708d-122">Include uppercase characters</span></span>
* <span data-ttu-id="9708d-123">Includere numeri</span><span class="sxs-lookup"><span data-stu-id="9708d-123">Include numbers</span></span>
* <span data-ttu-id="9708d-124">Includere uno dei simboli seguenti: @ # $ % ^ & \* - _ !</span><span class="sxs-lookup"><span data-stu-id="9708d-124">Include one of the following symbols: @ # $ % ^ & \* - _ !</span></span> <span data-ttu-id="9708d-125">+ = [ ] { } | \ : ‘ , .</span><span class="sxs-lookup"><span data-stu-id="9708d-125">+ = [ ] { } | \ : ‘ , .</span></span> <span data-ttu-id="9708d-126">?</span><span class="sxs-lookup"><span data-stu-id="9708d-126">?</span></span> <span data-ttu-id="9708d-127">/ \` ~ “ ( ) ;</span><span class="sxs-lookup"><span data-stu-id="9708d-127">/ \` ~ “ ( ) ;</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

<span data-ttu-id="9708d-128">Restituisce una risposta nel formato seguente:</span><span class="sxs-lookup"><span data-stu-id="9708d-128">Which gives you a reply in the following format:</span></span>

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

<span data-ttu-id="9708d-129">Copiare quindi il codice seguente in un file di testo nel sistema:</span><span class="sxs-lookup"><span data-stu-id="9708d-129">Next, copy the following into a text file on your system:</span></span>

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

<span data-ttu-id="9708d-130">Sostituire i primi quattro valori con i seguenti:</span><span class="sxs-lookup"><span data-stu-id="9708d-130">Replace the top four values with the following:</span></span>

- <span data-ttu-id="9708d-131">subscription: usare il valore *id* da `az account show` nell'interfaccia della riga di comando di Azure 2.0.</span><span class="sxs-lookup"><span data-stu-id="9708d-131">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="9708d-132">client: usare il valore *appId* dell'output di un'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="9708d-132">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="9708d-133">key: usare il valore *password* dell'output dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="9708d-133">key: use the *password* value from the service principal output.</span></span>
- <span data-ttu-id="9708d-134">tenant: usare il valore *tenant* dell'output dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="9708d-134">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="9708d-135">Salvare questo file nel sistema in una posizione sicura e leggibile dal codice.</span><span class="sxs-lookup"><span data-stu-id="9708d-135">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="9708d-136">È possibile usare questo file per il codice futuro, quindi è consigliabile archiviarlo in una posizione esterna rispetto all'applicazione descritta in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9708d-136">You may use this file for future code so it's recommended to store it somewhere external to the application in this article.</span></span>

<span data-ttu-id="9708d-137">Impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file di autenticazione nella shell.</span><span class="sxs-lookup"><span data-stu-id="9708d-137">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>   

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="9708d-138">Se si lavora in ambiente Windows, aggiungere la variabile alle proprietà del sistema.</span><span class="sxs-lookup"><span data-stu-id="9708d-138">If you're working in a windows environment, add the variable to your system properties.</span></span> <span data-ttu-id="9708d-139">Aprire una finestra di PowerShell con privilegi di amministratore, sostituire la seconda variabile con il percorso del file e quindi immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9708d-139">Open a PowerShell window with administrator privledges and, after replacing the second variable with the path to your file, enter the following command:</span></span>

```powershell
setx AZURE_AUTH_LOCATION "C:\<fullpath>\azureauth.properties" /m
```

## <a name="tooling"></a><span data-ttu-id="9708d-140">Strumenti</span><span class="sxs-lookup"><span data-stu-id="9708d-140">Tooling</span></span>

### <a name="create-a-new-maven-project"></a><span data-ttu-id="9708d-141">Creare un nuovo progetto Maven</span><span class="sxs-lookup"><span data-stu-id="9708d-141">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="9708d-142">Questa guida usa lo strumento di compilazione Maven per compilare ed eseguire il codice di esempio, ma con le librerie di Azure per Java si possono usare anche altri strumenti di compilazione, ad esempio Gradle.</span><span class="sxs-lookup"><span data-stu-id="9708d-142">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="9708d-143">Creare un progetto Maven dalla riga di comando in una nuova directory del sistema:</span><span class="sxs-lookup"><span data-stu-id="9708d-143">Create a Maven project from the command line in a new directory on your system:</span></span>

```
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp  \ 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

<span data-ttu-id="9708d-144">Viene creato un progetto Maven di base nella cartella `testAzureApp`.</span><span class="sxs-lookup"><span data-stu-id="9708d-144">This creates a basic Maven project under the `testAzureApp` folder.</span></span> <span data-ttu-id="9708d-145">Aggiungere le voci seguenti nel progetto `pom.xml` per importare le librerie usate nel codice di esempio in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9708d-145">Add the following entries into the project `pom.xml` to import the libraries used in the sample code in this tutorial.</span></span>

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

<span data-ttu-id="9708d-146">Aggiungere una voce `build` sotto l'elemento `project` di primo livello per usare [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) per eseguire gli esempi:</span><span class="sxs-lookup"><span data-stu-id="9708d-146">Add a `build` entry under the top-level `project` element to use the [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) to run the samples:</span></span>

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

### <a name="install-the-azure-toolkit-for-intellij"></a><span data-ttu-id="9708d-147">Installare Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="9708d-147">Install the Azure Toolkit for Intellij</span></span>

<span data-ttu-id="9708d-148">[Azure Toolkit](intellij/azure-toolkit-for-intellij-installation.md) è necessario se si prevede di distribuire app Web o API a livello di codice, ma non viene attualmente usato per altri tipi di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9708d-148">The [Azure toolkit](intellij/azure-toolkit-for-intellij-installation.md) is necessary if you're going to be deploying web apps or APIs programmatically but is not currently used for any other kinds of development.</span></span> <span data-ttu-id="9708d-149">Di seguito è riportato un riepilogo della procedura di installazione.</span><span class="sxs-lookup"><span data-stu-id="9708d-149">The following is a summary of the installation process.</span></span> <span data-ttu-id="9708d-150">Per i passaggi dettagliati, vedere [Installazione di Azure Toolkit for IntelliJ](intellij/azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="9708d-150">For detailed stpes, visit [Installing the Azure Toolkit for IntelliJ](intellij/azure-toolkit-for-intellij-installation.md).</span></span>

<span data-ttu-id="9708d-151">Selezionare il menu **File** e quindi selezionare **Settings** (Impostazioni).</span><span class="sxs-lookup"><span data-stu-id="9708d-151">Select the **File** menu and then select **Settings...**.</span></span> 

<span data-ttu-id="9708d-152">Selezionare **Browse repositories** (Sfoglia repository), quindi cercare "Azure" e installare **Azure toolkit for Intellij**.</span><span class="sxs-lookup"><span data-stu-id="9708d-152">Select **Browse repositories...** and then search "Azure" and install the **Azure toolkit for Intellij**.</span></span>

<span data-ttu-id="9708d-153">Riavviare IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="9708d-153">Restart Intellij.</span></span>

### <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="9708d-154">Installare Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="9708d-154">Install the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="9708d-155">[Azure Toolkit](eclipse/azure-toolkit-for-eclipse.md) è necessario se si prevede di distribuire app Web o API a livello di codice, ma non viene attualmente usato per altri tipi di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9708d-155">The [Azure toolkit](eclipse/azure-toolkit-for-eclipse.md) is necessary if you're going to be deploying web apps or APIs programmatically but is not currently used for any other kinds of development.</span></span> <span data-ttu-id="9708d-156">Di seguito è riportato un riepilogo della procedura di installazione.</span><span class="sxs-lookup"><span data-stu-id="9708d-156">The following is a summary of the installation process.</span></span> <span data-ttu-id="9708d-157">Per i passaggi dettagliati, vedere [Installazione di Azure Toolkit for Eclipse](eclipse/azure-toolkit-for-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="9708d-157">For detailed stpes, visit [Installing the Azure Toolkit for Eclipse](eclipse/azure-toolkit-for-eclipse.md).</span></span>

<span data-ttu-id="9708d-158">Dal menu **Help** (?) scegliere **Install New Software** (Installa nuovo software).</span><span class="sxs-lookup"><span data-stu-id="9708d-158">Select the **Help** menu and then select **Install New software**.</span></span>

<span data-ttu-id="9708d-159">Nel campo **Usa:** immettere `http://dl.microsoft.com/eclipse` e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="9708d-159">In the **Work with:** field enter `http://dl.microsoft.com/eclipse` and press enter.</span></span>

<span data-ttu-id="9708d-160">Selezionare quindi la casella di controllo accanto ad **Azure toolkit for Java** e deselezionare la casella di controllo **Contact all update sites during install to find required software** (Contatta tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto).</span><span class="sxs-lookup"><span data-stu-id="9708d-160">Then, select the checkbox next to **Azure toolkit for Java** and uncheck the checkbox for **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="9708d-161">Selezionare Next (Avanti).</span><span class="sxs-lookup"><span data-stu-id="9708d-161">Then select next.</span></span>

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="9708d-162">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="9708d-162">Create a Linux virtual machine</span></span>

<span data-ttu-id="9708d-163">Creare un nuovo file denominato `AzureApp.java` nella directory `src/main/java/com/fabirkam` del progetto e incollare il blocco di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9708d-163">Create a new file named `AzureApp.java` in the project's `src/main/java/com/fabirkam` directory and paste in the following block of code.</span></span> <span data-ttu-id="9708d-164">Aggiornare le variabili `userName` e `sshKey` con i valori reali del computer in uso.</span><span class="sxs-lookup"><span data-stu-id="9708d-164">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="9708d-165">Questo codice crea una nuova VM Linux denominata `testLinuxVM` in un gruppo di risorse `sampleResourceGroup` in esecuzione nell'area di Azure Stati Uniti orientali.</span><span class="sxs-lookup"><span data-stu-id="9708d-165">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

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

<span data-ttu-id="9708d-166">Eseguire l'esempio dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9708d-166">Run the sample from the command line:</span></span>

```
mvn compile exec:java
```

<span data-ttu-id="9708d-167">Verranno visualizzate alcune richieste e risposte REST nella console perché l'SDK esegue le chiamate sottostanti all'API REST di Azure per configurare la macchina virtuale e le risorse.</span><span class="sxs-lookup"><span data-stu-id="9708d-167">You'll see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="9708d-168">Al termine del programma, verificare la macchina virtuale nella sottoscrizione con l'interfaccia della riga di comando di Azure 2.0:</span><span class="sxs-lookup"><span data-stu-id="9708d-168">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="9708d-169">Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare la VM e le risorse.</span><span class="sxs-lookup"><span data-stu-id="9708d-169">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="9708d-170">Distribuire un'app Web da un repository di GitHub</span><span class="sxs-lookup"><span data-stu-id="9708d-170">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="9708d-171">Sostituire il metodo principale in `AzureApp.java` con quello seguente, aggiornando la variabile `appName` a un valore univoco prima di eseguire il codice.</span><span class="sxs-lookup"><span data-stu-id="9708d-171">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="9708d-172">Questo codice sviluppa un'applicazione Web dal ramo `master` di un repository di GitHub pubblico in una nuova [app Web del servizio app di Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) in esecuzione nel piano tariffario gratuito.</span><span class="sxs-lookup"><span data-stu-id="9708d-172">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

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

<span data-ttu-id="9708d-173">Eseguire il codice come prima usando Maven:</span><span class="sxs-lookup"><span data-stu-id="9708d-173">Run the code as before using Maven:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="9708d-174">Aprire un browser che punta all'applicazione usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9708d-174">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

<span data-ttu-id="9708d-175">Rimuovere l'app Web e il piano dalla sottoscrizione dopo avere verificato la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="9708d-175">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a><span data-ttu-id="9708d-176">Connettersi a un database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="9708d-176">Connect to an Azure SQL database</span></span>

<span data-ttu-id="9708d-177">Sostituire il metodo principale corrente in `AzureApp.java` con il codice seguente, impostando un valore reale per la variabile `dbPassword`.</span><span class="sxs-lookup"><span data-stu-id="9708d-177">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="9708d-178">Questo codice crea un nuovo database SQL con una regola del firewall che consente l'accesso remoto e quindi vi si connette usando il driver JBDC del database SQL.</span><span class="sxs-lookup"><span data-stu-id="9708d-178">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

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
<span data-ttu-id="9708d-179">Eseguire l'esempio dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9708d-179">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="9708d-180">Pulire quindi le risorse usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9708d-180">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="9708d-181">Scrivere un BLOB in un nuovo account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="9708d-181">Write a blob into a new storage account</span></span>

<span data-ttu-id="9708d-182">Sostituire il metodo principale corrente in `AzureApp.java` con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9708d-182">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="9708d-183">Questo codice crea un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e quindi usa le librerie di archiviazione di Azure per Java per creare un nuovo file di testo nel cloud.</span><span class="sxs-lookup"><span data-stu-id="9708d-183">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

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

<span data-ttu-id="9708d-184">Eseguire l'esempio dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9708d-184">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="9708d-185">È possibile cercare il file `helloazure.txt` nell'account di archiviazione tramite il portale di Azure o con [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span><span class="sxs-lookup"><span data-stu-id="9708d-185">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="9708d-186">Pulire l'account di archiviazione usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9708d-186">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="9708d-187">Esplorare altri esempi</span><span class="sxs-lookup"><span data-stu-id="9708d-187">Explore more samples</span></span>

<span data-ttu-id="9708d-188">Per altre informazioni su come usare le librerie di gestione di Azure per Java per gestire le risorse e l'automazione delle attività, vedere il codice di esempio per [macchine virtuali](java-sdk-azure-virtual-machine-samples.md), [app Web](java-sdk-azure-web-apps-samples.md) e [database SQL](java-sdk-azure-sql-database-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9708d-188">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](java-sdk-azure-virtual-machine-samples.md), [web apps](java-sdk-azure-web-apps-samples.md) and [SQL database](java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="9708d-189">Informazioni di riferimento e note sulla versione</span><span class="sxs-lookup"><span data-stu-id="9708d-189">Reference and release notes</span></span>

<span data-ttu-id="9708d-190">Le [informazioni di riferimento](http://docs.microsoft.com/java/api) sono disponibili per tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="9708d-190">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="9708d-191">Ottenere supporto e inviare commenti</span><span class="sxs-lookup"><span data-stu-id="9708d-191">Get help and give feedback</span></span>

<span data-ttu-id="9708d-192">Pubblicare le domande per la community in [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span><span class="sxs-lookup"><span data-stu-id="9708d-192">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="9708d-193">Segnalare bug e problemi in sospeso relativi alle librerie di Azure per Java nel [progetto GitHub](https://github.com/Azure/azure-sdk-for-java).</span><span class="sxs-lookup"><span data-stu-id="9708d-193">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>
