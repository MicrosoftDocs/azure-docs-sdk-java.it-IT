---
title: Aggiungere un certificato radice per Azure all'archivio CA Java
description: Informazioni su come aggiungere un certificato radice dell'autorità di certificazione (CA) all'archivio certificati CA Java (cacerts) per usarlo con Microsoft Azure.
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 07/02/2018
ms.author: robmcm
ms.openlocfilehash: 3f2de63f7eb1422ff1dd6db45d68e02f4af188b8
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37864041"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a><span data-ttu-id="1c158-103">Aggiunta di un certificato radice all'archivio certificati CA Java</span><span class="sxs-lookup"><span data-stu-id="1c158-103">Adding a root certificate to the Java CA certificates store</span></span>

<span data-ttu-id="1c158-104">Le applicazioni che usano i servizi di Azure, ad esempio il bus di servizio di Azure, devono considerare attendibile il certificato radice Baltimore CyberTrust.</span><span class="sxs-lookup"><span data-stu-id="1c158-104">Applications that use Azure services (such as Azure Service Bus) need to trust the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="1c158-105">Questo certificato potrebbe essere già installato nel sistema. In caso contrario, i passaggi di questa esercitazione illustreranno come usare lo strumento **keytool** di Oracle per aggiungere il certificato radice dell'autorità di certificazione (CA) necessario all'archivio certificati CA Java (cacerts) che verrà usato per i servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="1c158-105">This certificate may already be installed on your system, but if it is not, the steps in this tutorial will show you how to use Oracle's **keytool** to add the required certificate authority (CA) root certificate to the Java CA certificate (cacerts) store that you will use for Azure services.</span></span>

<span data-ttu-id="1c158-106">L'utilità keytool di Oracle è uno _strumento di gestione di chiavi e certificati_ che consente agli sviluppatori di gestire l'elenco dei certificati attendibili da usare con Java.</span><span class="sxs-lookup"><span data-stu-id="1c158-106">Oracle's keytool utility is a _Key and Certificate Management Tool_, which allows developers to manage the list of trusted certificates for use with Java.</span></span> <span data-ttu-id="1c158-107">È possibile usare lo strumento Keytool per aggiungere il certificato della CA prima di comprimere il JDK e di aggiungerlo alla cartella **approot** del progetto Azure. In alternativa, è possibile eseguire un'attività di avvio in Azure che usa lo strumento Keytool per aggiungere il certificato.</span><span class="sxs-lookup"><span data-stu-id="1c158-107">You can use keytool to add the CA certificate before zipping your JDK and adding it to your Azure project's **approot** folder, or you could run an Azure start-up task that uses keytool to add the certificate.</span></span>

