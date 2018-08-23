---
title: Distribuire un'app Spring Boot sul cloud con Maven e Azure
description: Informazioni su come distribuire un'app Spring Boot sul cloud con il plug-in Maven per le app Web di Azure.
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: robmcm;kevinzha;brborges
ms.date: 06/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: d58cafe3456150069ec8572c101c62d1b2c29c5d
ms.sourcegitcommit: e1a5d9687e006e8bf12d11747d45cf130a2c82af
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42703352"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a>Distribuire un'app Spring Boot nel cloud con il plug-in Maven per il servizio app di Azure

Questo articolo illustra l'uso del plug-in Maven per App Web del servizio app di Azure per distribuire un'applicazione Spring Boot di esempio.

> [!NOTE]
> 
> Il [plug-in Maven per App Web del servizio app di Azure](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) disponibile per [Apache Maven](http://maven.apache.org/) consente una facile integrazione del servizio app di Azure nei progetti Maven e semplifica il processo con cui gli sviluppatori distribuiscono app Web nel servizio app di Azure.

Prima di usare il plug-in Maven, verificarne l'ultima versione disponibile nel repository centrale Maven: [![Repository centrale Maven](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22) 

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura di questa esercitazione, saranno necessari i prerequisiti seguenti:

* Sottoscrizione di Azure. Se non se ne ha già una, è possibile iscriversi per ottenere un [account Azure gratuito].
* [Interfaccia della riga di comando di Azure].
* Java Development Kit (JDK) aggiornato, versione 1.7 o successiva.
* Lo strumento di compilazione [Maven] di Apache (versione 3).
* Un client [Git].

## <a name="clone-the-sample-spring-boot-web-app"></a>Clonare l'app Web Spring Boot di esempio

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

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a>Modificare il progetto per la distribuzione basata su WAR nel servizio app di Azure

In questa sezione si modificherà rapidamente il progetto Spring Boot per distribuirlo come file WAR nel servizio app di Azure, che per impostazione predefinita offre Tomcat come runtime. Per poter eseguire questa operazione, è necessario modificare due file:

- File `pom.xml` di Maven
- Classe Java `Application`

Iniziare con le impostazioni di Maven:

1. Aprire `pom.xml`

1. Aggiungere `<packaging>war</packaging>` subito dopo la definizione `<artifactId>` all'inizio:
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. Aggiungere la dipendenza seguente:
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

Aprire quindi la classe `Application`, possibilmente dopo che l'ambiente di sviluppo integrato ha rilevato le nuove dipendenze, e procedere con le modifiche seguenti:

1. Rendere la classe Application una sottoclasse di `SpringBootServletInitializer`:
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. Aggiungere il metodo seguente alla classe Application:
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```

L'applicazione è ora pronta per essere distribuita in Tomcat e qualsiasi altro runtime servlet (ad esempio, Jetty).

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a>Aggiungere il plug-in Maven per App Web del servizio app di Azure

In questa sezione si aggiungerà un plug-in Maven che automatizzerà l'intera distribuzione dell'applicazione in App Web del servizio app di Azure.

1. Aprire di nuovo `pom.xml`.

1. All'interno di `<properties>` impostare un formato di timestamp personalizzato con la proprietà `maven.build.timestamp.format`. Dato che il servizio app di Azure crea un URL pubblico per l'applicazione, questa impostazione verrà usata per generare il nome della distribuzione ed evitare conflitti con le distribuzioni live di altri utenti.
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. Nell'elemento `<plugins>` aggiungere quanto segue:
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

Con queste impostazioni, il progetto Maven è ora pronto per la distribuzione live in App Web del servizio app di Azure.

## <a name="install-and-log-in-to-azure-cli"></a>Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso

Il modo più semplice e facile per distribuire l'applicazione Spring Boot tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/). Verificare che sia installata.

1. Accedere all'account Azure con l'interfaccia della riga di comando di Azure:
   
   ```shell
   az login
   ```
   
   Seguire le istruzioni per completare il processo di accesso.

## <a name="optionally-customize-pomxml-before-deploying"></a>Personalizzare pom.xml prima della distribuzione (facoltativo)

Aprire il file `pom.xml` per l'applicazione Spring Boot in un editor di testo e quindi individuare l'elemento `<plugin>` per `azure-webapp-maven-plugin`. L'elemento dovrebbe essere simile all'esempio seguente:

   ```xml
  <plugins>
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
      <configuration>
         <resourceGroup>maven-projects</resourceGroup>
         <appName>${project.artifactId}-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>war</deploymentType>
      </configuration>
    </plugin>
  </plugins>
   ```

È possibile modificare diversi valori per il plug-in Maven. Una descrizione dettagliata di ognuno di questi elementi è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]. Dopo questa premessa, in questo articolo è opportuno evidenziare diversi valori:

| Elemento | DESCRIZIONE |
|---|---|
| `<version>` | Specifica la versione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]. Controllare la versione riportata nel [repository centrale Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) per assicurarsi di usare l'ultima versione. |
| `<resourceGroup>` | Specifica il gruppo di risorse di destinazione, che in questo esempio è `maven-plugin`. Se non esiste già, questo gruppo di risorse viene creato durante la distribuzione. |
| `<appName>` | Specifica il nome di destinazione dell'app Web. In questo esempio, il nome di destinazione è `maven-web-app-${maven.build.timestamp}` e il suffisso `${maven.build.timestamp}` viene accodato per evitare conflitti. Il timestamp è facoltativo. Come nome dell'app è possibile specificare qualsiasi stringa univoca. |
| `<region>` | Specifica l'area di destinazione, che in questo esempio è `westus`. Un elenco completo è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]. |
| `<javaVersion>` | Specifica la versione del runtime Java per l'app Web. Un elenco completo è disponibile nella documentazione del [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]. |
| `<deploymentType>` | Specifica il tipo di distribuzione per l'app Web. Il valore predefinito è `war`. |

## <a name="build-and-deploy-your-web-app-to-azure"></a>Compilare e distribuire l'app Web in Azure

Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure. A tale scopo, seguire questa procedura:

1. Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:
   ```shell
   mvn clean package
   ```

1. Distribuire l'app Web in Azure usando Maven. Ad esempio:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven distribuirà l'app Web in Azure. Se l'app Web non esiste già, verrà creata.

Dopo che è stata distribuita, l'app Web potrà essere gestita usando il [portale di Azure].

* L'app Web sarà elencata in **Servizi app**:

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:

   ![Individuazione dell'URL dell'app Web][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:

* [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]

* [Accedere ad Azure dall'interfaccia della riga di comando di Azure](/azure/xplat-cli-connect)

* [Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot in contenitore in Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informazioni di riferimento sulle impostazioni di Maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Portale di Azure]: https://portal.azure.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
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
