---
uid: web-api/overview/advanced/http-message-handlers
title: Gestionnaires de messages HTTP dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Vue d’ensemble des gestionnaires de messages HTTP dans API Web ASP.NET pour ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622583"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="29465-103">Gestionnaires de messages HTTP dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="29465-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="29465-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="29465-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="29465-105">Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et retourne une réponse http.</span><span class="sxs-lookup"><span data-stu-id="29465-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="29465-106">Les gestionnaires de messages dérivent de la classe abstraite **HttpMessageHandler** .</span><span class="sxs-lookup"><span data-stu-id="29465-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="29465-107">En général, une série de gestionnaires de messages sont chaînés ensemble.</span><span class="sxs-lookup"><span data-stu-id="29465-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="29465-108">Le premier gestionnaire reçoit une requête HTTP, effectue un traitement et transmet la requête au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="29465-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="29465-109">À un moment donné, la réponse est créée et reprend la chaîne.</span><span class="sxs-lookup"><span data-stu-id="29465-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="29465-110">Ce modèle est appelé gestionnaire de *délégation* .</span><span class="sxs-lookup"><span data-stu-id="29465-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="29465-111">Gestionnaires de messages côté serveur</span><span class="sxs-lookup"><span data-stu-id="29465-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="29465-112">Côté serveur, le pipeline de l’API Web utilise des gestionnaires de messages intégrés :</span><span class="sxs-lookup"><span data-stu-id="29465-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="29465-113">**Httpserver** obtient la demande de la part de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="29465-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="29465-114">**HttpRoutingDispatcher** distribue la demande en fonction de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="29465-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="29465-115">**HttpControllerDispatcher** envoie la demande à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="29465-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="29465-116">Vous pouvez ajouter des gestionnaires personnalisés au pipeline.</span><span class="sxs-lookup"><span data-stu-id="29465-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="29465-117">Les gestionnaires de messages sont bons pour les problèmes de coupe croisée qui fonctionnent au niveau des messages HTTP (plutôt que des actions de contrôleur).</span><span class="sxs-lookup"><span data-stu-id="29465-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="29465-118">Par exemple, un gestionnaire de messages peut :</span><span class="sxs-lookup"><span data-stu-id="29465-118">For example, a message handler might:</span></span>

- <span data-ttu-id="29465-119">En-têtes de demande de lecture ou de modification.</span><span class="sxs-lookup"><span data-stu-id="29465-119">Read or modify request headers.</span></span>
- <span data-ttu-id="29465-120">Ajoutez un en-tête de réponse aux réponses.</span><span class="sxs-lookup"><span data-stu-id="29465-120">Add a response header to responses.</span></span>
- <span data-ttu-id="29465-121">Validez les requêtes avant qu’elles atteignent le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="29465-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="29465-122">Ce diagramme montre deux gestionnaires personnalisés insérés dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="29465-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="29465-123">Du côté client, HttpClient utilise également des gestionnaires de messages.</span><span class="sxs-lookup"><span data-stu-id="29465-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="29465-124">Pour plus d’informations, consultez [gestionnaires de messages httpclient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="29465-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="29465-125">Gestionnaires de messages personnalisés</span><span class="sxs-lookup"><span data-stu-id="29465-125">Custom Message Handlers</span></span>

