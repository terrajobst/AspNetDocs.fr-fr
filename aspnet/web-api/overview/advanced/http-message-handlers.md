---
uid: web-api/overview/advanced/http-message-handlers
title: Gestionnaires de messages HTTP dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Vue d’ensemble des gestionnaires de messages HTTP dans API Web ASP.NET pour ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622583"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a>Gestionnaires de messages HTTP dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et retourne une réponse http. Les gestionnaires de messages dérivent de la classe abstraite **HttpMessageHandler** .

En général, une série de gestionnaires de messages sont chaînés ensemble. Le premier gestionnaire reçoit une requête HTTP, effectue un traitement et transmet la requête au gestionnaire suivant. À un moment donné, la réponse est créée et reprend la chaîne. Ce modèle est appelé gestionnaire de *délégation* .

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Gestionnaires de messages côté serveur

Côté serveur, le pipeline de l’API Web utilise des gestionnaires de messages intégrés :

- **Httpserver** obtient la demande de la part de l’hôte.
- **HttpRoutingDispatcher** distribue la demande en fonction de l’itinéraire.
- **HttpControllerDispatcher** envoie la demande à un contrôleur d’API Web.

Vous pouvez ajouter des gestionnaires personnalisés au pipeline. Les gestionnaires de messages sont bons pour les problèmes de coupe croisée qui fonctionnent au niveau des messages HTTP (plutôt que des actions de contrôleur). Par exemple, un gestionnaire de messages peut :

- En-têtes de demande de lecture ou de modification.
- Ajoutez un en-tête de réponse aux réponses.
- Validez les requêtes avant qu’elles atteignent le contrôleur.

Ce diagramme montre deux gestionnaires personnalisés insérés dans le pipeline :

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Du côté client, HttpClient utilise également des gestionnaires de messages. Pour plus d’informations, consultez [gestionnaires de messages httpclient](httpclient-message-handlers.md).

## <a name="custom-message-handlers"></a>Gestionnaires de messages personnalisés

Pour écrire un gestionnaire de messages personnalisé, dérivez de **System .net. http. DelegatingHandler** et substituez la méthode **SendAsync** . Cette méthode a la signature suivante :

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

La méthode prend un **HttpRequestMessage** comme entrée et retourne de manière asynchrone un **HttpResponseMessage**. Une implémentation classique effectue les opérations suivantes :

1. Traiter le message de demande.
2. Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.
3. Le gestionnaire interne retourne un message de réponse. (Cette étape est asynchrone.)
4. Traitez la réponse et renvoyez-la à l’appelant.

Voici un exemple simple :

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> L’appel à `base.SendAsync` est asynchrone. Si le gestionnaire effectue un travail après cet appel, utilisez le mot clé **await** , comme indiqué.

Un gestionnaire de délégation peut également ignorer le gestionnaire interne et créer directement la réponse :

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Si un gestionnaire de délégation crée la réponse sans appeler `base.SendAsync`, la demande ignore le reste du pipeline. Cela peut être utile pour un gestionnaire qui valide la requête (création d’une réponse d’erreur).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Ajout d’un gestionnaire au pipeline

Pour ajouter un gestionnaire de messages côté serveur, ajoutez le gestionnaire à la collection **HttpConfiguration. MessageHandlers** . Si vous avez utilisé le modèle « application Web ASP.NET MVC 4 » pour créer le projet, vous pouvez le faire à l’intérieur de la classe **WebApiConfig** :

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Les gestionnaires de messages sont appelés dans l’ordre dans lequel ils apparaissent dans la collection **MessageHandlers** . Étant donné qu’elles sont imbriquées, le message de réponse se déplace dans l’autre direction. Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.

Notez que vous n’avez pas besoin de définir les gestionnaires internes ; l’infrastructure d’API Web connecte automatiquement les gestionnaires de messages.

Si vous êtes [auto-hébergé](../older-versions/self-host-a-web-api.md), créez une instance de la classe **HttpSelfHostConfiguration** et ajoutez les gestionnaires à la collection **MessageHandlers** .

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Examinons à présent quelques exemples de gestionnaires de messages personnalisés.

## <a name="example-x-http-method-override"></a>Exemple : X-HTTP-Method-override

