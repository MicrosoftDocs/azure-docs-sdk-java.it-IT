---
title: Distribuire un'app Spring Boot in Kubernetes nel servizio Azure Kubernetes
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot in un cluster Kubernetes in Microsoft Azure.
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 42bb030a916cc5aaf1e20242518a0a400b8baa88
ms.sourcegitcommit: f33befab25a66a252b4c91c7aeb1b77cb32821bb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2019
ms.locfileid: "59745159"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a>Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Kubernetes

**[Kubernetes]** e **[Docker]** sono soluzioni open source che consentono agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni eseguite in contenitori.

Questa esercitazione illustra come combinare queste due diffuse tecnologie open source per sviluppare e distribuire un'applicazione Spring Boot in Microsoft Azure. In particolare, si userà *[Spring Boot]* per lo sviluppo dell'applicazione, *[Kubernetes]* per la distribuzione del contenitore e il [servizio Azure Kubernetes] per l'hosting dell'applicazione.

### <a name="prerequisites"></a>Prerequisiti

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

La procedura seguente illustra come creare un'applicazione Web di Spring Boot e come testarla in locale.

1. Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- o --
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Passare alla directory del progetto completato.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Usare Maven per compilare ed eseguire l'app di esempio.
   ```
   mvn package spring-boot:run
   ```

1. Testare l'app Web passando a `http://localhost:8080` oppure con il comando `curl` seguente:
   ```
   curl http://localhost:8080
   ```

1. Dovrebbe essere visualizzato il messaggio **Hello Docker World**

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Creare un'istanza di Registro Azure Container usando l'interfaccia della riga di comando di Azure

1. Aprire un prompt dei comandi.

1. Accedere all'account di Azure:
   ```azurecli
   az login
   ```

1. Scegliere la sottoscrizione di Azure:
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. Creare un gruppo di risorse per le risorse di Azure usate in questa esercitazione.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Creare un registro contenitori privato di Azure nel gruppo di risorse. L'esercitazione effettua il push dell'app di esempio come immagine Docker in questo registro nei passaggi successivi. Sostituire `wingtiptoysregistry` con un nome univoco per il registro.
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>Eseguire il push dell'app nel registro contenitori tramite Jib

1. Accedere a Registro Azure Container dall'interfaccia della riga di comando di Azure.
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio, "*C:\SpringBoot\gs-spring-boot-docker\complete*" o "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.