<span data-ttu-id="29465-126">Pour écrire un gestionnaire de messages personnalisé, dérivez de **System .net. http. DelegatingHandler** et substituez la méthode **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="29465-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="29465-127">Cette méthode a la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="29465-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="29465-128">La méthode prend un **HttpRequestMessage** comme entrée et retourne de manière asynchrone un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="29465-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="29465-129">Une implémentation classique effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="29465-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="29465-130">Traiter le message de demande.</span><span class="sxs-lookup"><span data-stu-id="29465-130">Process the request message.</span></span>
2. <span data-ttu-id="29465-131">Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="29465-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="29465-132">Le gestionnaire interne retourne un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="29465-132">The inner handler returns a response message.</span></span> <span data-ttu-id="29465-133">(Cette étape est asynchrone.)</span><span class="sxs-lookup"><span data-stu-id="29465-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="29465-134">Traitez la réponse et renvoyez-la à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="29465-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="29465-135">Voici un exemple simple :</span><span class="sxs-lookup"><span data-stu-id="29465-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="29465-136">L’appel à `base.SendAsync` est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="29465-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="29465-137">Si le gestionnaire effectue un travail après cet appel, utilisez le mot clé **await** , comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="29465-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="29465-138">Un gestionnaire de délégation peut également ignorer le gestionnaire interne et créer directement la réponse :</span><span class="sxs-lookup"><span data-stu-id="29465-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="29465-139">Si un gestionnaire de délégation crée la réponse sans appeler `base.SendAsync`, la demande ignore le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="29465-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="29465-140">Cela peut être utile pour un gestionnaire qui valide la requête (création d’une réponse d’erreur).</span><span class="sxs-lookup"><span data-stu-id="29465-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="29465-141">Ajout d’un gestionnaire au pipeline</span><span class="sxs-lookup"><span data-stu-id="29465-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="29465-142">Pour ajouter un gestionnaire de messages côté serveur, ajoutez le gestionnaire à la collection **HttpConfiguration. MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="29465-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="29465-143">Si vous avez utilisé le modèle « application Web ASP.NET MVC 4 » pour créer le projet, vous pouvez le faire à l’intérieur de la classe **WebApiConfig** :</span><span class="sxs-lookup"><span data-stu-id="29465-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="29465-144">Les gestionnaires de messages sont appelés dans l’ordre dans lequel ils apparaissent dans la collection **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="29465-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="29465-145">Étant donné qu’elles sont imbriquées, le message de réponse se déplace dans l’autre direction.</span><span class="sxs-lookup"><span data-stu-id="29465-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="29465-146">Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="29465-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="29465-147">Notez que vous n’avez pas besoin de définir les gestionnaires internes ; l’infrastructure d’API Web connecte automatiquement les gestionnaires de messages.</span><span class="sxs-lookup"><span data-stu-id="29465-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="29465-148">Si vous êtes [auto-hébergé](../older-versions/self-host-a-web-api.md), créez une instance de la classe **HttpSelfHostConfiguration** et ajoutez les gestionnaires à la collection **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="29465-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="29465-149">Examinons à présent quelques exemples de gestionnaires de messages personnalisés.</span><span class="sxs-lookup"><span data-stu-id="29465-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="29465-150">Exemple : X-HTTP-Method-override</span><span class="sxs-lookup"><span data-stu-id="29465-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="29465-151">X-HTTP-Method-override est un en-tête HTTP non standard.</span><span class="sxs-lookup"><span data-stu-id="29465-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="29465-152">Il est conçu pour les clients qui ne peuvent pas envoyer certains types de requêtes HTTP, tels que PUT ou DELETE.</span><span class="sxs-lookup"><span data-stu-id="29465-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="29465-153">Au lieu de cela, le client envoie une demande de publication et définit l’en-tête X-HTTP-Method-override sur la méthode souhaitée.</span><span class="sxs-lookup"><span data-stu-id="29465-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="29465-154">Exemple :</span><span class="sxs-lookup"><span data-stu-id="29465-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="29465-155">Voici un gestionnaire de messages qui ajoute la prise en charge de X-HTTP-Method-override :</span><span class="sxs-lookup"><span data-stu-id="29465-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="29465-156">Dans la méthode **SendAsync** , le gestionnaire vérifie si le message de demande est une demande postérieure et s’il contient l’en-tête X-http-Method-override.</span><span class="sxs-lookup"><span data-stu-id="29465-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="29465-157">Si c’est le cas, il valide la valeur d’en-tête, puis modifie la méthode de demande.</span><span class="sxs-lookup"><span data-stu-id="29465-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="29465-158">Enfin, le gestionnaire appelle `base.SendAsync` pour passer le message au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="29465-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="29465-159">Lorsque la demande atteint la classe **HttpControllerDispatcher** , **HttpControllerDispatcher** route la demande en fonction de la méthode de demande mise à jour.</span><span class="sxs-lookup"><span data-stu-id="29465-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="29465-160">Exemple : ajout d’un en-tête de réponse personnalisé</span><span class="sxs-lookup"><span data-stu-id="29465-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="29465-161">Voici un gestionnaire de messages qui ajoute un en-tête personnalisé à chaque message de réponse :</span><span class="sxs-lookup"><span data-stu-id="29465-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="29465-162">Tout d’abord, le gestionnaire appelle `base.SendAsync` pour transmettre la demande au gestionnaire de messages internes.</span><span class="sxs-lookup"><span data-stu-id="29465-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="29465-163">Le gestionnaire interne retourne un message de réponse, mais il le fait de façon asynchrone à l’aide d’une **tâche&lt;t&gt;** objet.</span><span class="sxs-lookup"><span data-stu-id="29465-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="29465-164">Le message de réponse n’est pas disponible tant que `base.SendAsync` ne se termine pas de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="29465-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="29465-165">Cet exemple utilise le mot clé **await** pour exécuter le travail de façon asynchrone une fois `SendAsync` terminé.</span><span class="sxs-lookup"><span data-stu-id="29465-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="29465-166">Si vous ciblez .NET Framework 4,0, utilisez la **tâche**&lt;t&gt; **. Méthode ContinueWith** :</span><span class="sxs-lookup"><span data-stu-id="29465-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="29465-167">Exemple : recherche d’une clé API</span><span class="sxs-lookup"><span data-stu-id="29465-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="29465-168">Certains services Web requièrent que les clients incluent une clé API dans leur demande.</span><span class="sxs-lookup"><span data-stu-id="29465-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="29465-169">L’exemple suivant montre comment un gestionnaire de messages peut vérifier les demandes pour une clé API valide :</span><span class="sxs-lookup"><span data-stu-id="29465-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="29465-170">Ce gestionnaire recherche la clé API dans la chaîne de requête d’URI.</span><span class="sxs-lookup"><span data-stu-id="29465-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="29465-171">(Pour cet exemple, nous partons du principe que la clé est une chaîne statique.</span><span class="sxs-lookup"><span data-stu-id="29465-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="29465-172">Une implémentation réelle utiliserait probablement une validation plus complexe.) Si la chaîne de requête contient la clé, le gestionnaire transmet la requête au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="29465-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="29465-173">Si la demande n’a pas de clé valide, le gestionnaire crée un message de réponse avec l’état 403, interdit.</span><span class="sxs-lookup"><span data-stu-id="29465-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="29465-174">Dans ce cas, le gestionnaire n’appelle pas `base.SendAsync`, donc le gestionnaire interne ne reçoit jamais la demande et le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="29465-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="29465-175">Par conséquent, le contrôleur peut supposer que toutes les demandes entrantes ont une clé API valide.</span><span class="sxs-lookup"><span data-stu-id="29465-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="29465-176">Si la clé d’API s’applique uniquement à certaines actions de contrôleur, envisagez d’utiliser un filtre d’action au lieu d’un gestionnaire de messages.</span><span class="sxs-lookup"><span data-stu-id="29465-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="29465-177">Les filtres d’action s’exécutent après l’exécution du routage URI.</span><span class="sxs-lookup"><span data-stu-id="29465-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="29465-178">Gestionnaires de messages par itinéraire</span><span class="sxs-lookup"><span data-stu-id="29465-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="29465-179">Les gestionnaires de la collection **HttpConfiguration. MessageHandlers** s’appliquent globalement.</span><span class="sxs-lookup"><span data-stu-id="29465-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="29465-180">Vous pouvez également ajouter un gestionnaire de messages à un itinéraire spécifique lorsque vous définissez l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="29465-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="29465-181">Dans cet exemple, si l’URI de la demande correspond à « route2 », la demande est distribuée à `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="29465-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="29465-182">Le diagramme suivant illustre le pipeline pour ces deux itinéraires :</span><span class="sxs-lookup"><span data-stu-id="29465-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="29465-183">Notez que `MessageHandler2` remplace le **HttpControllerDispatcher**par défaut.</span><span class="sxs-lookup"><span data-stu-id="29465-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="29465-184">Dans cet exemple, `MessageHandler2` crée la réponse, et les demandes qui correspondent à « route2 » n’accèdent jamais à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="29465-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="29465-185">Cela vous permet de remplacer l’ensemble du mécanisme du contrôleur d’API Web par votre propre point de terminaison personnalisé.</span><span class="sxs-lookup"><span data-stu-id="29465-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="29465-186">En guise d’alternative, un gestionnaire de messages par itinéraire peut déléguer à **HttpControllerDispatcher**, qui est ensuite distribué à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="29465-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="29465-187">Le code suivant montre comment configurer cet itinéraire :</span><span class="sxs-lookup"><span data-stu-id="29465-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
