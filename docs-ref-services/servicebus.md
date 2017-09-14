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
ms.openlocfilehash: 12b5f218008c208bfa85ef02820c56bbcf0b224a
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2017
---
# <a name="service-bus-libraries-for-java"></a><span data-ttu-id="e545f-104">Librerie del bus di servizio per Java</span><span class="sxs-lookup"><span data-stu-id="e545f-104">Service Bus libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e545f-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e545f-105">Overview</span></span>

<span data-ttu-id="e545f-106">Il bus di servizio è un servizio della piattaforma di messaggistica transazionale di livello aziendale che fornisce code altamente affidabili e argomenti di pubblicazione/sottoscrizione con funzionalità complete, ad esempio recapito ordinato, sessioni, partizionamento, pianificazione, sottoscrizioni complesse e gestione dei flussi di lavoro e delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="e545f-106">Service Bus is an enterprise-class, transactional messaging platform service that provides highly reliable queues and publish/subscribe topics with deep feature capabilities such as ordered delivery, sessions, partitioning, scheduling, complex subscriptions, as well as workflow and transaction handling.</span></span>

<span data-ttu-id="e545f-107">Le funzionalità del bus di servizio sono paragonabili e spesso superiori a quelle dei broker di messaggi legacy locali avanzati.</span><span class="sxs-lookup"><span data-stu-id="e545f-107">The Service Bus feature capabilities are comparable and often exceed those of high-end, on-premises legacy message brokers.</span></span> <span data-ttu-id="e545f-108">Le funzionalità del bus di servizio sono disponibili tramite protocolli basati su standard, ad esempio AMQP 1.0 e HTTPS, e tutti i comandi dei protocolli sono ampiamente documentati, consentendo la massima interoperabilità.</span><span class="sxs-lookup"><span data-stu-id="e545f-108">The Service Bus features are available via standards-based protocols like AMQP 1.0 and HTTPS and all protocol gestures are fully documented, allowing for broad interoperability.</span></span> 

<span data-ttu-id="e545f-109">Essendo incentrato su una messaggistica durevole con un livello elevato di disponibilità e affidabilità, il bus di servizio Premium offre prestazioni competitive per la velocità effettiva anche con distribuzioni di data center locali importanti, ma senza processi di selezione e acquisizione di hardware, pianificazione ed esecuzione delle distribuzioni e sessioni infinite di ottimizzazione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="e545f-109">Focusing on highly available and reliable durable messaging, the Service Bus Premium provides competitive throughput performance even with substantial local datacenter deployments, but without hardware selection and acquisition processes, deployment planning and execution, and endless performance optimization sessions.</span></span> 

<span data-ttu-id="e545f-110">Il bus di servizio Premium è un'offerta completamente gestita con capacità dedicata riservata per ogni tenant, che garantisce prestazioni prevedibili con un semplice modello di determinazione prezzi orientato alla capacità e con costi complessivi decisamente più bassi di quelli dei broker locali commerciali.</span><span class="sxs-lookup"><span data-stu-id="e545f-110">Service Bus Premium is a fully managed offering with dedicated capacity reserved for each tenant that yields predictable performance with a simple, capacity-oriented pricing model and at extremely lower overall cost than commercial on-premises brokers.</span></span> <span data-ttu-id="e545f-111">Per molti clienti, il bus di servizio Service Bus Premium può sostituire i cluster di messaggistica locali dedicati oggi stesso, anche se i carichi di lavoro collegati non vengono eseguiti nel cloud.</span><span class="sxs-lookup"><span data-stu-id="e545f-111">For many customers, Service Bus Premium can replace dedicated on-premises messaging clusters today, even if the attached workloads do not run in the cloud.</span></span> 

<span data-ttu-id="e545f-112">Per altre informazioni sul bus di servizio, vedere la [sezione sulla documentazione della messaggistica](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)</span><span class="sxs-lookup"><span data-stu-id="e545f-112">Learn more about Service Bus concepts [in the messaging documentation section](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)</span></span> 

