---
uid: web-api/overview/security/authentication-filters
title: Filtres d’authentification dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Un filtre d’authentification est un composant qui authentifie une requête HTTP. L’API Web 2 et MVC 5 prennent tous deux en charge les filtres d’authentification, mais elles diffèrent légèrement...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 2ef9e62a6c634237e920b6d7aba2127b835f959d
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075071"
---
# <a name="authentication-filters-in-aspnet-web-api-2"></a>Filtres d’authentification dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

> Un filtre d’authentification est un composant qui authentifie une requête HTTP. L’API Web 2 et MVC 5 prennent tous deux en charge les filtres d’authentification, mais elles diffèrent légèrement, principalement dans les conventions de nommage de l’interface de filtre. Cette rubrique décrit les filtres d’authentification de l’API Web.

Les filtres d’authentification vous permettent de définir un schéma d’authentification pour des contrôleurs ou des actions individuels. De cette façon, votre application peut prendre en charge différents mécanismes d’authentification pour différentes ressources HTTP.

Dans cet article, je vais vous montrer le code de l’exemple [d’authentification de base](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) sur [https://github.com/aspnet/samples](https://github.com/aspnet/samples). L’exemple montre un filtre d’authentification qui implémente le schéma d’authentification d’accès de base HTTP (RFC 2617). Le filtre est implémenté dans une classe nommée `IdentityBasicAuthenticationAttribute`. Je n’affiche pas l’intégralité du code de l’exemple, mais uniquement les parties qui illustrent comment écrire un filtre d’authentification.

## <a name="setting-an-authentication-filter"></a>Définition d’un filtre d’authentification

Comme d’autres filtres, les filtres d’authentification peuvent être appliqués par contrôleur, par action ou globalement à tous les contrôleurs d’API Web.

Pour appliquer un filtre d’authentification à un contrôleur, Décorez la classe de contrôleur avec l’attribut de filtre. Le code suivant définit le filtre `[IdentityBasicAuthentication]` sur une classe de contrôleur, ce qui active l’authentification de base pour toutes les actions du contrôleur.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Pour appliquer le filtre à une action, décorez l’action avec le filtre. Le code suivant définit le filtre `[IdentityBasicAuthentication]` sur la méthode `Post` du contrôleur.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Pour appliquer le filtre à tous les contrôleurs d’API Web, ajoutez-le à **GlobalConfiguration. filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implémentation d’un filtre d’authentification d’API Web

Dans l’API Web, les filtres d’authentification implémentent l’interface [System. Web. http. filters. IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) . Ils doivent également hériter de **System. Attribute**, afin d’être appliqués comme attributs.

L’interface **IAuthenticationFilter** a deux méthodes :

- **AuthenticateAsync** authentifie la demande en validant les informations d’identification dans la demande, le cas échéant.
- **ChallengeAsync** ajoute une stimulation d’authentification à la réponse http, si nécessaire.

Ces méthodes correspondent au workflow d’authentification défini dans [rfc 2612](http://tools.ietf.org/html/rfc2616) et [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Le client envoie les informations d’identification dans l’en-tête Authorization. Cela se produit généralement après que le client a reçu une réponse 401 (non autorisée) du serveur. Toutefois, un client peut envoyer des informations d’identification avec n’importe quelle demande, et non juste après avoir obtenu un 401.
2. Si le serveur n’accepte pas les informations d’identification, il renvoie une réponse 401 (non autorisée). La réponse inclut un en-tête WWW-Authenticate qui contient un ou plusieurs défis. Chaque défi spécifie un schéma d’authentification reconnu par le serveur.

Le serveur peut également retourner 401 à partir d’une demande anonyme. En fait, il s’agit généralement de la façon dont le processus d’authentification est initié :

1. Le client envoie une demande anonyme.
2. Le serveur renvoie 401.
3. Les clients renvoient la demande avec les informations d’identification.

Ce Flow comprend les étapes *d’authentification* et *d’autorisation* .

- L’authentification prouve l’identité du client.
- L’autorisation détermine si le client peut accéder à une ressource particulière.

Dans l’API Web, les filtres d’authentification gèrent l’authentification, mais pas l’autorisation. L’autorisation doit être effectuée par un filtre d’autorisation ou à l’intérieur de l’action du contrôleur.

Voici le Flow dans le pipeline de l’API Web 2 :

1. Avant d’appeler une action, l’API Web crée une liste des filtres d’authentification pour cette action. Cela comprend les filtres avec l’étendue de l’action, l’étendue du contrôleur et l’étendue globale.
2. L’API Web appelle **AuthenticateAsync** sur chaque filtre de la liste. Chaque filtre peut valider les informations d’identification dans la demande. Si un filtre valide correctement les informations d’identification, le filtre crée un **IPrincipal** et l’attache à la demande. Un filtre peut également déclencher une erreur à ce stade. Si c’est le cas, le reste du pipeline ne s’exécute pas.
3. En supposant qu’il n’y ait aucune erreur, la demande passe par le reste du pipeline.
4. Enfin, l’API Web appelle la méthode **ChallengeAsync** de chaque filtre d’authentification. Les filtres utilisent cette méthode pour ajouter une stimulation à la réponse, si nécessaire. En général (mais pas toujours) qui se produit en réponse à une erreur 401.

Les diagrammes suivants présentent deux cas possibles. Dans le premier, le filtre d’authentification authentifie correctement la demande, un filtre d’autorisation autorise la demande et l’action du contrôleur retourne 200 (OK).

![](authentication-filters/_static/image1.png)

Dans le deuxième exemple, le filtre d’authentification authentifie la demande, mais le filtre d’autorisation renvoie 401 (non autorisé). Dans ce cas, l’action du contrôleur n’est pas appelée. Le filtre d’authentification ajoute un en-tête WWW-Authenticate à la réponse.

![](authentication-filters/_static/image2.png)

D’autres combinaisons sont possibles&mdash;par exemple, si l’action du contrôleur autorise les demandes anonymes, vous pouvez avoir un filtre d’authentification mais aucune autorisation.

## <a name="implementing-the-authenticateasync-method"></a>Implémentation de la méthode AuthenticateAsync

La méthode **AuthenticateAsync** essaie d’authentifier la demande. Voici la signature de méthode :

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

La méthode **AuthenticateAsync** doit effectuer l’une des opérations suivantes :

1. Rien (aucune opération).
2. Créez un **IPrincipal** et définissez-le sur la demande.
3. Définissez un résultat d’erreur.

Option (1) signifie que la demande n’a pas d’informations d’identification compréhensibles par le filtre. Option (2) signifie que le filtre a correctement authentifié la demande. Option (3) signifie que la demande contient des informations d’identification non valides (comme le mot de passe incorrect), ce qui déclenche une réponse d’erreur.

Voici un aperçu général de l’implémentation de **AuthenticateAsync**.

1. Recherchez les informations d’identification dans la demande.
2. S’il n’y a aucune information d’identification, ne rien faire et retourner (pas d’opération).
3. S’il existe des informations d’identification, mais que le filtre ne reconnaît pas le schéma d’authentification, ne rien faire et retourner (pas d’opération). Un autre filtre dans le pipeline peut comprendre le schéma.
4. S’il existe des informations d’identification que le filtre comprend, essayez de les authentifier.
5. Si les informations d’identification sont incorrectes, retournez 401 en définissant `context.ErrorResult`.
6. Si les informations d’identification sont valides, créez un **IPrincipal** et définissez `context.Principal`.

Le code suivant illustre la méthode **AuthenticateAsync** de l’exemple [d’authentification de base](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) . Les commentaires indiquent chaque étape. Le code illustre plusieurs types d’erreurs : un en-tête d’autorisation sans informations d’identification, des informations d’identification incorrectes et un nom d’utilisateur/mot de passe incorrect.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Définition d’un résultat d’erreur

Si les informations d’identification ne sont pas valides, le filtre doit définir `context.ErrorResult` sur un **IHttpActionResult** qui crée une réponse d’erreur. Pour plus d’informations sur **IHttpActionResult**, consultez [résultats des actions dans l’API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

L’exemple d’authentification de base comprend une classe `AuthenticationFailureResult` qui convient à cet effet.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implémentation de ChallengeAsync

L’objectif de la méthode **ChallengeAsync** consiste à ajouter des défis d’authentification à la réponse, si nécessaire. Voici la signature de méthode :

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

La méthode est appelée sur chaque filtre d’authentification dans le pipeline de requête.

Il est important de comprendre que **ChallengeAsync** est appelé *avant* la création de la réponse http, voire avant l’exécution de l’action du contrôleur. Quand **ChallengeAsync** est appelé, `context.Result` contient un **IHttpActionResult**, qui est utilisé ultérieurement pour créer la réponse http. Ainsi, quand **ChallengeAsync** est appelé, vous ne connaissez pas encore la réponse http. La méthode **ChallengeAsync** doit remplacer la valeur d’origine de `context.Result` par un nouveau **IHttpActionResult**. Ce **IHttpActionResult** doit encapsuler le `context.Result`d’origine.

![](authentication-filters/_static/image3.png)

Je vais appeler le *résultat interne* **IHttpActionResult** et le nouveau **IHttpActionResult** le *résultat externe*. Le résultat externe doit effectuer les opérations suivantes :

1. Appelez le résultat interne pour créer la réponse HTTP.
2. Examinez la réponse.
3. Ajoutez une demande d’authentification à la réponse, si nécessaire.

L’exemple suivant est tiré de l’exemple d’authentification de base. Il définit un **IHttpActionResult** pour le résultat externe.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

La propriété `InnerResult` contient le **IHttpActionResult**interne. La propriété `Challenge` représente un en-tête www-Authentication. Notez que **ExecuteAsync** appelle d’abord `InnerResult.ExecuteAsync` pour créer la réponse http, puis ajoute le défi si nécessaire.

Vérifiez le code de réponse avant d’ajouter la stimulation. La plupart des schémas d’authentification ajoutent uniquement un défi si la réponse est 401, comme illustré ici. Toutefois, certains schémas d’authentification ajoutent un défi à une réponse de réussite. Par exemple, consultez [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Étant donné la classe `AddChallengeOnUnauthorizedResult`, le code réel dans **ChallengeAsync** est simple. Il vous suffit de créer le résultat et de l’attacher à `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Remarque : l’exemple d’authentification de base soustrait cette logique un peu, en le plaçant dans une méthode d’extension.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinaison de filtres d’authentification avec l’authentification au niveau de l’hôte

« Authentification au niveau de l’hôte » est l’authentification effectuée par l’hôte (par exemple, IIS), avant que la demande n’atteigne l’infrastructure de l’API Web.

Souvent, vous souhaiterez peut-être activer l’authentification au niveau de l’hôte pour le reste de votre application, mais la désactiver pour vos contrôleurs d’API Web. Par exemple, un scénario classique consiste à activer l’authentification par formulaire au niveau de l’hôte, mais à utiliser l’authentification basée sur les jetons pour l’API Web.

Pour désactiver l’authentification au niveau de l’hôte dans le pipeline de l’API Web, appelez `config.SuppressHostPrincipal()` dans votre configuration. Cela amène l’API Web à supprimer l’interface **IPrincipal** de toute demande qui entre dans le pipeline de l’API Web. En fait, il &quot;annule l’authentification&quot; la demande.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Ressources supplémentaires

[API Web ASP.net les filtres de sécurité](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
