---
title: Librerie del bus di servizio per Java
description: Documentazione di riferimento per le librerie di gestione e client Java per il bus di servizio
keywords: Azure, Java, SDK, API, messaggistica, amqp, qpid, JMS, pubsub, pub-sub, broker messaggi
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: service-bus
ms.openlocfilehash: 2099695784a59bf539aeae745df1fe38ec84f511
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593276"
---
# <a name="service-bus-libraries-for-java"></a>Librerie del bus di servizio per Java

## <a name="overview"></a>Panoramica

Il bus di servizio è un servizio della piattaforma di messaggistica transazionale di livello aziendale che fornisce code altamente affidabili e argomenti di pubblicazione/sottoscrizione con funzionalità complete, ad esempio recapito ordinato, sessioni, partizionamento, pianificazione, sottoscrizioni complesse e gestione dei flussi di lavoro e delle transazioni.

Le funzionalità del bus di servizio sono paragonabili e spesso superiori a quelle dei broker di messaggi legacy locali avanzati. Le funzionalità del bus di servizio sono disponibili tramite protocolli basati su standard, ad esempio AMQP 1.0 e HTTPS, e tutti i comandi dei protocolli sono ampiamente documentati, consentendo la massima interoperabilità. 

Essendo incentrato su una messaggistica durevole con un livello elevato di disponibilità e affidabilità, il bus di servizio Premium offre prestazioni competitive per la velocità effettiva anche con distribuzioni di data center locali importanti, ma senza processi di selezione e acquisizione di hardware, pianificazione ed esecuzione delle distribuzioni e sessioni infinite di ottimizzazione delle prestazioni. 

Il bus di servizio Premium è un'offerta completamente gestita con capacità dedicata riservata per ogni tenant, che garantisce prestazioni prevedibili con un semplice modello di determinazione prezzi orientato alla capacità e con costi complessivi decisamente più bassi di quelli dei broker locali commerciali. Per molti clienti, il bus di servizio Service Bus Premium può sostituire i cluster di messaggistica locali dedicati oggi stesso, anche se i carichi di lavoro collegati non vengono eseguiti nel cloud. 

Per altre informazioni sul bus di servizio, vedere la [sezione sulla documentazione della messaggistica](https://docs.microsoft.com/azure/service-bus-messaging/) 

Per gli sviluppatori Java, il bus di servizio fornisce un'API nativa supportata da Microsoft e può anche essere usato con librerie conformi con AMQP 1.0, ad esempio il provider JMS di Apache Qpid Proton.

## <a name="client-library"></a>Libreria client

Il client del bus di servizio ufficiale è disponibile nel [formato in codice sorgente su GitHub](https://github.com/azure/azure-service-bus-java), mentre i file binari e le origini compresse [sono disponibili su Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22).

**Il [repository di codice di esempio](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/) contiene esempi per:**
* Come usare [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java)
* Come usare [TopicClient e SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java)
* Come usare i messaggi [MessageSender e MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) dal bus di servizio.

Aggiungere una dipendenza al file `pom.xml` del progetto Maven per usare la libreria nel proprio progetto. Specificare la versione desiderata.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-servicebus</artifactId>
    <version>1.0.0</version>
</dependency>
```

```java
public class BasicSendReceiveWithQueueClient {
    // Connection String for the namespace can be obtained from the Azure portal under the
    // 'Shared Access policies' section.
    private static final String connectionString = "{connection string}";
    private static final String queueName = "{queue name}";
    private static IQueueClient queueClient;
    private static int totalSend = 100;
    private static int totalReceived = 0;

    public static void main(String[] args) throws Exception {

        Log.log("Starting BasicSendReceiveWithQueueClient sample");

        // create client
        Log.log("Create queue client.");
        queueClient = new QueueClient(new ConnectionStringBuilder(connectionString, queueName), ReceiveMode.PeekLock);

        // send and receive
        queueClient.registerMessageHandler(new MessageHandler(queueClient), new MessageHandlerOptions(1, false, Duration.ofMinutes(1)));
        for (int i = 0; i < totalSend; i++) {
            int j = i;
            Log.log("Sending message #%d.", j);
            queueClient.sendAsync(new Message("" + i)).thenRunAsync(() -> { Log.log("Sent message #%d.", j);});
        }

        while(totalReceived != totalSend) {
            Thread.sleep(1000);
        }

        Log.log("Received all messages, exiting the sample.");
        Log.log("Closing queue client.");
        queueClient.close();
    }

    static class MessageHandler implements IMessageHandler {
        private IQueueClient client;

        public MessageHandler(IQueueClient client) {
            this.client = client;
        }

        @Override
        public CompletableFuture<Void> onMessageAsync(IMessage iMessage) {
            Log.log("Received message with sq#: %d and lock token: %s.", iMessage.getSequenceNumber(), iMessage.getLockToken());
            return this.client.completeAsync(iMessage.getLockToken()).thenRunAsync(() -> {
                Log.log("Completed message sq#: %d and locktoken: %s", iMessage.getSequenceNumber(), iMessage.getLockToken());
                totalReceived++;
            });
        }

        @Override
        public void notifyException(Throwable throwable, ExceptionPhase exceptionPhase) {
            Log.log(exceptionPhase + "-" + throwable.getMessage());
        }
    }
}
```

> [!div class="nextstepaction"]
> [Esplorare le API Client](/java/api/overview/azure/servicebus/client)
> [Altri esempi qui (vedere anche in precedenza per altri dettagli)](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)

## <a name="management-api"></a>API di gestione

Creare e gestire spazi dei nomi, argomenti, code e sottoscrizioni con l'API di gestione.

**Alcuni esempi sono disponibili qui:**
* [Gestire le code del bus di servizio](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
* [Creare e sottoscrivere argomenti del bus di servizio](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)

**Usare l'API di gestione nel progetto:**
\
[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/java/api/overview/azure/servicebus/management)

Esplorare altri [esempi di codice Java per il bus di servizio di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=bus) disponibili per l'uso nelle app.
