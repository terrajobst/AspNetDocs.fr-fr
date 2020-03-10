---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mappage des utilisateurs Signalr aux connexions | Microsoft Docs
author: bradygaster
description: Cette rubrique montre comment conserver des informations sur les utilisateurs et leurs connexions. Patrick Fletcher a aidé à écrire cette rubrique. Versions logicielles utilisées dans cette rubrique...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536777"
---
# <a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="2bfee-105">Mappage des utilisateurs SignalR aux connexions</span><span class="sxs-lookup"><span data-stu-id="2bfee-105">Mapping SignalR Users to Connections</span></span>

<span data-ttu-id="2bfee-106">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2bfee-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="2bfee-107">Cette rubrique montre comment conserver des informations sur les utilisateurs et leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="2bfee-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="2bfee-108">Patrick Fletcher a aidé à écrire cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="2bfee-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2bfee-109">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="2bfee-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="2bfee-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2bfee-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2bfee-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2bfee-111">.NET 4.5</span></span>
> - <span data-ttu-id="2bfee-112">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="2bfee-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2bfee-113">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="2bfee-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="2bfee-114">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="2bfee-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="2bfee-115">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="2bfee-115">Questions and comments</span></span>
>
> <span data-ttu-id="2bfee-116">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="2bfee-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2bfee-117">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2bfee-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="2bfee-118">Introduction</span><span class="sxs-lookup"><span data-stu-id="2bfee-118">Introduction</span></span>

<span data-ttu-id="2bfee-119">Chaque client qui se connecte à un hub passe un ID de connexion unique. Vous pouvez récupérer cette valeur dans la propriété `Context.ConnectionId` du contexte du Hub.</span><span class="sxs-lookup"><span data-stu-id="2bfee-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="2bfee-120">Si votre application doit mapper un utilisateur à l’ID de connexion et conserver ce mappage, vous pouvez utiliser l’une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="2bfee-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="2bfee-121">Fournisseur de l’ID utilisateur (Signalr 2)</span><span class="sxs-lookup"><span data-stu-id="2bfee-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="2bfee-122">[Stockage en mémoire](#inmemory), tel qu’un dictionnaire</span><span class="sxs-lookup"><span data-stu-id="2bfee-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="2bfee-123">Groupe signalr pour chaque utilisateur</span><span class="sxs-lookup"><span data-stu-id="2bfee-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="2bfee-124">[Stockage externe et permanent](#database), tel qu’une table de base de données ou un stockage table Azure</span><span class="sxs-lookup"><span data-stu-id="2bfee-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="2bfee-125">Chacune de ces implémentations est présentée dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="2bfee-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="2bfee-126">Vous utilisez les méthodes `OnConnected`, `OnDisconnected`et `OnReconnected` de la classe `Hub` pour suivre l’état de la connexion utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="2bfee-127">La meilleure approche pour votre application dépend des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2bfee-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="2bfee-128">Le nombre de serveurs Web qui hébergent votre application.</span><span class="sxs-lookup"><span data-stu-id="2bfee-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="2bfee-129">Si vous avez besoin d’obtenir la liste des utilisateurs actuellement connectés.</span><span class="sxs-lookup"><span data-stu-id="2bfee-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="2bfee-130">Si vous devez conserver des informations sur les groupes et les utilisateurs lors du redémarrage de l’application ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="2bfee-131">Indique si la latence de l’appel d’un serveur externe est un problème.</span><span class="sxs-lookup"><span data-stu-id="2bfee-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="2bfee-132">Le tableau suivant indique l’approche qui fonctionne pour ces considérations.</span><span class="sxs-lookup"><span data-stu-id="2bfee-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="2bfee-133">Plusieurs serveurs</span><span class="sxs-lookup"><span data-stu-id="2bfee-133">More than one server</span></span> | <span data-ttu-id="2bfee-134">Obtient la liste des utilisateurs actuellement connectés</span><span class="sxs-lookup"><span data-stu-id="2bfee-134">Get list of currently connected users</span></span> | <span data-ttu-id="2bfee-135">Conserver les informations après le redémarrage</span><span class="sxs-lookup"><span data-stu-id="2bfee-135">Persist information after restarts</span></span> | <span data-ttu-id="2bfee-136">Performances optimales</span><span class="sxs-lookup"><span data-stu-id="2bfee-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="2bfee-137">Fournisseur UserID</span><span class="sxs-lookup"><span data-stu-id="2bfee-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="2bfee-138">En mémoire</span><span class="sxs-lookup"><span data-stu-id="2bfee-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="2bfee-139">Groupes à un seul utilisateur</span><span class="sxs-lookup"><span data-stu-id="2bfee-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="2bfee-140">Permanent, externe</span><span class="sxs-lookup"><span data-stu-id="2bfee-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="2bfee-141">Fournisseur IUserID</span><span class="sxs-lookup"><span data-stu-id="2bfee-141">IUserID provider</span></span>

