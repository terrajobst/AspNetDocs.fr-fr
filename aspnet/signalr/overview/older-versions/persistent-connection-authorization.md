---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente. Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536539"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="3216b-104">Authentification et autorisation pour les connexions persistantes SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="3216b-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="3216b-105">de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3216b-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="3216b-106">Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente.</span><span class="sxs-lookup"><span data-stu-id="3216b-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="3216b-107">Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr, consultez [Présentation de la sécurité](index.md).</span><span class="sxs-lookup"><span data-stu-id="3216b-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="3216b-108">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="3216b-108">Enforce authorization</span></span>

<span data-ttu-id="3216b-109">Pour appliquer des règles d’autorisation lors de l’utilisation d’un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , vous devez remplacer la méthode `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="3216b-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="3216b-110">Vous ne pouvez pas utiliser l’attribut `Authorize` avec des connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="3216b-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="3216b-111">La méthode `AuthorizeRequest` est appelée par l’infrastructure Signalr avant chaque demande pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée.</span><span class="sxs-lookup"><span data-stu-id="3216b-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="3216b-112">La méthode `AuthorizeRequest` n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via le mécanisme d’authentification standard de votre application.</span><span class="sxs-lookup"><span data-stu-id="3216b-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="3216b-113">L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="3216b-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="3216b-114">Vous pouvez ajouter n’importe quelle logique d’autorisation personnalisée dans la méthode AuthorizeRequest. par exemple, pour vérifier si un utilisateur appartient à un rôle particulier.</span><span class="sxs-lookup"><span data-stu-id="3216b-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
