---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guide de l’API des hubs ASP.NET signaler-client .NET (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients .NET, tels que le Windows Store (WinRT), WPF, Silverlight et les inconvénients...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623801"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>Guide de l’API des hubs ASP.NET signaler-client .NET (Signalr 1. x)

de [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients .NET, tels que les applications Windows Store (WinRT), WPF, Silverlight et console.
> 
> L’API Signalr hubs vous permet d’effectuer des appels de procédure distante (RPC) à partir d’un serveur vers des clients connectés et des clients vers le serveur. Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client. Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur. Signalr prend en charge tous les éléments de la plombation client à serveur pour vous.
> 
> Signalr offre également une API de bas niveau appelée connexions persistantes. Pour obtenir une présentation de Signalr, de hubs et de connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application Signalr complète, consultez [signalr-prise en main](../getting-started/index.md).

## <a name="overview"></a>Présentation

Ce document contient les sections suivantes :

- [Installation du client](#clientsetup)
- [Comment établir une connexion](#establishconnection)

    - [Connexions inter-domaines à partir de clients Silverlight](#slcrossdomain)
- [Comment configurer la connexion](#configureconnection)

    - [Comment définir le nombre maximal de connexions simultanées dans les clients WPF](#maxconnections)
    - [Comment spécifier des paramètres de chaîne de requête](#querystring)
    - [Comment spécifier la méthode de transport](#transport)
    - [Comment spécifier des en-têtes HTTP](#httpheaders)
    - [Comment spécifier des certificats clients](#clientcertificate)
- [Comment créer le proxy Hub](#proxy)
- [Comment définir des méthodes sur le client que le serveur peut appeler](#callclient)

    - [Méthodes sans paramètres](#clientmethodswithoutparms)
    - [Méthodes avec des paramètres, spécification de types de paramètres](#clientmethodswithparmtypes)
    - [Méthodes avec des paramètres, spécification d’objets dynamiques pour les paramètres](#clientmethodswithdynamparms)
    - [Comment supprimer un gestionnaire](#removehandler)
- [Comment appeler des méthodes de serveur à partir du client](#callserver)
- [Comment gérer les événements de durée de vie de connexion](#connectionlifetime)
- [Gestion des erreurs](#handleerrors)
- [Comment activer la journalisation côté client](#logging)
- [Exemples de code d’application de console WPF, Silverlight et console pour les méthodes clientes que le serveur peut appeler](#wpfsl)

Pour obtenir un exemple de projets clients .NET, consultez les ressources suivantes :

- [Gustavo-Armenta/signalr-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on github.com (exemples d’applications de console WinRT, Silverlight).
- [DamianEdwards/signalr-MoveShapeDemo/MoveShape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) sur GitHub.com (exemple WPF).
- [Signalr/Microsoft. Aspnet. signalr. client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) sur GitHub.com (exemple d’application console).

Pour obtenir de la documentation sur la programmation du serveur ou des clients JavaScript, consultez les ressources suivantes :

- [Guide de l’API signalr hubs-serveur](../guide-to-the-api/hubs-api-guide-server.md)
- [Guide de l’API signalr hubs-client JavaScript](../guide-to-the-api/hubs-api-guide-javascript-client.md)

Les liens vers les rubriques de référence sur les API concernent la version .NET 4,5 de l’API. Si vous utilisez .NET 4, consultez [la version .net 4 des rubriques de l’API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuration cliente

Installez le package NuGet [Microsoft. Aspnet. signalr. client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (et non le package [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ). Ce package prend en charge WinRT, Silverlight, WPF, l’application console et les clients Windows Phone, pour .NET 4 et .NET 4,5.

Si la version de Signalr sur le client est différente de celle que vous avez sur le serveur, Signalr est souvent en mesure de s’adapter à la différence. Par exemple, lorsque Signalr version 2,0 est libéré et que vous l’installez sur le serveur, le serveur prend en charge les clients sur lesquels 1.1. x est installé, ainsi que les clients sur lesquels 2,0 est installé. Si la différence entre la version sur le serveur et la version sur le client est trop importante, Signalr lève une exception `InvalidOperationException` lorsque le client tente d’établir une connexion. Le message d’erreur est «`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`».

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Comment établir une connexion

Avant de pouvoir établir une connexion, vous devez créer un objet `HubConnection` et créer un proxy. Pour établir la connexion, appelez la méthode `Start` sur l’objet `HubConnection`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Pour les clients JavaScript, vous devez inscrire au moins un gestionnaire d’événements avant d’appeler la méthode `Start` pour établir la connexion. Cela n’est pas nécessaire pour les clients .NET. Pour les clients JavaScript, le code proxy généré crée automatiquement des proxies pour tous les concentrateurs qui existent sur le serveur, et l’inscription d’un gestionnaire est la manière dont vous indiquez les hubs que votre client envisage d’utiliser. Mais pour un client .NET que vous créez manuellement des proxies de Hub, Signalr suppose que vous utiliserez un Hub pour lequel vous créez un proxy.

L’exemple de code utilise l’URL « /signalr » par défaut pour se connecter à votre service Signalr. Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.net signalr hubs-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

La méthode `Start` s’exécute de façon asynchrone. Pour vous assurer que les lignes de code suivantes ne s’exécutent pas tant que la connexion n’est pas établie, utilisez `await` dans une méthode asynchrone ASP.NET 4,5 ou `.Wait()` dans une méthode synchrone. N’utilisez pas `.Wait()` dans un client WinRT.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

La classe `HubConnection` est thread-safe.

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Connexions inter-domaines à partir de clients Silverlight

Pour plus d’informations sur l’activation des connexions inter-domaines à partir de clients Silverlight, consultez Mise à [disposition d’un service au-delà des limites de domaine](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Comment configurer la connexion

Avant d’établir une connexion, vous pouvez spécifier l’une des options suivantes :

- Limite de connexions simultanées.
- Paramètres de chaîne de requête.
- Méthode de transport.
- En-têtes HTTP.
- Certificats clients.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Comment définir le nombre maximal de connexions simultanées dans les clients WPF

Dans les clients WPF, vous devrez peut-être augmenter le nombre maximal de connexions simultanées de la valeur 2 par défaut. La valeur recommandée est 10.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Pour plus d’informations, consultez [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Comment spécifier des paramètres de chaîne de requête

Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion. L’exemple suivant montre comment définir un paramètre de chaîne de requête dans le code client.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Comment spécifier la méthode de transport

Dans le cadre du processus de connexion, un client Signalr négocie normalement avec le serveur pour déterminer le meilleur transport pris en charge par le serveur et le client. Si vous savez déjà quel transport vous souhaitez utiliser, vous pouvez ignorer ce processus de négociation. Pour spécifier la méthode de transport, transmettez un objet de transport à la méthode Start. L’exemple suivant montre comment spécifier la méthode de transport dans le code client.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

L’espace de noms [Microsoft. Aspnet. signalr. client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) contient les classes suivantes que vous pouvez utiliser pour spécifier le transport.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible uniquement lorsque le serveur et le client utilisent le .net 4,5.)
- [Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (choisit automatiquement le transport le mieux pris en charge par le client et le serveur. Il s’agit du transport par défaut. Le passage de cette valeur à la méthode `Start` a le même effet que de ne pas passer quoi que ce soit.)

Le transport ForeverFrame n’est pas inclus dans cette liste, car il n’est utilisé que par les navigateurs.

Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez Guide de l' [API ASP.net signalr Hub-Server-How pour obtenir des informations sur le client à partir de la propriété de contexte](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Pour plus d’informations sur les transports et les secours, consultez [Présentation de signalr-transports et de secours](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Comment spécifier des en-têtes HTTP

Pour définir des en-têtes HTTP, utilisez la propriété `Headers` sur l’objet de connexion. L’exemple suivant montre comment ajouter un en-tête HTTP.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Comment spécifier des certificats clients

Pour ajouter des certificats clients, utilisez la méthode `AddClientCertificate` sur l’objet de connexion.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Comment créer le proxy Hub

Pour définir des méthodes sur le client qu’un hub peut appeler à partir du serveur, et appeler des méthodes sur un concentrateur sur le serveur, créez un proxy pour le concentrateur en appelant `CreateHubProxy` sur l’objet de connexion. La chaîne que vous transmettez à `CreateHubProxy` est le nom de votre classe de concentrateur, ou le nom spécifié par l’attribut `HubName` si celui-ci a été utilisé sur le serveur. La correspondance de noms ne respecte pas la casse.

**Classe de concentrateur sur le serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Créer un proxy client pour la classe de concentrateur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Si vous décorez votre classe de concentrateur avec un attribut `HubName`, utilisez ce nom.

**Classe de concentrateur sur le serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Créer un proxy client pour la classe de concentrateur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

L’objet proxy est thread-safe. En fait, si vous appelez `HubConnection.CreateHubProxy` plusieurs fois avec le même `hubName`, vous recevez le même objet de `IHubProxy` mis en cache.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Comment définir des méthodes sur le client que le serveur peut appeler

Pour définir une méthode que le serveur peut appeler, utilisez la méthode `On` du proxy pour inscrire un gestionnaire d’événements.

La correspondance de nom de méthode ne respecte pas la casse. Par exemple, `Clients.All.UpdateStockPrice` sur le serveur exécute `updateStockPrice`, `updatestockprice`ou `UpdateStockPrice` sur le client.

Différentes plateformes clientes ont des exigences différentes quant à la façon dont vous écrivez le code de la méthode pour mettre à jour l’interface utilisateur. Les exemples indiqués concernent les clients WinRT (Windows Store .NET). Les exemples d’applications WPF, Silverlight et de console sont fournis dans [une section distincte plus loin dans cette rubrique](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Méthodes sans paramètres

Si la méthode que vous gérez n’a pas de paramètres, utilisez la surcharge non générique de la méthode `On` :

**Code serveur appelant la méthode cliente sans paramètres**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Code client WinRT pour la méthode appelée à partir du serveur sans paramètres ([voir les exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Méthodes avec des paramètres, spécification des types de paramètres

Si la méthode que vous gérez possède des paramètres, spécifiez les types des paramètres comme types génériques de la méthode `On`. Il existe des surcharges génériques de la méthode `On` pour vous permettre de spécifier jusqu’à 8 paramètres (4 sur Windows Phone 7). Dans l’exemple suivant, un paramètre est envoyé à la méthode `UpdateStockPrice`.

**Code serveur appelant la méthode cliente avec un paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Classe stock utilisée pour le paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**Code client WinRT pour une méthode appelée à partir du serveur avec un paramètre ([Voir exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Méthodes avec des paramètres, spécification d’objets dynamiques pour les paramètres

Comme alternative à la spécification de paramètres comme types génériques de la méthode `On`, vous pouvez spécifier des paramètres en tant qu’objets dynamiques :

**Code serveur appelant la méthode cliente avec un paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Classe stock utilisée pour le paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**Code client WinRT pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre ([consultez Exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Comment supprimer un gestionnaire

Pour supprimer un gestionnaire, appelez sa méthode `Dispose`.

**Code client pour une méthode appelée à partir du serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Code client pour supprimer le gestionnaire**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Comment appeler des méthodes de serveur à partir du client

Pour appeler une méthode sur le serveur, utilisez la méthode `Invoke` sur le proxy Hub.

Si la méthode serveur n’a pas de valeur de retour, utilisez la surcharge non générique de la méthode `Invoke`.

**Code serveur pour une méthode qui n’a pas de valeur de retour**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Code client appelant une méthode qui n’a pas de valeur de retour**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Si la méthode serveur a une valeur de retour, spécifiez le type de retour comme type générique de la méthode `Invoke`.

**Code serveur pour une méthode qui a une valeur de retour et prend un paramètre de type complexe**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Classe stock utilisée pour le paramètre et la valeur de retour**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Code client appelant une méthode qui a une valeur de retour et accepte un paramètre de type complexe, dans une méthode Async ASP.NET 4,5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode synchrone**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

La méthode `Invoke` s’exécute de façon asynchrone et retourne un objet `Task`. Si vous ne spécifiez pas `await` ou `.Wait()`, la ligne de code suivante s’exécutera avant la fin de l’exécution de la méthode que vous appelez.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Comment gérer les événements de durée de vie de connexion

Signalr fournit les événements de durée de vie de connexion suivants que vous pouvez gérer :

- `Received`: déclenché quand des données sont reçues sur la connexion. Fournit les données reçues.
- `ConnectionSlow`: déclenché lorsque le client détecte une connexion lente ou souvent en cours de suppression.
- `Reconnecting`: déclenché lorsque le transport sous-jacent commence à se reconnecter.
- `Reconnected`: déclenché lorsque le transport sous-jacent s’est reconnecté.
- `StateChanged`: déclenché lorsque l’état de la connexion change. Fournit l’ancien État et le nouvel État. Pour plus d’informations sur les valeurs d’état de connexion [, consultez énumération ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: déclenché lorsque la connexion a été déconnectée.

Par exemple, si vous souhaitez afficher des messages d’avertissement pour les erreurs qui ne sont pas irrécupérables, mais qui entraînent des problèmes de connexion intermittents, tels que la lenteur ou la suppression fréquente de la connexion, gérez l’événement `ConnectionSlow`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie de connexion dans signalr](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Gestion des erreurs

Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet exception renvoyé par Signalr après une erreur contient un minimum d’informations sur l’erreur. Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`» l’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Pour gérer les erreurs déclenchées par Signalr, vous pouvez ajouter un gestionnaire pour l’événement `Error` sur l’objet de connexion.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Pour gérer les erreurs des appels de méthode, encapsulez le code dans un bloc try-catch.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Comment activer la journalisation côté client

Pour activer la journalisation côté client, définissez les propriétés `TraceLevel` et `TraceWriter` sur l’objet de connexion.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Exemples de code d’application de console WPF, Silverlight et console pour les méthodes clientes que le serveur peut appeler

Les exemples de code présentés précédemment pour la définition des méthodes clientes que le serveur peut appeler s’appliquent aux clients WinRT. Les exemples suivants montrent le code équivalent pour les clients de l’application console, Silverlight et WPF.

### <a name="methods-without-parameters"></a>Méthodes sans paramètres

**Code client WPF pour la méthode appelée à partir du serveur sans paramètres**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Code client Silverlight pour la méthode appelée à partir du serveur sans paramètres**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Code client d’application console pour la méthode appelée à partir du serveur sans paramètres**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Méthodes avec des paramètres, spécification des types de paramètres

**Code client WPF pour une méthode appelée à partir du serveur avec un paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Code client Silverlight pour une méthode appelée à partir du serveur avec un paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Code client de l’application console pour une méthode appelée à partir du serveur avec un paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Méthodes avec des paramètres, spécification d’objets dynamiques pour les paramètres

**Code client WPF pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Code client Silverlight pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Code client d’application console pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