<span data-ttu-id="1c158-108">A partire dal 15 aprile 2013 è iniziata la migrazione di Azure dal certificato radice GTE CyberTrust Global al certificato radice Baltimore CyberTrust.</span><span class="sxs-lookup"><span data-stu-id="1c158-108">Beginning April 15, 2013, Azure began migrating from the GTE CyberTrust Global root certificate to the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="1c158-109">I passaggi seguenti illustrano come usare keytool per aggiungere il certificato radice Baltimore CyberTrust all'archivio certificati CA Java (cacerts).</span><span class="sxs-lookup"><span data-stu-id="1c158-109">The following steps show you how to use keytool to add the Baltimore CyberTrust root certificate to your Java CA certificate (cacerts) store.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="1c158-110">È possibile usare i passaggi descritti in questo articolo per configurare Java SDK in modo da considerare attendibili certificati radice di altre autorità di certificazione attendibili.</span><span class="sxs-lookup"><span data-stu-id="1c158-110">You can use the steps in this article to configure your Java SDK to trust the root certificates from other trusted certificate authorities.</span></span> <span data-ttu-id="1c158-111">Ad esempio, si può scegliere un certificato radice nell'elenco dei [certificati radice GeoTrust](http://www.geotrust.com/resources/root-certificates/).</span><span class="sxs-lookup"><span data-stu-id="1c158-111">For example, you might choose a root certificate from the list of certificates at [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/).</span></span>
> 

## <a name="determining-which-root-certificates-are-installed"></a><span data-ttu-id="1c158-112">Individuazione dei certificati radice installati</span><span class="sxs-lookup"><span data-stu-id="1c158-112">Determining which root certificates are installed</span></span>

<span data-ttu-id="1c158-113">Il certificato Baltimore potrebbe essere già installato nell'archivio cacerts. È quindi necessario seguire questa procedura per determinare se è già presente.</span><span class="sxs-lookup"><span data-stu-id="1c158-113">The Baltimore certificate might already be installed in your cacerts store, so you need to use the following steps to determine if it has already been installed.</span></span>

1. <span data-ttu-id="1c158-114">Al prompt dei comandi di amministratore passare alla cartella **jdk\jre\lib\security** di JDK e quindi eseguire questo comando per elencare i certificati installati nel sistema:</span><span class="sxs-lookup"><span data-stu-id="1c158-114">At an administrator command prompt, navigate to your JDK's **jdk\jre\lib\security** folder, and then run the following command to list the certificates that are installed on your system:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

1. <span data-ttu-id="1c158-115">Se viene richiesta la password dell'archivio, la password predefinita è **changeit**.</span><span class="sxs-lookup"><span data-stu-id="1c158-115">If you are prompted for the store password, the default password is **changeit**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="1c158-116">Se si vuole cambiare la password dell'archivio, vedere la documentazione di keytool all'indirizzo <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span><span class="sxs-lookup"><span data-stu-id="1c158-116">If you want to change the store password, see the keytool documentation at <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>
   > 

1. <span data-ttu-id="1c158-117">Se non viene visualizzato un certificato con l'identificazione personale `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, usare la procedura descritta nella sezione seguente per scaricare e installare il certificato.</span><span class="sxs-lookup"><span data-stu-id="1c158-117">If you do not see the certificate with the thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, use the steps in the following section to download and install the certificate.</span></span>

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a><span data-ttu-id="1c158-118">Per aggiungere un certificato radice all'archivio cacerts</span><span class="sxs-lookup"><span data-stu-id="1c158-118">To add a root certificate to the cacerts store</span></span>

1. <span data-ttu-id="1c158-119">Scaricare il certificato radice Baltimore CyberTrust da <https://cacert.omniroot.com/bc2025.crt> e salvarlo in un file locale con estensione **cer** nella cartella **jdk\jre\lib\security**.</span><span class="sxs-lookup"><span data-stu-id="1c158-119">Download the Baltimore CyberTrust root certificate from <https://cacert.omniroot.com/bc2025.crt>, and save to a local file with extension **.cer** in your **jdk\jre\lib\security** folder.</span></span> <span data-ttu-id="1c158-120">Per questo esempio, si supponga di aver scaricato il certificato radice Baltimore CyberTrust con il nome **bc2025.cer**.</span><span class="sxs-lookup"><span data-stu-id="1c158-120">For this example, assume that you downloaded the Baltimore CyberTrust root certificate file as **bc2025.cer**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="1c158-121">Il certificato radice Baltimore CyberTrust ha il numero di serie `02:00:00:b9` e l'identificazione personale SHA1 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.</span><span class="sxs-lookup"><span data-stu-id="1c158-121">The Baltimore CyberTrust root certificate has a serial number of `02:00:00:b9`, and a SHA1 thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.</span></span>
   > 

2. <span data-ttu-id="1c158-122">Importare il certificato nell'archivio cacerts con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1c158-122">Import the certificate to the cacerts store by using the following command:</span></span>

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   <span data-ttu-id="1c158-123">Dove:</span><span class="sxs-lookup"><span data-stu-id="1c158-123">Where:</span></span>

   |  <span data-ttu-id="1c158-124">Parametro</span><span class="sxs-lookup"><span data-stu-id="1c158-124">Parameter</span></span>   |                              <span data-ttu-id="1c158-125">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="1c158-125">Description</span></span>                               |
   |--------------|------------------------------------------------------------------------|
   | `keystore`   | <span data-ttu-id="1c158-126">Specifica l'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="1c158-126">Specifies the certificate store.</span></span>                                       |
   | `importcert` | <span data-ttu-id="1c158-127">Specifica che si sta importando un certificato.</span><span class="sxs-lookup"><span data-stu-id="1c158-127">Specifies that you are importing a certificate.</span></span>                        |
   | `alias`      | <span data-ttu-id="1c158-128">Specifica un alias per il certificato.</span><span class="sxs-lookup"><span data-stu-id="1c158-128">Specifies an alias for the certificate.</span></span>                                |
   | `file`       | <span data-ttu-id="1c158-129">Specifica il nome file del certificato radice che viene importato.</span><span class="sxs-lookup"><span data-stu-id="1c158-129">Specifies the filename of the root certificate that you are importing.</span></span> |


3. <span data-ttu-id="1c158-130">Se viene richiesto di considerare attendibile il certificato, verificare che l'identificazione personale sia `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` e digitare **y** se è corretta.</span><span class="sxs-lookup"><span data-stu-id="1c158-130">If you are prompted to trust the certificate, verify the thumbprint as `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, and type **y** if the thumbprint is correct.</span></span>

4. <span data-ttu-id="1c158-131">Eseguire il comando seguente per verificare se il certificato CA è stato importato correttamente:</span><span class="sxs-lookup"><span data-stu-id="1c158-131">Run the following command to ensure the CA certificate has been successfully imported:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

<span data-ttu-id="1c158-132">Dopo aver aggiunto il certificato radice a JDK, è possibile comprimere il contenuto di JDK e aggiungerlo alla cartella **approot** del progetto Azure.</span><span class="sxs-lookup"><span data-stu-id="1c158-132">After you have successfully added the root certificate to your JDK, you can zip the contents of JDK and add it to your Azure project's **approot** folder.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c158-133">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="1c158-133">Next steps</span></span>

<span data-ttu-id="1c158-134">Per altre informazioni sull'utilità keytool, vedere <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span><span class="sxs-lookup"><span data-stu-id="1c158-134">For more information about the keytool utility, see <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>

<span data-ttu-id="1c158-135">Per altre informazioni su Java, vedere [Azure for Java developers](/java/azure) (Azure per sviluppatori Java).</span><span class="sxs-lookup"><span data-stu-id="1c158-135">For more information about Java, see [Azure for Java developers](/java/azure).</span></span>

<!-- For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx). -->
