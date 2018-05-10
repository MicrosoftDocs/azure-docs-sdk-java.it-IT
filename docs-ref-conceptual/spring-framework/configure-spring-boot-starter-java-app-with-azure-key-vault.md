---
title: Come usare l'utilità di avvio Spring Boot per Azure Key Vault
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Key Vault.
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 1dda697cac80a6cad3ebbbbf8a5a4f18b515dfd8
ms.sourcegitcommit: 798f4d4199d3be9fc5c9f8bf7a754d7393de31ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>Come usare l'utilità di avvio Spring Boot per Azure Key Vault

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'app con **[Spring Initializr]**, che usa l'utilità di avvio Spring Boot per Azure Key Vault per recuperare una stringa di connessione archiviata come segreto in un insieme di credenziali delle chiavi.

## <a name="prerequisites"></a>prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) versione 1.7 o successiva.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-an-app-using-the-spring-initialzr"></a>Creare un'app usando Spring Initialzr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi di **Group** (Gruppo) e **Artifact** (Elemento) per l'applicazione e quindi fare clic sul collegamento relativo a **Switch to the full version** (Passa alla versione completa) di Spring Initializr.

   ![Specificare il nome del gruppo e dell'elemento][secrets-01]

1. Scorrere verso il basso fino alla sezione **Azure** e selezionare la casella **Azure Key Vault**.

   ![Selezionare l'utilità di avvio Azure Key Vault][secrets-02]

1. Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).

   ![Generare il progetto Spring Boot][secrets-03]

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>Accedere ad Azure e selezionare la sottoscrizione da usare

1. Aprire un prompt dei comandi.

1. Accedere all'account Azure con l'interfaccia della riga di comando di Azure:

   ```azurecli
   az login
   ```
   Seguire le istruzioni per completare il processo di accesso.

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```
   Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. Specificare il GUID per l'account che si vuole usare con Azure, ad esempio:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a>Creare e configurare un nuovo insieme di credenziali delle chiavi di Azure usando l'interfaccia della riga di comando di Azure

1. Creare un gruppo di risorse per le risorse di Azure che verranno usate per l'insieme di credenziali delle chiavi, ad esempio:
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `name` | Specifica un nome univoco per il gruppo di risorse. |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del gruppo di risorse, ad esempio:  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

1. Creare un'entità servizio di Azure dalla registrazione per l'applicazione, ad esempio:
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `name` | Specifica il nome dell'entità servizio di Azure. |

   L'interfaccia della riga di comando di Azure restituirà un messaggio di stato JSON contenente *appId* e *password*, che si useranno in seguito come ID client e password client, ad esempio:

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

1. Creare un nuovo insieme di credenziali delle chiavi nel gruppo di risorse, ad esempio:
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `name` | Specifica un nome univoco per l'insieme di credenziali delle chiavi. |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse. |
   | `enabled-for-deployment` | Specifica l'[opzione di distribuzione dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/en-us/cli/azure/keyvault). |
   | `enabled-for-disk-encryption` | Specifica l'[opzione di crittografia dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/en-us/cli/azure/keyvault). |
   | `enabled-for-template-deployment` | Specifica l'[opzione di crittografia dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/en-us/cli/azure/keyvault). |
   | `sku` | Specifica l'[opzione SKU dell'insieme di credenziali delle chiavi](https://docs.microsoft.com/en-us/cli/azure/keyvault). |
   | `query` | Specifica un valore da recuperare dalla risposta, corrispondente all'URI dell'insieme di credenziali delle chiavi che sarà necessario per completare questa esercitazione. |

   L'interfaccia della riga di comando di Azure visualizzerà l'URI per l'insieme di credenziali delle chiavi, che verrà usato in un secondo momento, ad esempio:  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

1. Impostare il criterio di accesso per l'entità servizio di Azure creata prima, ad esempio:
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `name` | Specifica il nome dell'insieme di credenziali delle chiavi precedente. |
   | `secret-permission` | Specifica i [criteri di sicurezza](https://docs.microsoft.com/en-us/cli/azure/keyvault) per l'insieme di credenziali delle chiavi. |
   | `spn` | Specifica il GUID della registrazione per l'applicazione precedente. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione dei criteri di sicurezza, ad esempio:  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "wingtiptoyskeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "wingtiptoysresources",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

1. Archiviare un segreto nel nuovo insieme di credenziali delle chiavi, ad esempio:
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `vault-name` | Specifica il nome dell'insieme di credenziali delle chiavi precedente. |
   | `name` | Specifica il nome del segreto. |
   | `value` | Specifica il valore del segreto. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del segreto, ad esempio:  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://wingtiptoyskeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://wingtiptoys.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a>Configurare e compilare l'applicazione Spring Boot

1. Estrarre i file dai file di archivio del progetto Spring Boot scaricati prima in una directory.

1. Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.

1. Aggiungere i valori per l'insieme di credenziali delle chiavi usando i valori dei passaggi completati prima in questa esercitazione, ad esempio:
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   Dove:
   | Parametro | DESCRIZIONE |
   |---|---|
   | `azure.keyvault.uri` | Specifica l'URI di quando è stato creato l'insieme di credenziali delle chiavi. |
   | `azure.keyvault.client-id` | Specifica il GUID *appId* di quando è stata creata l'entità servizio. |
   | `azure.keyvault.client-key` | Specifica il GUID *password* di quando è stata creata l'entità servizio. |

1. Passare al file di codice sorgente principale del progetto, ad esempio: */src/main/java/com/wingtiptoys/secrets*.

1. Aprire il file Java principale dell'applicazione in un file in un editor di testo, ad esempio: *SecretsApplication.java* e aggiungere le righe seguenti al file:

   ```java
   package com.wingtiptoys.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   Questo esempio di codice recupera la stringa di connessione dall'insieme di credenziali delle chiavi e lo visualizza nella riga di comando.

1. Salvare e chiudere il file Java.

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Passare alla directory in cui si trova il file *pom.xml* per l'app Spring Boot:

1. Compilare l'applicazione Spring Boot con Maven, ad esempio:

   ```bash
   mvn clean package
   ```

   Maven visualizzerà i risultati della compilazione.

   ![Stato di compilazione dell'applicazione Spring Boot][build-application-01]

1. Eseguire l'applicazione Spring Boot con Maven. L'applicazione visualizzerà la stringa di connessione dall'insieme di credenziali delle chiavi, Ad esempio: 

   ```bash
   mvn spring-boot:run
   ```

   ![Messaggio in fase di esecuzione di Spring Boot][build-application-02]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'uso di Azure Key Vault, vedere gli articoli seguenti:

* [Documentazione su Key Vault].

* [Introduzione all'insieme di credenziali delle chiavi di Azure]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-web-app-on-azure.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio contenitore di Azure](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni su come usare Azure con Java, vedere [Azure per sviluppatori Java] e [Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)].

<!-- URL List -->

[Documentazione su Key Vault]: /azure/key-vault/
[Introduzione all'insieme di credenziali delle chiavi di Azure]: /azure/key-vault/key-vault-get-started
[Azure per sviluppatori Java]: https://docs.microsoft.com/java/azure/
[account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services (Strumenti Java per Visual Studio Team Services)]: https://java.visualstudio.com/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
