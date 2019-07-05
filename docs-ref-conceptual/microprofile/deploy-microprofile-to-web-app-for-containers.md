---
title: Distribuire un servizio MicroProfile basato su Java in app Web per contenitori di Azure
description: Informazioni su come distribuire un servizio MicroProfile usando Docker e app Web per contenitori di Azure
services: container-registry;app-service
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: container-registry;app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 4ecfbf92bc30bc491c991e30111cce8375da7f1a
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533596"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="6591e-103">Distribuire un servizio MicroProfile basato su Java in app Web per contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="6591e-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="6591e-104">MicroProfile è una soluzione ottimale per compilare applicazioni Java estremamente piccole che è possibile distribuire in modo semplice e rapido in servizi come l'[app Web per contenitori di Azure](https://azure.microsoft.com/services/app-service/containers/).</span><span class="sxs-lookup"><span data-stu-id="6591e-104">MicroProfile is a great way to build exceedingly small Java applications that you can quickly and easily deploy to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="6591e-105">In questa esercitazione viene creato un semplice microservizio basato su MicroProfile che viene eseguito in un contenitore Docker, distribuito in un'istanza di [Registro Azure Container](https://azure.microsoft.com/services/container-registry/) e quindi ospitato con l'app Web per contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="6591e-105">In this tutorial, you create a simple, MicroProfile-based microservice that you containerize into a Docker container, deploy to an [Azure container registry instance](https://azure.microsoft.com/services/container-registry/), and then host by using Azure Web App for Containers.</span></span>

> [!NOTE]
> <span data-ttu-id="6591e-106">Questa procedura funziona con qualsiasi implementazione di MicroProfile.io purché l'immagine del contenitore Docker sia autoeseguibile, ovvero includa il runtime.</span><span class="sxs-lookup"><span data-stu-id="6591e-106">This procedure works with any implementation of MicroProfile.io, as long the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

<span data-ttu-id="6591e-107">In questo esempio si usano [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile 1.3](https://microprofile.io/) per creare un file WAR (Web Application Requirement) Java di piccole dimensioni (all'incirca 5.000 byte) e quindi aggiungerlo a un pacchetto in un'immagine Docker di circa 174 MB.</span><span class="sxs-lookup"><span data-stu-id="6591e-107">In this sample, you use [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a small (approximately 5,000 bytes) Java web application requirement (WAR) file, and then package it into a Docker image of approximately 174 megabytes (MB).</span></span> <span data-ttu-id="6591e-108">L'immagine Docker contiene tutto il necessario per una distribuzione in contenitori dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="6591e-108">The Docker image contains everything necessary for a fully containerized deployment of the web app.</span></span>

<span data-ttu-id="6591e-109">Non è necessario distribuire di nuovo l'intera immagine Docker da 174 MB ogni volta che si modifica il codice sorgente dell'applicazione perché Docker carica solo le differenze, che hanno dimensioni nettamente inferiori.</span><span class="sxs-lookup"><span data-stu-id="6591e-109">An entire 174 MB Docker image doesn't need to be redeployed whenever the application source code is changed, because Docker uploads only the differences (which are significantly smaller).</span></span> <span data-ttu-id="6591e-110">Di conseguenza, l'esecuzione di una nuova versione di un'applicazione MicroProfile tramite una pipeline CI/CD è quindi estremamente efficiente e veloce, con una ridotta possibilità di errore e una rapida iterazione dello sviluppo.</span><span class="sxs-lookup"><span data-stu-id="6591e-110">Consequently, the process of executing a new release of a MicroProfile application via a CI/CD pipeline is extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="6591e-111">In questa esercitazione si inizierà con la creazione e l'esecuzione del codice in locale prima di distribuire il codice come app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="6591e-111">You'll work through this tutorial by first creating and running the code locally and then deploying it as a web app on Azure.</span></span> <span data-ttu-id="6591e-112">In entrambe le fasi è possibile usare Docker per semplificare e standardizzare queste attività.</span><span class="sxs-lookup"><span data-stu-id="6591e-112">In both phases, you can depend on Docker to simplify and standardize your efforts.</span></span> <span data-ttu-id="6591e-113">Prima di iniziare, verrà creata un'istanza di Registro Azure Container nella quale archiviare i contenitori Docker.</span><span class="sxs-lookup"><span data-stu-id="6591e-113">Before you begin, you'll create an Azure container registry instance for storing your Docker containers.</span></span>

## <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="6591e-114">Creare un'istanza di Registro Azure Container</span><span class="sxs-lookup"><span data-stu-id="6591e-114">Create an Azure container registry instance</span></span>

<span data-ttu-id="6591e-115">Oltre al [portale di Azure](http://portal.azure.com) è possibile usare altri metodi per creare l'istanza di Registro Azure Container, ad esempio l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="6591e-115">You can use the [Azure portal](http://portal.azure.com) to create the Azure container registry instance, but there are other choices also, such as the Azure CLI.</span></span> <span data-ttu-id="6591e-116">Per creare una nuova istanza di Registro Azure Container, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="6591e-116">To create a new Azure container registry instance, do the following:</span></span>

1. <span data-ttu-id="6591e-117">Accedere al [portale di Azure](http://portal.azure.com) e creare una nuova risorsa di Registro Azure Container.</span><span class="sxs-lookup"><span data-stu-id="6591e-117">Sign in to the [Azure portal](http://portal.azure.com) and create a new Azure container registry resource.</span></span> <span data-ttu-id="6591e-118">Specificare un nome per il registro. Questo nome deve essere impostato come proprietà `docker.registry` nel file *pom.xml*.</span><span class="sxs-lookup"><span data-stu-id="6591e-118">Provide a registry name (this name should be set as the `docker.registry` property in the *pom.xml* file).</span></span> <span data-ttu-id="6591e-119">Modificare le impostazioni predefinite secondo le esigenze, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6591e-119">Change the defaults as you want, and then select **Create**.</span></span>

1. <span data-ttu-id="6591e-120">Dopo circa 30 secondi, quando l'istanza di Registro Container diventa disponibile, selezionare l'istanza e quindi, nel riquadro sinistro, selezionare il collegamento **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="6591e-120">In about 30 seconds, when the container registry instance is live, select the container registry instance and then, in the left pane, select the **Access keys** link.</span></span> 

    <span data-ttu-id="6591e-121">Assicurarsi di abilitare l'impostazione *Utente amministratore* in modo da poter accedere a questa istanza di Registro Container dai computer ed eseguirvi il push dei contenitori Docker,</span><span class="sxs-lookup"><span data-stu-id="6591e-121">Be sure to enable the *admin user* setting, so that you can access this container registry instance from your machines and push Docker containers into it.</span></span> <span data-ttu-id="6591e-122">nonché per consentire l'accesso dall'istanza dell'app Web per contenitori di Azure che verrà configurata più avanti.</span><span class="sxs-lookup"><span data-stu-id="6591e-122">Doing so also enables access from the Azure Web App for Containers instance that you'll set up soon.</span></span>

1. <span data-ttu-id="6591e-123">Nel riquadro **Chiavi di accesso** copiare i valori di **nome utente** e **password** e incollarli nel file *settings.xml* globale di Maven.</span><span class="sxs-lookup"><span data-stu-id="6591e-123">In the **Access keys** pane, copy the **username** and **password** values and paste them into your global Maven *settings.xml* file.</span></span> <span data-ttu-id="6591e-124">Per altre informazioni sulle impostazioni Maven, visitare il sito Web del [progetto Apache Maven](https://maven.apache.org/settings.html).</span><span class="sxs-lookup"><span data-stu-id="6591e-124">For more information about Maven settings, go to the [Apache Maven Project](https://maven.apache.org/settings.html) website.</span></span> 

   <span data-ttu-id="6591e-125">Come riferimento, ecco un esempio di file *${user.home}/.m2/settings.xml*:</span><span class="sxs-lookup"><span data-stu-id="6591e-125">For your reference, here's an example of the *${user.home}/.m2/settings.xml* file:</span></span>

    ```xml
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
          <server>
            <id>jogilescr.azurecr.io</id>
            <username>jogilescr</username>
            <password>ojoirshois.this-isn't-real.hrihslirhlishrglih</password>
          </server>
        </servers>
    </settings>
    ```

<span data-ttu-id="6591e-126">A questo punto, dopo aver creato l'istanza di Registro Container, si può passare alla compilazione e all'esecuzione dell'applicazione MicroProfile in locale.</span><span class="sxs-lookup"><span data-stu-id="6591e-126">Now that you've created your container registry instance, you can move on to building and running your MicroProfile application locally.</span></span>

## <a name="create-your-microprofile-application"></a><span data-ttu-id="6591e-127">Creare l'applicazione MicroProfile</span><span class="sxs-lookup"><span data-stu-id="6591e-127">Create your MicroProfile application</span></span>

<span data-ttu-id="6591e-128">Questo esempio di codice si basa su un'applicazione di esempio disponibile in GitHub.</span><span class="sxs-lookup"><span data-stu-id="6591e-128">This example code is based on a sample application that's available on GitHub.</span></span> <span data-ttu-id="6591e-129">Per clonare il file nel computer, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="6591e-129">To clone the code onto your machine, enter the following commands:</span></span>

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

<span data-ttu-id="6591e-130">In questa directory è presente un file *pom.xml* che viene usato per specificare il progetto nel formato usato dallo strumento di compilazione Maven.</span><span class="sxs-lookup"><span data-stu-id="6591e-130">This directory contains a *pom.xml* file that you use to specify the project in the format that's used by the Maven build tool.</span></span> <span data-ttu-id="6591e-131">Questo file può essere modificato in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="6591e-131">You can edit the file to suit your own needs.</span></span> <span data-ttu-id="6591e-132">In particolare, i valori delle proprietà `docker.registry` e `docker.name` devono essere sostituiti con i valori delle proprietà `docker.registry` e `docker.name` create durante la configurazione dell'istanza di Registro Azure Container.</span><span class="sxs-lookup"><span data-stu-id="6591e-132">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` properties that were created when you set up the Azure container registry instance.</span></span>

<span data-ttu-id="6591e-133">Un altro file importante presente in questa directory è il documento Dockerfile, riprodotto di seguito:</span><span class="sxs-lookup"><span data-stu-id="6591e-133">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="6591e-134">Il documento Dockerfile crea un nuovo contenitore Docker basato sul contenitore Docker Payara Micro</span><span class="sxs-lookup"><span data-stu-id="6591e-134">The Dockerfile creates a new Docker container that's based on the Payara Micro Docker Container.</span></span> <span data-ttu-id="6591e-135">e ne esegue la copia nel file WAR creato durante il processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="6591e-135">It copies in the WAR file that was created as part of your build process.</span></span> <span data-ttu-id="6591e-136">Espone inoltre la porta 8080 in modo che il servizio sia accessibile quando è in esecuzione in un contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="6591e-136">It also exposes port 8080 so that you can access the service after it's up and running within a Docker container.</span></span>

<span data-ttu-id="6591e-137">Quando si apre la directory *src*, viene individuata la classe `Application` riprodotta di seguito:</span><span class="sxs-lookup"><span data-stu-id="6591e-137">When you open the *src* directory, you'll eventually discover the `Application` class that's reproduced here:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="6591e-138">L'annotazione *@ApplicationPath("/api")* specifica l'endpoint di base per questo microservizio.</span><span class="sxs-lookup"><span data-stu-id="6591e-138">The *@ApplicationPath("/api")* annotation specifies the base endpoint for this microservice.</span></span> <span data-ttu-id="6591e-139">In altre parole, in tutti gli endpoint l'annotazione */api* precede il resto dell'URL necessario per accedere a qualsiasi endpoint REST specifico.</span><span class="sxs-lookup"><span data-stu-id="6591e-139">That is, for all endpoints, */api* precedes the rest of the URL that's required to access any specific REST endpoint.</span></span>

<span data-ttu-id="6591e-140">All'interno del pacchetto *api* è presente una classe denominata `API` che contiene il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6591e-140">Inside the *api* package is a class named `API`, which contains the following code:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld.api;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

import static javax.ws.rs.core.MediaType.TEXT_HTML;

@ApplicationScoped
@Path("/")
public class API {

    @GET
    @Path("/helloworld")
    @Produces(TEXT_HTML)
    public String info() {
        return "Hello, world!";
    }
}
```

<span data-ttu-id="6591e-141">Usando l'annotazione *@Path("/helloworld")* , è possibile vedere che questo endpoint REST, se combinato con l'annotazione */api* specificata nella classe `Application`, sarà */api/helloworld*.</span><span class="sxs-lookup"><span data-stu-id="6591e-141">Through the use of the *@Path("/helloworld")* annotation, you can see that this REST endpoint, when it's combined with the */api* that's specified in the `Application` class, will be */api/helloworld*.</span></span> <span data-ttu-id="6591e-142">Quando si chiama questo endpoint usando una richiesta HTTP GET, il metodo produce testo/HTML e si tratta semplicemente di una stringa "Hello, world!" hardcoded.</span><span class="sxs-lookup"><span data-stu-id="6591e-142">When you call this endpoint by using an HTTP GET request, you can see that the method produces text/html and, in fact, it's simply a hard-coded string, "Hello, world!"</span></span>

<span data-ttu-id="6591e-143">Fino a questo punto, in questo articolo è stato esaminato tutto il codice necessario per creare un microservizio con MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="6591e-143">So far, this article has covered all the code that's required for you to create a microservice by using MicroProfile.</span></span> <span data-ttu-id="6591e-144">Ora è possibile usare Maven per la compilazione, l'inserimento in un contenitore Docker e l'esecuzione nell'ambiente locale, eseguendo le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6591e-144">You can now use Maven to build it, containerize it into a Docker container, and run it locally by doing the following:</span></span>

1. <span data-ttu-id="6591e-145">Eseguire `mvn clean package` e attendere il completamento dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="6591e-145">Run `mvn clean package`, and wait until it is complete.</span></span>

1. <span data-ttu-id="6591e-146">Eseguire `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`.</span><span class="sxs-lookup"><span data-stu-id="6591e-146">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`.</span></span> <span data-ttu-id="6591e-147">Ad esempio, *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*, dove *\<docker.registry>* è *jogilescr.azurecr.io* e *\<docker.name>* è *samples/docker-helloworld*.</span><span class="sxs-lookup"><span data-stu-id="6591e-147">For example, *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*, where *\<docker.registry>* is *jogilescr.azurecr.io* and *\<docker.name>* is *samples/docker-helloworld*.</span></span>

1. <span data-ttu-id="6591e-148">Provare ad accedere a [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) e [http://localhost:8080/health](http://localhost:8080/health) nel Web browser.</span><span class="sxs-lookup"><span data-stu-id="6591e-148">Try to access [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="6591e-149">Se viene visualizzata la risposta "Hello, world!" prevista</span><span class="sxs-lookup"><span data-stu-id="6591e-149">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="6591e-150">(e informazioni relative all'integrità per l'endpoint [/health](http://localhost:8080/health)), l'applicazione MicroProfile è stata distribuita correttamente nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="6591e-150">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you've successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="push-the-container-to-the-azure-container-registry-instance"></a><span data-ttu-id="6591e-151">Eseguire il push del contenitore nell'istanza di Registro Azure Container</span><span class="sxs-lookup"><span data-stu-id="6591e-151">Push the container to the Azure container registry instance</span></span>

<span data-ttu-id="6591e-152">Ora che è stata compilata ed eseguita l'applicazione MicroProfile nel computer locale, eseguire il push di questo contenitore nell'istanza di Registro Container.</span><span class="sxs-lookup"><span data-stu-id="6591e-152">Now that you've successfully built and run your MicroProfile application on your local machine, push this container to your container registry instance.</span></span> 

> [!NOTE]
> <span data-ttu-id="6591e-153">Anche se in questo articolo si usa un'istanza di Registro Azure Container, è possibile usare qualsiasi altra istanza purché il file *pom.xml* venga modificato in modo da puntare al percorso corrispondente.</span><span class="sxs-lookup"><span data-stu-id="6591e-153">Although this article uses an Azure container registry instance, any container registry instance should work, as long as the *pom.xml* file is edited to point to the relevant location.</span></span>

1. <span data-ttu-id="6591e-154">Per pulire, compilare e creare un'immagine Docker locale, eseguire `mvn clean package`.</span><span class="sxs-lookup"><span data-stu-id="6591e-154">To clean, compile, and create a local Docker image, run `mvn clean package`.</span></span>
2. <span data-ttu-id="6591e-155">Per eseguire il push del contenitore nell'istanza di Registro Azure Container, eseguire `mvn dockerfile:push`.</span><span class="sxs-lookup"><span data-stu-id="6591e-155">To push the container to the Azure container registry instance, run `mvn dockerfile:push`.</span></span>

<span data-ttu-id="6591e-156">L'immagine del contenitore Docker verrà ora caricata nell'istanza di Registro Azure Container,</span><span class="sxs-lookup"><span data-stu-id="6591e-156">You now have your Docker container image uploaded to the Azure container registry instance.</span></span> <span data-ttu-id="6591e-157">ma non è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6591e-157">However, it's not yet running.</span></span> <span data-ttu-id="6591e-158">È necessario distribuirla in un'istanza dell'app Web per contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="6591e-158">You now have to deploy it to an Azure Web App for Containers instance.</span></span> 

## <a name="create-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="6591e-159">Creare un'istanza dell'app Web per contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="6591e-159">Create an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="6591e-160">Nel [portale di Azure](http://portal.azure.com), selezionare **Web e dispositivi mobili** nel riquadro sinistro e quindi eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6591e-160">In the [Azure portal](http://portal.azure.com), in the left pane, select **Web + Mobile**, and then do the following:</span></span>

   <span data-ttu-id="6591e-161">a.</span><span class="sxs-lookup"><span data-stu-id="6591e-161">a.</span></span> <span data-ttu-id="6591e-162">Specificare un nome.</span><span class="sxs-lookup"><span data-stu-id="6591e-162">Specify a name.</span></span> <span data-ttu-id="6591e-163">Il nome diventerà l'URL pubblico dell'app Web, quindi è opportuno scegliere un nome facile da ricordare.</span><span class="sxs-lookup"><span data-stu-id="6591e-163">The name will become the public URL of the web app, so it's a good idea to pick a name that you can easily remember.</span></span> <span data-ttu-id="6591e-164">Se si preferisce, è possibile aggiungere in seguito un nome di dominio personalizzato.</span><span class="sxs-lookup"><span data-stu-id="6591e-164">You can add a custom domain name later, if you want.</span></span>

   <span data-ttu-id="6591e-165">b.</span><span class="sxs-lookup"><span data-stu-id="6591e-165">b.</span></span> <span data-ttu-id="6591e-166">Nella sezione **Configura contenitore** selezionare **Registro Azure Container** in **Origine immagine** e quindi, nell'elenco a discesa, selezionare l'immagine corretta.</span><span class="sxs-lookup"><span data-stu-id="6591e-166">In the **Configure container** section, under **Image source**, select **Azure Container Registry** and then, in the drop-down list, select the correct image.</span></span>

   <span data-ttu-id="6591e-167">c.</span><span class="sxs-lookup"><span data-stu-id="6591e-167">c.</span></span> <span data-ttu-id="6591e-168">Non è necessario specificare alcun valore nel campo **File di avvio**.</span><span class="sxs-lookup"><span data-stu-id="6591e-168">You don't need to specify a value in the **Startup File** field.</span></span>

1. <span data-ttu-id="6591e-169">Dopo aver creato l'istanza selezionarla e quindi scegliere **Impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="6591e-169">After you've created the instance, select it, and then select **Application Settings**.</span></span> <span data-ttu-id="6591e-170">Come valore di **Chiave** immettere **WEBSITES_PORT** e come valore di **Valore** immettere **8080**.</span><span class="sxs-lookup"><span data-stu-id="6591e-170">For **Key**, enter **WEBSITES_PORT**, and for **Value**, enter **8080**.</span></span> <span data-ttu-id="6591e-171">Queste impostazioni indicano ad Azure la porta da esporre nel contenitore, nonché il numero di porta (80) per il mapping esterno.</span><span class="sxs-lookup"><span data-stu-id="6591e-171">These settings tell Azure which port to expose in the container and to map it to port 80 externally.</span></span>

1. <span data-ttu-id="6591e-172">(Facoltativo) Selezionare il collegamento **Contenitore Docker** e quindi abilitare **Distribuzione continua**.</span><span class="sxs-lookup"><span data-stu-id="6591e-172">(Optional) Select the **Docker Container** link, and then enable **Continuous Deployment**.</span></span> <span data-ttu-id="6591e-173">In questo modo ogni volta che si aggiorna l'immagine dell'istanza di Registro Azure Container, questa viene aggiornata automaticamente nell'istanza dell'app Web per contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="6591e-173">By doing so, whenever you update the Azure container registry instance image, it's automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="6591e-174">A questo punto dovrebbe essere possibile accedere alle istanze ospitate in Azure agli indirizzi `http://<appname>.azurewebsites.net/microprofile/api/helloworld` e `http://<appname>.azurewebsites.net/health`.</span><span class="sxs-lookup"><span data-stu-id="6591e-174">You should now be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="6591e-175">Summary</span><span class="sxs-lookup"><span data-stu-id="6591e-175">Summary</span></span>

<span data-ttu-id="6591e-176">In questa esercitazione è stato esaminato il processo di creazione di un semplice microservizio basato su MicroProfile, successivamente inserito in un contenitore Docker e quindi eseguito nell'ambiente locale e pubblicato in Azure.</span><span class="sxs-lookup"><span data-stu-id="6591e-176">In this tutorial, you've stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, run it locally, and published it to Azure.</span></span> <span data-ttu-id="6591e-177">È possibile estendere il microservizio per offrire altre funzionalità utili.</span><span class="sxs-lookup"><span data-stu-id="6591e-177">You can extend your microservice to provide additional useful functionality.</span></span> <span data-ttu-id="6591e-178">Per altre informazioni, vedere [MicroProfile.io](https://microprofile.io/).</span><span class="sxs-lookup"><span data-stu-id="6591e-178">For more information, go to [MicroProfile.io](https://microprofile.io/).</span></span>