1. Aggiornare la raccolta `<properties>` nel file *pom.xml* con il nome registro dell'istanza di Registro Azure Container e la versione più recente di [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.0.2</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aggiornare la raccolta `<plugins>` nel file *pom.xml* in modo che `<plugin>` contenga `jib-maven-plugin`.

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
        </to>
     </configuration>
   </plugin>
   ```

1. Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire questo comando per creare l'immagine ed eseguire il push dell'immagine nel registro:

   ```
   mvn compile jib:build
   ```

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>Creare un cluster Kubernetes nel servizio Azure Container usando l'interfaccia della riga di comando di Azure

1. Creare un cluster Kubernetes nel servizio Azure Kubernetes. Il comando seguente crea un cluster *kubernetes* nel gruppo di risorse *wingtiptoys-kubernetes*, con *wingtiptoys-akscluster* come nome del cluster e *wingtiptoys-kubernetes* come prefisso DNS:
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   Il completamento di questo comando può richiedere alcuni minuti.

1. Quando si usa Registro Azure Container con il servizio Azure Kubernetes è necessario concedere al servizio Azure Kubernetes l'accesso in pull a Registro Azure Container. Azure crea un'entità servizio predefinita quando si crea il servizio Azure Kubernetes. Eseguire questi script in bash o PowerShell per concedere al servizio Azure Kubernetes l'accesso a Registro Azure Container. Per altre informazioni, vedere [Eseguire l'autenticazione con Registro Azure Container dal servizio Azure Kubernetes].

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  -- o --

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. Installare `kubectl` usando l'interfaccia della riga di comando di Azure. È possibile che gli utenti Linux debbano aggiungere al comando il prefisso `sudo`, perché distribuisce l'interfaccia della riga di comando di Kubernetes in `/usr/local/bin`.
   ```azurecli
   az aks install-cli
   ```

1. Scaricare le informazioni sulla configurazione del cluster, in modo da consentire la gestione del cluster dall'interfaccia Web di Kubernetes e `kubectl`. 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>Distribuire l'immagine nel cluster Kubernetes

Questa esercitazione distribuisce l'app usando `kubectl`, quindi consente di esplorare la distribuzione tramite l'interfaccia Web di Kubernetes.

### <a name="deploy-with-kubectl"></a>Eseguire la distribuzione con kubectl

1. Aprire un prompt dei comandi.

1. Eseguire il contenitore nel cluster Kubernetes usando il comando `kubectl run`. Specificare un nome di servizio per l'app in Kubernetes e il nome completo dell'immagine. Ad esempio: 
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   In questo comando:

   * Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `run`.

   * Il parametro `--image` specifica il nome combinato del server di accesso e dell'immagine come `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`.

1. Esporre esternamente il cluster Kubernetes usando il comando `kubectl expose`. Specificare il nome del servizio, la porta TCP pubblica usata per accedere all'app e la porta di destinazione interna su cui è in ascolto l'app. Ad esempio: 
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   In questo comando:

   * Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `expose deployment`.

   * Il parametro `--type` specifica che il cluster usa il bilanciamento del carico

   * Il parametro `--port` specifica la porta TCP pubblica, ovvero 80. Si accede all'app tramite questa porta.

   * Il parametro `--target-port` specifica la porta TCP interna, ovvero 8080. Il servizio di bilanciamento del carico inoltra le richieste all'app su questa porta.

1. Dopo la distribuzione dell'app nel cluster, eseguire query sull'indirizzo IP esterno e aprirlo nel Web browser:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![Esplorare l'app di esempio in Azure][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a>Eseguire la distribuzione con l'interfaccia Web di Kubernetes

1. Aprire un prompt dei comandi.

1. Aprire il sito Web di configurazione per il cluster Kubernetes nel browser predefinito:
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. All'apertura del sito Web di configurazione di Kubernetes nel browser, fare clic sul collegamento **deploy a containerized app** (Distribuire un'app inclusa in contenitori):

   ![Sito Web di configurazione di Kubernetes][KB01]

1. Quando viene visualizzata la pagina **Resource Creation** (Creazione risorse), specificare le opzioni seguenti:

   a. Selezionare **CREATE AN APP** (CREA APP).

   b. Immettere il nome dell'applicazione Spring Boot per **App name** (Nome app), ad esempio: "*gs-spring-boot-docker*".

   c. Immettere il server di accesso e l'immagine del contenitore dai passaggi precedenti in **Container image** (Immagine del contenitore), ad esempio: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".

   d. Scegliere **External** (Esterno) per **Service** (Servizio).

   e. Specificare le porte esterne ed interne nelle caselle di testo **Port** (Porta) e **Target port** (Porta di destinazione).

   ![Sito Web di configurazione di Kubernetes][KB02]


1. Fare clic su **Deploy** (Distribuisci) per distribuire il contenitore.

   ![Distribuzione di Kubernetes][KB05]

1. Al termine della distribuzione dell'applicazione, l'applicazione Spring Boot verrà elencata in **Services** (Servizi).

   ![Servizi Kubernetes][KB06]

1. Se si fa clic sul collegamento per **External endpoints** (Endpoint esterni), è possibile visualizzare l'applicazione Spring Boot in esecuzione in Azure.

   ![Servizi Kubernetes][KB07]

   ![Esplorare l'app di esempio in Azure][SB02]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso di Spring Boot in Azure, vedere l'articolo seguente:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per altre informazioni sulla distribuzione di un'applicazione Java in Kubernetes con Visual Studio Code, vedere le [esercitazioni su Java in Visual Studio Code].

Per altre informazioni sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).

I collegamenti seguenti forniscono informazioni aggiuntive sulla creazione di applicazioni Spring Boot:

* Per altre informazioni sulla creazione di una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.

I collegamenti seguenti forniscono informazioni aggiuntive sull'uso di Kubernetes con Azure:

* [Introduzione ai cluster Kubernetes nel servizio Azure Kubernetes](/azure/aks/intro-kubernetes)

Per altre informazioni sull'uso dell'interfaccia della riga di comando di Kubernetes, vedere la guida dell'utente di **kubectl** all'indirizzo <https://kubernetes.io/docs/user-guide/kubectl/>.

Il sito Web Kubernetes include alcuni articoli relativi all'uso delle immagini nei registri privati:

* [Configuring Service Accounts for Pods] (Configurazione degli account del servizio per i pod)
* [Namespaces] (Spazi dei nomi)
* [Pulling an Image from a Private Registry] (Effettuare il pull di un'immagine da un registro privato)

Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].

Per altre informazioni sull'esecuzione iterativa e il debug di contenitori direttamente nel servizio Azure Kubernetes con Azure Dev Spaces, vedere [Guida introduttiva ad Azure Dev Spaces con Java]

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Servizio Azure Kubernetes]: https://azure.microsoft.com/services/kubernetes-service/
[Azure per sviluppatori Java]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Uso di Azure DevOps e Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/
[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/ (Configurazione degli account del servizio per i pod)
[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/ (Spazi dei nomi)
[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ (Effettuare il pull di un'immagine da un registro privato)

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[Eseguire l'autenticazione con Registro Azure Container dal servizio Azure Kubernetes]: /azure/container-registry/container-registry-auth-aks/
[Esercitazioni su Java in Visual Studio Code]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Guida introduttiva ad Azure Dev Spaces con Java]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
