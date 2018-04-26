---
title: Librerie di Azure Active Directory per Java
description: Documentazione di riferimento per le librerie di gestione e client di Azure Active Directory per Java
keywords: Azure, Java, SDK, API, SQL, autenticazione, AAD, Active Directory, Graph, OAuth 2.0
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: active-directory
ms.openlocfilehash: 28063a1a4299fd78ba76533d0ffdc0346434eea2
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="azure-active-directory-libraries-for-java"></a><span data-ttu-id="5d180-104">Librerie di Azure Active Directory per Java</span><span class="sxs-lookup"><span data-stu-id="5d180-104">Azure Active Directory libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="5d180-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5d180-105">Overview</span></span>

<span data-ttu-id="5d180-106">Con [Azure Active Directory](/azure/active-directory/active-directory-whatis) è possibile consentire l'accesso degli utenti e controllare l'accesso ad applicazioni e API.</span><span class="sxs-lookup"><span data-stu-id="5d180-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

<span data-ttu-id="5d180-107">Per iniziare a usare Azure AD, vedere [Accesso e disconnessione all'app Web Java con Azure AD](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java).</span><span class="sxs-lookup"><span data-stu-id="5d180-107">To get started with Azure AD, see [Java web app sign-in and sign-out with Azure AD](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="5d180-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="5d180-108">Client library</span></span>

<span data-ttu-id="5d180-109">Configurare l'autenticazione OAuth2, OpenID Connect o Active Directory Graph e l'accesso Single Sign-On [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) con [Azure Active Directory Authentication Library (ADAL) per Java](https://github.com/AzureAD/azure-activedirectory-library-for-java).</span><span class="sxs-lookup"><span data-stu-id="5d180-109">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Java](https://github.com/AzureAD/azure-activedirectory-library-for-java).</span></span>

<span data-ttu-id="5d180-110">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.</span><span class="sxs-lookup"><span data-stu-id="5d180-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="5d180-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="5d180-111">Example</span></span>

<span data-ttu-id="5d180-112">Recuperare un token JSON Web (token JWT) per un utente in un tenant di Active Directory usando l'[API Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d180-112">Retrieve a JSON Web Token (JWT) for a user in your an Active Directory tenant using Azure Active Directory's [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api).</span></span> <span data-ttu-id="5d180-113">Questo token può quindi essere usato per autenticare l'utente in un'applicazione o un'API.</span><span class="sxs-lookup"><span data-stu-id="5d180-113">This token can then be used to authenticate the user with an application or API.</span></span>

```java
ExecutorService service = Executors.newFixedThreadPool(1);
AuthenticationContext context = new AuthenticationContext(AUTHORITY, false, service);
Future<AuthenticationResult> future = context.acquireToken(
    "https://graph.windows.net", YOUR_TENANT_ID, username, password,
    null);
AuthenticationResult result = future.get();
System.out.println("Access Token - " + result.getAccessToken());
System.out.println("Refresh Token - " + result.getRefreshToken());
System.out.println("ID Token - " + result.getIdToken());
```

## <a name="management-api"></a><span data-ttu-id="5d180-114">API di gestione</span><span class="sxs-lookup"><span data-stu-id="5d180-114">Management API</span></span>

<span data-ttu-id="5d180-115">Configurare il [controllo degli accessi in base al ruolo](/azure/active-directory/role-based-access-control-what-is) e assegnare le identità (ad esempio gli utenti e le [entità servizio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) a tali ruoli con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="5d180-115">Configure [role based access control](/azure/active-directory/role-based-access-control-what-is) and assign identities (such as users and [service principals](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) to those roles with the management API.</span></span> 

<span data-ttu-id="5d180-116">[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="5d180-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="5d180-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="5d180-117">Example</span></span> 

<span data-ttu-id="5d180-118">Creare una nuova entità servizio e assegnare il ruolo Collaboratore.</span><span class="sxs-lookup"><span data-stu-id="5d180-118">Create a new service principal and assign it the Contributor role.</span></span>

```java
ServicePrincipal sp = Azure.servicePrincipals().define(spName)
    .withNewApplication("http://" + spName)
    .create();
RoleAssignment roleAssignment2 = authenticated.roleAssignments()
    .define("contribRoleAssignment")
    .forServicePrincipal(sp)
    .withBuiltInRole(BuiltInRole.CONTRIBUTOR)
    .withSubscriptionScope("862f67bc-d3ae-4243-bec7-3da6dca77717")
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d180-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="5d180-119">Explore the Management APIs</span></span>](/java/api/overview/azure/activedirectory/management)


## <a name="samples"></a><span data-ttu-id="5d180-120">Esempi</span><span class="sxs-lookup"><span data-stu-id="5d180-120">Samples</span></span>

<span data-ttu-id="5d180-121">[Gestire gruppi, utenti e ruoli](https://github.com/Azure-Samples/aad-java-browse-graph-and-manage-roles)  </span><span class="sxs-lookup"><span data-stu-id="5d180-121">[Manage groups, users, and roles](https://github.com/Azure-Samples/aad-java-browse-graph-and-manage-roles)  </span></span>  
<span data-ttu-id="5d180-122">[Consentire l'accesso e la disconnessione degli utenti in un'app Web Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span><span class="sxs-lookup"><span data-stu-id="5d180-122">[Sign-in and sign-out users in a Java web app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span></span>  
<span data-ttu-id="5d180-123">[Accedere a un'API con Azure AD usando un'app della riga di comando](https://github.com/Azure-Samples/active-directory-java-native-headless) </span><span class="sxs-lookup"><span data-stu-id="5d180-123">[Access an API with Azure AD using a command line app](https://github.com/Azure-Samples/active-directory-java-native-headless) </span></span>  
[<span data-ttu-id="5d180-124">Chiamare l'API Graph di Azure AD da un'app Web Java</span><span class="sxs-lookup"><span data-stu-id="5d180-124">Call the Active AD Graph API from your Java web app</span></span>](https://github.com/Azure-Samples/active-directory-java-graphapi-web/)  

<span data-ttu-id="5d180-125">Esplorare altri [esempi di codice Java per Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="5d180-125">Explore more [sample Java code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java) you can use in your apps.</span></span>
