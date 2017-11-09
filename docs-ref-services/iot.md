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
# <a name="azure-iot-libraries-for-java"></a><span data-ttu-id="9e5aa-104">Librerie di Azure IoT per Java</span><span class="sxs-lookup"><span data-stu-id="9e5aa-104">Azure IoT libraries for Java</span></span>

<span data-ttu-id="9e5aa-105">[Hub IoT di Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub) consente di connettersi agli asset IoT (Internet delle cose) per monitorarli e controllarli.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-105">Connect, monitor, and control Internet of Things assets with [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).</span></span>

<span data-ttu-id="9e5aa-106">Per iniziare a usare Hub IoT di Azure, vedere [Connettere il dispositivo all'hub IoT usando Java](/azure/iot-hub/iot-hub-java-java-getstarted).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-106">To get started with Azure IoT Hub, see [Connect your device to your IoT hub using Java](/azure/iot-hub/iot-hub-java-java-getstarted).</span></span>

## <a name="iot-service-library"></a><span data-ttu-id="9e5aa-107">Libreria di servizi IoT</span><span class="sxs-lookup"><span data-stu-id="9e5aa-107">IoT Service library</span></span>

<span data-ttu-id="9e5aa-108">Consente di registrare i dispositivi e inviare messaggi dal cloud ai dispositivi registrati mediante la libreria di servizi IoT.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-108">Register devices and send messages from the cloud to registered devices using the IoT Service library.</span></span>

<span data-ttu-id="9e5aa-109">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a><span data-ttu-id="9e5aa-110">Libreria di dispositivi IoT</span><span class="sxs-lookup"><span data-stu-id="9e5aa-110">Iot Device library</span></span>

<span data-ttu-id="9e5aa-111">Consente di inviare messaggi al cloud e ricevere messaggi nei dispositivi usando la libreria di dispositivi IoT.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-111">Send messages to the cloud and receive messages on devices using the IoT Device library.</span></span>

<span data-ttu-id="9e5aa-112">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-112">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9e5aa-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="9e5aa-113">Explore the Client APIs</span></span>](/java/api/overview/azure/iot/clientlibrary)   

## <a name="example"></a><span data-ttu-id="9e5aa-114">Esempio</span><span class="sxs-lookup"><span data-stu-id="9e5aa-114">Example</span></span>

<span data-ttu-id="9e5aa-115">Inviare un messaggio da Hub IoT di Azure a un dispositivo.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-115">Send a message from Azure IoT Hub to a device.</span></span>

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


## <a name="samples"></a><span data-ttu-id="9e5aa-116">Esempi</span><span class="sxs-lookup"><span data-stu-id="9e5aa-116">Samples</span></span>

<span data-ttu-id="9e5aa-117">[Esempi di dispositivo IoT](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span><span class="sxs-lookup"><span data-stu-id="9e5aa-117">[IoT Device samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span></span>  
[<span data-ttu-id="9e5aa-118">Esempi di servizio IoT</span><span class="sxs-lookup"><span data-stu-id="9e5aa-118">IoT Service samples</span></span>](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

<span data-ttu-id="9e5aa-119">Esplorare altri [esempi di codice Java per Hub IoT di Azure](https://azure.microsoft.com/resources/samples/?platform=java&term=iot) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-119">Explore more [sample Java code for Azure IoT](https://azure.microsoft.com/resources/samples/?platform=java&term=iot) you can use in your apps.</span></span>
