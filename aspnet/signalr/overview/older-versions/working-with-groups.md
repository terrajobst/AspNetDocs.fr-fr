---
uid: signalr/overview/older-versions/working-with-groups
title: Utilisation des groupes dans Signalr 1. x | Microsoft Docs
author: bradygaster
description: Cette rubrique explique comment conserver les informations d’appartenance à un groupe à l’aide de l’API Hub.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5f50dc162d6cdcfbf2261e6a751f5f99078d5c54
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579365"
---
# <a name="working-with-groups-in-signalr-1x"></a>Utilisation des groupes dans SignalR 1.x

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique explique comment ajouter des utilisateurs à des groupes et rendre des informations d’appartenance à un groupe persistantes.

## <a name="overview"></a>Présentation

Dans Signalr, les groupes fournissent une méthode de diffusion des messages aux sous-ensembles spécifiés de clients connectés. Un groupe peut avoir un nombre quelconque de clients et un client peut être membre d’un nombre quelconque de groupes. Vous n’êtes pas obligé de créer explicitement des groupes. En effet, un groupe est créé automatiquement la première fois que vous spécifiez son nom dans un appel à groups. Add, et il est supprimé lorsque vous supprimez la dernière connexion de l’appartenance à celui-ci. Pour une introduction à l’utilisation de groupes, consultez [Comment gérer l’appartenance à un groupe à partir de la classe Hub](index.md) dans le Guide de serveur de l’API hubs.

Il n’existe aucune API pour obtenir une liste d’appartenance à un groupe ou une liste de groupes. Signalr envoie des messages aux clients et aux groupes en fonction d’un modèle Pub/Sub, et le serveur ne gère pas les listes de groupes ou d’appartenances aux groupes. Cela permet d’optimiser l’évolutivité, car chaque fois que vous ajoutez un nœud à une batterie de serveurs Web, tout état géré par Signalr doit être propagé vers le nouveau nœud.

Lorsque vous ajoutez un utilisateur à un groupe à l’aide de la méthode `Groups.Add`, l’utilisateur reçoit les messages dirigés vers ce groupe pendant la durée de la connexion actuelle, mais l’appartenance de l’utilisateur à ce groupe n’est pas conservée au-delà de la connexion actuelle. Si vous souhaitez conserver définitivement des informations sur les groupes et l’appartenance au groupe, vous devez stocker ces données dans un référentiel tel qu’une base de données ou un stockage table Azure. Ensuite, chaque fois qu’un utilisateur se connecte à votre application, vous récupérez à partir du référentiel auquel appartient l’utilisateur et vous ajoutez manuellement cet utilisateur à ces groupes.

Lorsqu’il se reconnecte après une interruption temporaire, l’utilisateur rejoint automatiquement les groupes précédemment affectés. La rejointure automatique d’un groupe s’applique uniquement lors de la reconnexion, et non lors de l’établissement d’une nouvelle connexion. Un jeton signé numériquement est transmis à partir du client qui contient la liste des groupes précédemment affectés. Si vous souhaitez vérifier si l’utilisateur appartient aux groupes demandés, vous pouvez remplacer le comportement par défaut.

Cette rubrique comporte les sections suivantes :

