---
title: Distribuire un'app Web Spring Boot nel servizio app di Azure per Container
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot come app Web Linux in Microsoft Azure.
services: azure app service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: azure app service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 407b852e24ef88d2fb075bd064f1acf2b107ddc1
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270861"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a>Distribuire un'applicazione Web Spring Boot nel servizio app di Azure per Container

Questa esercitazione illustra l'uso di [Docker] per distribuire l'applicazione [Spring Boot] in contenitori e distribuire l'immagine Docker in un host Linux nel [servizio app di Azure](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura di questa esercitazione, sono necessari i prerequisiti seguenti:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* [Interfaccia della riga di comando di Azure].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* Lo strumento di compilazione [Maven] di Apache (versione 3).
* Un client [Git].
* Un client [Docker].

> [!NOTE]
>
> A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Creare l'app Web introduttiva di Spring Boot in Docker

I passaggi seguenti illustrano i passaggi necessari per creare una semplice applicazione Web Spring Boot e testarla localmente.

1. Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   - o-
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory appena creata, ad esempio:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Passare alla directory del progetto completato. Ad esempio:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Compilare il file JAR usando Maven. Ad esempio:
   ```
   mvn package
   ```

1. Dopo aver creato l'app Web, passare alla directory `target` in cui si trova il file JAR e avviare l'app Web. Ad esempio:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Testare l'app Web esplorandola localmente tramite un Web browser. Ad esempio, se è disponibile un cURL e il server Tomcat è configurato per l'esecuzione sulla porta 80:
   ```
   curl http://localhost
   ```

1. Dovrebbe essere visualizzato il messaggio **Hello Docker World**

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Creare un'istanza di Registro Azure Container da usare come registro Docker privato

La procedura seguente illustra come usare il portale di Azure per creare un'istanza di Registro Azure Container.

> [!NOTE]
>
> Se si vuole usare l'interfaccia della riga di comando di Azure anziché il portale di Azure, seguire i passaggi in [Creare un registro per contenitori Docker privati usando l'interfaccia della riga di comando di Azure 2.0](/azure/container-registry/container-registry-get-started-azure-cli).
>

1. Aprire il [portale di Azure] ed effettuare l'accesso.

   Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.

1. Fare clic sull'icona di menu **+ Nuovo**, su **Contenitori** e quindi su **Registro Azure Container**.
   
   ![Creare una nuova istanza di Registro Azure Container][AR01]

1. Quando viene visualizzata la pagina delle informazioni per il modello di Registro Azure Container, fare clic su **Crea**. 

   ![Creare una nuova istanza di Registro Azure Container][AR02]

1. Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.

   ![Configurare le impostazioni di Registro Azure Container][AR03]

1. Una volta creato il registro contenitori, passare al registro contenitori stesso nel portale di Azure e quindi fare clic su **Chiavi di accesso**. Prendere nota del nome utente e della password per i passaggi successivi.

   ![Chiavi di accesso a Registro Azure Container][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Configurare Maven per l'uso delle chiavi di accesso di Registro Azure Container

1. Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio "*C:\SpringBoot\gs-spring-boot-docker\complete*" o " */users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.

1. Aggiornare la raccolta `<properties>` nel file *pom.xml* con l'ultima versione di [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin), oltre che con il valore del server di accesso e le impostazioni di accesso per Registro Azure Container della sezione precedente di questa esercitazione. Ad esempio:

   ```xml
   <properties>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. Aggiungere [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) alla raccolta `<plugins>` nel file *pom.xml*, specificare l'immagine di base in `<from>/<image>` e il nome dell'immagine finale `<to>/<image>`, quindi specificare il nome utente e la password della sezione precedente in `<to>/<auth>`. Ad esempio:

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire il comando seguente per ricompilare l'applicazione ed effettuare il push del contenitore in Registro Azure Container:

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> Quando si usa Jib per eseguire il push dell'immagine nel Registro Azure Container, l'immagine non rispetterà *Dockerfile*. Per i dettagli, vedere [questo](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html) documento.
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Creare un'app Web in Linux nel servizio app di Azure usando l'immagine del contenitore

1. Aprire il [portale di Azure] ed effettuare l'accesso.

2. Fare clic sull'icona di menu **+ Crea una risorsa**, quindi su **Web** e infine su **App Web per contenitori**.
   
   ![Creare una nuova app Web nel portale di Azure][LX01]

3. Quando viene visualizzata la pagina **App Web in Linux**, immettere le informazioni seguenti:

   a. Immettere un nome univoco per **Nome app**, ad esempio "*wingtiptoyslinux*"

   b. Scegliere una **Sottoscrizione** dall'elenco a discesa.

   c. Selezionare un **Gruppo di risorse** esistente o specificare un nome per creare uno nuovo.

   d. Scegliere *Linux* come **Sistema operativo**.

   e. Fare clic su **Piano di servizio app/Località**  e scegliere un piano esistente oppure fare clic su **Crea nuovo** per crearne uno nuovo.

   f. Fare clic su **Configura contenitore** e immettere le informazioni seguenti:

   * Scegliere **Contenitore singolo** e **Registro Azure Container**.

   * **Registro di sistema**: scegliere il nome del contenitore creato in precedenza, ad esempio "*wingtiptoysregistry*"

   * **Immagine**: scegliere il nome dell'immagine, ad esempio "*gs-spring-boot-docker*"
   
   * **Tag**: scegliere il tag per l'immagine, ad esempio "*latest*"
   
   * **File di avvio**: lasciare vuoto questo campo perché l'immagine include già un comando di avvio
   
   e. Dopo aver immesso tutte queste informazioni, fare clic su **Applica**.

   ![Configurare le impostazioni dell'app Web][LX02]

4. Fare clic su **Create**(Crea).

> [!NOTE]
>
> Azure eseguirà automaticamente il mapping delle richieste Internet al server Tomcat incorporato in esecuzione sulla porta standard 80 o 8080. Se tuttavia il server Tomcat incorporato è stato configurato per l'esecuzione su una porta personalizzata, è necessario aggiungere all'app Web una variabile di ambiente che definisce la porta del server Tomcat incorporato. A tale scopo, seguire questa procedura:
>
> 1. Aprire il [portale di Azure] ed effettuare l'accesso.
> 
> 2. Fare clic sull'icona **Servizi app** e selezionare l'app Web nell'elenco.
>
> 4. Fare clic su **Configurazione**. (Elemento 1 nell'immagine seguente.)
>
> 5. Nella sezione **Impostazioni applicazione** aggiungere una nuova impostazione denominata **PORT** e immettere il numero di porta personalizzato come valore. (Elementi 2, 3 e 4 nell'immagine seguente.)
>
> 6. Fare clic su **Save**. (Elemento 5 nell'immagine seguente.)
>
> ![Salvataggio di un numero di porta personalizzato nel portale di Azure][LX03]
>

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

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)
* [Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per maggiori dettagli sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).

Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.

Per altre informazioni su come iniziare a creare una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.

Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure per sviluppatori Java]: /java/azure/
[Portale di Azure]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Uso di Azure DevOps e Java]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