X-HTTP-Method-override est un en-tête HTTP non standard. Il est conçu pour les clients qui ne peuvent pas envoyer certains types de requêtes HTTP, tels que PUT ou DELETE. Au lieu de cela, le client envoie une demande de publication et définit l’en-tête X-HTTP-Method-override sur la méthode souhaitée. Exemple :

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Voici un gestionnaire de messages qui ajoute la prise en charge de X-HTTP-Method-override :

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

Dans la méthode **SendAsync** , le gestionnaire vérifie si le message de demande est une demande postérieure et s’il contient l’en-tête X-http-Method-override. Si c’est le cas, il valide la valeur d’en-tête, puis modifie la méthode de demande. Enfin, le gestionnaire appelle `base.SendAsync` pour passer le message au gestionnaire suivant.

Lorsque la demande atteint la classe **HttpControllerDispatcher** , **HttpControllerDispatcher** route la demande en fonction de la méthode de demande mise à jour.

## <a name="example-adding-a-custom-response-header"></a>Exemple : ajout d’un en-tête de réponse personnalisé

Voici un gestionnaire de messages qui ajoute un en-tête personnalisé à chaque message de réponse :

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Tout d’abord, le gestionnaire appelle `base.SendAsync` pour transmettre la demande au gestionnaire de messages internes. Le gestionnaire interne retourne un message de réponse, mais il le fait de façon asynchrone à l’aide d’une **tâche&lt;t&gt;** objet. Le message de réponse n’est pas disponible tant que `base.SendAsync` ne se termine pas de façon asynchrone.

Cet exemple utilise le mot clé **await** pour exécuter le travail de façon asynchrone une fois `SendAsync` terminé. Si vous ciblez .NET Framework 4,0, utilisez la **tâche**&lt;t&gt; **. Méthode ContinueWith** :

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Exemple : recherche d’une clé API

Certains services Web requièrent que les clients incluent une clé API dans leur demande. L’exemple suivant montre comment un gestionnaire de messages peut vérifier les demandes pour une clé API valide :

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Ce gestionnaire recherche la clé API dans la chaîne de requête d’URI. (Pour cet exemple, nous partons du principe que la clé est une chaîne statique. Une implémentation réelle utiliserait probablement une validation plus complexe.) Si la chaîne de requête contient la clé, le gestionnaire transmet la requête au gestionnaire interne.

Si la demande n’a pas de clé valide, le gestionnaire crée un message de réponse avec l’état 403, interdit. Dans ce cas, le gestionnaire n’appelle pas `base.SendAsync`, donc le gestionnaire interne ne reçoit jamais la demande et le contrôleur. Par conséquent, le contrôleur peut supposer que toutes les demandes entrantes ont une clé API valide.

> [!NOTE]
> Si la clé d’API s’applique uniquement à certaines actions de contrôleur, envisagez d’utiliser un filtre d’action au lieu d’un gestionnaire de messages. Les filtres d’action s’exécutent après l’exécution du routage URI.

## <a name="per-route-message-handlers"></a>Gestionnaires de messages par itinéraire

Les gestionnaires de la collection **HttpConfiguration. MessageHandlers** s’appliquent globalement.

Vous pouvez également ajouter un gestionnaire de messages à un itinéraire spécifique lorsque vous définissez l’itinéraire :

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

Dans cet exemple, si l’URI de la demande correspond à « route2 », la demande est distribuée à `MessageHandler2`. Le diagramme suivant illustre le pipeline pour ces deux itinéraires :

![](http-message-handlers/_static/image4.png)

Notez que `MessageHandler2` remplace le **HttpControllerDispatcher**par défaut. Dans cet exemple, `MessageHandler2` crée la réponse, et les demandes qui correspondent à « route2 » n’accèdent jamais à un contrôleur. Cela vous permet de remplacer l’ensemble du mécanisme du contrôleur d’API Web par votre propre point de terminaison personnalisé.

En guise d’alternative, un gestionnaire de messages par itinéraire peut déléguer à **HttpControllerDispatcher**, qui est ensuite distribué à un contrôleur.

![](http-message-handlers/_static/image5.png)

Le code suivant montre comment configurer cet itinéraire :

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
