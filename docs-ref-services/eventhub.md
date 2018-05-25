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
ms.openlocfilehash: b6646ef27edace4247090e749c9a52cd6a33a82c
ms.sourcegitcommit: 3d3460289ab6b9165c2cf6a3dd56eafd0692501e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="azure-event-hub-libraries-for-java"></a>Librerie di Hub eventi di Azure per Java

## <a name="overview"></a>Panoramica

[Hub eventi di Azure](/azure/event-hubs/event-hubs-what-is-event-hubs) consente di raccogliere e gestire milioni di eventi al secondo da dispositivi e applicazioni IoT connessi.

Per iniziare a usare Hub eventi di Azure, vedere [Ricevere eventi da Hub eventi di Azure usando Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).


## <a name="client-library"></a>Libreria client

Consente di inviare eventi a un hub eventi di Azure e utilizzare ed elaborare gli eventi da un hub eventi con la libreria client di Hub eventi.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la [libreria client](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs) nel progetto.
 

## <a name="example"></a>Esempio

Inviare eventi a un hub eventi.

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                                            .setNamespaceName(namespaceName)
                                            .setEventHubName(eventHubName)
                                            .setSasKeyName(sasKeyName)
                                            .setSasKey(sasKey);
final EventHubClient ehClient = EventHubClient.createSync(connStr.toString());

final byte[] payloadBytes = "Test AMQP message".getBytes("UTF-8");
final EventData sendEvent = new EventData(payloadBytes);

ehClient.sendSync(sendEvent);
```


> [!div class="nextstepaction"]
> [Esplorare le API client](/java/api/overview/azure/eventhubs/client)



## <a name="samples"></a>Esempi

[Esplorare l'API del piano dati di Hub eventi con gli esempi][1]

[Scrivere in un hub eventi con JMS ed eseguire la lettura da Apache Storm][2]

[Eseguire la lettura e la scrittura da Hub eventi usando una topologia ibrida .NET/Java][3] 

[1]: https://github.com/Azure/azure-event-hubs/tree/master/samples/Java
[2]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[3]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

Esplorare altri [esempi di codice Java per Hub eventi di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=event) disponibili per l'uso nelle app.

