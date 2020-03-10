---
uid: signalr/overview/security/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes Signalr | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente. Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578875"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="d8327-104">Authentification et autorisation pour les connexions persistantes SignalR</span><span class="sxs-lookup"><span data-stu-id="d8327-104">Authentication and Authorization for SignalR Persistent Connections</span></span>

<span data-ttu-id="d8327-105">de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d8327-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d8327-106">Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente.</span><span class="sxs-lookup"><span data-stu-id="d8327-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="d8327-107">Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr, consultez [Présentation de la sécurité](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="d8327-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d8327-108">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="d8327-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="d8327-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d8327-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="d8327-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d8327-110">.NET 4.5</span></span>
> - <span data-ttu-id="d8327-111">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="d8327-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d8327-112">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="d8327-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="d8327-113">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d8327-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="d8327-114">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="d8327-114">Questions and comments</span></span>
>
> <span data-ttu-id="d8327-115">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="d8327-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d8327-116">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d8327-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="d8327-117">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="d8327-117">Enforce authorization</span></span>

<span data-ttu-id="d8327-118">Pour appliquer des règles d’autorisation lors de l’utilisation d’un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , vous devez remplacer la méthode `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="d8327-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="d8327-119">Vous ne pouvez pas utiliser l’attribut `Authorize` avec des connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="d8327-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="d8327-120">La méthode `AuthorizeRequest` est appelée par l’infrastructure Signalr avant chaque demande pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée.</span><span class="sxs-lookup"><span data-stu-id="d8327-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="d8327-121">La méthode `AuthorizeRequest` n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via le mécanisme d’authentification standard de votre application.</span><span class="sxs-lookup"><span data-stu-id="d8327-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="d8327-122">L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="d8327-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="d8327-123">Vous pouvez ajouter n’importe quelle logique d’autorisation personnalisée dans la méthode AuthorizeRequest. par exemple, pour vérifier si un utilisateur appartient à un rôle particulier.</span><span class="sxs-lookup"><span data-stu-id="d8327-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