<span data-ttu-id="2bfee-142">Cette fonctionnalité permet aux utilisateurs de spécifier ce que l’userId est basé sur un IRequest via une nouvelle interface IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="2bfee-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="2bfee-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="2bfee-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="2bfee-144">Par défaut, il y aura une implémentation qui utilise le `IPrincipal.Identity.Name` de l’utilisateur comme nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="2bfee-145">Pour modifier cette valeur, enregistrez votre implémentation de `IUserIdProvider` avec l’hôte global au démarrage de votre application :</span><span class="sxs-lookup"><span data-stu-id="2bfee-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="2bfee-146">À partir d’un concentrateur, vous pouvez envoyer des messages à ces utilisateurs via l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="2bfee-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="2bfee-147">**Envoi d’un message à un utilisateur spécifique**</span><span class="sxs-lookup"><span data-stu-id="2bfee-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="2bfee-148">Stockage en mémoire</span><span class="sxs-lookup"><span data-stu-id="2bfee-148">In-memory storage</span></span>

<span data-ttu-id="2bfee-149">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans un dictionnaire stocké en mémoire.</span><span class="sxs-lookup"><span data-stu-id="2bfee-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="2bfee-150">Le dictionnaire utilise une `HashSet` pour stocker l’ID de connexion. À tout moment, un utilisateur peut avoir plusieurs connexions à l’application Signalr.</span><span class="sxs-lookup"><span data-stu-id="2bfee-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="2bfee-151">Par exemple, un utilisateur connecté via plusieurs appareils ou plusieurs onglets de navigateur a plusieurs ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="2bfee-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="2bfee-152">Si l’application s’arrête, toutes les informations sont perdues, mais elles sont à nouveau renseignées à mesure que les utilisateurs rétablissent leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="2bfee-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="2bfee-153">Le stockage en mémoire ne fonctionne pas si votre environnement comprend plusieurs serveurs Web, car chaque serveur possède un regroupement distinct de connexions.</span><span class="sxs-lookup"><span data-stu-id="2bfee-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="2bfee-154">Le premier exemple montre une classe qui gère le mappage des utilisateurs aux connexions.</span><span class="sxs-lookup"><span data-stu-id="2bfee-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="2bfee-155">La clé pour HashSet sera le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="2bfee-156">L’exemple suivant montre comment utiliser la classe de mappage de connexion à partir d’un Hub.</span><span class="sxs-lookup"><span data-stu-id="2bfee-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="2bfee-157">L’instance de la classe est stockée dans un nom de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="2bfee-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="2bfee-158">Groupes à un seul utilisateur</span><span class="sxs-lookup"><span data-stu-id="2bfee-158">Single-user groups</span></span>

