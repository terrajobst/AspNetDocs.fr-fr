---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Présentation de ScaleOut dans Signalr 1. x | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 04/29/2013
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9bad72d31a0ebc491910ebb128b3b3a7fb537958
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536567"
---
# <a name="introduction-to-scaleout-in-signalr-1x"></a>Introduction au scale-out dans SignalR 1.x

par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En général, il existe deux façons de mettre à l’échelle une application Web : *montée* en puissance et *montée*en charge.

- La montée en puissance implique l’utilisation d’un serveur de plus grande taille (ou d’une machine virtuelle plus grande) avec davantage de RAM, de processeurs, etc.
- La montée en charge signifie l’ajout de serveurs supplémentaires pour gérer la charge.

Le problème de la mise à l’échelle est que vous atteignez rapidement une limite de la taille de la machine. Au-delà, vous devez effectuer une montée en charge. Toutefois, lorsque vous mettez à l’échelle, les clients peuvent être acheminés vers différents serveurs. Un client connecté à un serveur ne reçoit pas les messages envoyés à partir d’un autre serveur.

![](scaleout-in-signalr/_static/image1.png)

Une solution consiste à transférer les messages entre les serveurs, à l’aide d’un composant appelé *backplane*. Quand un fond de panier est activé, chaque instance d’application envoie des messages au fond de panier et le fond de panier les transfère aux autres instances de l’application. (En électronique, un fond de panier est un groupe de connecteurs parallèles. Par analogie, un fond de panier Signalr connecte plusieurs serveurs.)

![](scaleout-in-signalr/_static/image2.png)

Signalr propose actuellement trois plans de perfectionnement :

- **Azure Service Bus**. Service Bus est une infrastructure de messagerie qui permet aux composants d’envoyer des messages d’une manière faiblement couplée.
- **ReDim**. Redims est un magasin clé-valeur en mémoire. Redims prend en charge un modèle de publication/abonnement (« Pub/Sub ») pour l’envoi de messages.
- **SQL Server**. Le fond de panier SQL Server écrit des messages dans des tables SQL. Le fond de panier utilise Service Broker pour une messagerie efficace. Toutefois, il fonctionne également si Service Broker n’est pas activé.

Si vous déployez votre application sur Azure, envisagez d’utiliser la Azure Service Bus fond de panier. Si vous effectuez un déploiement sur votre propre batterie de serveurs, envisagez les SQL Server ou les redirections ReDim.

Les rubriques suivantes contiennent des didacticiels pas à pas pour chaque fond de panier :

- [Montée en puissance parallèle de SignalR avec Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Montée en puissance parallèle de SignalR avec Redis](scaleout-with-redis.md)
- [Montée en puissance parallèle de SignalR avec SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implémentation

Dans Signalr, chaque message est envoyé via un bus de messages. Un bus de messages implémente l’interface [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , qui fournit une abstraction de publication/abonnement. Ces plans fonctionnent en remplaçant le **IMessageBus** par défaut par un bus conçu pour ce backplane. Par exemple, le bus de messages pour les insertions est [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), et il utilise le mécanisme de [Pub/Sub](http://redis.io/topics/pubsub) redims pour envoyer et recevoir des messages.

Chaque instance de serveur se connecte au fond de panier via le bus. Lorsqu’un message est envoyé, il passe au fond de panier et le fond de panier l’envoie à tous les serveurs. Lorsqu’un serveur reçoit un message du fond de panier, il place le message dans son cache local. Le serveur remet ensuite les messages aux clients à partir de son cache local.

Pour chaque connexion client, la progression du client lors de la lecture du flux de messages est suivie à l’aide d’un curseur. (Un curseur représente une position dans le flux de message.) Si un client se déconnecte, puis se reconnecte, il demande au bus les messages arrivés après la valeur de curseur du client. La même chose se produit lorsqu’une connexion utilise une [interrogation longue](../getting-started/introduction-to-signalr.md#transports). Après la fin d’une demande d’interrogation longue, le client ouvre une nouvelle connexion et demande les messages arrivés après le curseur.

Le mécanisme de curseur fonctionne même si un client est routé vers un autre serveur lors de la reconnexion. Le fond de panier est conscient de tous les serveurs et peu importe le serveur auquel se connecte le client.

## <a name="limitations"></a>Limitations

À l’aide d’un fond de panier, le débit maximal de messages est plus faible que lorsque les clients communiquent directement avec un nœud de serveur unique. Cela est dû au fait que le fond de panier transmet chaque message à chaque nœud, de sorte que le fond de panier peut devenir un goulot d’étranglement. Le fait que cette limitation soit un problème dépend de l’application. Par exemple, voici quelques scénarios de Signalr classiques :

- [Diffusion](tutorial-server-broadcast-with-aspnet-signalr.md) sur le serveur (par exemple, cotation boursière) : les plans sont adaptés à ce scénario, car le serveur contrôle la vitesse à laquelle les messages sont envoyés.
- [Client à client](tutorial-getting-started-with-signalr.md) (par exemple, conversation) : dans ce scénario, le fond de panier peut être un goulot d’étranglement si le nombre de messages est mis à l’échelle avec le nombre de clients ; autrement dit, si le taux de messages augmente proportionnellement au fur et à mesure que d’autres clients se joignent.
- [Haute fréquence en temps réel](tutorial-high-frequency-realtime-with-signalr.md) (par exemple, jeux en temps réel) : un backplane n’est pas recommandé pour ce scénario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Activation du suivi pour Signalr ScaleOut

Pour activer le suivi des plans, ajoutez les sections suivantes au fichier Web. config, sous l’élément de **configuration** racine :

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
