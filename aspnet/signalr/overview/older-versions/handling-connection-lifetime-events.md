---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Comprendre et gérer les événements de durée de vie de connexion dans Signalr 1. x | Microsoft Docs
author: bradygaster
description: Cet article explique comment utiliser les événements exposés par l’API hubs.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fb671e730a1d41c07b350bf1d64ac1d0b1be55c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536903"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Comprendre et gérer les événements de durée de vie de connexion dans Signalr 1. x

de [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article fournit une vue d’ensemble de la connexion Signalr, des événements de reconnexion et de déconnexion que vous pouvez gérer, ainsi que des paramètres de délai d’attente et de conservation que vous pouvez configurer.
> 
> Cet article suppose que vous avez déjà une connaissance des événements Signalr et de la durée de vie de la connexion. Pour une présentation de Signalr, consultez [signalr-vue d’ensemble-prise en main](index.md). Pour obtenir la liste des événements de durée de vie des connexions, consultez les ressources suivantes :
> 
> - [Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](index.md)
> - [Comment gérer les événements de durée de vie de connexion dans les clients JavaScript](index.md)
> - [Comment gérer les événements de durée de vie de connexion dans les clients .NET](index.md)

## <a name="overview"></a>Présentation

Cet article contient les sections suivantes :

- [Scénarios et terminologie de la durée de vie de la connexion](#terminology)

    - [Connexions signalr, connexions de transport et connexions physiques](#signalrvstransport)
    - [Scénarios de déconnexion de transport](#transportdisconnect)
    - [Scénarios de déconnexion du client](#clientdisconnect)
    - [Scénarios de déconnexion du serveur](#serverdisconnect)
- [Paramètres de délai d’expiration et de conservation](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [Mission](#keepalive)
    - [Comment modifier le délai d’attente et les paramètres KeepAlive](#changetimeout)
- [Comment avertir l’utilisateur des déconnexions](#notifydisconnect)
- [Comment se reconnecter continuellement](#continuousreconnect)
- [Comment déconnecter un client dans le code serveur](#disconnectclientfromserver)

Les liens vers les rubriques de référence sur les API concernent la version .NET 4,5 de l’API. Si vous utilisez .NET 4, consultez [la version .net 4 des rubriques de l’API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Scénarios et terminologie de la durée de vie de la connexion

Le gestionnaire d’événements `OnReconnected` dans un concentrateur Signalr peut s’exécuter directement après `OnConnected` mais pas après `OnDisconnected` pour un client donné. La raison pour laquelle vous pouvez avoir une reconnexion sans déconnexion est qu’il existe plusieurs façons d’utiliser le mot « Connection » dans Signalr.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Connexions signalr, connexions de transport et connexions physiques

Cet article fait la distinction entre les *connexions signalr*, les *connexions de transport*et les *connexions physiques*:

- La **connexion signalr** fait référence à une relation logique entre un client et une URL de serveur, gérée par l’API signalr et identifiée de façon unique par un ID de connexion. Les données relatives à cette relation sont conservées par Signalr et utilisées pour établir une connexion de transport. La relation se termine et Signalr supprime les données lorsque le client appelle la méthode `Stop` ou lorsqu’une limite de délai d’attente est atteinte alors que Signalr tente de rétablir une connexion de transport perdue.
- La **connexion de transport** fait référence à une relation logique entre un client et un serveur, gérée par l’une des quatre API de transport : WebSockets, les événements envoyés par le serveur, la trame permanente ou l’interrogation longue. Signalr utilise l’API de transport pour créer une connexion de transport, et l’API de transport dépend de l’existence d’une connexion réseau physique pour créer la connexion de transport. La connexion de transport se termine lorsque Signalr le termine ou lorsque l’API de transport détecte que la connexion physique est interrompue.
- La **connexion physique** fait référence aux liens réseau physiques (câbles, signaux sans fil, routeurs, etc.) qui facilitent la communication entre un ordinateur client et un ordinateur serveur. La connexion physique doit être présente afin d’établir une connexion de transport, et une connexion de transport doit être établie afin d’établir une connexion Signalr. Toutefois, l’interruption de la connexion physique ne met pas toujours immédiatement fin à la connexion de transport ou à la connexion Signalr, comme expliqué plus loin dans cette rubrique.

Dans le diagramme suivant, la connexion Signalr est représentée par l’API hubs et la couche Signalr de l’API PersistentConnection, la connexion de transport est représentée par la couche de transport et la connexion physique est représentée par les lignes entre le serveur et les clients.

![Diagramme d’architecture signalr](handling-connection-lifetime-events/_static/image1.png)

Lorsque vous appelez la méthode `Start` dans un client Signalr, vous fournissez au code client Signalr toutes les informations dont il a besoin pour établir une connexion physique à un serveur. Le code client signalr utilise ces informations pour effectuer une requête HTTP et établir une connexion physique qui utilise l’une des quatre méthodes de transport. En cas d’échec de la connexion de transport ou de défaillance du serveur, la connexion Signalr ne disparaît pas immédiatement, car le client dispose toujours des informations nécessaires pour rétablir automatiquement une nouvelle connexion de transport à la même URL Signalr. Dans ce scénario, aucune intervention de l’application utilisateur n’est impliquée, et lorsque le code client Signalr établit une nouvelle connexion de transport, il ne démarre pas une nouvelle connexion Signalr. La continuité de la connexion Signalr est reflétée dans le fait que l’ID de connexion, qui est créé lorsque vous appelez la méthode `Start`, ne change pas.

Le gestionnaire d’événements `OnReconnected` sur le Hub s’exécute lorsqu’une connexion de transport est automatiquement rétablie après avoir été perdue. Le gestionnaire d’événements `OnDisconnected` s’exécute à la fin d’une connexion Signalr. Une connexion Signalr peut se terminer de l’une des manières suivantes :

- Si le client appelle la méthode `Stop`, un message d’arrêt est envoyé au serveur et le client et le serveur terminent la connexion Signalr immédiatement.
- Après la perte de connectivité entre le client et le serveur, le client tente de se reconnecter et le serveur attend que le client se reconnecte. Si les tentatives de reconnexion échouent et que le délai d’attente de déconnexion se termine, le client et le serveur terminent la connexion Signalr. Le client cesse de tenter de se reconnecter, et le serveur supprime sa représentation de la connexion Signalr.
- Si le client cesse de s’exécuter sans avoir la possibilité d’appeler la méthode `Stop`, le serveur attend que le client se reconnecte, puis met fin à la connexion Signalr après le délai de déconnexion.
- Si le serveur arrête de fonctionner, le client tente de se reconnecter (recrée la connexion de transport), puis met fin à la connexion Signalr après le délai d’attente de déconnexion.

En l’absence de problèmes de connexion, si l’application utilisateur met fin à la connexion Signalr en appelant la méthode `Stop`, la connexion Signalr et la connexion de transport commencent et se terminent à la même heure. Les sections suivantes décrivent plus en détail les autres scénarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scénarios de déconnexion de transport

Les connexions physiques peuvent être lentes ou il peut y avoir des interruptions de connectivité. En fonction de facteurs tels que la longueur de l’interruption, la connexion de transport peut être abandonnée. Signalr essaie ensuite de rétablir la connexion de transport. Parfois, l’API de connexion de transport détecte l’interruption et abandonne la connexion de transport, et signale immédiatement que la connexion est perdue. Dans d’autres scénarios, ni l’API de connexion de transport, ni signaler ne prend immédiatement en compte la perte de connectivité. Pour tous les transports à l’exception de l’interrogation longue, le client Signalr utilise une fonction appelée *KeepAlive* pour vérifier la perte de connectivité que l’API de transport ne peut pas détecter. Pour plus d’informations sur les connexions à interrogation longue, consultez [expiration et paramètres KeepAlive](#timeoutkeepalive) plus loin dans cette rubrique.

Lorsqu’une connexion est inactive, le serveur envoie régulièrement un paquet KeepAlive au client. À compter de la date d’écriture de cet article, la fréquence par défaut est toutes les 10 secondes. En écoutant ces paquets, les clients peuvent déterminer s’il existe un problème de connexion. Si un paquet KeepAlive n’est pas reçu quand cela est attendu, après un bref moment, le client suppose qu’il y a des problèmes de connexion tels que la lenteur ou les interruptions. Si le KeepAlive n’est toujours pas reçu après une période plus longue, le client suppose que la connexion a été supprimée et commence à tenter de se reconnecter.

Le diagramme suivant illustre les événements client et serveur qui sont déclenchés dans un scénario classique lorsqu’il y a des problèmes avec la connexion physique qui ne sont pas immédiatement reconnus par l’API de transport. Le diagramme s’applique aux circonstances suivantes :

- Le transport est de la trame WebSocket, de l’image permanente ou des événements envoyés par le serveur.
- Il y a différentes périodes d’interruption dans la connexion réseau physique.
- L’API de transport ne prend pas en charge les interruptions, donc Signalr s’appuie sur la fonctionnalité KeepAlive pour les détecter.

![Déconnexions de transport](handling-connection-lifetime-events/_static/image2.png)

Si le client passe en mode de reconnexion mais ne peut pas établir de connexion de transport dans le délai imparti, le serveur met fin à la connexion Signalr. Lorsque cela se produit, le serveur exécute la méthode de `OnDisconnected` du Hub et met en file d’attente un message de déconnexion à envoyer au client au cas où le client se connecte pour se connecter ultérieurement. Si le client se reconnecte, il reçoit la commande Disconnect et appelle la méthode `Stop`. Dans ce scénario, `OnReconnected` n’est pas exécutée lorsque le client se reconnecte, et `OnDisconnected` n’est pas exécutée lorsque le client appelle `Stop`. Le diagramme suivant illustre ce scénario.

![Interruptions de transport-délai d’expiration du serveur](handling-connection-lifetime-events/_static/image3.png)

Les événements de durée de vie de connexion Signalr qui peuvent être déclenchés sur le client sont les suivants :

- événement client `ConnectionSlow`.

    Déclenché lorsqu’une proportion prédéfinie du délai d’attente de conservation de la durée de conservation a été dépassée depuis la réception du dernier message ou du ping KeepAlive. La période d’avertissement du délai d’attente de conservation par défaut est 2/3 du délai d’expiration de conservation. Le délai d’expiration de conservation est de 20 secondes. l’avertissement se produit donc à environ 13 secondes.

    Par défaut, le serveur envoie des pings KeepAlive toutes les 10 secondes, et le client vérifie les pings KeepAlive environ toutes les 2 secondes (un tiers de la différence entre la valeur du délai d’expiration KeepAlive et la valeur de l’avertissement de délai d’attente KeepAlive).

    Si l’API de transport est informée d’une déconnexion, Signalr peut être informé de la déconnexion avant que la période d’avertissement du délai d’attente de conservation de connexion ne soit dépassée. Dans ce cas, l’événement `ConnectionSlow` n’est pas déclenché, et Signalr accède directement à l’événement `Reconnecting`.
- événement client `Reconnecting`.

    Déclenché lorsque (a) l’API de transport détecte que la connexion est perdue, ou (b) le délai d’attente de la conservation des connexions persistantes est dépassé depuis la réception du dernier message ou du ping KeepAlive. Le code client Signalr commence la tentative de reconnexion. Vous pouvez gérer cet événement si vous souhaitez que votre application prenne des mesures lorsqu’une connexion de transport est perdue. Le délai d’expiration de la conservation par défaut est actuellement de 20 secondes.

    Si votre code client tente d’appeler une méthode de concentrateur alors que Signalr est en mode de reconnexion, Signalr essaiera d’envoyer la commande. La plupart du temps, ces tentatives échouent, mais dans certains cas, elles peuvent réussir. Pour les transports d’événements envoyés par le serveur, indéfiniment et de longue durée, Signalr utilise deux canaux de communication : l’un utilisé par le client pour envoyer des messages et l’autre pour recevoir des messages. Le canal utilisé pour la réception est le canal ouvert de manière permanente et c’est celui qui est fermé lorsque la connexion physique est interrompue. Le canal utilisé pour l’envoi reste disponible. par conséquent, si la connectivité physique est restaurée, un appel de méthode du client au serveur peut être réussi avant que le canal de réception ne soit rétabli. La valeur de retour n’est pas reçue tant que signaler n’a pas rouvert le canal utilisé pour la réception.
- événement client `Reconnected`.

    Déclenché lorsque la connexion de transport est rétablie. Le gestionnaire d’événements `OnReconnected` dans le Hub s’exécute.
- `Closed` événement client (`disconnected` dans JavaScript).

    Déclenché lorsque le délai d’attente de déconnexion expire alors que le code du client Signalr tente de se reconnecter après avoir perdu la connexion de transport. Le délai d’attente de déconnexion par défaut est de 30 secondes. (Cet événement est également déclenché lorsque la connexion se termine, car la méthode `Stop` est appelée.)

Les interruptions de connexion de transport qui ne sont pas détectées par l’API de transport et qui ne retardent pas la réception des pings KeepAlive à partir du serveur pendant plus longtemps que la période d’avertissement du délai d’attente KeepAlive peuvent ne pas entraîner le déclenchement d’événements de durée de vie de connexion.

Certains environnements réseau ferment délibérément des connexions inactives, et une autre fonction des paquets KeepAlive est de vous aider à éviter cela en laissant ces réseaux savoir qu’une connexion Signalr est en cours d’utilisation. Dans les cas extrêmes, la fréquence par défaut des pings KeepAlive peut ne pas suffire pour empêcher les connexions fermées. Dans ce cas, vous pouvez configurer des pings KeepAlive à envoyer plus souvent. Pour plus d’informations, consultez la section [paramètres de délai d’attente et conservation](#timeoutkeepalive) plus loin dans cette rubrique.

> [!NOTE] 
> 
> [!IMPORTANT]
> La séquence d’événements décrite ici n’est pas garantie. Signalr effectue chaque tentative d’augmentation des événements de durée de vie de la connexion de manière prévisible en fonction de ce schéma, mais il existe de nombreuses variantes d’événements réseau et de nombreuses façons dont les infrastructures de communication sous-jacentes telles que les API de transport les gèrent. Par exemple, l’événement `Reconnected` peut ne pas être déclenché lorsque le client se reconnecte, ou le gestionnaire de `OnConnected` sur le serveur peut s’exécuter lorsque la tentative d’établissement d’une connexion échoue. Cette rubrique décrit uniquement les effets qui seraient normalement produits par certaines circonstances typiques.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scénarios de déconnexion du client

Dans un navigateur client, le code client Signalr qui gère une connexion Signalr s’exécute dans le contexte JavaScript d’une page Web. C’est pourquoi la connexion Signalr doit se terminer lorsque vous naviguez d’une page à une autre, et c’est pourquoi vous avez plusieurs connexions avec plusieurs ID de connexion si vous vous connectez à partir de plusieurs fenêtres ou onglets du navigateur. Quand l’utilisateur ferme une fenêtre ou un onglet de navigateur, ou accède à une nouvelle page ou actualise la page, la connexion Signalr se termine immédiatement, car le code client Signalr gère cet événement de navigateur pour vous et appelle la méthode `Stop`. Dans ces scénarios, ou sur n’importe quelle plateforme cliente lorsque votre application appelle la méthode `Stop`, le gestionnaire d’événements `OnDisconnected` s’exécute immédiatement sur le serveur et le client déclenche l’événement `Closed` (l’événement est nommé `disconnected` dans JavaScript).

Si une application cliente ou l’ordinateur sur lequel elle s’exécute s’arrête ou passe en mode veille (par exemple, lorsque l’utilisateur ferme l’ordinateur portable), le serveur n’est pas informé de ce qui s’est passé. En ce qui concerne le serveur, la perte du client peut être due à une interruption de la connectivité et le client peut tenter de se reconnecter. Par conséquent, dans ces scénarios, le serveur attend de permettre au client de se reconnecter, et `OnDisconnected` ne s’exécute pas tant que le délai d’attente de déconnexion n’a pas expiré (environ 30 secondes par défaut). Le diagramme suivant illustre ce scénario.

![Défaillance de l’ordinateur client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scénarios de déconnexion du serveur

Lorsqu’un serveur est mis hors connexion, il redémarre, échoue, les recyclages de domaine d’application, etc., le résultat peut être similaire à une connexion perdue, ou l’API de transport et Signalr peut savoir immédiatement que le serveur a disparu, et Signalr peut commencer à tenter de se reconnecter sans déclencher l’événement `ConnectionSlow`. Si le client passe en mode de reconnexion, et si le serveur récupère ou redémarre ou si un nouveau serveur est mis en ligne avant l’expiration du délai d’attente de déconnexion, le client se reconnecte au serveur restauré ou nouveau. Dans ce cas, la connexion Signalr continue sur le client et l’événement `Reconnected` est déclenché. Sur le premier serveur, `OnDisconnected` n’est jamais exécutée, et sur le nouveau serveur, `OnReconnected` est exécutée même si `OnConnected` n’a jamais été exécutée pour ce client sur ce serveur. (L’effet est le même si le client se reconnecte au même serveur après un redémarrage ou un recyclage de domaine d’application, car lorsque le serveur redémarre, il n’a pas de mémoire pour l’activité de connexion précédente.) Le diagramme suivant part du principe que l’API de transport est informée de la connexion perdue immédiatement, de sorte que l’événement `ConnectionSlow` n’est pas déclenché.

![Défaillance du serveur et reconnexion](handling-connection-lifetime-events/_static/image5.png)

Si un serveur n’est pas disponible dans le délai de déconnexion, la connexion Signalr se termine. Dans ce scénario, l’événement `Closed` (`disconnected` dans les clients JavaScript) est déclenché sur le client, mais `OnDisconnected` n’est jamais appelé sur le serveur. Le diagramme suivant suppose que l’API de transport ne prend pas en charge la connexion perdue. elle est donc détectée par la fonctionnalité KeepAlive Signalr et l’événement `ConnectionSlow` est déclenché.

![Échec et délai d’expiration du serveur](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Paramètres de délai d’expiration et de conservation

Les valeurs par défaut `ConnectionTimeout`, `DisconnectTimeout`et `KeepAlive` sont appropriées pour la plupart des scénarios, mais elles peuvent être modifiées si votre environnement a des besoins spéciaux. Par exemple, si votre environnement réseau ferme des connexions inactives pendant 5 secondes, vous devrez peut-être réduire la valeur KeepAlive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Ce paramètre représente la durée pendant laquelle une connexion de transport doit rester ouverte et en attente d’une réponse avant sa fermeture et l’ouverture d’une nouvelle connexion. La valeur par défaut est 110 secondes.

Ce paramètre s’applique uniquement lorsque la fonctionnalité KeepAlive est désactivée, ce qui s’applique normalement uniquement au transport d’interrogation longue. Le diagramme suivant illustre l’effet de ce paramètre sur une connexion de transport d’interrogation longue.

![Connexion de transport d’interrogation longue](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Ce paramètre représente la durée d’attente après la perte d’une connexion de transport avant le déclenchement de l’événement `Disconnected`. La valeur par défaut est de 30 secondes. Lorsque vous définissez `DisconnectTimeout`, `KeepAlive` est automatiquement défini sur 1/3 de la valeur de `DisconnectTimeout`.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Ce paramètre représente la durée d’attente avant l’envoi d’un paquet KeepAlive sur une connexion inactive. La valeur par défaut est 10 secondes. Cette valeur ne doit pas être supérieure à 1/3 de la valeur `DisconnectTimeout`.

Si vous souhaitez définir à la fois `DisconnectTimeout` et `KeepAlive`, définissez `KeepAlive` après `DisconnectTimeout`. Dans le cas contraire, votre `KeepAlive` paramètre sera remplacé lorsque `DisconnectTimeout` définira automatiquement `KeepAlive` sur 1/3 de la valeur du délai d’attente.

Si vous souhaitez désactiver la fonctionnalité KeepAlive, affectez à `KeepAlive` la valeur null. La fonctionnalité KeepAlive est automatiquement désactivée pour le transport d’interrogation longue.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Comment modifier le délai d’attente et les paramètres KeepAlive

Pour modifier les valeurs par défaut de ces paramètres, définissez-les dans `Application_Start` dans votre fichier *global. asax* , comme indiqué dans l’exemple suivant. Les valeurs affichées dans l’exemple de code sont les mêmes que les valeurs par défaut.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Comment avertir l’utilisateur des déconnexions

Dans certaines applications, vous souhaiterez peut-être afficher un message à l’utilisateur lorsqu’il y a des problèmes de connectivité. Vous disposez de plusieurs options pour savoir quand et comment procéder. Les exemples de code suivants concernent un client JavaScript qui utilise le proxy généré.

- Gérez l’événement `connectionSlow` pour afficher un message dès que Signalr est conscient des problèmes de connexion, avant qu’il ne passe en mode de reconnexion.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gérez l’événement `reconnecting` pour afficher un message lorsque Signalr est conscient d’une déconnexion et passe en mode de reconnexion.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gérez l’événement `disconnected` pour afficher un message quand une tentative de reconnexion a expiré. Dans ce scénario, le seul moyen de rétablir une connexion avec le serveur consiste à redémarrer la connexion Signalr en appelant la méthode `Start`, qui créera un nouvel ID de connexion. L’exemple de code suivant utilise un indicateur pour s’assurer que vous émettez la notification uniquement après un délai de reconnexion, et non après une fin normale à la connexion Signalr provoquée par l’appel de la méthode `Stop`.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Comment se reconnecter continuellement

Dans certaines applications, vous souhaiterez peut-être rétablir automatiquement une connexion une fois qu’elle a été perdue et que la tentative de reconnexion a expiré. Pour ce faire, vous pouvez appeler la méthode `Start` à partir de votre gestionnaire d’événements `Closed` (`disconnected` gestionnaire d’événements sur les clients JavaScript). Vous souhaiterez peut-être attendre un certain temps avant d’appeler `Start` afin d’éviter que cela soit trop fréquent lorsque le serveur ou la connexion physique ne sont pas disponibles. L’exemple de code suivant est destiné à un client JavaScript qui utilise le proxy généré.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un problème potentiel à connaître dans les clients mobiles est que les tentatives de reconnexion continue lorsque le serveur ou la connexion physique n’est pas disponible peuvent entraîner un vidage de batterie inutile.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Comment déconnecter un client dans le code serveur

Signalr version 1.1.1 ne dispose pas d’une API serveur intégrée pour déconnecter les clients. Il existe des [plans pour l’ajout de cette fonctionnalité à l’avenir](https://github.com/SignalR/SignalR/issues/2101). Dans la version actuelle de Signalr, la façon la plus simple de déconnecter un client du serveur consiste à implémenter une méthode de déconnexion sur le client et à appeler cette méthode à partir du serveur. L’exemple de code suivant montre une méthode de déconnexion pour un client JavaScript qui utilise le proxy généré.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sécurité : ni cette méthode de déconnexion des clients ni l’API intégrée proposée ne concernent le scénario des clients piratés qui exécutent du code malveillant, puisque les clients peuvent se reconnecter ou que le code piraté peut supprimer la méthode `stopClient` ou changer ce qu’elle fait. L’emplacement approprié pour implémenter la protection contre les dénis de service avec état n’est pas dans l’infrastructure ou la couche serveur, mais plutôt dans l’infrastructure frontale.