<span data-ttu-id="2bfee-159">Vous pouvez créer un groupe pour chaque utilisateur, puis envoyer un message à ce groupe lorsque vous souhaitez atteindre uniquement cet utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="2bfee-160">Le nom de chaque groupe est le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="2bfee-161">Si un utilisateur a plusieurs connexions, chaque ID de connexion est ajouté au groupe de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="2bfee-162">Vous ne devez pas supprimer manuellement l’utilisateur du groupe lorsque l’utilisateur se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="2bfee-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="2bfee-163">Cette action est effectuée automatiquement par l’infrastructure Signalr.</span><span class="sxs-lookup"><span data-stu-id="2bfee-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="2bfee-164">L’exemple suivant montre comment implémenter des groupes d’utilisateurs uniques.</span><span class="sxs-lookup"><span data-stu-id="2bfee-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="2bfee-165">Stockage externe et permanent</span><span class="sxs-lookup"><span data-stu-id="2bfee-165">Permanent, external storage</span></span>

<span data-ttu-id="2bfee-166">Cette rubrique montre comment utiliser une base de données ou un stockage table Azure pour stocker les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="2bfee-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="2bfee-167">Cette approche fonctionne lorsque vous avez plusieurs serveurs Web, car chaque serveur Web peut interagir avec le même référentiel de données.</span><span class="sxs-lookup"><span data-stu-id="2bfee-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="2bfee-168">Si vos serveurs Web cessent de fonctionner ou si l’application redémarre, la méthode `OnDisconnected` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="2bfee-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="2bfee-169">Par conséquent, il est possible que votre dépôt de données contiendra des enregistrements pour les ID de connexion qui ne sont plus valides.</span><span class="sxs-lookup"><span data-stu-id="2bfee-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="2bfee-170">Pour nettoyer ces enregistrements orphelins, vous pouvez invalider les connexions qui ont été créées en dehors d’une plage de temps adaptée à votre application.</span><span class="sxs-lookup"><span data-stu-id="2bfee-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="2bfee-171">Les exemples de cette section incluent une valeur pour le suivi lors de la création de la connexion, mais n’indiquent pas comment nettoyer les anciens enregistrements, car vous pouvez le faire en tant que processus en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="2bfee-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="2bfee-172">Base de données</span><span class="sxs-lookup"><span data-stu-id="2bfee-172">Database</span></span>

<span data-ttu-id="2bfee-173">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="2bfee-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="2bfee-174">Vous pouvez utiliser n’importe quelle technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2bfee-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="2bfee-175">Ces modèles d’entité correspondent aux tables et aux champs de la base de données.</span><span class="sxs-lookup"><span data-stu-id="2bfee-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="2bfee-176">La structure de vos données peut varier considérablement en fonction des exigences de votre application.</span><span class="sxs-lookup"><span data-stu-id="2bfee-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="2bfee-177">Le premier exemple montre comment définir une entité utilisateur qui peut être associée à de nombreuses entités de connexion.</span><span class="sxs-lookup"><span data-stu-id="2bfee-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="2bfee-178">Ensuite, à partir du Hub, vous pouvez suivre l’état de chaque connexion avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2bfee-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="2bfee-179">Stockage de table Azure</span><span class="sxs-lookup"><span data-stu-id="2bfee-179">Azure table storage</span></span>

<span data-ttu-id="2bfee-180">L’exemple de stockage de table Azure suivant est similaire à l’exemple de base de données.</span><span class="sxs-lookup"><span data-stu-id="2bfee-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="2bfee-181">Il n’inclut pas toutes les informations dont vous pourriez avoir besoin pour vous familiariser avec le service de stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="2bfee-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="2bfee-182">Pour plus d’informations, consultez [utilisation du stockage de tables à partir de .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="2bfee-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="2bfee-183">L’exemple suivant montre une entité de table pour le stockage des informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="2bfee-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="2bfee-184">Elle partitionne les données par nom d’utilisateur et identifie chaque entité par l’ID de connexion, de sorte qu’un utilisateur peut avoir plusieurs connexions à tout moment.</span><span class="sxs-lookup"><span data-stu-id="2bfee-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="2bfee-185">Dans le Hub, vous suivez l’état de la connexion de chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2bfee-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
