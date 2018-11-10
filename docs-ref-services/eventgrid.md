---
title: Librerie di Griglia di eventi di Azure per Java
description: Documentazione di riferimento per le librerie Griglia di eventi di Azure per Java
keywords: Azure, Java, SDK, API, griglia di eventi, messaggistica, basato sugli eventi
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 07/11/2017
ms.topic: managed-reference
ms.devlang: java
ms.service: event-grid
ms.openlocfilehash: 35acbdeede2fbc541128029ad43f149209a057c4
ms.sourcegitcommit: db36eeb667a0bfe6d113953bb0583240db61b0f4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134629"
---
# <a name="azure-event-grid-libraries-for-java"></a><span data-ttu-id="ccf16-104">Librerie di Griglia di eventi di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="ccf16-104">Azure Event Grid libraries for Java</span></span>

<span data-ttu-id="ccf16-105">È possibile creare applicazioni basate su eventi che rimangono in ascolto e reagiscono a eventi dai servizi di Azure e da origini personalizzate usando la gestione semplice di eventi basati su HTTP con Griglia di eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="ccf16-105">Build event-driven applications that listen and react to events from Azure services and custom sources using simple HTTP-based event handling with Azure Event Grid.</span></span>

<span data-ttu-id="ccf16-106">[Altre informazioni](/azure/event-grid/overview) su Griglia di eventi di Azure e introduzione all'[esercitazione sugli eventi di archiviazione BLOB di Azure](/azure/storage/blobs/storage-blob-event-quickstart).</span><span class="sxs-lookup"><span data-stu-id="ccf16-106">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="client-sdk"></a><span data-ttu-id="ccf16-107">Client SDK</span><span class="sxs-lookup"><span data-stu-id="ccf16-107">Client SDK</span></span>

<span data-ttu-id="ccf16-108">Creare eventi, eseguire l'autenticazione e inserire commenti negli argomenti usando Client SDK per Griglia di eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="ccf16-108">Create events, authenticate, and post to topics using the Azure Event Grid Client SDK.</span></span>

<span data-ttu-id="ccf16-109">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ccf16-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventgrid</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="publish-events"></a><span data-ttu-id="ccf16-110">Pubblicare eventi</span><span class="sxs-lookup"><span data-stu-id="ccf16-110">Publish events</span></span>

<span data-ttu-id="ccf16-111">Il codice seguente esegue l'autenticazione con Azure e pubblica un oggetto `List` di eventi di `EventGridEvent` di tipo personalizzato, in questo esempio `Contoso.Items.ItemsReceived`, in un argomento.</span><span class="sxs-lookup"><span data-stu-id="ccf16-111">The following code authenticates with Azure and publishes a `List` of  `EventGridEvent` events of a custom type (in this example, `Contoso.Items.ItemsReceived` ) to a topic.</span></span> <span data-ttu-id="ccf16-112">La chiave e l'indirizzo dell'endpoint dell'argomento usati nell'esempio possono essere recuperati dall'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="ccf16-112">The topic key and endpoint address used in the sample can be retrieved from the Azure CLI:</span></span>

```azurecli-interactive
az eventgrid topic show -g gridResourceGroup -n topicName
az eventgrid topic key list -g gridResourceGroup -n topicName
```

```java
// Create an event grid client.
TopicCredentials topicCredentials = new TopicCredentials(System.getenv("EVENTGRID_TOPIC_KEY"));
EventGridClient client = new EventGridClientImpl(topicCredentials);

// Publish custom events to the EventGrid.
System.out.println("Publish custom events to the EventGrid");
List<EventGridEvent> eventsList = new ArrayList<>();
for (int i = 0; i < 5; i++) {
    eventsList.add(new EventGridEvent(
        UUID.randomUUID().toString(),
        String.format("Door%d", i),
        new ContosoItemReceivedEventData("Contoso Item SKU #1"),
        "Contoso.Items.ItemReceived",
        DateTime.now(),
        "2.0"
    ));
}

String eventGridEndpoint = String.format("https://%s/", new URI(System.getenv("EVENTGRID_TOPIC_ENDPOINT")).getHost());

client.publishEvents(eventGridEndpoint, eventsList);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ccf16-113">Esplorare le API del client di Griglia di eventi</span><span class="sxs-lookup"><span data-stu-id="ccf16-113">Explore the Event Grid Client APIs</span></span>](/java/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="ccf16-114">SDK di gestione</span><span class="sxs-lookup"><span data-stu-id="ccf16-114">Management SDK</span></span>

<span data-ttu-id="ccf16-115">Creare, aggiornare o eliminare istanze, argomenti e sottoscrizioni di Griglia di eventi con Event Grid Management SDK.</span><span class="sxs-lookup"><span data-stu-id="ccf16-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the Event Grid management SDK.</span></span>

<span data-ttu-id="ccf16-116">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ccf16-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.16.0</version>
</dependency>
```   

<span data-ttu-id="ccf16-117">L'esempio seguente crea una sottoscrizione di Griglia di eventi, proveniente dagli esempi di [EventGrid per Java](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)</span><span class="sxs-lookup"><span data-stu-id="ccf16-117">The following example creates an Event Grid subscription, taken from the [EventGrid Java samples](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)</span></span>

```java
// Create an event grid subscription.
//
System.out.println("Creating an Azure EventGrid Subscription");

EventSubscription eventSubscription = eventGridManager.eventSubscriptions().define(eventSubscriptionName)
    .withScope(eventGridTopic.id())
    .withDestination(new EventHubEventSubscriptionDestination()
        .withResourceId(eventHub.id()))
    .withFilter(new EventSubscriptionFilter()
        .withIsSubjectCaseSensitive(false)
        .withSubjectBeginsWith("")
        .withSubjectEndsWith(""))
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ccf16-118">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="ccf16-118">Explore the management APIs</span></span>](/java/api/overview/azure/eventgrid/management)