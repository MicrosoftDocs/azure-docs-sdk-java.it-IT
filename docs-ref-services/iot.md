---
title: Librerie di Hub IoT di Azure per Java
description: Documentazione di riferimento per le librerie di Hub IoT di Azure per Java
keywords: Azure, Java, SDK, API, evento, IoT, flussi, dispositivi, Hub IoT
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: iot-hub
ms.openlocfilehash: c1af3dae0fe37eb4919db02da87beed193c547a7
ms.sourcegitcommit: acc83bb537d77568b2a5427479d6354d6ae30885
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2017
---
# <a name="azure-iot-libraries-for-java"></a>Librerie di Azure IoT per Java

[Hub IoT di Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub) consente di connettersi agli asset IoT (Internet delle cose) per monitorarli e controllarli.

Per iniziare a usare Hub IoT di Azure, vedere [Connettere il dispositivo all'hub IoT usando Java](/azure/iot-hub/iot-hub-java-java-getstarted).

## <a name="iot-service-library"></a>Libreria di servizi IoT

Consente di registrare i dispositivi e inviare messaggi dal cloud ai dispositivi registrati mediante la libreria di servizi IoT.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a>Libreria di dispositivi IoT

Consente di inviare messaggi al cloud e ricevere messaggi nei dispositivi usando la libreria di dispositivi IoT.

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Esplorare le API client](/java/api/overview/azure/iot/clientlibrary)   

## <a name="example"></a>Esempio

Inviare un messaggio da Hub IoT di Azure a un dispositivo.

```java
Message messageToSend = new Message(messageText);
messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
messageToSend.setMessageId(java.util.UUID.randomUUID().toString());

// set message properties
Map<String, String> propertiesToSend = new HashMap<String, String>();
propertiesToSend.put(messagePropertyKey,messagePropertyKey);
messageToSend.setProperties(propertiesToSend);

CompletableFuture<Void> future = serviceClient.sendAsync(deviceId, messageToSend);
try {
    future.get();
}
catch (ExecutionException e) {
    System.out.println("Exception : " + e.getMessage());
}
```


## <a name="samples"></a>Esempi

[Esempi di dispositivo IoT](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)     
[Esempi di servizio IoT](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

Esplorare altri [esempi di codice Java per Hub IoT di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=iot) disponibili per l'uso nelle app.
