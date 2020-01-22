---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Présentation de Signalr | Microsoft Docs
author: bradygaster
description: Cet article décrit ce que Signalr et les solutions qu’il a été conçu pour créer.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519399"
---
# <a name="introduction-to-signalr"></a>Introduction à SignalR

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article décrit ce que Signalr et les solutions qu’il a été conçu pour créer. 
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>Qu’est-ce que Signalr ?

ASP.NET Signalr est une bibliothèque pour les développeurs ASP.NET qui simplifie le processus d’ajout de fonctionnalités Web en temps réel aux applications. La fonctionnalité Web en temps réel est la possibilité d’envoyer du contenu de code serveur à des clients connectés instantanément dès qu’il est disponible, plutôt que de laisser le serveur attendre qu’un client demande de nouvelles données.

Signalr peut être utilisé pour ajouter n’importe quel type de fonctionnalité Web « en temps réel » à votre application ASP.NET. Alors que la conversation est souvent utilisée comme exemple, vous pouvez en faire beaucoup plus. Chaque fois qu’un utilisateur actualise une page Web pour voir les nouvelles données, ou que la page implémente une [longue interrogation](http://en.wikipedia.org/wiki/Push_technology#Long_polling) pour récupérer de nouvelles données, il s’agit d’un candidat à l’utilisation de signalr. Les exemples incluent des tableaux de bord et des applications de surveillance, des applications collaboratives (telles que la modification simultanée de documents), des mises à jour de progression des travaux et des formulaires en temps réel.

Signalr offre également de nouveaux types d’applications Web qui requièrent des mises à jour à fréquence élevée du serveur, par exemple, des jeux en temps réel.

Signalr fournit une API simple pour la création d’appels de procédure distante (RPC) de serveur à client qui appellent des fonctions JavaScript dans les navigateurs clients (et d’autres plateformes clientes) à partir du code .NET côté serveur. Signalr comprend également l’API pour la gestion des connexions (par exemple, les événements de connexion et de déconnexion) et le regroupement de connexions.

![Appel de méthodes avec Signalr](introduction-to-signalr/_static/image1.png)

SignalR traite automatiquement la gestion des connexions et vous permet de diffuser des messages à tous les clients connectés simultanément, comme une salle de conversation. Vous pouvez également envoyer des messages à des clients spécifiques. La connexion entre le client et le serveur est permanente, contrairement à une connexion HTTP classique qui est rétablie pour chaque communication.

Signalr prend en charge les fonctionnalités « push du serveur », dans lesquelles le code serveur peut appeler le code client dans le navigateur à l’aide d’appels de procédure distante (RPC), plutôt que du modèle de demande-réponse actuellement sur le Web.

Les applications signalr peuvent être mises à l’échelle jusqu’à des milliers de clients à l’aide de fournisseurs de montée en puissance parallèle intégrés et tiers.

Les fournisseurs intégrés incluent les éléments suivants :
* [Service Bus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

Les fournisseurs tiers incluent :
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

Signalr est open source, accessible via [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>Signalr et WebSocket

Signalr utilise le nouveau transport WebSocket lorsque celui-ci est disponible et revient aux transports plus anciens, le cas échéant. Bien que vous puissiez certainement écrire votre application à l’aide de WebSocket directement, l’utilisation de Signalr signifie qu’un grand nombre des fonctionnalités supplémentaires que vous devez implémenter est déjà fait pour vous. Plus important encore, cela signifie que vous pouvez coder votre application pour tirer parti de WebSocket sans avoir à vous soucier de la création d’un chemin de code distinct pour les clients plus anciens. Signalr vous évite également d’avoir à vous soucier des mises à jour de WebSocket, car Signalr est mis à jour pour prendre en charge les modifications apportées au transport sous-jacent, ce qui fournit à votre application une interface cohérente entre les différentes versions de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transports et secours

Signalr est une abstraction sur certains des transports requis pour effectuer un travail en temps réel entre le client et le serveur. Une connexion Signalr démarre en tant que HTTP et est ensuite promue en connexion WebSocket si elle est disponible. WebSocket est le transport idéal pour Signalr, car il optimise l’utilisation de la mémoire du serveur, possède la latence la plus faible et possède les fonctionnalités les plus sous-jacentes (telles que la communication en duplex intégral entre le client et le serveur), mais il possède également le plus strict Configuration requise : WebSocket requiert que le serveur utilise Windows Server 2012 ou Windows 8, et .NET Framework 4,5. Si ces exigences ne sont pas satisfaites, Signalr tente d’utiliser d’autres transports pour établir ses connexions.

### <a name="html-5-transports"></a>Transports HTML 5

Ces transports dépendent de la prise en charge de [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si le navigateur client ne prend pas en charge la norme HTML 5, les transports plus anciens seront utilisés.

- **WebSocket** (si le serveur et le navigateur indiquent qu’ils peuvent prendre en charge WebSocket). WebSocket est le seul transport qui établit une véritable connexion bidirectionnelle permanente entre le client et le serveur. Toutefois, WebSocket présente également les exigences les plus strictes. Il est entièrement pris en charge uniquement dans les dernières versions de Microsoft Internet Explorer, Google Chrome et Mozilla Firefox, et n’a qu’une implémentation partielle dans d’autres navigateurs tels que Opera et Safari.
- Le **serveur a envoyé des événements**, également appelés EventSource (si le navigateur prend en charge les événements envoyés par le serveur, qui sont fondamentalement tous les navigateurs, à l’exception d’Internet Explorer).

### <a name="comet-transports"></a>Transports Comet

Les transports suivants sont basés sur le modèle d’application Web [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) , dans lequel un navigateur ou un autre client gère une requête http longue, que le serveur peut utiliser pour transmettre des données au client sans que le client le demande spécifiquement.

- **Frame indéfiniment** (pour Internet Explorer uniquement). L’image permanente crée un IFrame masqué qui effectue une demande à un point de terminaison sur le serveur qui ne se termine pas. Le serveur envoie ensuite continuellement le script au client qui est immédiatement exécuté, en fournissant une connexion en temps réel unidirectionnelle entre le serveur et le client. La connexion du client au serveur utilise une connexion distincte entre le serveur et la connexion au client, et comme une requête HTTP standard, une nouvelle connexion est créée pour chaque élément de données à envoyer.
- **Interrogation longue Ajax**. L’interrogation longue ne crée pas de connexion permanente, mais interroge le serveur à l’aide d’une demande qui reste ouverte jusqu’à ce que le serveur réponde. à partir de là, la connexion se ferme et une nouvelle connexion est demandée immédiatement. Cela peut entraîner une certaine latence pendant la réinitialisation de la connexion.

Pour plus d’informations sur les transports pris en charge dans les configurations, consultez [plateformes prises en charge](supported-platforms.md).

### <a name="transport-selection-process"></a>Processus de sélection du transport

La liste suivante présente les étapes utilisées par Signalr pour décider du transport à utiliser.

1. Si le navigateur est Internet Explorer 8 ou version antérieure, l’interrogation longue est utilisée.
2. Si JSONP est configuré (autrement dit, si le paramètre `jsonp` a la valeur `true` lorsque la connexion est démarrée), une interrogation longue est utilisée.
3. Si une connexion inter-domaines est établie (autrement dit, si le point de terminaison Signalr n’est pas dans le même domaine que la page d’hébergement), WebSocket est utilisé si les critères suivants sont satisfaits :

   - Le client prend en charge CORS (partage des ressources Cross-Origin). Pour plus d’informations sur les clients qui prennent en charge CORS, consultez [cors à caniuse.com](http://www.caniuse.com/CORS).
   - Le client prend en charge WebSocket
   - Le serveur prend en charge WebSocket

     Si l’un de ces critères n’est pas respecté, l’interrogation longue sera utilisée. Pour plus d’informations sur les connexions inter-domaines, consultez [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Si JSONP n’est pas configuré et que la connexion n’est pas inter-domaines, WebSocket sera utilisé si le client et le serveur le prennent en charge.
5. Si le client ou le serveur ne prend pas en charge WebSocket, les événements envoyés par le serveur sont utilisés s’ils sont disponibles.
6. Si le serveur a envoyé des événements n’est pas disponible, une tentative d’image permanente est effectuée.
7. Si Forever Frame échoue, l’interrogation longue est utilisée.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Surveillance des transports

Vous pouvez déterminer le transport utilisé par votre application en activant la journalisation sur votre Hub et en ouvrant la fenêtre de console dans votre navigateur.

Pour activer la journalisation des événements de votre Hub dans un navigateur, ajoutez la commande suivante à votre application cliente :

`$.connection.hub.logging = true;`

- Dans Internet Explorer, ouvrez les outils de développement en appuyant sur F12, puis cliquez sur l’onglet Console.

    ![Console dans Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- Dans Chrome, ouvrez la console en appuyant sur Ctrl + Maj + J.

    ![Console dans Google Chrome](introduction-to-signalr/_static/image3.png)

Une fois la console ouverte et la journalisation activées, vous pouvez voir quel transport est utilisé par Signalr.

![Console dans Internet Explorer présentant le transport WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Spécification d’un transport

La négociation d’un transport prend un certain temps et des ressources client/serveur. Si les fonctionnalités du client sont connues, un transport peut être spécifié lors du démarrage de la connexion cliente. L’extrait de code suivant illustre le démarrage d’une connexion à l’aide du transport d’interrogation longue Ajax, comme dans le cas où le client ne prenait pas en charge un autre protocole :

`connection.start({ transport: 'longPolling' });`

Vous pouvez spécifier un ordre de secours si vous souhaitez qu’un client essaie des transports spécifiques dans l’ordre. L’extrait de code suivant montre comment essayer WebSocket et échouer, en accédant directement à une longue interrogation.

`connection.start({ transport: ['webSockets','longPolling'] });`

Les constantes de chaîne pour la spécification de transports sont définies comme suit :

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Connexions et hubs

L’API Signalr contient deux modèles pour la communication entre les clients et les serveurs : connexions persistantes et concentrateurs.

Une connexion représente un point de terminaison simple pour l’envoi de messages de destinataire unique, groupés ou de diffusion. L’API de connexion persistante (représentée dans le code .NET par la classe PersistentConnection) donne au développeur un accès direct au protocole de communication de bas niveau que Signalr expose. L’utilisation du modèle de communication connexions sera familière pour les développeurs qui ont utilisé des API basées sur la connexion, telles que Windows Communication Foundation.

Un Hub est un pipeline plus élevé basé sur l’API de connexion qui permet à votre client et à votre serveur d’appeler directement des méthodes. Signalr gère la distribution à travers les limites de l’ordinateur comme s’il s’agissait de Magic, ce qui permet aux clients d’appeler des méthodes sur le serveur aussi facilement que les méthodes locales, et vice versa. L’utilisation du modèle de communication hubs sera familière aux développeurs qui ont utilisé des API d’appel à distance telles que .NET Remoting. L’utilisation d’un hub vous permet également de passer des paramètres fortement typés aux méthodes, en activant la liaison de modèle.

### <a name="architecture-diagram"></a>Diagramme de l’architecture

Le diagramme suivant montre la relation entre les concentrateurs, les connexions persistantes et les technologies sous-jacentes utilisées pour les transports.

![Diagramme d’architecture signalr montrant les API, les transports et les clients](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Fonctionnement des hubs

Lorsque le code côté serveur appelle une méthode sur le client, un paquet est envoyé dans le transport actif qui contient le nom et les paramètres de la méthode à appeler (lorsqu’un objet est envoyé en tant que paramètre de méthode, il est sérialisé à l’aide de JSON). Le client met ensuite en correspondance le nom de la méthode avec les méthodes définies dans le code côté client. En cas de correspondance, la méthode cliente est exécutée à l’aide des données de paramètre désérialisées.

L’appel de méthode peut être surveillé à l’aide d’outils tels que [Fiddler.](http://fiddler2.com/) L’illustration suivante montre un appel de méthode envoyé à partir d’un serveur Signalr à un client de navigateur Web dans le volet journaux de Fiddler. L’appel de méthode est envoyé à partir d’un hub appelé `MoveShapeHub`, et la méthode appelée est appelée `updateShape`.

![Affichage du journal Fiddler indiquant le trafic Signalr](introduction-to-signalr/_static/image6.png)

Dans cet exemple, le nom du concentrateur est identifié par le paramètre `H` ; le nom de la méthode est identifié par le paramètre `M` et les données envoyées à la méthode sont identifiées par le paramètre `A`. L’application qui a généré ce message est créée dans le didacticiel en [temps réel à haute fréquence](tutorial-high-frequency-realtime-with-signalr.md) .

### <a name="choosing-a-communication-model"></a>Choix d’un modèle de communication

La plupart des applications doivent utiliser l’API hubs. L’API connections peut être utilisée dans les circonstances suivantes :

- Le format du message réel envoyé doit être spécifié.
- Le développeur préfère travailler avec un modèle de messagerie et de distribution plutôt qu’un modèle d’appel distant.
- Une application existante qui utilise un modèle de messagerie est reportée pour utiliser Signalr.
