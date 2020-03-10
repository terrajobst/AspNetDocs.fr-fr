---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mappage des utilisateurs Signalr aux connexions dans Signalr 1. x | Microsoft Docs
author: bradygaster
description: Cette rubrique montre comment conserver des informations sur les utilisateurs et leurs connexions.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558484"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Mappage des utilisateurs SignalR aux connexions dans SignalR 1.x

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique montre comment conserver des informations sur les utilisateurs et leurs connexions.

## <a name="introduction"></a>Introduction

Chaque client qui se connecte à un hub passe un ID de connexion unique. Vous pouvez récupérer cette valeur dans la propriété `Context.ConnectionId` du contexte du Hub. Si votre application doit mapper un utilisateur à l’ID de connexion et conserver ce mappage, vous pouvez utiliser l’une des options suivantes :

- [Stockage en mémoire](#inmemory), tel qu’un dictionnaire
- [Groupe signalr pour chaque utilisateur](#groups)
- [Stockage externe et permanent](#database), tel qu’une table de base de données ou un stockage table Azure

Chacune de ces implémentations est présentée dans cette rubrique. Vous utilisez les méthodes `OnConnected`, `OnDisconnected`et `OnReconnected` de la classe `Hub` pour suivre l’état de la connexion utilisateur.

La meilleure approche pour votre application dépend des éléments suivants :

- Le nombre de serveurs Web qui hébergent votre application.
- Si vous avez besoin d’obtenir la liste des utilisateurs actuellement connectés.
- Si vous devez conserver des informations sur les groupes et les utilisateurs lors du redémarrage de l’application ou du serveur.
- Indique si la latence de l’appel d’un serveur externe est un problème.

Le tableau suivant indique l’approche qui fonctionne pour ces considérations.

|  | Plusieurs serveurs | Obtient la liste des utilisateurs actuellement connectés | Conserver les informations après le redémarrage | Performances optimales |
| --- | --- | --- | --- | --- |
| En mémoire |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Groupes à un seul utilisateur | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Permanent, externe | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Stockage en mémoire

Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans un dictionnaire stocké en mémoire. Le dictionnaire utilise une `HashSet` pour stocker l’ID de connexion. À tout moment, un utilisateur peut avoir plusieurs connexions à l’application Signalr. Par exemple, un utilisateur connecté via plusieurs appareils ou plusieurs onglets de navigateur a plusieurs ID de connexion.

Si l’application s’arrête, toutes les informations sont perdues, mais elles sont à nouveau renseignées à mesure que les utilisateurs rétablissent leurs connexions. Le stockage en mémoire ne fonctionne pas si votre environnement comprend plusieurs serveurs Web, car chaque serveur possède un regroupement distinct de connexions.

Le premier exemple montre une classe qui gère le mappage des utilisateurs aux connexions. La clé pour HashSet sera le nom de l’utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

L’exemple suivant montre comment utiliser la classe de mappage de connexion à partir d’un Hub. L’instance de la classe est stockée dans un nom de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Groupes à un seul utilisateur

Vous pouvez créer un groupe pour chaque utilisateur, puis envoyer un message à ce groupe lorsque vous souhaitez atteindre uniquement cet utilisateur. Le nom de chaque groupe est le nom de l’utilisateur. Si un utilisateur a plusieurs connexions, chaque ID de connexion est ajouté au groupe de l’utilisateur.

Vous ne devez pas supprimer manuellement l’utilisateur du groupe lorsque l’utilisateur se déconnecte. Cette action est effectuée automatiquement par l’infrastructure Signalr.

L’exemple suivant montre comment implémenter des groupes d’utilisateurs uniques.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Stockage externe et permanent

Cette rubrique montre comment utiliser une base de données ou un stockage table Azure pour stocker les informations de connexion. Cette approche fonctionne lorsque vous avez plusieurs serveurs Web, car chaque serveur Web peut interagir avec le même référentiel de données. Si vos serveurs Web cessent de fonctionner ou si l’application redémarre, la méthode `OnDisconnected` n’est pas appelée. Par conséquent, il est possible que votre dépôt de données contiendra des enregistrements pour les ID de connexion qui ne sont plus valides. Pour nettoyer ces enregistrements orphelins, vous pouvez invalider les connexions qui ont été créées en dehors d’une plage de temps adaptée à votre application. Les exemples de cette section incluent une valeur pour le suivi lors de la création de la connexion, mais n’indiquent pas comment nettoyer les anciens enregistrements, car vous pouvez le faire en tant que processus en arrière-plan.

### <a name="database"></a>Base de données

Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans une base de données. Vous pouvez utiliser n’importe quelle technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide de Entity Framework. Ces modèles d’entité correspondent aux tables et aux champs de la base de données. La structure de vos données peut varier considérablement en fonction des exigences de votre application.

Le premier exemple montre comment définir une entité utilisateur qui peut être associée à de nombreuses entités de connexion.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Ensuite, à partir du Hub, vous pouvez suivre l’état de chaque connexion avec le code indiqué ci-dessous.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Stockage de table Azure

L’exemple de stockage de table Azure suivant est similaire à l’exemple de base de données. Il n’inclut pas toutes les informations dont vous pourriez avoir besoin pour vous familiariser avec le service de stockage table Azure. Pour plus d’informations, consultez [utilisation du stockage de tables à partir de .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

L’exemple suivant montre une entité de table pour le stockage des informations de connexion. Elle partitionne les données par nom d’utilisateur et identifie chaque entité par l’ID de connexion, de sorte qu’un utilisateur peut avoir plusieurs connexions à tout moment.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

Dans le Hub, vous suivez l’état de la connexion de chaque utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
