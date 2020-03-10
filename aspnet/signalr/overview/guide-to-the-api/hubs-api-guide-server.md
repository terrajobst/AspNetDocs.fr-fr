---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guide de l’API hubs Signalr ASP.NET-C#serveur () | Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à la programmation du côté serveur de l’API ASP.NET Signalr hubs pour Signalr version 2, avec des exemples de code qui illustrent...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536749"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guide de l’API hubs Signalr ASP.NET-C#serveur ()

de [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document fournit une introduction à la programmation du côté serveur de l’API ASP.NET Signalr hubs pour Signalr version 2, avec des exemples de code illustrant des options communes.
> 
> L’API Signalr hubs vous permet d’effectuer des appels de procédure distante (RPC) à partir d’un serveur vers des clients connectés et des clients vers le serveur. Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client. Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur. Signalr prend en charge tous les éléments de la plombation client à serveur pour vous.
> 
> Signalr offre également une API de bas niveau appelée connexions persistantes. Pour une présentation de Signalr, de hubs et de connexions persistantes, consultez [Présentation de signalr 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans cette rubrique
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Signalr version 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versions de la rubrique
> 
> Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Présentation

Ce document contient les sections suivantes :

- [Comment inscrire un intergiciel (middleware) Signalr](#route)

    - [URL/signalr](#signalrurl)
    - [Configuration des options de Signalr](#options)
- [Comment créer et utiliser des classes de concentrateur](#hubclass)

    - [Durée de vie des objets Hub](#transience)
    - [Casse mixte des noms de Hub dans les clients JavaScript](#hubnames)
    - [Plusieurs hubs](#multiplehubs)
    - [Hubs fortement typés](#stronglytypedhubs)
- [Comment définir des méthodes dans la classe de concentrateur que les clients peuvent appeler](#hubmethods)

    - [Casse mixte des noms de méthode dans les clients JavaScript](#methodnames)
    - [Quand s’exécuter de façon asynchrone](#asyncmethods)
    - [Définir des surcharges](#overloads)
    - [Signalement de la progression des appels de méthode de concentrateur](#progress)
- [Comment appeler des méthodes clientes à partir de la classe de concentrateur](#callfromhub)

    - [Sélection des clients qui recevront le RPC](#selectingclients)
    - [Aucune validation de la compilation pour les noms de méthode](#dynamicmethodnames)
    - [Correspondance de nom de méthode ne respectant pas la casse](#caseinsensitive)
    - [Exécution asynchrone](#asyncclient)
- [Comment gérer l’appartenance à un groupe à partir de la classe de concentrateur](#groupsfromhub)

    - [Exécution asynchrone de méthodes Add et Remove](#asyncgroupmethods)
    - [Persistance de l’appartenance aux groupes](#grouppersistence)
    - [Groupes à un seul utilisateur](#singleusergroups)
- [Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](#connectionlifetime)

    - [Quand OnConnected, OnDisconnected et OnReconnected sont appelés](#onreconnected)
    - [L’état de l’appelant n’est pas rempli](#nocallerstate)
- [Comment obtenir des informations sur le client à partir de la propriété de contexte](#contextproperty)
- [Comment passer l’état entre les clients et la classe de concentrateur](#passstate)
- [Comment gérer les erreurs dans la classe de concentrateur](#handleErrors)
- [Comment appeler des méthodes clientes et gérer des groupes à partir de l’extérieur de la classe de concentrateur](#callfromoutsidehub)

    - [Appel des méthodes clientes](#callingclientsoutsidehub)
    - [Gestion de l’appartenance à un groupe](#managinggroupsoutsidehub)
- [Comment activer le suivi](#tracing)
- [Comment personnaliser le pipeline hubs](#hubpipeline)

Pour plus d’informations sur la façon de programmer des clients, consultez les ressources suivantes :

- [Guide de l’API signalr hubs-client JavaScript](hubs-api-guide-javascript-client.md)
- [Guide de l’API signalr hubs-client .NET](hubs-api-guide-net-client.md)

Les composants serveur pour Signalr 2 sont uniquement disponibles dans .NET 4,5. Les serveurs qui exécutent .NET 4,0 doivent utiliser Signalr v1. x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Comment inscrire un intergiciel (middleware) Signalr

Pour définir l’itinéraire que les clients utiliseront pour se connecter à votre Hub, appelez la méthode `MapSignalR` lorsque l’application démarre. `MapSignalR` est une [méthode d’extension](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) pour la classe `OwinExtensions`. L’exemple suivant montre comment définir l’itinéraire des hubs Signalr à l’aide d’une classe de démarrage OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Si vous ajoutez la fonctionnalité Signalr à une application MVC ASP.NET, assurez-vous que l’itinéraire Signalr est ajouté avant les autres itinéraires. Pour plus d’informations, consultez [Didacticiel : prise en main avec signalr 2 et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>URL/signalr

Par défaut, l’URL de routage que les clients utiliseront pour se connecter à votre Hub est « /signalr ». (Ne confondez pas cette URL avec l’URL « /signalr/hubs », qui est destinée au fichier JavaScript généré automatiquement. Pour plus d’informations sur le proxy généré, consultez Guide de l' [API signalr hubs-client JavaScript-le proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).)

Il peut y avoir des circonstances exceptionnelles qui rendent cette URL de base inutilisable pour Signalr. par exemple, vous avez un dossier dans votre projet nommé *signalr* et vous ne souhaitez pas modifier le nom. Dans ce cas, vous pouvez modifier l’URL de base, comme indiqué dans les exemples suivants (remplacez « /signalr » dans l’exemple de code par l’URL souhaitée).

**Code serveur qui spécifie l’URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Code client JavaScript qui spécifie l’URL (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Code client JavaScript qui spécifie l’URL (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Code client .NET qui spécifie l’URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configuration des options de Signalr

Les surcharges de la méthode `MapSignalR` vous permettent de spécifier une URL personnalisée, un résolveur de dépendance personnalisé et les options suivantes :

- Activez les appels inter-domaines à l’aide de CORS ou JSONP à partir des clients de navigateur.

    En général, si le navigateur charge une page à partir de `http://contoso.com`, la connexion Signalr se trouve dans le même domaine, à `http://contoso.com/signalr`. Si la page de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, il s’agit d’une connexion inter-domaines. Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut. Pour plus d’informations, consultez [Guide de l’API ASP.net signalr hubs-JavaScript client-Guide pratique pour établir une connexion inter-domaines](hubs-api-guide-javascript-client.md#crossdomain).
- Activer les messages d’erreur détaillés.

    Lorsque des erreurs se produisent, le comportement par défaut de Signalr est d’envoyer aux clients un message de notification sans détails sur ce qui s’est produit. L’envoi d’informations détaillées sur les erreurs aux clients n’est pas recommandé en production, car les utilisateurs malveillants peuvent utiliser les informations contenues dans les attaques contre votre application. Pour la résolution des problèmes, vous pouvez utiliser cette option pour activer temporairement un rapport d’erreurs plus informatif.
- Désactivez les fichiers de proxy JavaScript générés automatiquement.

    Par défaut, un fichier JavaScript avec des proxies pour vos classes de concentrateur est généré en réponse à l’URL « /signalr/hubs ». Si vous ne souhaitez pas utiliser les proxys JavaScript, ou si vous souhaitez générer ce fichier manuellement et faire référence à un fichier physique dans vos clients, vous pouvez utiliser cette option pour désactiver la génération de proxy. Pour plus d’informations, consultez [Guide de l’API signalr hubs-client JavaScript-comment créer un fichier physique pour le proxy généré par signalr](hubs-api-guide-javascript-client.md#manualproxy).

L’exemple suivant montre comment spécifier l’URL de connexion Signalr et ces options dans un appel à la méthode `MapSignalR`. Pour spécifier une URL personnalisée, remplacez « /signalr » dans l’exemple par l’URL que vous souhaitez utiliser.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Comment créer et utiliser des classes de concentrateur

Pour créer un Hub, créez une classe qui dérive de [Microsoft. Aspnet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). L’exemple suivant illustre une classe de concentrateur simple pour une application de conversation.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

Dans cet exemple, un client connecté peut appeler la méthode `NewContosoChatMessage` et, le cas contraire, les données reçues sont diffusées à tous les clients connectés.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durée de vie des objets Hub

Vous n’instanciez pas la classe de concentrateur ou appelez ses méthodes à partir de votre propre code sur le serveur. tout ce qui est effectué pour vous par le pipeline hubs Signalr. Signalr crée une nouvelle instance de votre classe de concentrateur chaque fois qu’il doit gérer une opération de concentrateur, par exemple lorsqu’un client se connecte, se déconnecte ou effectue un appel de méthode au serveur.

Étant donné que les instances de la classe de concentrateur sont temporaires, vous ne pouvez pas les utiliser pour maintenir l’état d’un appel de méthode à la suivante. Chaque fois que le serveur reçoit un appel de méthode d’un client, une nouvelle instance de votre classe de concentrateur traite le message. Pour maintenir l’état via plusieurs connexions et appels de méthode, utilisez une autre méthode, telle qu’une base de données, ou une variable statique sur la classe de concentrateur, ou une autre classe qui ne dérive pas de `Hub`. Si vous conservez des données en mémoire, à l’aide d’une méthode telle qu’une variable statique sur la classe de concentrateur, les données seront perdues lors du recyclage du domaine d’application.

Si vous souhaitez envoyer des messages à des clients à partir de votre propre code qui s’exécute en dehors de la classe de concentrateur, vous ne pouvez pas le faire en instanciant une instance de classe de concentrateur, mais vous pouvez le faire en obtenant une référence à l’objet de contexte Signalr pour votre classe de concentrateur. Pour plus d’informations, consultez [How to Call client Methods and Manage Groups outside the Hub Class](#callfromoutsidehub) plus loin dans cette rubrique.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Casse mixte des noms de Hub dans les clients JavaScript

Par défaut, les clients JavaScript font référence aux hubs à l’aide d’une version à casse mixte du nom de classe. Signalr effectue automatiquement cette modification afin que le code JavaScript puisse être conforme aux conventions JavaScript. L’exemple précédent est appelé `contosoChatHub` dans du code JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Si vous souhaitez spécifier un autre nom que les clients doivent utiliser, ajoutez l’attribut `HubName`. Lorsque vous utilisez un attribut `HubName`, il n’y a pas de changement de nom en casse mixte sur les clients JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Plusieurs hubs

Vous pouvez définir plusieurs classes de concentrateur dans une application. Dans ce cas, la connexion est partagée, mais les groupes sont distincts :

- Tous les clients utilisent la même URL pour établir une connexion Signalr avec votre service (« /signalr » ou votre URL personnalisée si vous en avez spécifié un), et cette connexion est utilisée pour tous les concentrateurs définis par le service.

    Il n’existe aucune différence de performances pour plusieurs hubs par rapport à la définition de toutes les fonctionnalités de concentrateur dans une seule classe.
- Tous les hubs obtiennent les mêmes informations de requête HTTP.

    Étant donné que tous les hubs partagent la même connexion, les seules informations de requête HTTP que le serveur obtient sont celles contenues dans la requête HTTP d’origine qui établit la connexion Signalr. Si vous utilisez la demande de connexion pour transmettre des informations du client au serveur en spécifiant une chaîne de requête, vous ne pouvez pas fournir des chaînes de requête différentes à différents hubs. Tous les hubs recevront les mêmes informations.
- Le fichier des proxies JavaScript générés contiendra des proxies pour tous les hubs dans un même fichier.

    Pour plus d’informations sur les proxys JavaScript, consultez [Guide de l’API signalr hubs-client JavaScript-le proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).
- Les groupes sont définis dans des hubs.

    Dans Signalr, vous pouvez définir des groupes nommés à diffuser pour des sous-ensembles de clients connectés. Les groupes sont gérés séparément pour chaque concentrateur. Par exemple, un groupe nommé « administrateurs » inclut un ensemble de clients pour votre classe de `ContosoChatHub`, et le même nom de groupe fait référence à un autre ensemble de clients pour votre classe de `StockTickerHub`.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hubs fortement typés

Pour définir une interface pour vos méthodes de concentrateur que votre client peut référencer (et activer IntelliSense sur vos méthodes de concentrateur), dérivez votre Hub de `Hub<T>` (introduit dans Signalr 2,1) au lieu de `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Comment définir des méthodes dans la classe de concentrateur que les clients peuvent appeler

Pour exposer une méthode sur le concentrateur que vous souhaitez pouvoir appeler à partir du client, déclarez une méthode publique, comme indiqué dans les exemples suivants.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Vous pouvez spécifier un type de retour et des paramètres, y compris des types complexes et des tableaux, comme C# vous le feriez dans n’importe quelle méthode. Toutes les données que vous recevez dans les paramètres ou qui reviennent à l’appelant sont communiquées entre le client et le serveur à l’aide de JSON, et Signalr gère automatiquement la liaison des objets complexes et des tableaux d’objets.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Casse mixte des noms de méthode dans les clients JavaScript

Par défaut, les clients JavaScript font référence aux méthodes de concentrateur à l’aide d’une version à casse mixte du nom de la méthode. Signalr effectue automatiquement cette modification afin que le code JavaScript puisse être conforme aux conventions JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si vous souhaitez spécifier un autre nom que les clients doivent utiliser, ajoutez l’attribut `HubMethodName`.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quand s’exécuter de façon asynchrone

Si la méthode est à durée d’exécution longue ou si elle doit faire l’objet d’une attente, par exemple une recherche de base de données ou un appel de service Web, rendez la méthode de concentrateur asynchrone en retournant une [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (à la place de `void` retour) ou une [tâche&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) objet (à la place de `T` type de retour). Lorsque vous retournez un objet `Task` à partir de la méthode, Signalr attend la fin de la `Task`, puis renvoie le résultat non encapsulé au client, de sorte qu’il n’y a aucune différence dans le code de l’appel de méthode dans le client.

La création d’une méthode de concentrateur asynchrone évite de bloquer la connexion lorsqu’elle utilise le transport WebSocket. Lorsqu’une méthode de concentrateur s’exécute de façon synchrone et que le transport est WebSocket, les appels suivants de méthodes sur le concentrateur à partir du même client sont bloqués jusqu’à ce que la méthode de concentrateur se termine.

L’exemple suivant illustre la même méthode codée pour s’exécuter de façon synchrone ou asynchrone, suivie du code JavaScript client qui fonctionne pour l’appel de l’une ou l’autre version.

**Synchronise**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Synchrone**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Pour plus d’informations sur l’utilisation des méthodes asynchrones dans ASP.NET 4,5, consultez [utilisation de méthodes asynchrones dans ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Définir des surcharges

Si vous souhaitez définir des surcharges pour une méthode, le nombre de paramètres dans chaque surcharge doit être différent. Si vous Différenciez une surcharge simplement en spécifiant des types de paramètres différents, votre classe de concentrateur se compilera, mais le service Signalr lèvera une exception au moment de l’exécution lorsque les clients essaieront d’appeler l’une des surcharges.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Signalement de la progression des appels de méthode de concentrateur

Signalr 2,1 ajoute la prise en charge du [modèle de rapport de progression](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduit dans .net 4,5. Pour implémenter la création de rapports de progression, définissez un paramètre de `IProgress<T>` pour votre méthode de concentrateur à laquelle votre client peut accéder :

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Lors de l’écriture d’une méthode serveur longue, il est important d’utiliser un modèle de programmation asynchrone comme Async/await plutôt que de bloquer le thread Hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Comment appeler des méthodes clientes à partir de la classe de concentrateur

Pour appeler des méthodes clientes à partir du serveur, utilisez la propriété `Clients` dans une méthode de votre classe de concentrateur. L’exemple suivant montre le code serveur qui appelle `addNewMessageToPage` sur tous les clients connectés et le code client qui définit la méthode dans un client JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

L’appel d’une méthode cliente est une opération asynchrone et retourne un `Task`. Utilisez `await`:

* Pour vous assurer que le message est envoyé sans erreur. 
* Pour activer l’interception et la gestion des erreurs dans un bloc try-catch.

**Client JavaScript utilisant le proxy généré**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Vous ne pouvez pas obtenir une valeur de retour d’une méthode cliente ; la syntaxe telle que `int x = Clients.All.add(1,1)` ne fonctionne pas.

Vous pouvez spécifier des types complexes et des tableaux pour les paramètres. L’exemple suivant passe un type complexe au client dans un paramètre de méthode.

**Code serveur qui appelle une méthode cliente à l’aide d’un objet complexe**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Code serveur qui définit l’objet complexe**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Sélection des clients qui recevront le RPC

La propriété clients retourne un objet [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) qui fournit plusieurs options pour spécifier les clients qui recevront le RPC :

- Tous les clients connectés.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Seul le client appelant.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Tous les clients à l’exception du client appelant.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Client spécifique identifié par l’ID de connexion.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Cet exemple appelle `addContosoChatMessageToPage` sur le client appelant et a le même effet que l’utilisation de `Clients.Caller`.
- Tous les clients connectés, à l’exception des clients spécifiés, identifiés par l’ID de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Tous les clients connectés dans un groupe spécifié.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifiés par l’ID de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Tous les clients connectés dans un groupe spécifié, à l’exception du client appelant.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Utilisateur spécifique, identifié par userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Par défaut, il s’agit de `IPrincipal.Identity.Name`, mais vous pouvez le modifier en [inscrivant une implémentation de IUserIdProvider auprès de l’hôte global](mapping-users-to-connections.md#IUserIdProvider).
- Tous les clients et groupes dans une liste d’ID de connexion.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Liste de groupes.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Utilisateur par nom.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Liste de noms d’utilisateurs (introduite dans Signalr 2,1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Aucune validation de la compilation pour les noms de méthode

Le nom de la méthode que vous spécifiez est interprété comme un objet dynamique, ce qui signifie qu’il n’y a pas de validation IntelliSense ou de compilation pour celui-ci. L’expression est évaluée au moment de l’exécution. Lorsque l’appel de méthode s’exécute, Signalr envoie le nom de la méthode et les valeurs de paramètre au client, et si le client a une méthode qui correspond au nom, cette méthode est appelée et les valeurs de paramètre lui sont passées. Si aucune méthode correspondante n’est trouvée sur le client, aucune erreur n’est générée. Pour plus d’informations sur le format des données que Signalr transmet au client en arrière-plan quand vous appelez une méthode cliente, consultez [Présentation de signalr](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Correspondance de nom de méthode ne respectant pas la casse

La correspondance de nom de méthode ne respecte pas la casse. Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur exécute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`ou `addContosoChatMessageToPage` sur le client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Exécution asynchrone

La méthode que vous appelez s’exécute de façon asynchrone. Tout code qui vient après un appel de méthode à un client s’exécute immédiatement sans attendre que Signalr termine la transmission des données aux clients, sauf si vous spécifiez que les lignes de code suivantes doivent attendre la fin de la méthode. L’exemple de code suivant montre comment exécuter deux méthodes client séquentiellement.

**Utilisation d’await (.NET 4,5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Si vous utilisez `await` pour attendre qu’une méthode client se termine avant l’exécution de la ligne de code suivante, cela ne signifie pas que les clients recevront réellement le message avant l’exécution de la ligne de code suivante. La « saisie semi-automatique » d’un appel de méthode client signifie uniquement que Signalr a effectué tout ce qui est nécessaire pour envoyer le message. Si vous avez besoin de vérifier que les clients ont reçu le message, vous devez programmer ce mécanisme vous-même. Par exemple, vous pouvez coder une méthode `MessageReceived` sur le Hub et, dans la méthode `addContosoChatMessageToPage` sur le client, vous pouvez appeler `MessageReceived` une fois que vous avez effectué le travail que vous devez effectuer sur le client. Dans `MessageReceived` dans le Hub, vous pouvez effectuer toutes les tâches qui dépendent de la réception et du traitement réels de l’appel de la méthode d’origine.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Utilisation d’une variable chaîne comme nom de méthode

Si vous souhaitez appeler une méthode cliente en utilisant une variable de chaîne comme nom de méthode, effectuez un cast `Clients.All` (ou `Clients.Others`, `Clients.Caller`, etc.) en `IClientProxy` puis appelez [Invoke (MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Comment gérer l’appartenance à un groupe à partir de la classe de concentrateur

Dans Signalr, les groupes fournissent une méthode de diffusion des messages aux sous-ensembles spécifiés de clients connectés. Un groupe peut avoir un nombre quelconque de clients et un client peut être membre d’un nombre quelconque de groupes.

Pour gérer l’appartenance à un groupe, utilisez les méthodes [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) et [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) fournies par la propriété `Groups` de la classe Hub. L’exemple suivant montre les méthodes `Groups.Add` et `Groups.Remove` utilisées dans les méthodes de concentrateur qui sont appelées par le code client, suivies par le code client JavaScript qui les appelle.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Client JavaScript utilisant le proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Vous n’êtes pas obligé de créer explicitement des groupes. En effet, un groupe est créé automatiquement la première fois que vous spécifiez son nom dans un appel à `Groups.Add`, et il est supprimé lorsque vous supprimez la dernière connexion de l’appartenance à celui-ci.

Il n’existe aucune API pour obtenir une liste d’appartenance à un groupe ou une liste de groupes. Signalr envoie des messages aux clients et aux groupes en fonction d’un [modèle Pub/Sub](http://en.wikipedia.org/wiki/Publish/subscribe), et le serveur ne gère pas les listes de groupes ou d’appartenances aux groupes. Cela permet d’optimiser l’évolutivité, car chaque fois que vous ajoutez un nœud à une batterie de serveurs Web, tout état géré par Signalr doit être propagé vers le nouveau nœud.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Exécution asynchrone de méthodes Add et Remove

Les méthodes `Groups.Add` et `Groups.Remove` s’exécutent de façon asynchrone. Si vous souhaitez ajouter un client à un groupe et envoyer immédiatement un message au client à l’aide du groupe, vous devez vous assurer que la méthode `Groups.Add` se termine en premier. L’exemple de code suivant montre comment procéder.

**Ajout d’un client à un groupe, puis messagerie du client**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistance de l’appartenance aux groupes

Signalr effectue le suivi des connexions et non des utilisateurs. par conséquent, si vous souhaitez qu’un utilisateur se trouve dans le même groupe chaque fois que l’utilisateur établit une connexion, vous devez appeler `Groups.Add` chaque fois que l’utilisateur établit une nouvelle connexion.

Après une perte temporaire de connectivité, Signalr peut parfois restaurer la connexion automatiquement. Dans ce cas, Signalr restaure la même connexion, et non l’établissement d’une nouvelle connexion, et l’appartenance au groupe du client est donc automatiquement restaurée. Cela est possible même lorsque l’interruption temporaire est le résultat d’un redémarrage ou d’une défaillance du serveur, car l’état de la connexion pour chaque client, y compris les appartenances aux groupes, fait l’aller-retour au client. Si un serveur tombe en panne et est remplacé par un nouveau serveur avant l’expiration de la connexion, un client peut se reconnecter automatiquement au nouveau serveur et s’inscrire à nouveau dans les groupes dont il est membre.

Lorsqu’une connexion ne peut pas être restaurée automatiquement après une perte de connectivité ou lorsque la connexion expire, ou lorsque le client se déconnecte (par exemple, lorsqu’un navigateur accède à une nouvelle page), les appartenances aux groupes sont perdues. La prochaine fois que l’utilisateur se connecte, une nouvelle connexion est établie. Pour conserver les appartenances aux groupes lorsque le même utilisateur établit une nouvelle connexion, votre application doit suivre les associations entre les utilisateurs et les groupes, et restaurer les appartenances aux groupes chaque fois qu’un utilisateur établit une nouvelle connexion.

Pour plus d’informations sur les connexions et les reconnexions, consultez [Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](#connectionlifetime) plus loin dans cette rubrique.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Groupes à un seul utilisateur

Les applications qui utilisent Signalr doivent généralement effectuer le suivi des associations entre les utilisateurs et les connexions afin de savoir quel utilisateur a envoyé un message et quels utilisateurs doivent recevoir un message. Les groupes sont utilisés dans l’un des deux modèles couramment utilisés pour cela.

- Groupes à un seul utilisateur.

    Vous pouvez spécifier le nom d’utilisateur comme nom de groupe et ajouter l’ID de connexion actuel au groupe chaque fois que l’utilisateur se connecte ou se reconnecte. Envoyer des messages à l’utilisateur que vous envoyez au groupe. L’inconvénient de cette méthode est que le groupe ne vous offre pas la possibilité de déterminer si l’utilisateur est en ligne ou hors connexion.
- Effectuer le suivi des associations entre les noms d’utilisateur et les ID de connexion.

    Vous pouvez stocker une association entre chaque nom d’utilisateur et un ou plusieurs ID de connexion dans un dictionnaire ou une base de données, et mettre à jour les données stockées chaque fois que l’utilisateur se connecte ou se déconnecte. Pour envoyer des messages à l’utilisateur, vous spécifiez les ID de connexion. L’inconvénient de cette méthode est qu’elle nécessite plus de mémoire.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur

Les raisons typiques de la gestion des événements de durée de vie de la connexion sont de savoir si un utilisateur est connecté ou non, et d’effectuer le suivi de l’association entre les noms d’utilisateur et les ID de connexion. Pour exécuter votre propre code lorsque les clients se connectent ou se déconnectent, substituez les méthodes virtuelles `OnConnected`, `OnDisconnected`et `OnReconnected` de la classe de concentrateur, comme indiqué dans l’exemple suivant.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Quand OnConnected, OnDisconnected et OnReconnected sont appelés

Chaque fois qu’un navigateur accède à une nouvelle page, une nouvelle connexion doit être établie, ce qui signifie que Signalr exécutera la méthode `OnDisconnected` suivie de la méthode `OnConnected`. Signalr crée toujours un nouvel ID de connexion lorsqu’une nouvelle connexion est établie.

La méthode `OnReconnected` est appelée lorsqu’il existe une interruption temporaire de la connectivité à partir de laquelle Signalr peut récupérer automatiquement, par exemple, lorsqu’un câble est temporairement déconnecté et reconnecté avant que la connexion n’expire. La méthode `OnDisconnected` est appelée lorsque le client est déconnecté et Signalr ne peut pas se reconnecter automatiquement, par exemple lorsqu’un navigateur accède à une nouvelle page. Par conséquent, une séquence possible d’événements pour un client donné est `OnConnected`, `OnReconnected``OnDisconnected`; ou `OnConnected`, `OnDisconnected`. Vous ne verrez pas la séquence `OnConnected`, `OnDisconnected``OnReconnected` pour une connexion donnée.

La méthode `OnDisconnected` n’est pas appelée dans certains scénarios, par exemple lorsqu’un serveur tombe en panne ou que le domaine d’application est recyclé. Lorsqu’un autre serveur est en ligne ou que le domaine d’application termine son recyclage, certains clients peuvent être en mesure de se reconnecter et de déclencher l’événement `OnReconnected`.

Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie de connexion dans signalr](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>L’état de l’appelant n’est pas rempli

Les méthodes de gestionnaire d’événements de durée de vie de connexion sont appelées à partir du serveur, ce qui signifie que tous les États que vous placez dans l’objet `state` sur le client ne seront pas remplis dans la propriété `Caller` sur le serveur. Pour plus d’informations sur l’objet `state` et la propriété `Caller`, consultez Guide pratique [pour passer l’état entre les clients et la classe Hub](#passstate) plus loin dans cette rubrique.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Comment obtenir des informations sur le client à partir de la propriété de contexte

Pour obtenir des informations sur le client, utilisez la propriété `Context` de la classe Hub. La propriété `Context` retourne un objet [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) qui fournit l’accès aux informations suivantes :

- ID de connexion du client appelant.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    L’ID de connexion est un GUID assigné par Signalr (vous ne pouvez pas spécifier la valeur dans votre propre code). Il existe un ID de connexion pour chaque connexion, et le même ID de connexion est utilisé par tous les hubs si vous avez plusieurs hubs dans votre application.
- Données d’en-tête HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Vous pouvez également récupérer des en-têtes HTTP à partir de `Context.Headers`. La raison pour laquelle plusieurs références à la même chose est que `Context.Headers` a été créé en premier, la propriété `Context.Request` a été ajoutée ultérieurement et `Context.Headers` a été conservée pour la compatibilité descendante.
- Données de chaîne de requête.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Vous pouvez également obtenir des données de chaîne de requête à partir de `Context.QueryString`.

    La chaîne de requête que vous recevez dans cette propriété est celle qui a été utilisée avec la requête HTTP qui a établi la connexion Signalr. Vous pouvez ajouter des paramètres de chaîne de requête dans le client en configurant la connexion, qui est un moyen pratique de transmettre des données sur le client du client au serveur. L’exemple suivant montre une façon d’ajouter une chaîne de requête dans un client JavaScript lorsque vous utilisez le proxy généré.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Pour plus d’informations sur la définition des paramètres de chaîne de requête, consultez les guides d’API pour les clients [JavaScript](hubs-api-guide-javascript-client.md) et [.net](hubs-api-guide-net-client.md) .

    Vous pouvez trouver la méthode de transport utilisée pour la connexion dans les données de la chaîne de requête, ainsi que d’autres valeurs utilisées en interne par Signalr :

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    La valeur de `transportMethod` sera « WebSockets », « serverSentEvents », « foreverFrame » ou « longPolling ». Notez que si vous activez cette valeur dans la `OnConnected` méthode de gestionnaire d’événements, dans certains scénarios, vous pouvez obtenir initialement une valeur de transport qui n’est pas la méthode de transport négociée finale pour la connexion. Dans ce cas, la méthode lèvera une exception et sera appelée à nouveau ultérieurement lorsque la méthode de transport finale sera établie.
- Internes.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Vous pouvez également récupérer des cookies à partir de `Context.RequestCookies`.
- informations utilisateur.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Objet HttpContext pour la demande :

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilisez cette méthode au lieu d’obtenir `HttpContext.Current` pour obtenir l’objet `HttpContext` pour la connexion Signalr.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Comment passer l’état entre les clients et la classe de concentrateur

Le proxy client fournit un objet `state` dans lequel vous pouvez stocker les données que vous souhaitez transmettre au serveur avec chaque appel de méthode. Sur le serveur, vous pouvez accéder à ces données dans la propriété `Clients.Caller` dans les méthodes de concentrateur qui sont appelées par les clients. La propriété `Clients.Caller` n’est pas remplie pour les méthodes de gestionnaire d’événements de durée de vie de connexion `OnConnected`, `OnDisconnected`et `OnReconnected`.

La création ou la mise à jour de données dans l’objet `state` et la propriété `Clients.Caller` fonctionnent dans les deux sens. Vous pouvez mettre à jour les valeurs du serveur et elles sont renvoyées au client.

L’exemple suivant montre un code JavaScript client qui stocke l’État pour la transmission au serveur avec chaque appel de méthode.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

L’exemple suivant montre le code équivalent dans un client .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Dans votre classe de concentrateur, vous pouvez accéder à ces données dans la propriété `Clients.Caller`. L’exemple suivant montre le code qui récupère l’État référencé dans l’exemple précédent.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Ce mécanisme de persistance de l’État n’est pas destiné à de grandes quantités de données, car tout ce que vous placez dans la propriété `state` ou `Clients.Caller` est un aller-retour avec chaque appel de méthode. Elle est utile pour des éléments plus petits, tels que des noms d’utilisateurs ou des compteurs.

Dans VB.NET ou dans un concentrateur fortement typé, l’objet d’état de l’appelant n’est pas accessible via `Clients.Caller`; Utilisez plutôt `Clients.CallerState` (introduite dans Signalr 2,1) :

**Utilisation de CallerState dansC#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Utilisation de CallerState dans Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Comment gérer les erreurs dans la classe de concentrateur

Pour gérer les erreurs qui se produisent dans vos méthodes de classe de concentrateur, commencez par vous assurer d’observer les exceptions des opérations asynchrones (telles que l’appel de méthodes clientes) à l’aide de `await`. Utilisez ensuite une ou plusieurs des méthodes suivantes :

- Encapsulez votre code de méthode dans des blocs try-catch et journalisez l’objet exception. À des fins de débogage, vous pouvez envoyer l’exception au client, mais pour des raisons de sécurité, il n’est pas recommandé d’envoyer des informations détaillées aux clients en production.
- Créez un module de pipeline hubs qui gère la méthode [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) . L’exemple suivant montre un module de pipeline qui journalise des erreurs, suivi du code dans Startup.cs qui injecte le module dans le pipeline hubs.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Utilisez la classe `HubException` (introduite dans Signalr 2). Cette erreur peut être levée à partir de n’importe quel appel de concentrateur. Le constructeur `HubError` prend un message de type chaîne et un objet pour stocker les données d’erreur supplémentaires. Signalr effectue automatiquement la sérialisation de l’exception et l’envoie au client, où elle sera utilisée pour rejeter ou faire échouer l’appel de la méthode de concentrateur.

    Les exemples de code suivants montrent comment lever une `HubException` pendant un appel de Hub et comment gérer l’exception sur les clients JavaScript et .NET.

    **Code serveur illustrant la classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Code client JavaScript illustrant la réponse à la levée d’un HubException dans un Hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Code client .NET illustrant la réponse à la levée d’un HubException dans un Hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Pour plus d’informations sur les modules de pipeline Hub, consultez [Comment personnaliser le pipeline hubs](#hubpipeline) plus loin dans cette rubrique.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Comment activer le suivi

Pour activer le suivi côté serveur, ajoutez un élément System. Diagnostics à votre fichier Web. config, comme indiqué dans cet exemple :

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Lorsque vous exécutez l’application dans Visual Studio, vous pouvez afficher les journaux dans la fenêtre **sortie** .

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Comment appeler des méthodes clientes et gérer des groupes à partir de l’extérieur de la classe de concentrateur

Pour appeler des méthodes clientes à partir d’une classe différente de celle de votre classe de concentrateur, vous pouvez obtenir une référence à l’objet de contexte Signalr pour le Hub et l’utiliser pour appeler des méthodes sur le client ou gérer des groupes.

L’exemple suivant `StockTicker` classe obtient l’objet de contexte, le stocke dans une instance de la classe, stocke l’instance de classe dans une propriété statique et utilise le contexte de l’instance de classe singleton pour appeler la méthode `updateStockPrice` sur les clients qui sont connectés à un concentrateur nommé `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Si vous devez utiliser le contexte plusieurs fois dans un objet à durée de vie longue, obtenez la référence une fois et enregistrez-la au lieu de la récupérer à chaque fois. L’obtention du contexte une fois garantit que Signalr envoie des messages aux clients dans la même séquence que celle dans laquelle vos méthodes de concentrateur effectuent des appels de méthode client. Pour obtenir un didacticiel qui montre comment utiliser le contexte Signalr pour un Hub, consultez [diffusion sur le serveur avec ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Appel des méthodes clientes

Vous pouvez spécifier les clients qui recevront le RPC, mais vous avez moins d’options que lorsque vous appelez à partir d’une classe de concentrateur. Cela est dû au fait que le contexte n’est pas associé à un appel particulier d’un client, de sorte que toutes les méthodes qui nécessitent une connaissance de l’ID de connexion actuel, telles que `Clients.Others`ou `Clients.Caller`ou `Clients.OthersInGroup`, ne sont pas disponibles. Les options ci-dessous sont disponibles :

- Tous les clients connectés.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Client spécifique identifié par l’ID de connexion.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Tous les clients connectés, à l’exception des clients spécifiés, identifiés par l’ID de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Tous les clients connectés dans un groupe spécifié.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifiés par l’ID de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Si vous appelez votre classe non Hub à partir de méthodes dans votre classe de concentrateur, vous pouvez passer l’ID de connexion actuel et l’utiliser avec `Clients.Client`, `Clients.AllExcept`ou `Clients.Group` pour simuler `Clients.Caller`, `Clients.Others`ou `Clients.OthersInGroup`. Dans l’exemple suivant, la classe `MoveShapeHub` passe l’ID de connexion à la classe `Broadcaster` afin que la classe `Broadcaster` puisse simuler `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gestion de l’appartenance à un groupe

Pour la gestion des groupes, vous disposez des mêmes options que dans une classe de concentrateur.

- Ajouter un client à un groupe

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Supprimer un client d’un groupe

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Comment personnaliser le pipeline hubs

Signalr vous permet d’injecter votre propre code dans le pipeline de concentrateur. L’exemple suivant montre un module de pipeline Hub personnalisé qui journalise chaque appel de méthode entrante reçu du client et l’appel de méthode sortante appelé sur le client :

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Le code suivant dans le fichier *Startup.cs* inscrit le module pour qu’il s’exécute dans le pipeline du Hub :

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Il existe de nombreuses méthodes que vous pouvez substituer. Pour obtenir une liste complète, consultez [méthodes HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