<span data-ttu-id="e545f-113">Per gli sviluppatori Java, il bus di servizio fornisce un'API nativa supportata da Microsoft e può anche essere usato con librerie conformi con AMQP 1.0, ad esempio il provider JMS di Apache Qpid Proton.</span><span class="sxs-lookup"><span data-stu-id="e545f-113">For Java developers, Service Bus provides a Microsoft supported native API and Service Bus can also be used with AMQP 1.0 compliant libraries such as Apache Qpid Proton's JMS provider.</span></span>

<span data-ttu-id="e545f-114">Il client del bus di servizio ufficiale è disponibile nel [formato in codice sorgente su GitHub](https://github.com/azure/azure-service-bus-java), mentre i file binari e le origini compresse [sono disponibili su Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22).</span><span class="sxs-lookup"><span data-stu-id="e545f-114">The official Service Bus client is available in [source code form on GitHub](https://github.com/azure/azure-service-bus-java) and binaries and packaged sources [are available on Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22).</span></span> 


## <a name="client-library"></a><span data-ttu-id="e545f-115">Libreria client</span><span class="sxs-lookup"><span data-stu-id="e545f-115">Client library</span></span>


<span data-ttu-id="e545f-116">Aggiungere una dipendenza al file `pom.xml` del progetto Maven per usare la libreria nel proprio progetto.</span><span class="sxs-lookup"><span data-stu-id="e545f-116">Add a dependency to your Maven project's `pom.xml` file to use the library in your own project.</span></span> <span data-ttu-id="e545f-117">Specificare la versione desiderata.</span><span class="sxs-lookup"><span data-stu-id="e545f-117">Specify the version as desired.</span></span>

<span data-ttu-id="e545f-118">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e545f-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-servicebus</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="examples"></a><span data-ttu-id="e545f-119">esempi</span><span class="sxs-lookup"><span data-stu-id="e545f-119">Examples</span></span>

<span data-ttu-id="e545f-120">Il [repository dei codici di esempio](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/) contiene esempi su come applicare [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java), [TopicClient, SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java), [MessageSender e MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) ai messaggi dal bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="e545f-120">The [sample code repository](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/) contains samples for how to [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java) and [TopicClient and SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java) and [MessageSender and MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) messages from Service Bus.</span></span>


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
> [<span data-ttu-id="e545f-121">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="e545f-121">Explore the Client APIs</span></span>](/java/api/overview/azure/servicebus/clientlibrary)

## <a name="management-api"></a><span data-ttu-id="e545f-122">API di gestione</span><span class="sxs-lookup"><span data-stu-id="e545f-122">Management API</span></span>

<span data-ttu-id="e545f-123">Creare e gestire spazi dei nomi, argomenti, code e sottoscrizioni con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="e545f-123">Create and manage namespaces, topics, queues, and subscriptions with the management API.</span></span>

<span data-ttu-id="e545f-124">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e545f-124">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.2.1</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e545f-125">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="e545f-125">Explore the Management APIs</span></span>](/java/api/overview/azure/servicebus/managementapi)


## <a name="examples"></a><span data-ttu-id="e545f-126">esempi</span><span class="sxs-lookup"><span data-stu-id="e545f-126">Examples</span></span>

<span data-ttu-id="e545f-127">[Gestire le code del bus di servizio](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
[Creare e sottoscrivere argomenti del bus di servizio](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)</span><span class="sxs-lookup"><span data-stu-id="e545f-127">[Manage Service Bus queues](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
[Create and subscribe to Service Bus topics](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)</span></span>

<span data-ttu-id="e545f-128">Esplorare altri [esempi di codice Java per il bus di servizio di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=bus) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="e545f-128">Explore more [sample Java code for Azure Service Bus](https://azure.microsoft.com/resources/samples/?platform=java&term=bus) you can use in your apps.</span></span>