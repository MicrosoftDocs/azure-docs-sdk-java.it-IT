---
title: Configurare MicroProfile con Azure Key Vault
description: Informazioni su come inserire i segreti in un servizio web MicroProfile con Azure Key Vault
services: key-vault
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: fa93788f9b600d963f35ea599a58d309d3a3ee52
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240824"
---
# <a name="configure-microprofile-with-azure-key-vault"></a>Configurare MicroProfile con Azure Key Vault

Questa esercitazione illustra come configurare un'applicazione [MicroProfile](http://microprofile.io) per il recupero di segreti da [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) usando le [API MicroProfile Config](https://microprofile.io/project/eclipse/microprofile-config) per creare una connessione diretta ad Azure Key Vault. Con le API MicroProfile Config, gli sviluppatori hanno a disposizione un'API standard per il recupero e l'inserimento dei dati di configurazione nei propri microservizi.

Prima di passare ai dettagli, si esaminerà rapidamente la combinazione di Azure Key Vault e API MicroProfile Config che consente di scrivere nel proprio codice. Di seguito è riportato un frammento di codice di un campo in una classe annotato con `@Inject` e `@ConfigProperty`. Il valore `name` specificato nell'annotazione è il nome della proprietà da cercare in Azure Key Vault, mentre `defaultValue` è il valore che verrà impostato se la chiave non viene rilevata. Il risultato finale è che il valore archiviato in Azure Key Vault, o il valore predefinito, verrà inserito automaticamente nel campo in fase di esecuzione, semplificando il lavoro degli sviluppatori che non dovranno più passare i valori tra costruttori e metodi setter, ma lasceranno la gestione di queste attività a MicroProfile.

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

È anche possibile accedere direttamente a MicroProfile Config per richiedere i segreti necessari, ad esempio:

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

Questo esempio usa [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile](https://microprofile.io/) per creare un piccolo file WAR Java che può essere eseguito localmente nel computer. Non dimostra come inserire il codice in contenitori Docker o eseguirne il push in Azure, ma la sezione dei collegamenti al termine di questa esercitazione contiene rimandi ad altre utili esercitazioni che illustrano queste operazioni.

Questo esempio usa una libreria gratuita e open source che crea un'origine di configurazione con l'API MicroProfile Config per Azure Key Vault. Per altre informazioni su questa libreria e per recuperare il codice, vedere la [pagina GitHub del progetto](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault). Usando questa libreria, il codice riportato in questa esercitazione si concentra solo sulla configurazione della libreria stessa seguita dall'inserimento delle chiavi nel codice. Non è necessario scrivere codice specifico per Azure.

Di seguito sono indicati i passaggi necessari per eseguire questo codice nel computer locale, iniziando con la creazione di una risorsa di Azure Key Vault.

## <a name="creating-an-azure-key-vault-resource"></a>Creazione di una risorsa di Azure Key Vault

Si userà l'interfaccia della riga di comando di Azure per creare la risorsa di Azure Key Vault e popolarla con un solo segreto.

1. Verrà prima creata un'entità servizio di Azure per ottenere l'ID client e la chiave per l'accesso a Key Vault:

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

Si userà `microprofile-keyvault-service-principal` come nome dell'entità servizio creata nel passaggio precedente. La risposta di Azure per questa operazione sarà nella forma seguente, qui leggermente offuscata:

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

Di particolare importanza sono i valori `appID` e `password`, che in seguito verranno usati rispettivamente come `client ID` e `key`.

Ora che è stata creata l'entità servizio, è facoltativamente possibile creare un gruppo di risorse. Se è già disponibile un gruppo di risorse, è possibile omettere questo passaggio. Si noti che per ottenere un elenco di posizioni di gruppi di risorse è possibile chiamare `az account list-locations` e usare il valore `name` di tale elenco per specificare la posizione in cui creare il gruppo di risorse.

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

Verrà ora creata una risorsa di Azure Key Vault. Si noti che il nome dell'istanza di Key Vault verrà usato per fare riferimento all'istanza in seguito, quindi scegliere un nome facile da ricordare.

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

È anche necessario concedere le autorizzazioni appropriate all'entità servizio creata in precedenza, affinché possa accedere ai segreti di Key Vault. Si noti che il valore appID è il valore `appId` del passaggio in cui è stata creata l'entità servizio, ovvero `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79`. Usare tuttavia l'output del terminale.

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

Ora è possibile eseguire il push di un segreto in Key Vault. Si userà il nome chiave `demo-key` e il valore della chiave sarà `demo-value`:

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

L'operazione è terminata. Key Vault è ora in esecuzione in Azure con un solo segreto. A questo punto è possibile clonare questo repository e configurarlo per l'uso di questa risorsa nell'app.

## <a name="getting-up-and-running-locally"></a>Esecuzione in locale

Questo esempio si basa su un'applicazione di esempio disponibile in GitHub, che verrà clonata per poi esaminarne il codice. Seguire questa procedura per clonare il codice nel computer:

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. Passare a `src/main/resources/META-INF/microprofile-config.properties` e modificare le proprietà nel file microprofile-config.properties con i dettagli precedenti.

1. Provare a eseguire il server usando `mvn clean package payara-micro:start`.

1. Provare ad accedere a [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) nel Web browser. Verrà visualizzata una risposta semplice che indica la lettura dei valori da Azure Key Vault.

## <a name="summary"></a>Summary

Questa applicazione di esempio usa l'API MicroProfile Config, Azure Key Vault e la libreria [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) gratuita e open source per consentire un facile inserimento di dati di configurazione e segreti nei propri servizi Web MicroProfile.
