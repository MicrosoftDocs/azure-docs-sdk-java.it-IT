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
ms.openlocfilehash: 1eb0e7d7a718a1c106adebbf89011f6e3fa1504e
ms.sourcegitcommit: c2019ba6da6c7c28b17b5a85f89e49bb5e570ba4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040249"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="2511b-103">Distribuire un servizio MicroProfile basato su Java in app Web per contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="2511b-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="2511b-104">MicroProfile è un ottimo modo per compilare applicazioni Java estremamente piccole che possono essere rapidamente e facilmente distribuite in servizi come [app Web per contenitori di Azure](https://azure.microsoft.com/services/app-service/containers/).</span><span class="sxs-lookup"><span data-stu-id="2511b-104">MicroProfile is a great way to build exceedingly tiny Java applications that can be quickly and easily deployed to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="2511b-105">In questa esercitazione verrà creato un semplice microservizio basato su MicroProfile che verrà eseguito in un contenitore Docker, distribuito in un [Registro contenitori di Azure](https://azure.microsoft.com/services/container-registry/) e quindi ospitato con app Web per contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-105">In this tutorial we will create a simple MicroProfile-based microservice that is then containerized into a Docker container, deployed into an [Azure Container Registry](https://azure.microsoft.com/services/container-registry/), and then hosted using Azure Web App for Containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="2511b-106">Questa procedura funziona con qualsiasi implementazione di MicroProfile.io finché l'immagine del contenitore Docker è autoeseguibile, ovvero include il runtime.</span><span class="sxs-lookup"><span data-stu-id="2511b-106">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

<span data-ttu-id="2511b-107">Più concretamente, questo esempio usa [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile 1.3](https://microprofile.io/) per creare un piccolo file WAR Java (5.085 byte nel computer degli autori) e impacchettarlo in un'immagine Docker di circa 174 MB.</span><span class="sxs-lookup"><span data-stu-id="2511b-107">More concretely, this sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a tiny Java war file (5,085 bytes on the authors machine), and then packages it up into a Docker image (which is approximately 174 megabytes).</span></span> <span data-ttu-id="2511b-108">Questa immagine Docker contiene tutto il necessario per una distribuzione in contenitori di questa app Web.</span><span class="sxs-lookup"><span data-stu-id="2511b-108">This Docker image contains everything necessary for a fully-containerised deployment of this webapp.</span></span>

<span data-ttu-id="2511b-109">Data la modalità di funzionamento di Docker, spesso non è necessario distribuire di nuovo l'intera immagine Docker da 174 MB ogni volta che si modifica il codice sorgente dell'applicazione. Docker caricherà infatti solo le differenze, che hanno dimensioni nettamente inferiori.</span><span class="sxs-lookup"><span data-stu-id="2511b-109">Because of the way Docker works, it is often the case that the entire 174 megabyte Docker image does not need to be redeployed whenever the application source code is changed, as Docker will only upload the differences (which is significantly smaller).</span></span> <span data-ttu-id="2511b-110">L'esecuzione di una nuova versione di un'applicazione MicroProfile tramite una pipeline di integrazione continua/distribuzione continua è quindi estremamente efficiente e veloce, con una ridotta possibilità di errore e una rapida iterazione dello sviluppo.</span><span class="sxs-lookup"><span data-stu-id="2511b-110">This makes the process of executing a new release of a MicroProfile application via a CI/CD pipeline extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="2511b-111">Questa esercitazione inizierà con la creazione e l'esecuzione del codice in locale, quindi il codice verrà distribuito come app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-111">We will work through this tutorial firstly by creating and running the code locally, and then we will deploy this as a web app on Azure.</span></span> <span data-ttu-id="2511b-112">In entrambi i casi, Docker semplificherà e standardizzerà queste attività.</span><span class="sxs-lookup"><span data-stu-id="2511b-112">In both cases we will depend on Docker to simplify and standardize our efforts.</span></span> <span data-ttu-id="2511b-113">Prima di iniziare, verrà creato un Registro contenitori di Azure nel quale archiviare i contenitori Docker.</span><span class="sxs-lookup"><span data-stu-id="2511b-113">Before we begin, we will create an Azure Container Registry to store our Docker containers in.</span></span>

## <a name="creating-an-azure-container-registry"></a><span data-ttu-id="2511b-114">Creazione di un Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="2511b-114">Creating an Azure Container Registry</span></span>

<span data-ttu-id="2511b-115">Il Registro contenitori di Azure verrà creato usando il [portale di Azure](http://portal.azure.com), ma sono disponibili alternative come l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-115">We will use the [Azure Portal](http://portal.azure.com) for creating the Azure Container Registry, but note that there are alternate choices such as the Azure CLI.</span></span> <span data-ttu-id="2511b-116">Seguire questa procedura per creare un nuovo Registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="2511b-116">Follow the steps below to create a new Azure Container Registry:</span></span>

1. <span data-ttu-id="2511b-117">Accedere al [portale di Azure](http://portal.azure.com) e creare una nuova risorsa Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-117">Log in to the [Azure Portal](http://portal.azure.com) and create a new Azure Container Registry resource.</span></span> <span data-ttu-id="2511b-118">Specificare un nome per il registro. Questo nome dovrà essere impostato come proprietà `docker.registry` in `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="2511b-118">Provide a registry name (note that this is the name that should be set as the `docker.registry` property in `pom.xml`).</span></span> <span data-ttu-id="2511b-119">Modificare le impostazioni predefinite secondo le esigenze, quindi fare clic su "Crea".</span><span class="sxs-lookup"><span data-stu-id="2511b-119">Change the defaults as you wish, and then click 'create'.</span></span>

1. <span data-ttu-id="2511b-120">Quando il registro contenitori diventa disponibile (circa 30 secondi dopo aver fatto clic su "Crea"), fare clic sul registro contenitori, quindi sul collegamento "Chiavi di accesso" nell'area dei menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="2511b-120">Once the container registry is live (which is about 30 seconds after clicking 'create'), click on the container registry, and click on the 'Access keys' link in the left-menu area.</span></span> <span data-ttu-id="2511b-121">Qui è necessario abilitare l'impostazione "Utente amministratore" per rendere il registro contenitori accessibile dai computer (per il push dei contenitori Docker) e per consentire l'accesso dall'istanza di app Web per contenitori di Azure che verrà configurata più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2511b-121">In here, you need to enable the 'admin user' setting, so that this container registry can be accessed from our machines (to push docker containers into), and also to enable access from the Azure Web Apps for Containers instance we will setup soon.</span></span>

1. <span data-ttu-id="2511b-122">Nell'area "Chiavi di accesso" notare i valori `username` e `password`</span><span class="sxs-lookup"><span data-stu-id="2511b-122">Whilst you are in the 'Access keys' area, note the `username` and `password` values.</span></span> <span data-ttu-id="2511b-123">che verranno copiati e incollati nel file Maven globale `settings.xml`. Per altre informazioni sulle impostazioni Maven, vedere il sito Web del [progetto Apache Maven](https://maven.apache.org/settings.html).</span><span class="sxs-lookup"><span data-stu-id="2511b-123">We will copy / paste these into our global Maven `settings.xml` file  (for more information on Maven settings, refer to the [Apache Maven Project](https://maven.apache.org/settings.html) website).</span></span> <span data-ttu-id="2511b-124">A titolo di riferimento, di seguito è riportata una versione offuscata del file `${user.home}/.m2/settings.xml` presente nel sistema degli autori:</span><span class="sxs-lookup"><span data-stu-id="2511b-124">For reference, here is an obfuscated version of the `${user.home}/.m2/settings.xml` file on the authors system:</span></span>

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

<span data-ttu-id="2511b-125">Terminata l'operazione si può passare alla compilazione e all'esecuzione dell'applicazione MicroProfile nell'ambiente locale.</span><span class="sxs-lookup"><span data-stu-id="2511b-125">Now that this is complete, we can move on with building and running our MicroProfile application locally.</span></span>

## <a name="creating-our-microprofile-application"></a><span data-ttu-id="2511b-126">Creazione dell'applicazione MicroProfile</span><span class="sxs-lookup"><span data-stu-id="2511b-126">Creating our MicroProfile application</span></span>

<span data-ttu-id="2511b-127">Questo esempio si basa su un'applicazione di esempio disponibile in GitHub, che verrà clonata per poi esaminarne il codice.</span><span class="sxs-lookup"><span data-stu-id="2511b-127">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="2511b-128">Seguire questa procedura per clonare il codice nel computer:</span><span class="sxs-lookup"><span data-stu-id="2511b-128">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

<span data-ttu-id="2511b-129">In questa directory è presente un file `pom.xml` che viene usato per specificare il progetto nel formato usato dallo strumento di compilazione Maven.</span><span class="sxs-lookup"><span data-stu-id="2511b-129">In this directory there is a `pom.xml` file that is used to specify the project in the format used by the Maven build tool.</span></span> <span data-ttu-id="2511b-130">Questo file può essere modificato in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="2511b-130">This file can be edited to suit your own needs.</span></span> <span data-ttu-id="2511b-131">In particolare, le proprietà `docker.registry` e `docker.name` devono essere sostituite con i valori `docker.registry` e `docker.name` creati durante la configurazione del Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-131">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` created when the Azure Container Registry was setup.</span></span>

<span data-ttu-id="2511b-132">Un altro file importante presente in questa directory è il documento Dockerfile, riprodotto di seguito:</span><span class="sxs-lookup"><span data-stu-id="2511b-132">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="2511b-133">Questo documento Dockerfile crea semplicemente un nuovo contenitore Docker basato sul contenitore Docker Payara Micro ed esegue la copia nel file WAR creato nell'ambito del processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="2511b-133">This Dockerfile simply creates a new Docker container based on the Payara Micro Docker Container, and copies in the .war file that is created as part of our build process.</span></span> <span data-ttu-id="2511b-134">Espone anche la porta 8080 per rendere il servizio accessibile quando è in esecuzione in un contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="2511b-134">It also exposes port 8080 so that we may access the service once it is up and running within a Docker container.</span></span>

<span data-ttu-id="2511b-135">Nella directory `src` è anche presente la classe `Application` riprodotta di seguito:</span><span class="sxs-lookup"><span data-stu-id="2511b-135">Diving into the `src` directory, we will eventually discover the `Application` class reproduced below:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="2511b-136">L'annotazione `@ApplicationPath("/api")` specifica l'endpoint di base per questo microservizio. In altre parole, in tutti gli endpoint `/api` precederà il resto dell'URL necessario per accedere a qualsiasi endpoint REST specifico.</span><span class="sxs-lookup"><span data-stu-id="2511b-136">The `@ApplicationPath("/api")` annotation specifies the base endpoint for this microservice - that is, that all endpoints will have `/api` preceed the rest of the URL required to access any specific REST endpoint.</span></span>

<span data-ttu-id="2511b-137">All'interno del pacchetto `api` è presente una classe denominata `API` che contiene il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2511b-137">Inside the `api` package is a class named `API`, which contains the following code:</span></span>

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

<span data-ttu-id="2511b-138">Con l'uso dell'annotazione `@Path("/helloworld")` è possibile vedere che questo endpoint REST, se in combinazione con il valore `/api` specificato nella classe `Application`, sarà `/api/helloworld`.</span><span class="sxs-lookup"><span data-stu-id="2511b-138">Through the use of the `@Path("/helloworld")` annotation, we can see that this REST endpoint, when combined with the `/api` specified in the `Application` class, will be `/api/helloworld`.</span></span> <span data-ttu-id="2511b-139">Quando questo endpoint viene chiamato usando una richiesta HTTP GET, il metodo produrrà testo/HTML e si tratta semplicemente di una stringa "Hello, world!" hardcoded.</span><span class="sxs-lookup"><span data-stu-id="2511b-139">When this endpoint is called using an HTTP GET request, we can see that the method will produce text/html, and in fact it is simply a hard-coded string "Hello, world!".</span></span>

<span data-ttu-id="2511b-140">È stato così esaminato tutto il codice necessario per creare un microservizio con MicroProfile.</span><span class="sxs-lookup"><span data-stu-id="2511b-140">We have now covered all the code required to create a microservice using MicroProfile.</span></span> <span data-ttu-id="2511b-141">Ora è possibile usare Maven per la compilazione, l'inserimento in un contenitore Docker e l'esecuzione nell'ambiente locale.</span><span class="sxs-lookup"><span data-stu-id="2511b-141">We can now use Maven to build it, containerize it into a Docker container, and run it locally.</span></span> <span data-ttu-id="2511b-142">A tale scopo seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="2511b-142">We can do that with the following steps:</span></span>

1. <span data-ttu-id="2511b-143">Eseguire `mvn clean package` e attendere il termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="2511b-143">Run `mvn clean package` and wait until it successfully completes.</span></span>

1. <span data-ttu-id="2511b-144">Eseguire `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`, ad esempio `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`, se `docker.registry` è `jogilescr.azurecr.io` e `docker.name` è `samples/docker-helloworld`.</span><span class="sxs-lookup"><span data-stu-id="2511b-144">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`, for example, `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`, if your `docker.registry` is `jogilescr.azurecr.io` and `docker.name` is `samples/docker-helloworld`.</span></span>

1. <span data-ttu-id="2511b-145">Provare ad accedere a [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) e [http://localhost:8080/health](http://localhost:8080/health) nel Web browser.</span><span class="sxs-lookup"><span data-stu-id="2511b-145">Try accessing [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="2511b-146">Se viene visualizzata la risposta "Hello, world!" prevista</span><span class="sxs-lookup"><span data-stu-id="2511b-146">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="2511b-147">(e informazioni relative all'integrità per l'endpoint [/health](http://localhost:8080/health)), l'applicazione MicroProfile è stata distribuita correttamente nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="2511b-147">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you have successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="pushing-to-the-azure-container-registry"></a><span data-ttu-id="2511b-148">Esecuzione del push nel Registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="2511b-148">Pushing to the Azure Container Registry</span></span>

<span data-ttu-id="2511b-149">Ora che è stata compilata ed eseguita l'applicazione MicroProfile nel computer locale, il passo successivo è eseguire il push di questo contenitore nel registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="2511b-149">Now that we have successfully built and run our MicroProfile application on our local machine, the next step is to push this container into our container registry.</span></span> <span data-ttu-id="2511b-150">In questa esercitazione si usa il Registro contenitori di Azure, ma è possibile usare qualsiasi registro contenitori purché il file `pom.xml` venga modificato in modo da puntare al percorso corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2511b-150">In this tutorial we are using the Azure Container Registry, but any container registry will work (as long as the `pom.xml` file is edited to point to the relevant location).</span></span>

1. <span data-ttu-id="2511b-151">Eseguire `mvn clean package` per pulire, compilare e creare un'immagine Docker locale.</span><span class="sxs-lookup"><span data-stu-id="2511b-151">Run `mvn clean package` to clean, compile, and create a local docker image.</span></span>
2. <span data-ttu-id="2511b-152">Eseguire `mvn dockerfile:push` per effettuare il push nel Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-152">Run `mvn dockerfile:push` to push to the Azure Container Registry.</span></span>

<span data-ttu-id="2511b-153">A questo punto, l'immagine del contenitore Docker è stata caricata nel Registro contenitori di Azure, ma non è ancora in esecuzione perché è necessario distribuirla in un'istanza di app Web per contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-153">At this stage you now have your docker container image uploaded to the Azure Container Registry, but it is not yet running as we now have to deploy it into an Azure Web App for Containers instance.</span></span> <span data-ttu-id="2511b-154">Questa operazione verrà eseguita ora.</span><span class="sxs-lookup"><span data-stu-id="2511b-154">We will now do that.</span></span>

## <a name="creating-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="2511b-155">Creazione di un'istanza di app Web per contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="2511b-155">Creating an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="2511b-156">Tornare al [portale di Azure](http://portal.azure.com) e creare una nuova istanza di app Web per contenitori, sotto la voce "Web e dispositivi mobili" nel menu.</span><span class="sxs-lookup"><span data-stu-id="2511b-156">Return to the [Azure Portal](http://portal.azure.com) and create a new Web App for Containers instance (located under the 'Web + Mobile' heading in the menu).</span></span> <span data-ttu-id="2511b-157">Alcune informazioni utili:</span><span class="sxs-lookup"><span data-stu-id="2511b-157">A few pointers:</span></span>

   1. <span data-ttu-id="2511b-158">Il nome specificato qui sarà l'URL pubblico dell'app Web (anche se è possibile aggiungere un dominio personalizzato in seguito), quindi è opportuno scegliere un nome facile da ricordare.</span><span class="sxs-lookup"><span data-stu-id="2511b-158">The name you specify here will be the public URL of the web app (although a custom domain can be added later if desired), so it is a good idea to pick a name that you can easily remember.</span></span>

   1. <span data-ttu-id="2511b-159">Quando si passa alla sezione "Configura contenitore" è possibile selezionare "Registro contenitori di Azure" per "Origine immagine", quindi selezionare l'immagine corretta dagli elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="2511b-159">When you get to the 'Configure container' section, you can select 'Azure Container Registry' for the 'Image source', and then select the correct image from the drop-down lists.</span></span>

   1. <span data-ttu-id="2511b-160">Non è necessario specificare alcun valore nel campo "File di avvio".</span><span class="sxs-lookup"><span data-stu-id="2511b-160">You do not need to specify any value in the 'Startup File' field.</span></span>

1. <span data-ttu-id="2511b-161">Dopo aver creato l'istanza (operazione molto rapida), fare clic su di essa e quindi sulla voce di menu "Impostazioni applicazione".</span><span class="sxs-lookup"><span data-stu-id="2511b-161">Once the instance is created (again, it is very quick), click on it and then click on the 'Application Settings' menu item.</span></span> <span data-ttu-id="2511b-162">Qui è necessario aggiungere una nuova impostazione applicazione, in cui la chiave è `WEBSITES_PORT` e il valore è `8080`.</span><span class="sxs-lookup"><span data-stu-id="2511b-162">In here you need to add a new application setting, where the key is `WEBSITES_PORT` and the value is `8080`.</span></span> <span data-ttu-id="2511b-163">Si indica così ad Azure quale porta si vuole esporre nel contenitore. Di questa porta verrà eseguito il mapping alla porta 80 esternamente.</span><span class="sxs-lookup"><span data-stu-id="2511b-163">This tells Azure which port you want to expose in the container, and it will be mapped to port 80 externally.</span></span>

1. <span data-ttu-id="2511b-164">Facoltativamente, fare clic sul collegamento "Contenitore Docker" e abilitare "Distribuzione continua" in modo che ogni volta che si aggiorna l'immagine del Registro contenitori di Azure, questa venga aggiornata automaticamente nell'istanza di app Web per contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-164">Optionally, click on the 'Docker Container' link, and enable 'Continuous Deployment', so that whenever you update the Azure Container Registry image it is automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="2511b-165">Dovrebbe essere possibile accedere alle istanze ospitate in Azure agli indirizzi `http://<appname>.azurewebsites.net/microprofile/api/helloworld` e `http://<appname>.azurewebsites.net/health`.</span><span class="sxs-lookup"><span data-stu-id="2511b-165">You should be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="2511b-166">Summary</span><span class="sxs-lookup"><span data-stu-id="2511b-166">Summary</span></span>

<span data-ttu-id="2511b-167">Con questa esercitazione è stato esaminato il processo di creazione di un semplice microservizio basato su MicroProfile, successivamente inserito in un contenitore Docker e quindi eseguito nell'ambiente locale e pubblicato in Azure.</span><span class="sxs-lookup"><span data-stu-id="2511b-167">Through this tutorial we have stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, and we have run it locally and published it to Azure.</span></span> <span data-ttu-id="2511b-168">L'estensione del microservizio per mettere a disposizione altre funzioni utili non rientra nell'ambito di questa esercitazione. Sono tuttavia disponibili molti consigli ed esercitazioni su MicroProfile in Internet. È consigliabile vedere [MicroProfile.io](https://microprofile.io/) per altri contenuti.</span><span class="sxs-lookup"><span data-stu-id="2511b-168">Extending our microservice to provide more useful functionality is outside the scope of this tutorial, but there is a wealth of tutorials and advice on MicroProfile on the internet, and readers are encouraged to review [MicroProfile.io](https://microprofile.io/) for more content.</span></span>
