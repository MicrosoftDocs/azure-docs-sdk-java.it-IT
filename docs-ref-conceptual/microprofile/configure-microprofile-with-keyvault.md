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
ms.openlocfilehash: c405711813176823f2ddee6b4002f75c2b25fdb5
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533618"
---
# <a name="configure-microprofile-by-using-azure-key-vault"></a>Configurare MicroProfile con Azure Key Vault

Questo articolo illustra come configurare un'applicazione [MicroProfile](http://microprofile.io) per il recupero di segreti da un [insieme di credenziali delle chiavi di Azure](https://azure.microsoft.com/services/key-vault/). In questo processo si usano le [API MicroProfile Config](https://microprofile.io/project/eclipse/microprofile-config) per creare una connessione diretta a un insieme di credenziali delle chiavi di Azure. Con le API MicroProfile Config, gli sviluppatori hanno a disposizione un'API standard per il recupero e l'inserimento dei dati di configurazione nei propri microservizi.

Prima di iniziare, esaminare rapidamente la combinazione di Azure Key Vault e API MicroProfile Config che consente di scrivere nel proprio codice. Il frammento di codice seguente si riferisce a un campo in una classe annotato con `@Inject` e `@ConfigProperty`. Il valore *name* specificato nell'annotazione corrisponde al nome della proprietà da cercare nell'insieme di credenziali delle chiavi, mentre *defaultValue* è il valore che verrà impostato se la chiave non viene rilevata. Il risultato è che il valore archiviato nell'insieme di credenziali delle chiavi, ovvero il valore predefinito, viene inserito automaticamente nel campo in fase di esecuzione. Questa azione può semplificare le operazioni, in quanto non è più necessario passare valori in costruttori e metodi setter. Questa attività viene infatti gestita da MicroProfile.

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

Per richiedere i segreti, è anche possibile accedere direttamente a MicroProfile Config. Ad esempio:

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

In questo esempio di codice si usa [Payara Micro](https://www.payara.fish/payara_micro) e [MicroProfile](https://microprofile.io/) per creare un piccolo file WAR (Web Application Requirement) Java che è possibile eseguire localmente nel computer. Non viene illustrato come inserire il codice in contenitori Docker o eseguirne il push in Azure, ma la sezione al termine di questo articolo contiene collegamenti ad altre utili esercitazioni che illustrano queste operazioni.

Nell'esempio si usa una libreria gratuita e open source che crea un'origine di configurazione con l'API MicroProfile Config nell'insieme di credenziali delle chiavi. Per altre informazioni su questa libreria e per esaminare il codice, vedere la [pagina GitHub del progetto](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault). Se si usa la libreria, il codice usato in questa esercitazione si concentra solo sulla configurazione della libreria stessa seguita dall'inserimento delle chiavi nel codice. Non è necessario scrivere codice specifico per Azure.

Per eseguire questo codice nel computer locale, iniziando con la creazione di una risorsa dell'insieme di credenziali delle chiavi, seguire le istruzioni fornite nelle sezioni successive.

## <a name="create-a-key-vault-resource"></a>Creare una risorsa dell'insieme di credenziali delle chiavi

In questa sezione si usa l'interfaccia della riga di comando di Azure per creare una risorsa dell'insieme di credenziali delle chiavi e popolarla con un solo segreto.

1. Creare un'entità servizio di Azure. Questo passaggio consente di ottenere l'ID client e la chiave necessarie per accedere all'insieme di credenziali delle chiavi:

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    Si userà *microprofile-keyvault-service-principal* per sostituire *\<service_principal_name>* nel passaggio precedente. La risposta restituita da Azure sarà simile alla seguente:

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    Di particolare interesse sono i valori di *appId* e *password*. Tali valori verranno usati più avanti in questo articolo come *ID client* e *chiave*.

1. (Facoltativo) Dopo aver creato un'entità servizio, è possibile creare un gruppo di risorse. Se il gruppo di risorse che si vuole usare è già disponibile, è possibile ignorare questo passaggio. Per ottenere un elenco di località dei gruppi di risorse è possibile chiamare `az account list-locations` e usare il valore *name* di tale elenco per specificare la località in cui creare il gruppo di risorse.

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. Creare una risorsa dell'insieme di credenziali delle chiavi. Il nome dell'insieme di credenziali delle chiavi verrà usato per fare riferimento all'insieme, quindi scegliere un nome facile da ricordare.

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. Concedere le autorizzazioni appropriate all'entità servizio creata in precedenza, affinché possa accedere ai segreti dell'insieme di credenziali delle chiavi. Il valore di appId nel codice seguente corrisponde al valore di *appId* del passaggio 1, in cui è stata creata l'entità servizio. Il valore di *appId* nel passaggio 1 era *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*, ma è necessario usare il valore visualizzato nell'output del proprio terminale.

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. A questo punto è possibile eseguire il push di un segreto nell'insieme di credenziali delle chiavi. Usare *demo-key* come nome della chiave e impostarne il valore su *demo-value*:

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

L'operazione è terminata. È ora disponibile un insieme di credenziali delle chiavi in esecuzione in Azure con un singolo segreto. A questo punto è possibile clonare questo repository e configurarlo per l'uso di questa risorsa nell'app.

## <a name="get-up-and-running-locally"></a>Esecuzione in locale

Questo esempio si basa su un'applicazione di esempio disponibile in GitHub, che verrà clonata per poi esaminarne il codice. 

1. Clonare il file nel computer immettendo i comandi seguenti:

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. Passare a *src/main/resources/META-INF/microprofile-config.properties* e quindi modificare le proprietà nel file *microprofile-config.properties* usando i valori dei passaggi precedenti.

1. Provare a eseguire il server usando `mvn clean package payara-micro:start`.

1. Provare ad accedere a [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) nel Web browser. Verrà visualizzata una risposta semplice che indica la lettura dei valori dall'insieme di credenziali delle chiavi.

## <a name="summary"></a>Summary

Questa applicazione di esempio combina l'API MicroProfile Config, Azure Key Vault e la libreria gratuita e open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) per consentire un facile inserimento di segreti e dati di configurazione nei propri servizi Web MicroProfile.
