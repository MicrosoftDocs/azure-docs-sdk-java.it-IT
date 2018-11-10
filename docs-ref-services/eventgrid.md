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
# <a name="azure-event-grid-libraries-for-java"></a>Librerie di Griglia di eventi di Azure per Java

È possibile creare applicazioni basate su eventi che rimangono in ascolto e reagiscono a eventi dai servizi di Azure e da origini personalizzate usando la gestione semplice di eventi basati su HTTP con Griglia di eventi di Azure.

[Altre informazioni](/azure/event-grid/overview) su Griglia di eventi di Azure e introduzione all'[esercitazione sugli eventi di archiviazione BLOB di Azure](/azure/storage/blobs/storage-blob-event-quickstart). 

## <a name="client-sdk"></a>Client SDK

Creare eventi, eseguire l'autenticazione e inserire commenti negli argomenti usando Client SDK per Griglia di eventi di Azure.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventgrid</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="publish-events"></a>Pubblicare eventi

Il codice seguente esegue l'autenticazione con Azure e pubblica un oggetto `List` di eventi di `EventGridEvent` di tipo personalizzato, in questo esempio `Contoso.Items.ItemsReceived`, in un argomento. La chiave e l'indirizzo dell'endpoint dell'argomento usati nell'esempio possono essere recuperati dall'interfaccia della riga di comando di Azure:

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
> [Esplorare le API del client di Griglia di eventi](/java/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a>SDK di gestione

Creare, aggiornare o eliminare istanze, argomenti e sottoscrizioni di Griglia di eventi con Event Grid Management SDK.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.16.0</version>
</dependency>
```   

L'esempio seguente crea una sottoscrizione di Griglia di eventi, proveniente dagli esempi di [EventGrid per Java](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)

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
> [Esplorare le API di gestione](/java/api/overview/azure/eventgrid/management)