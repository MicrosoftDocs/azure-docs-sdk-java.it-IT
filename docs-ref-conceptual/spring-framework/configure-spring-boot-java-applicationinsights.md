---
title: Configurare un'app Spring Boot Initializer per l'uso di Azure Application Insights Spring Boot Starter
description: Configurare un'applicazione Spring Boot creata con Spring Initializr per l'uso di Application Insights Spring Boot Starter.
services: Application-Insights
documentationcenter: java
author: dhaval24
manager: alexklim
editor: ''
ms.assetid: ''
ms.author: dhdoshi
ms.date: 05/19/2018
ms.devlang: java
ms.service: Azure Monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.workload: na
ms.openlocfilehash: 0e57bfb304185b8b98dedfdecb2e0374c4a72fe5
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090774"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a>Configurare un'app Spring Boot Initializer per l'uso di Application Insights

Questo articolo illustra come creare un'applicazione Spring Boot con **[Spring Initializr]**, che usa Azure Application Insights Spring Boot Starter per il monitoraggio end-to-end delle applicazioni Java nel cloud.

> [!NOTE]
> 
> Questa utilità di avvio è attualmente in versione *BETA (anteprima pubblica)*<em>.</em>

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [Vantaggi per gli abbonati MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) versione 1.7 e 1.8.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Creare un'applicazione personalizzata tramite Spring Initializr

1. Passare a [https://start.spring.io/](https://start.spring.io/).

1. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento) e quindi selezionare la dipendenza Web nella sezione delle dipendenze.

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr userà i nomi in **Group** (Gruppo) e **Artifact** (Elemento) per creare il nome del pacchetto, ad esempio *com.example.demo*.
   >

1. Fare clic sul pulsante **Generate Project** (Genera progetto).

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

1. Dopo l'estrazione dei file nel sistema locale, l'applicazione Spring Boot personalizzata sarà pronta per la modifica.

   ![File del progetto Spring Boot personalizzato][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a>Creare una risorsa di Application Insights in Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Nuovo**.

   ![Portale di Azure][AZ01]

1. Fare clic su **Strumenti di gestione** e quindi su **Application Insights**.

   ![Portale di Azure][AZ02]

1. Nella pagina per la **nuova risorsa di Application Insights** specificare le informazioni seguenti:

   * Immettere il **nome** della risorsa di Application Insights.
   * Scegliere Applicazione Web Java in **Tipo di applicazione**.
   * Specificare **Sottoscrizione**, **Gruppo di risorse** e **Località**.
   * Selezionare l'opzione Aggiungi al dashboard, se si vuole aggiungere la risorsa al portale di Azure.

   Dopo aver specificato queste opzioni, fare clic su **Crea** per creare la risorsa di Application Insights.

   ![Portale di Azure][AZ03]

1. Al termine della creazione, la risorsa verrà elencata nel **dashboard** di Azure, nonché nelle pagine **Tutte le risorse**. È possibile fare clic sulla risorsa di Application Insights in una di queste posizioni per aprirne la pagina di panoramica. Nella pagina di panoramica copiare la **chiave di strumentazione**.

   ![Portale di Azure][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a>Configurare l'applicazione Spring Boot scaricata per l'uso di Application Insights

1. Individuare il file *POM.xml* nella directory radice dell'app e aggiungere la dipendenza seguente nella sezione delle dipendenze. 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.0.0-BETA</version>
</dependency>
```

1. Individuare il file *application.properties* nella directory *resources* dell'app o creare il file se non esiste già.

   ![Individuare il file application.properties][RE01]

1. Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate con le credenziali corrette:

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   Per altri modi di ottimizzare Application Insights, vedere il [file leggimi di Application Insights Spring Boot Starter](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).

   > [!NOTE]
   > 
   > È possibile usare chiavi di strumentazione di Application Insights diverse (ossia diverse risorse) per profili diversi, come PROD, DEV e così via. Per altre informazioni, vedere le [proprietà specifiche dei profili di Spring Boot]. 

1. Salvare e chiudere il file *application.properties*.

1. Creare una cartella denominata *controller* nella cartella di origine del pacchetto, ad esempio:

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   -oppure-

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. Creare un nuovo file denominato *TestController.java* nella cartella *controller*. Aprire il file in un editor di testo e aggiungere il codice seguente:

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   Sarà necessario sostituire `com.example.demo` con il nome del pacchetto per il progetto.

1. Salvare e chiudere il file *TestController.java*.

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Testare l'app Web passando a http://localhost:8080/sample/hello con un Web browser oppure usare una sintassi simile all'esempio seguente, se è disponibile curl:

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   Verrà visualizzato il messaggio "hello!" dal controller di esempio. Application Insights raccoglierà automaticamente la richiesta e la invierà come elemento di telemetria con l'evento, le metriche, la dipendenza e la traccia personalizzati associati, come specificato nella logica del controller. 

   Dopo alcuni secondi verranno visualizzati i dati nel portale di Azure. 

   ![Portale di Azure][AZ05]

   È possibile fare clic sul riquadro Mappa delle applicazioni per visualizzare i componenti di alto livello e la reciproca interazione. È consigliabile usare questo riquadro per ottenere una panoramica generale dell'intera applicazione. Ogni microservizio Spring Boot viene riconosciuto in base al nome dell'applicazione Spring. Ricordare di impostarlo.

   ![Portale di Azure][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a>Configurare l'applicazione Spring Boot per l'invio di log log4j ad Application Insights

1. Modificare il file POM.xml del progetto e aggiungere/modificare la sezione delle dipendenze come segue. 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.0.0-BETA</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. Salvare e chiudere il file *POM.xml*.

3. Nella cartella \src\main\resources creare un nuovo file *log4j2.xml* e configurarlo. Ad esempio: 

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```
4. Compilare ed eseguire di nuovo l'applicazione Spring Boot come illustrato sopra. 

In pochi secondi, nel portale di Azure verranno visualizzati tutti i log Spring disponibili. 

![Portale di Azure][AZ06]

È anche possibile esaminare i messaggi di log dettagliati ed eseguire analisi nel portale Analisi. 

![Portale di Azure][AZ07]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure](deploy-spring-boot-java-app-on-kubernetes.md)

Application Insights supporta la raccolta automatica delle dipendenze esterne e la correlazione con le richieste in ingresso. La raccolta automatica è attualmente supportata per Oracle, MsSQL, MySQL e Redis. Per altri dettagli sull'abilitazione della raccolta automatica, vedere l'articolo su [come usare l'agente Java di Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-java-agent).

Per altre informazioni su Azure Application Insights e sulle relative funzionalità di monitoraggio, vedere la home page di **[Application Insights]**.

Per altre informazioni su dettagli aggiuntivi della configurazione di Application Insights Spring Boot Starter, vedere questo [collegamento](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).

Per richieste di funzionalità e potenziali bug, aprire problemi nel repository [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues).

Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Strumenti Java per Visual Studio Team Services].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, diversi pacchetti Spring Boot di esempio sono disponibili all'indirizzo [https://github.com/spring-guides/](https://github.com/spring-guides/). Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Strumenti Java per Visual Studio Team Services]: https://java.visualstudio.com/
[Vantaggi per gli abbonati MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Proprietà specifiche dei profili di Spring Boot]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)
[Application Insights]: https://docs.microsoft.com/en-us/azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
