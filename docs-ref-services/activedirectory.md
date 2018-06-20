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
ms.locfileid: "31823784"
---
# <a name="azure-active-directory-libraries-for-java"></a>Librerie di Azure Active Directory per Java

## <a name="overview"></a>Panoramica

Con [Azure Active Directory](/azure/active-directory/active-directory-whatis) è possibile consentire l'accesso degli utenti e controllare l'accesso ad applicazioni e API.

Per iniziare a usare Azure AD, vedere [Accesso e disconnessione all'app Web Java con Azure AD](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java).

## <a name="client-library"></a>Libreria client

Configurare l'autenticazione OAuth2, OpenID Connect o Active Directory Graph e l'accesso Single Sign-On [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) con [Azure Active Directory Authentication Library (ADAL) per Java](https://github.com/AzureAD/azure-activedirectory-library-for-java).

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare la libreria client nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a>Esempio

Recuperare un token JSON Web (token JWT) per un utente in un tenant di Active Directory usando l'[API Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) di Azure Active Directory. Questo token può quindi essere usato per autenticare l'utente in un'applicazione o un'API.

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

## <a name="management-api"></a>API di gestione

Configurare il [controllo degli accessi in base al ruolo](/azure/active-directory/role-based-access-control-what-is) e assegnare le identità (ad esempio gli utenti e le [entità servizio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) a tali ruoli con l'API di gestione. 

[Aggiungere una dipendenza](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) al file `pom.xml` di Maven per usare l'API di gestione nel progetto.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a>Esempio 

Creare una nuova entità servizio e assegnare il ruolo Collaboratore.

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
> [Esplorare le API di gestione](/java/api/overview/azure/activedirectory/management)


## <a name="samples"></a>Esempi

[Gestire gruppi, utenti e ruoli](https://github.com/Azure-Samples/aad-java-browse-graph-and-manage-roles)    
[Consentire l'accesso e la disconnessione degli utenti in un'app Web Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)    
[Accedere a un'API con Azure AD usando un'app della riga di comando](https://github.com/Azure-Samples/active-directory-java-native-headless)   
[Chiamare l'API Graph di Azure AD da un'app Web Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web/)  

Esplorare altri [esempi di codice Java per Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java) disponibili per l'uso nelle app.
