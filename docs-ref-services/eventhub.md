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
# <a name="azure-event-hub-libraries-for-java"></a>Librerie di Hub eventi di Azure per Java

## <a name="overview"></a>Panoramica

[Hub eventi di Azure](/azure/event-hubs/event-hubs-what-is-event-hubs) consente di raccogliere e gestire milioni di eventi al secondo da dispositivi e applicazioni IoT connessi.

Per iniziare a usare Hub eventi di Azure, vedere [Ricevere eventi da Hub eventi di Azure usando Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).


## <a name="client-library"></a>Libreria client

Consente di inviare eventi a un hub eventi di Azure e utilizzare ed elaborare gli eventi da un hub eventi con la libreria client di Hub eventi.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a>Esempio

Inviare eventi a un hub eventi.

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [Esplorare le API client](/java/api/overview/azure/eventhub/client)


## <a name="samples"></a>Esempi

[Scrivere in un hub eventi con JMS ed eseguire la lettura da Apache Storm][1]
[Leggere e scrivere da Hub eventi con una topologia ibrida .NET/Java][2] 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

Esplorare altri [esempi di codice Java per Hub eventi di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=event) disponibili per l'uso nelle app.

