---
title: Librerie di Hub eventi di Azure per Java
description: Documentazione di riferimento per le librerie di Hub eventi di Azure per Java
keywords: Azure, Java, SDK, API, hub eventi, IoT, elaborazione del flusso
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: event-hub
ms.openlocfilehash: 076906ff3cafcb4eba97b0a022e5214d7834517c
ms.sourcegitcommit: 02b70b9f5d34415c337601f0b818f7e0985fd884
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="3a996-104">Librerie di Hub eventi di Azure per Java</span><span class="sxs-lookup"><span data-stu-id="3a996-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="3a996-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3a996-105">Overview</span></span>

<span data-ttu-id="3a996-106">[Hub eventi di Azure](/azure/event-hubs/event-hubs-what-is-event-hubs) consente di raccogliere e gestire milioni di eventi al secondo da dispositivi e applicazioni IoT connessi.</span><span class="sxs-lookup"><span data-stu-id="3a996-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="3a996-107">Per iniziare a usare Hub eventi di Azure, vedere [Ricevere eventi da Hub eventi di Azure usando Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span><span class="sxs-lookup"><span data-stu-id="3a996-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="3a996-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="3a996-108">Client library</span></span>

<span data-ttu-id="3a996-109">Consente di inviare eventi a un hub eventi di Azure e utilizzare ed elaborare gli eventi da un hub eventi con la libreria client di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="3a996-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="3a996-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="3a996-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="3a996-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="3a996-111">Example</span></span>

<span data-ttu-id="3a996-112">Inviare eventi a un hub eventi.</span><span class="sxs-lookup"><span data-stu-id="3a996-112">Send an event to an event hub.</span></span>

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3a996-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="3a996-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhub/client)


## <a name="samples"></a><span data-ttu-id="3a996-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="3a996-114">Samples</span></span>

<span data-ttu-id="3a996-115">[Scrivere in un hub eventi con JMS ed eseguire la lettura da Apache Storm][1]
[Leggere e scrivere da Hub eventi con una topologia ibrida .NET/Java][2]</span><span class="sxs-lookup"><span data-stu-id="3a996-115">[Write to Event Hub via JMS and read from Apache Storm][1]
[Read and write from EventHubs using a hybrid .NET/Java topology][2]</span></span> 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="3a996-116">Esplorare altri [esempi di codice Java per Hub eventi di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=event) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="3a996-116">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