- [Ajout et suppression d’utilisateurs](#add)
- [Appel de membres d’un groupe](#call)
- [Stockage de l’appartenance à un groupe dans une base de données](#storedatabase)
- [Stockage de l’appartenance aux groupes dans le stockage table Azure](#storeazuretable)
- [Vérification de l’appartenance au groupe lors de la reconnexion](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Ajout et suppression d’utilisateurs

Pour ajouter ou supprimer des utilisateurs d’un groupe, appelez les méthodes [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , puis transmettez l’ID de connexion et le nom du groupe de l’utilisateur en tant que paramètres. Vous n’avez pas besoin de supprimer manuellement un utilisateur d’un groupe lorsque la connexion se termine.

L’exemple suivant montre les méthodes `Groups.Add` et `Groups.Remove` utilisées dans les méthodes de concentrateur.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Les méthodes `Groups.Add` et `Groups.Remove` s’exécutent de façon asynchrone.

Si vous souhaitez ajouter un client à un groupe et envoyer immédiatement un message au client à l’aide du groupe, vous devez vous assurer que la méthode groups. Add se termine en premier. Les exemples de code suivants montrent comment procéder, l’un à l’aide du code qui fonctionne dans .NET 4,5, et l’autre à l’aide du code qui fonctionne dans .NET 4.

#### <a name="asynchronous-net-45-example"></a>Exemple .NET 4,5 asynchrone

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Exemple de .NET 4 asynchrone

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

En général, vous ne devez pas inclure `await` lors de l’appel de la méthode `Groups.Remove`, car l’ID de connexion que vous essayez de supprimer n’est peut-être plus disponible. Dans ce cas, `TaskCanceledException` est levée après l’expiration de la demande. Si votre application doit s’assurer que l’utilisateur a été supprimé du groupe avant d’envoyer un message au groupe, vous pouvez ajouter `await` avant groups. Remove, puis intercepter l’exception `TaskCanceledException` qui peut être levée.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Appel de membres d’un groupe

Vous pouvez envoyer des messages à tous les membres d’un groupe ou uniquement aux membres spécifiés du groupe, comme indiqué dans les exemples suivants.

- **Tous les** clients connectés dans un groupe spécifié. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Tous les clients connectés dans un groupe spécifié, **à l’exception des clients spécifiés**, identifiés par l’ID de connexion. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Tous les clients connectés dans un groupe spécifié **, à l’exception du client appelant**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Stockage de l’appartenance à un groupe dans une base de données

Les exemples suivants montrent comment conserver des informations sur les groupes et les utilisateurs dans une base de données. Vous pouvez utiliser n’importe quelle technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide de Entity Framework. Ces modèles d’entité correspondent aux tables et aux champs de la base de données. La structure de vos données peut varier considérablement en fonction des exigences de votre application. Cet exemple comprend une classe nommée `ConversationRoom` qui est propre à une application qui permet aux utilisateurs de joindre des conversations sur différents sujets, tels que des sports ou des jardins. Cet exemple comprend également une classe pour les connexions. La classe de connexion n’est pas absolument requise pour le suivi de l’appartenance à un groupe, mais fait souvent partie d’une solution robuste pour le suivi des utilisateurs.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Ensuite, dans le Hub, vous pouvez récupérer les informations de groupe et d’utilisateur à partir de la base de données et ajouter manuellement l’utilisateur aux groupes appropriés. L’exemple n’inclut pas de code pour le suivi des connexions utilisateur. Dans cet exemple, le mot clé `await` n’est pas appliqué avant `Groups.Add` car un message n’est pas immédiatement envoyé aux membres du groupe. Si vous souhaitez envoyer un message à tous les membres du groupe immédiatement après avoir ajouté le nouveau membre, vous souhaiterez appliquer le mot clé `await` pour vous assurer que l’opération asynchrone est terminée.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Stockage de l’appartenance aux groupes dans le stockage table Azure

L’utilisation du stockage table Azure pour stocker les informations sur les groupes et les utilisateurs est semblable à l’utilisation d’une base de données. L’exemple suivant montre une entité de table qui stocke le nom d’utilisateur et le nom de groupe.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

Dans le Hub, vous récupérez les groupes affectés lorsque l’utilisateur se connecte.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Vérification de l’appartenance au groupe lors de la reconnexion

Par défaut, Signalr réattribue automatiquement un utilisateur aux groupes appropriés lors de la reconnexion après une interruption temporaire, par exemple lorsqu’une connexion est supprimée et rétablie avant l’expiration du délai de connexion. Les informations de groupe de l’utilisateur sont transmises dans un jeton lors de la reconnexion, et ce jeton est vérifié sur le serveur. Pour plus d’informations sur le processus de vérification pour rattacher des utilisateurs à des groupes, consultez [rejoindre des groupes lors de la reconnexion](index.md).

En général, vous devez utiliser le comportement par défaut de rejointure automatique des groupes lors de la reconnexion. Les groupes signalr ne sont pas conçus comme un mécanisme de sécurité pour restreindre l’accès aux données sensibles. Toutefois, si votre application doit doubler la vérification de l’appartenance à un groupe d’un utilisateur lors de la reconnexion, vous pouvez remplacer le comportement par défaut. La modification du comportement par défaut peut ajouter une charge à votre base de données, car l’appartenance au groupe d’un utilisateur doit être récupérée pour chaque reconnexion, et non uniquement lorsque l’utilisateur se connecte.

Si vous devez vérifier l’appartenance au groupe lors de la reconnexion, créez un module de pipeline Hub qui retourne la liste des groupes attribués, comme indiqué ci-dessous.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Ensuite, ajoutez ce module au pipeline Hub, comme indiqué ci-dessous.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
