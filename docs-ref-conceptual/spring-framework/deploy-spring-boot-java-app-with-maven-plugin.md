---
title: Distribuire un'app Spring Boot basata su file JAR nel cloud con Maven e Azure
description: Informazioni su come distribuire un'app Spring Boot nel cloud con il plug-in Maven per app Web di Azure per Linux.
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.author: robmcm
ms.date: 10/18/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: dc3038fed6859203f36e0c4dc9a9b01e81a7c4c5
ms.sourcegitcommit: dae7511a9d93ca7f388d5b0e05dc098e22c2f2f6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49962495"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a>Distribuire un'app Web Spring Boot basata su file JAR nel servizio app di Azure in Linux

Questo articolo illustra l'uso del [plug-in Maven per app Web del servizio app di Azure](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) per distribuire un'applicazione Spring Boot inserita in un pacchetto JAR Java SE nel [servizio app di Azure in Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/). Preferire la distribuzione con Java SE a [Tomcat e file WAR](/azure/app-service/containers/quickstart-java) quando si vogliono consolidare le dipendenze, il runtime e la configurazione dell'app in un singolo artefatto distribuibile.


Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per completare i passaggi di questa esercitazione, devono essere installati e configurati gli strumenti seguenti:

* [Interfaccia della riga di comando di Azure](/cli/azure/), in locale o tramite [Azure Cloud Shell](https://shell.azure.com).
* [Java Development Kit (JDK)](https://www.azul.com/downloads/azure-only/zulu/), versione 1.7 o successiva.
* Apache [Maven](https://maven.apache.org/), versione 3.
* Un client [Git](https://git-scm.com/downloads).

## <a name="install-and-sign-in-to-azure-cli"></a>Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso

Il modo più semplice e facile per distribuire l'applicazione Spring Boot tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/).

Accedere all'account Azure con l'interfaccia della riga di comando di Azure:
   
   ```shell
   az login
   ```
   
Seguire le istruzioni per completare il processo di accesso.

## <a name="clone-the-sample-app"></a>Clonare l'app di esempio

In questa sezione sarà clonata e testata in locale un'applicazione Spring Boot completata.

1. Aprire un prompt dei comandi o una finestra del terminale e creare una directory locale in cui salvare l'applicazione Spring Boot, quindi passare a tale directory. Ad esempio:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- o --
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. Clonare il progetto di esempio di [introduzione a Spring Boot] nella directory creata, ad esempio:
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. Passare alla directory del progetto completato. Ad esempio:
   ```shell
   cd gs-spring-boot/complete
   ```

1. Compilare il file JAR usando Maven. Ad esempio:
   ```shell
   mvn clean package
   ```

1. Dopo che è stata creata, avviare l'app Web con Maven, ad esempio:
   ```shell
   mvn spring-boot:run
   ```

1. Testare l'app Web esplorandola localmente tramite un Web browser. Se è disponibile curl, ad esempio, è possibile usare il comando seguente:
   ```shell
   curl http://localhost:8080
   ```

1. Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!)

## <a name="configure-maven-plugin-for-azure-app-service"></a>Configurare il plug-in Maven per il servizio app di Azure

In questa sezione si configurerà il progetto Spring Boot `pom.xml` in modo che Maven possa distribuire l'app nel servizio app di Azure in Linux.

1. Aprire `pom.xml` in un editor di codice.

2. Nella sezione `<build>` di pom.xml aggiungere la voce `<plugin>` seguente all'interno del tag `<plugins>`.

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
    <configuration>
      <deploymentType>jar</deploymentType>

      <!-- configure app to run on port 80, required by App Service -->
      <appSettings>
        <property> 
          <name>JAVA_OPTS</name> 
          <value>-Dserver.port=80</value> 
        </property> 
      </appSettings>

      <!-- Web App information -->
      <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
      <appName>${WEBAPP_NAME}</appName>
      <region>${REGION}</region>  

      <!-- Java Runtime Stack for Web App on Linux-->
      <linuxRuntime>jre8</linuxRuntime>
    </configuration>
   </plugin>
   ```

3. Aggiornare i segnaposto seguenti nella configurazione del plug-in:

| Placeholder | DESCRIZIONE |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Nome del nuovo gruppo di risorse in cui creare l'app Web. Inserendo tutte le risorse per un'app in un gruppo è possibile gestirle insieme. Ad esempio, eliminando il gruppo di risorse si eliminano tutte le risorse associate all'app. Aggiornare questo valore con un nuovo nome univoco di gruppo di risorse, ad esempio *TestResources*. Questo nome di gruppo di risorse verrà usato per pulire tutte le risorse di Azure in una sezione successiva. |
| `WEBAPP_NAME` | Il nome dell'app sarà incluso nel nome host dell'app Web durante la distribuzione in Azure (WEBAPP_NAME.azurewebsites.net). Aggiornare questo valore con un nome univoco per l'app Web di Azure, che ospiterà l'app Java, ad esempio *contoso*. |
| `REGION` | Un'area di Azure in cui l'app web è ospitata, ad esempio `westus2`. È possibile ottenere un elenco di aree da Cloud Shell o dall’interfaccia della riga di comando utilizzando il comando `az account list-locations`. |

Un elenco completo delle opzioni di configurazione è disponibile nelle [informazioni di riferimento sul plug-in Maven in GitHub](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin).

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure. A tale scopo, seguire questa procedura:

1. Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:
   ```shell
   mvn clean package
   ```

1. Distribuire l'app Web in Azure usando Maven. Ad esempio:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven distribuirà l'app Web in Azure. Se l'app Web o il piano dell'app Web non esiste già, verrà creato.

Dopo che è stata distribuita, l'app Web potrà essere gestita tramite il [portale di Azure].

* L'app Web sarà elencata in **Servizi app**:

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:

   ![Individuazione dell'URL dell'app Web][AP02]

Verificare il completamento della distribuzione con lo stesso comando curl eseguito in precedenza, usando l'URL dell'app Web riportato nel portale invece di `localhost`. Si dovrebbe visualizzare il messaggio seguente: **Greetings from Spring Boot!** (Benvenuti in Spring Boot!) 

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:

* [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]

* [Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot in contenitore in Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informazioni di riferimento sulle impostazioni di Maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Portale di Azure]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Introduzione a Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
