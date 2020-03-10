---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Authentification et autorisation dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Donne une vue d’ensemble de l’authentification et de l’autorisation dans API Web ASP.NET.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598643"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentication and Authorization in ASP.NET Web API (Authentification et autorisation dans l’API Web ASP.NET)

par [Mike Wasson](https://github.com/MikeWasson)

Vous avez créé une API Web, mais vous souhaitez maintenant contrôler l’accès à celle-ci. Dans cette série d’articles, nous allons examiner certaines options pour sécuriser une API Web contre les utilisateurs non autorisés. Cette série couvre à la fois l’authentification et l’autorisation.

- L' *authentification* est la connaissance de l’identité de l’utilisateur. Par exemple, Alice se connecte avec son nom d’utilisateur et son mot de passe, et le serveur utilise le mot de passe pour authentifier Alice.
- L' *autorisation* consiste à décider si un utilisateur est autorisé à effectuer une action. Par exemple, Alice a l’autorisation d’obtenir une ressource, mais pas de créer une ressource.

Le premier article de la série donne une vue d’ensemble de l’authentification et de l’autorisation dans API Web ASP.NET. D’autres rubriques décrivent des scénarios d’authentification courants pour l’API Web.

> [!NOTE]
> Merci aux personnes ayant consulté cette série et à fournir des commentaires précieux : Rick Anderson, Broderick, Barry Dorrans, Tom Dykstra, Hongmei GE, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Authentification

L’API Web suppose que l’authentification se produit dans l’hôte. Pour l’hébergement Web, l’hôte est IIS, qui utilise des modules HTTP pour l’authentification. Vous pouvez configurer votre projet pour utiliser l’un des modules d’authentification intégrés à IIS ou ASP.NET, ou écrire votre propre module HTTP pour effectuer une authentification personnalisée.

Lorsque l’hôte authentifie l’utilisateur, il crée un *principal*, qui est un objet [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) qui représente le contexte de sécurité dans lequel le code s’exécute. L’hôte joint le principal au thread actuel en définissant **thread. CurrentPrincipal**. Le principal contient un objet **Identity** associé qui contient des informations sur l’utilisateur. Si l’utilisateur est authentifié, la propriété **Identity. IsAuthenticated** retourne la **valeur true**. Pour les demandes anonymes, **IsAuthenticated** retourne **false**. Pour plus d’informations sur les principaux, consultez [sécurité basée sur les rôles](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Gestionnaires de messages HTTP pour l’authentification

Au lieu d’utiliser l’hôte pour l’authentification, vous pouvez placer la logique d’authentification dans un [Gestionnaire de messages http](../advanced/http-message-handlers.md). Dans ce cas, le gestionnaire de messages examine la requête HTTP et définit le principal.

Quand devez-vous utiliser des gestionnaires de messages pour l’authentification ? Voici quelques compromis :

- Un module HTTP voit toutes les demandes qui passent par le pipeline ASP.NET. Un gestionnaire de messages ne voit que les requêtes qui sont acheminées vers l’API Web.
- Vous pouvez définir des gestionnaires de messages par itinéraire, ce qui vous permet d’appliquer un schéma d’authentification à un itinéraire spécifique.
- Les modules HTTP sont spécifiques à IIS. Les gestionnaires de messages étant indépendants des hôtes, ils peuvent être utilisés avec l’hébergement Web et l’auto-hébergement.
- Les modules HTTP participent à la journalisation IIS, à l’audit, etc.
- Les modules HTTP s’exécutent plus tôt dans le pipeline. Si vous gérez l’authentification dans un gestionnaire de messages, le principal n’est pas défini tant que le gestionnaire n’est pas exécuté. En outre, le principal revient au principal précédent lorsque la réponse quitte le gestionnaire de messages.

En règle générale, si vous n’avez pas besoin de prendre en charge l’auto-hébergement, un module HTTP est une meilleure option. Si vous devez prendre en charge l’auto-hébergement, envisagez un gestionnaire de messages.

### <a name="setting-the-principal"></a>Définition du principal

Si votre application effectue une logique d’authentification personnalisée, vous devez définir le principal à deux emplacements :

- **Thread. CurrentPrincipal**. Cette propriété est la méthode standard pour définir le principal du thread dans .NET.
- **HttpContext. Current. User**. Cette propriété est spécifique à ASP.NET.

Le code suivant montre comment définir le principal :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Pour l’hébergement Web, vous devez définir le principal dans les deux emplacements. dans le cas contraire, le contexte de sécurité peut devenir incohérent. Pour l’auto-hébergement, toutefois, **HttpContext. Current** a la valeur null. Pour garantir que votre code est indépendant de l’hôte, recherchez la valeur null avant de l’assigner à **HttpContext. Current**, comme indiqué.

## <a name="authorization"></a>Authorization

L’autorisation se produit plus tard dans le pipeline, plus près du contrôleur. Cela vous permet de choisir des options plus granulaires lorsque vous accordez l’accès aux ressources.

- Les *filtres d’autorisation* s’exécutent avant l’action du contrôleur. Si la demande n’est pas autorisée, le filtre renvoie une réponse d’erreur et l’action n’est pas appelée.
- Dans une action de contrôleur, vous pouvez obtenir le principal actuel à partir de la propriété **ApiController. User** . Par exemple, vous pouvez filtrer une liste de ressources en fonction du nom d’utilisateur, en retournant uniquement les ressources qui appartiennent à cet utilisateur.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Utilisation de l’attribut [Authorize]

L’API Web fournit un filtre d’autorisation intégré, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Ce filtre vérifie si l’utilisateur est authentifié. Si ce n’est pas le cas, elle retourne le code d’état HTTP 401 (non autorisé), sans appeler l’action.

Vous pouvez appliquer le filtre globalement, au niveau du contrôleur ou au niveau des actions individuelles.

**Globalement**: pour restreindre l’accès à chaque contrôleur d’API Web, ajoutez le filtre **AuthorizeAttribute** à la liste des filtres globaux :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Contrôleur**: pour restreindre l’accès pour un contrôleur spécifique, ajoutez le filtre en tant qu’attribut au contrôleur :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Action**: pour restreindre l’accès à des actions spécifiques, ajoutez l’attribut à la méthode d’action :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Vous pouvez également restreindre le contrôleur, puis autoriser l’accès anonyme à des actions spécifiques, à l’aide de l’attribut `[AllowAnonymous]`. Dans l’exemple suivant, la méthode `Post` est restreinte, mais la méthode `Get` autorise l’accès anonyme.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Dans les exemples précédents, le filtre autorise tout utilisateur authentifié à accéder aux méthodes restreintes ; Seuls les utilisateurs anonymes sont conservés. Vous pouvez également limiter l’accès à des utilisateurs spécifiques ou à des utilisateurs de rôles spécifiques :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Le filtre **AuthorizeAttribute** pour les contrôleurs d’API Web se trouve dans l’espace de noms **System. Web. http** . Il existe un filtre similaire pour les contrôleurs MVC dans l’espace de noms **System. Web. Mvc** , qui n’est pas compatible avec les contrôleurs d’API Web.

### <a name="custom-authorization-filters"></a>Filtres d’autorisation personnalisés

Pour écrire un filtre d’autorisation personnalisé, dérivez de l’un de ces types :

- **AuthorizeAttribute**. Étendez cette classe pour exécuter une logique d’autorisation basée sur l’utilisateur actuel et les rôles de l’utilisateur.
- **AuthorizationFilterAttribute**. Étendez cette classe pour exécuter une logique d’autorisation synchrone qui n’est pas nécessairement basée sur l’utilisateur ou le rôle actuel.
- **IAuthorizationFilter**. Implémentez cette interface pour exécuter une logique d’autorisation asynchrone ; par exemple, si votre logique d’autorisation effectue des e/s asynchrones ou des appels réseau. (Si votre logique d’autorisation est liée à l’UC, il est plus simple de dériver de **AuthorizationFilterAttribute**, car vous n’avez pas besoin d’écrire une méthode asynchrone.)

Le diagramme suivant montre la hiérarchie de classes pour la classe **AuthorizeAttribute** .

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorisation à l’intérieur d’une action de contrôleur

Dans certains cas, vous pouvez autoriser la poursuite d’une demande, mais modifier le comportement en fonction du principal. Par exemple, les informations que vous retournez peuvent changer en fonction du rôle de l’utilisateur. Dans une méthode de contrôleur, vous pouvez obtenir le principal actuel à partir de la propriété **ApiController. User** .

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
