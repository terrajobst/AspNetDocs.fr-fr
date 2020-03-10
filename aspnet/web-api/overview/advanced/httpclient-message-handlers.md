---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestionnaires de messages HttpClient dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Créer des gestionnaires de messages personnalisés pour API Web ASP.NET dans ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557644"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="068d3-103">Gestionnaires de messages HttpClient dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="068d3-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="068d3-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="068d3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="068d3-105">Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et retourne une réponse http.</span><span class="sxs-lookup"><span data-stu-id="068d3-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="068d3-106">En général, une série de gestionnaires de messages sont chaînés ensemble.</span><span class="sxs-lookup"><span data-stu-id="068d3-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="068d3-107">Le premier gestionnaire reçoit une requête HTTP, effectue un traitement et transmet la requête au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="068d3-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="068d3-108">À un moment donné, la réponse est créée et reprend la chaîne.</span><span class="sxs-lookup"><span data-stu-id="068d3-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="068d3-109">Ce modèle est appelé gestionnaire de *délégation* .</span><span class="sxs-lookup"><span data-stu-id="068d3-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="068d3-110">Côté client, la classe **httpclient** utilise un gestionnaire de messages pour traiter les demandes.</span><span class="sxs-lookup"><span data-stu-id="068d3-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="068d3-111">Le gestionnaire par défaut est **HttpClientHandler**, qui envoie la demande sur le réseau et obtient la réponse du serveur.</span><span class="sxs-lookup"><span data-stu-id="068d3-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="068d3-112">Vous pouvez insérer des gestionnaires de messages personnalisés dans le pipeline client :</span><span class="sxs-lookup"><span data-stu-id="068d3-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="068d3-113">API Web ASP.NET utilise également des gestionnaires de messages côté serveur.</span><span class="sxs-lookup"><span data-stu-id="068d3-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="068d3-114">Pour plus d’informations, consultez [gestionnaires de messages http](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="068d3-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="068d3-115">Gestionnaires de messages personnalisés</span><span class="sxs-lookup"><span data-stu-id="068d3-115">Custom Message Handlers</span></span>

<span data-ttu-id="068d3-116">Pour écrire un gestionnaire de messages personnalisé, dérivez de **System .net. http. DelegatingHandler** et substituez la méthode **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="068d3-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="068d3-117">Voici la signature de méthode :</span><span class="sxs-lookup"><span data-stu-id="068d3-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="068d3-118">La méthode prend un **HttpRequestMessage** comme entrée et retourne de manière asynchrone un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="068d3-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="068d3-119">Une implémentation classique effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="068d3-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="068d3-120">Traiter le message de demande.</span><span class="sxs-lookup"><span data-stu-id="068d3-120">Process the request message.</span></span>
2. <span data-ttu-id="068d3-121">Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="068d3-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="068d3-122">Le gestionnaire interne retourne un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="068d3-122">The inner handler returns a response message.</span></span> <span data-ttu-id="068d3-123">(Cette étape est asynchrone.)</span><span class="sxs-lookup"><span data-stu-id="068d3-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="068d3-124">Traitez la réponse et renvoyez-la à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="068d3-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="068d3-125">L’exemple suivant illustre un gestionnaire de messages qui ajoute un en-tête personnalisé à la demande sortante :</span><span class="sxs-lookup"><span data-stu-id="068d3-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="068d3-126">L’appel à `base.SendAsync` est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="068d3-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="068d3-127">Si le gestionnaire effectue une opération après cet appel, utilisez le mot clé **await** pour reprendre l’exécution une fois la méthode terminée.</span><span class="sxs-lookup"><span data-stu-id="068d3-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="068d3-128">L’exemple suivant montre un gestionnaire qui journalise les codes d’erreur.</span><span class="sxs-lookup"><span data-stu-id="068d3-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="068d3-129">La journalisation elle-même n’est pas très intéressante, mais l’exemple montre comment accéder à la réponse à l’intérieur du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="068d3-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="068d3-130">Ajout de gestionnaires de messages au pipeline client</span><span class="sxs-lookup"><span data-stu-id="068d3-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="068d3-131">Pour ajouter des gestionnaires personnalisés à **httpclient**, utilisez la méthode **HttpClientFactory. Create** :</span><span class="sxs-lookup"><span data-stu-id="068d3-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="068d3-132">Les gestionnaires de messages sont appelés dans l’ordre dans lequel vous les transmettez à la méthode **Create** .</span><span class="sxs-lookup"><span data-stu-id="068d3-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="068d3-133">Étant donné que les gestionnaires sont imbriqués, le message de réponse se déplace dans l’autre direction.</span><span class="sxs-lookup"><span data-stu-id="068d3-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="068d3-134">Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="068d3-134">That is, the last handler is the first to get the response message.</span></span>
