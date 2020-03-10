---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guide de l’API des hubs ASP.NET Signalr-client JavaScript | Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients JavaScript, tels que les navigateurs et le Windows Store (WinJS) applicat...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536658"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guide de l’API Hub Signalr ASP.NET-client JavaScript

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients JavaScript, tels que les navigateurs et les applications du Windows Store (WinJS).
>
> L’API Signalr hubs vous permet d’effectuer des appels de procédure distante (RPC) à partir d’un serveur vers des clients connectés et des clients vers le serveur. Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client. Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur. Signalr prend en charge tous les éléments de la plombation client à serveur pour vous.
>
> Signalr offre également une API de bas niveau appelée connexions persistantes. Pour une présentation de Signalr, de hubs et de connexions persistantes, consultez [Présentation de signalr](../getting-started/introduction-to-signalr.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans cette rubrique
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> - .NET 4.5
> - Signalr version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de cette rubrique
>
> Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Présentation

Ce document contient les sections suivantes :

- [Le proxy généré et ce qu’il fait pour vous](#genproxy)

    - [Quand utiliser le proxy généré](#cantusegenproxy)
- [Installation du client](#clientsetup)

    - [Comment référencer le proxy généré dynamiquement](#dynamicproxy)
    - [Comment créer un fichier physique pour le proxy généré par Signalr](#manualproxy)
- [Comment établir une connexion](#establishconnection)

    - [$. Connection. Hub est le même objet que $. hubConnection () crée](#connequivalence)
    - [Exécution asynchrone de la méthode Start](#asyncstart)
- [Comment établir une connexion inter-domaines](#crossdomain)
- [Comment configurer la connexion](#configureconnection)

    - [Comment spécifier des paramètres de chaîne de requête](#querystring)
    - [Comment spécifier la méthode de transport](#transport)
- [Obtention d’un proxy pour une classe de concentrateur](#getproxy)
- [Comment définir des méthodes sur le client que le serveur peut appeler](#callclient)
- [Comment appeler des méthodes de serveur à partir du client](#callserver)
- [Comment gérer les événements de durée de vie de connexion](#connectionlifetime)
- [Gestion des erreurs](#handleerrors)
- [Comment activer la journalisation côté client](#logging)

Pour obtenir de la documentation sur la programmation du serveur ou des clients .NET, consultez les ressources suivantes :

- [Guide de l’API signalr hubs-serveur](hubs-api-guide-server.md)
- [Guide de l’API signalr hubs-client .NET](hubs-api-guide-net-client.md)

Le composant serveur Signalr 2 est disponible uniquement sur .NET 4,5 (bien qu’il existe un client .NET pour Signalr 2 sur .NET 4,0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Le proxy généré et ce qu’il fait pour vous

Vous pouvez programmer un client JavaScript pour communiquer avec un service Signalr avec ou sans proxy généré par Signalr. Le proxy vous permet de simplifier la syntaxe du code que vous utilisez pour vous connecter, d’écrire les méthodes appelées par le serveur et d’appeler des méthodes sur le serveur.

Lorsque vous écrivez du code pour appeler des méthodes de serveur, le proxy généré vous permet d’utiliser une syntaxe qui semble que vous exécutiez une fonction locale : vous pouvez écrire `serverMethod(arg1, arg2)` au lieu de `invoke('serverMethod', arg1, arg2)`. La syntaxe de proxy générée active également une erreur immédiate et intelligible côté client si vous tapez un nom de méthode de serveur de manière invoulue. Et si vous créez manuellement le fichier qui définit les proxies, vous pouvez également obtenir la prise en charge IntelliSense pour écrire du code qui appelle des méthodes de serveur.

Supposons, par exemple, que vous disposiez de la classe de concentrateur suivante sur le serveur :

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Les exemples de code suivants montrent le code JavaScript qui ressemble à l’appel de la méthode `NewContosoChatMessage` sur le serveur et la réception des appels de la méthode `addContosoChatMessageToPage` à partir du serveur.

**Avec le proxy généré**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sans le proxy généré**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quand utiliser le proxy généré

Si vous souhaitez inscrire plusieurs gestionnaires d’événements pour une méthode cliente appelée par le serveur, vous ne pouvez pas utiliser le proxy généré. Dans le cas contraire, vous pouvez choisir d’utiliser le proxy généré ou non en fonction de vos préférences de codage. Si vous choisissez de ne pas l’utiliser, vous ne devez pas faire référence à l’URL « signalr/hubs » dans un élément `script` dans votre code client.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuration cliente

Un client JavaScript requiert des références à jQuery et au fichier JavaScript du noyau Signalr. La version jQuery doit être 1.6.4 ou les versions majeures ultérieures, telles que 1.7.2, 1.8.2 ou 1.9.1. Si vous décidez d’utiliser le proxy généré, vous avez également besoin d’une référence au fichier JavaScript du proxy généré par Signalr. L’exemple suivant montre à quoi les références peuvent ressembler dans une page HTML qui utilise le proxy généré.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Ces références doivent être incluses dans cet ordre : jQuery First, Signalr Core après cela et les proxys de Signalr en dernier.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Comment référencer le proxy généré dynamiquement

Dans l’exemple précédent, la référence au proxy généré par Signalr consiste à générer dynamiquement du code JavaScript, et non un fichier physique. Signalr crée le code JavaScript pour le proxy à la volée et le transmet au client en réponse à l’URL « /signalr/hubs ». Si vous avez spécifié une autre URL de base pour les connexions Signalr sur le serveur dans votre méthode `MapSignalR`, l’URL du fichier proxy généré dynamiquement est votre URL personnalisée avec « /hubs » ajouté.

> [!NOTE]
> Pour les clients JavaScript Windows 8 (Windows Store), utilisez le fichier de proxy physique au lieu de celui généré dynamiquement. Pour plus d’informations, consultez [comment créer un fichier physique pour le proxy généré par signalr](#manualproxy) plus loin dans cette rubrique.

Dans une vue Razor ASP.NET MVC 4 ou 5, utilisez le tilde pour faire référence à la racine de l’application dans votre référence de fichier proxy :

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Pour plus d’informations sur l’utilisation de Signalr dans MVC 5, consultez [prise en main avec signalr et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Dans une vue Razor ASP.NET MVC 3, utilisez `Url.Content` pour la référence de votre fichier proxy :

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Dans une application ASP.NET Web Forms, utilisez `ResolveClientUrl` pour la référence de votre fichier proxys ou enregistrez-la via ScriptManager à l’aide d’un chemin d’accès relatif à la racine de l’application (à partir d’un tilde) :

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

En règle générale, utilisez la même méthode pour spécifier l’URL « /signalr/hubs » que vous utilisez pour les fichiers CSS ou JavaScript. Si vous spécifiez une URL sans utiliser de tilde, dans certains scénarios, votre application fonctionnera correctement lorsque vous testerez dans Visual Studio à l’aide de IIS Express mais échouera avec une erreur 404 quand vous effectuerez le déploiement sur IIS complet. Pour plus d’informations, consultez **résolution des références aux ressources au niveau** de la racine dans les [serveurs Web dans Visual Studio pour les projets Web ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) sur le site MSDN.

Quand vous exécutez un projet Web dans Visual Studio 2017 en mode débogage et que vous utilisez Internet Explorer comme navigateur, vous pouvez voir le fichier proxy dans **Explorateur de solutions** sous **scripts**.

Pour afficher le contenu du fichier, double-cliquez sur **hubs**. Si vous n’utilisez pas Visual Studio 2012 ou 2013 et Internet Explorer, ou si vous n’êtes pas en mode débogage, vous pouvez également récupérer le contenu du fichier en accédant à l’URL « /signalR/hubs ». Par exemple, si votre site s’exécute sur `http://localhost:56699`, accédez à `http://localhost:56699/SignalR/hubs` dans votre navigateur.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Comment créer un fichier physique pour le proxy généré par Signalr

Comme alternative au proxy généré dynamiquement, vous pouvez créer un fichier physique qui contient le code proxy et référencer ce fichier. Vous pouvez le faire pour contrôler le comportement de la mise en cache ou du regroupement, ou pour obtenir IntelliSense quand vous encodez des appels à des méthodes de serveur.

Pour créer un fichier proxy, procédez comme suit :

1. Installez le package NuGet [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Ouvrez une invite de commandes et accédez au dossier *Tools* qui contient le fichier signalr. exe. Le dossier Tools se trouve à l’emplacement suivant :

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Entrez la commande suivante :

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Le chemin d’accès à votre *fichier. dll* est généralement le dossier *bin* dans le dossier de votre projet.

    Cette commande crée un fichier nommé *Server. js* dans le même dossier que *signalr. exe*.
4. Placez le fichier *Server. js* dans un dossier approprié de votre projet, renommez-le en fonction de votre application et ajoutez une référence à celui-ci à la place de la référence « signalr/hubs ».

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Comment établir une connexion

Avant de pouvoir établir une connexion, vous devez créer un objet de connexion, créer un proxy et inscrire des gestionnaires d’événements pour les méthodes qui peuvent être appelées à partir du serveur. Lorsque le proxy et les gestionnaires d’événements sont configurés, établissez la connexion en appelant la méthode `start`.

Si vous utilisez le proxy généré, vous n’êtes pas obligé de créer l’objet de connexion dans votre propre code, car le code proxy généré le fait pour vous.

<a id="nogenconnection"></a>

**Établir une connexion (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Établir une connexion (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

L’exemple de code utilise l’URL « /signalr » par défaut pour se connecter à votre service Signalr. Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.net signalr hubs-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).

Par défaut, l’emplacement du concentrateur est le serveur actif. Si vous vous connectez à un autre serveur, spécifiez l’URL avant d’appeler la méthode `start`, comme indiqué dans l’exemple suivant :

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalement, vous inscrivez les gestionnaires d’événements avant d’appeler la méthode `start` pour établir la connexion. Si vous souhaitez inscrire certains gestionnaires d’événements après avoir établi la connexion, vous pouvez le faire, mais vous devez inscrire au moins l’un de vos gestionnaires d’événements avant d’appeler la méthode `start`. Cela peut être dû au fait qu’il peut y avoir de nombreux hubs dans une application, mais que vous ne souhaitez pas déclencher l’événement `OnConnected` sur chaque concentrateur si vous n’utilisez que l’un d’entre eux. Lorsque la connexion est établie, la présence d’une méthode cliente sur le proxy d’un concentrateur est ce qui indique à Signalr de déclencher l’événement `OnConnected`. Si vous n’inscrivez aucun gestionnaire d’événements avant d’appeler la méthode `start`, vous serez en mesure d’appeler des méthodes sur le concentrateur, mais la méthode `OnConnected` du concentrateur ne sera pas appelée et aucune méthode client ne sera appelée à partir du serveur.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub est le même objet que $. hubConnection () crée

Comme vous pouvez le voir dans les exemples, lorsque vous utilisez le proxy généré, `$.connection.hub` fait référence à l’objet de connexion. Il s’agit du même objet que celui obtenu en appelant `$.hubConnection()` lorsque vous n’utilisez pas le proxy généré. Le code proxy généré crée la connexion pour vous en exécutant l’instruction suivante :

![Création d’une connexion dans le fichier proxy généré](hubs-api-guide-javascript-client/_static/image3.png)

Lorsque vous utilisez le proxy généré, vous pouvez faire quoi que ce soit avec `$.connection.hub` que vous pouvez faire avec un objet de connexion lorsque vous n’utilisez pas le proxy généré.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Exécution asynchrone de la méthode Start

La méthode `start` s’exécute de façon asynchrone. Elle retourne un [objet différé jQuery](http://api.jquery.com/category/deferred-object/), ce qui signifie que vous pouvez ajouter des fonctions de rappel en appelant des méthodes telles que `pipe`, `done`et `fail`. Si vous avez du code que vous souhaitez exécuter après l’établissement de la connexion, par exemple un appel à une méthode de serveur, placez ce code dans une fonction de rappel ou appelez-le à partir d’une fonction de rappel. La méthode de rappel `.done` est exécutée une fois que la connexion a été établie, et après la fin de l’exécution du code que vous avez dans votre méthode de gestionnaire d’événements `OnConnected` sur le serveur.

Si vous placez l’instruction « Now Connected » de l’exemple précédent comme ligne de code suivante après l’appel de la méthode `start` (et non dans un rappel `.done`), la ligne `console.log` s’exécutera avant l’établissement de la connexion, comme illustré dans l’exemple suivant :

![Méthode incorrecte pour écrire du code qui s’exécute après l’établissement de la connexion](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Comment établir une connexion inter-domaines

En général, si le navigateur charge une page à partir de `http://contoso.com`, la connexion Signalr se trouve dans le même domaine, à `http://contoso.com/signalr`. Si la page de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, il s’agit d’une connexion inter-domaines. Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.

Dans Signalr 1. x, les demandes inter-domaines étaient contrôlées par un seul indicateur EnableCrossDomain. Cet indicateur contrôlait à la fois les demandes JSONP et CORS. Pour une plus grande flexibilité, toute la prise en charge de CORS a été supprimée du composant serveur de Signalr (les clients JavaScript continuent d’utiliser CORS normalement s’il est détecté que le navigateur le prend en charge) et le nouveau middleware OWIN a été mis à disposition pour prendre en charge ces scénarios.

Si JSONP est requis sur le client (pour prendre en charge les requêtes inter-domaines dans les navigateurs plus anciens), vous devez l’activer explicitement en définissant `EnableJSONP` sur l’objet `HubConfiguration` à `true`, comme indiqué ci-dessous. JSONP est désactivé par défaut, car il est moins sécurisé que CORS.

**Ajout de Microsoft. Owin. cors à votre projet :** Pour installer cette bibliothèque, exécutez la commande suivante dans la console du gestionnaire de package :

`Install-Package Microsoft.Owin.Cors`

Cette commande ajoute la version 2.1.0 du package à votre projet.

### <a name="calling-usecors"></a>Appel de UseCors

 L’extrait de code suivant montre comment implémenter des connexions inter-domaines dans Signalr 2.

**Implémentation de requêtes inter-domaines dans Signalr 2**

Le code suivant montre comment activer CORS ou JSONP dans un projet Signalr 2. Cet exemple de code utilise `Map` et `RunSignalR` au lieu de `MapSignalR`, de sorte que l’intergiciel (middleware) CORS s’exécute uniquement pour les demandes Signalr qui requièrent la prise en charge de CORS (plutôt que pour tout le trafic au niveau du chemin spécifié dans `MapSignalR`). La carte peut également être utilisée pour tout autre intergiciel qui doit s’exécuter pour un préfixe d’URL spécifique, plutôt que pour l’application entière.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - N’affectez pas la valeur true à `jQuery.support.cors` dans votre code.
>
>     ![N’affectez pas la valeur true à jQuery. support. cors](hubs-api-guide-javascript-client/_static/image7.png)
>
>     Signalr gère l’utilisation de CORS. L’affectation de la valeur true à `jQuery.support.cors` désactive JSONP car Signalr peut supposer que le navigateur prend en charge CORS.
> - Lorsque vous vous connectez à une URL localhost, Internet Explorer 10 ne considère pas qu’il s’agit d’une connexion inter-domaines. l’application fonctionnera donc localement avec IE 10, même si vous n’avez pas activé les connexions inter-domaines sur le serveur.
> - Pour plus d’informations sur l’utilisation de connexions inter-domaines avec Internet Explorer 9, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Pour plus d’informations sur l’utilisation de connexions inter-domaines avec chrome, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - L’exemple de code utilise l’URL « /signalr » par défaut pour se connecter à votre service Signalr. Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.net signalr hubs-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Comment configurer la connexion

Avant d’établir une connexion, vous pouvez spécifier des paramètres de chaîne de requête ou spécifier la méthode de transport.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Comment spécifier des paramètres de chaîne de requête

Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion. Les exemples suivants montrent comment définir un paramètre de chaîne de requête dans le code client.

**Définir une valeur de chaîne de requête avant d’appeler la méthode Start (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Définir une valeur de chaîne de requête avant d’appeler la méthode Start (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Comment spécifier la méthode de transport

Dans le cadre du processus de connexion, un client Signalr négocie normalement avec le serveur pour déterminer le meilleur transport pris en charge par le serveur et le client. Si vous connaissez déjà le transport que vous souhaitez utiliser, vous pouvez contourner ce processus de négociation en spécifiant la méthode de transport lorsque vous appelez la méthode `start`.

**Code client qui spécifie la méthode de transport (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Code client qui spécifie la méthode de transport (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Vous pouvez également spécifier plusieurs méthodes de transport dans l’ordre dans lequel vous souhaitez que Signalr les teste :

**Code client qui spécifie un schéma de secours de transport personnalisé (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Code client qui spécifie un schéma de secours de transport personnalisé (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Vous pouvez utiliser les valeurs suivantes pour spécifier la méthode de transport :

- WebSockets
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Les exemples suivants montrent comment déterminer quelle méthode de transport est utilisée par une connexion.

**Code client qui affiche la méthode de transport utilisée par une connexion (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Code client qui affiche la méthode de transport utilisée par une connexion (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez Guide de l' [API ASP.net signalr Hub-Server-How pour obtenir des informations sur le client à partir de la propriété de contexte](hubs-api-guide-server.md#contextproperty). Pour plus d’informations sur les transports et les secours, consultez [Présentation de signalr-transports et de secours](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Obtention d’un proxy pour une classe de concentrateur

Chaque objet de connexion que vous créez encapsule des informations sur une connexion à un service Signalr qui contient une ou plusieurs classes de concentrateur. Pour communiquer avec une classe de concentrateur, vous utilisez un objet proxy que vous créez vous-même (si vous n’utilisez pas le proxy généré) ou qui est généré pour vous.

Sur le client, le nom du proxy est une version à casse mixte du nom de la classe de concentrateur. Signalr effectue automatiquement cette modification afin que le code JavaScript puisse être conforme aux conventions JavaScript.

**Classe de concentrateur sur le serveur**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obtenir une référence au proxy client généré pour le Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Créer un proxy client pour la classe de concentrateur (sans proxy généré)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Si vous décorez votre classe de concentrateur avec un attribut `HubName`, utilisez le nom exact sans changer la casse.

**Classe de concentrateur sur le serveur avec l’attribut HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obtenir une référence au proxy client généré pour le Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Créer un proxy client pour la classe de concentrateur (sans proxy généré)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Comment définir des méthodes sur le client que le serveur peut appeler

Pour définir une méthode que le serveur peut appeler à partir d’un concentrateur, ajoutez un gestionnaire d’événements au proxy de concentrateur à l’aide de la propriété `client` du proxy généré, ou appelez la méthode `on` si vous n’utilisez pas le proxy généré. Les paramètres peuvent être des objets complexes.

Ajoutez le gestionnaire d’événements avant d’appeler la méthode `start` pour établir la connexion. (Si vous souhaitez ajouter des gestionnaires d’événements après avoir appelé la méthode `start`, consultez la remarque dans [Comment établir une connexion](#establishconnection) plus haut dans ce document et utilisez la syntaxe indiquée pour définir une méthode sans utiliser le proxy généré.)

La correspondance de nom de méthode ne respecte pas la casse. Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur exécute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`ou `addcontosochatmessagetopage` sur le client.

**Définir la méthode sur le client (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Autre façon de définir la méthode sur le client (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Définir la méthode sur le client (sans le proxy généré, ou lors de l’ajout après l’appel de la méthode Start)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Code serveur qui appelle la méthode cliente**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Les exemples suivants incluent un objet complexe en tant que paramètre de méthode.

**Définir la méthode sur le client qui prend un objet complexe (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Définir la méthode sur le client qui prend un objet complexe (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Code serveur qui définit l’objet complexe**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Code serveur qui appelle la méthode cliente à l’aide d’un objet complexe**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Comment appeler des méthodes de serveur à partir du client

Pour appeler une méthode de serveur à partir du client, utilisez la propriété `server` du proxy généré ou la méthode `invoke` sur le proxy Hub si vous n’utilisez pas le proxy généré. Les paramètres ou la valeur de retour peuvent être des objets complexes.

Transmettez une version à casse mixte du nom de la méthode sur le concentrateur. Signalr effectue automatiquement cette modification afin que le code JavaScript puisse être conforme aux conventions JavaScript.

Les exemples suivants montrent comment appeler une méthode de serveur qui n’a pas de valeur de retour et comment appeler une méthode de serveur qui a une valeur de retour.

**Méthode serveur sans attribut HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Code serveur qui définit l’objet complexe passé dans un paramètre**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Code client qui appelle la méthode serveur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Code client qui appelle la méthode serveur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Si vous avez décoré la méthode de concentrateur avec un attribut `HubMethodName`, utilisez ce nom sans modifier la casse.

**Méthode serveur** avec un attribut HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Code client qui appelle la méthode serveur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Code client qui appelle la méthode serveur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Les exemples précédents montrent comment appeler une méthode de serveur qui n’a pas de valeur de retour. Les exemples suivants montrent comment appeler une méthode de serveur qui a une valeur de retour.

**Code serveur pour une méthode qui a une valeur de retour**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**Classe stock utilisée pour la valeur de** retour

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Code client qui appelle la méthode serveur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Code client qui appelle la méthode serveur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Comment gérer les événements de durée de vie de connexion

Signalr fournit les événements de durée de vie de connexion suivants que vous pouvez gérer :

- `starting`: déclenché avant l’envoi de données sur la connexion.
- `received`: déclenché quand des données sont reçues sur la connexion. Fournit les données reçues.
- `connectionSlow`: déclenché lorsque le client détecte une connexion lente ou souvent en cours de suppression.
- `reconnecting`: déclenché lorsque le transport sous-jacent commence à se reconnecter.
- `reconnected`: déclenché lorsque le transport sous-jacent s’est reconnecté.
- `stateChanged`: déclenché lorsque l’état de la connexion change. Fournit l’ancien État et le nouvel État (connexion, connexion, reconnexion ou déconnexion).
- `disconnected`: déclenché lorsque la connexion a été déconnectée.

Par exemple, si vous souhaitez afficher des messages d’avertissement lorsque des problèmes de connexion peuvent entraîner des retards perceptibles, gérez l’événement `connectionSlow`.

**Gérer l’événement connectionSlow (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Gérer l’événement connectionSlow (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie de connexion dans signalr](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Gestion des erreurs

Le client JavaScript Signalr fournit un événement `error` auquel vous pouvez ajouter un gestionnaire. Vous pouvez également utiliser la méthode Fail pour ajouter un gestionnaire pour les erreurs qui résultent d’un appel de méthode serveur.

Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet exception renvoyé par Signalr après une erreur contient un minimum d’informations sur l’erreur. Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`» l’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

L’exemple suivant montre comment ajouter un gestionnaire pour l’événement d’erreur.

**Ajouter un gestionnaire d’erreurs (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Ajouter un gestionnaire d’erreurs (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

L’exemple suivant montre comment gérer une erreur à partir d’un appel de méthode.

**Gérer une erreur à partir d’un appel de méthode (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Gérer une erreur à partir d’un appel de méthode (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Si l’appel d’une méthode échoue, l’événement `error` est également déclenché, de sorte que votre code dans le gestionnaire de méthode `error` et dans le rappel de méthode `.fail` s’exécute.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Comment activer la journalisation côté client

Pour activer la journalisation côté client sur une connexion, définissez la propriété `logging` sur l’objet de connexion avant d’appeler la méthode `start` pour établir la connexion.

**Activer la journalisation (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Activer la journalisation (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Pour afficher les journaux, ouvrez les outils de développement de votre navigateur et accédez à l’onglet Console. Pour obtenir un didacticiel qui contient des instructions pas à pas et des captures d’écran qui montrent comment procéder, consultez diffusion sur le [serveur avec ASP.net signalr-activer la journalisation](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).
