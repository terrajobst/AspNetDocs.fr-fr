---
uid: web-api/overview/advanced/http-message-handlers
title: Gestionnaires de messages HTTP dans l’API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Une vue d’ensemble de gestionnaires de messages HTTP dans l’API Web ASP.NET pour ASP.NET 4.x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115546"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="57b5a-103">Gestionnaires de messages HTTP dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="57b5a-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="57b5a-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="57b5a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="57b5a-105">Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et renvoie une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="57b5a-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="57b5a-106">Gestionnaires de messages de dérivent de la classe abstraite **HttpMessageHandler** classe.</span><span class="sxs-lookup"><span data-stu-id="57b5a-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="57b5a-107">En règle générale, une série de gestionnaires de messages sont chaînées ensemble.</span><span class="sxs-lookup"><span data-stu-id="57b5a-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="57b5a-108">Le premier gestionnaire reçoit une requête HTTP effectue un traitement et donne la demande au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="57b5a-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="57b5a-109">À un moment donné, la réponse est créée et remonte la chaîne.</span><span class="sxs-lookup"><span data-stu-id="57b5a-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="57b5a-110">Ce modèle est appelé un *délégation* gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="57b5a-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="57b5a-111">Gestionnaires de messages côté serveur</span><span class="sxs-lookup"><span data-stu-id="57b5a-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="57b5a-112">Sur le côté serveur, le pipeline API Web utilise des gestionnaires de messages intégrés :</span><span class="sxs-lookup"><span data-stu-id="57b5a-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="57b5a-113">**Ftpserver** Obtient la demande à partir de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="57b5a-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="57b5a-114">**HttpRoutingDispatcher** distribue la demande en fonction de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="57b5a-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="57b5a-115">**HttpControllerDispatcher** envoie la demande à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="57b5a-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="57b5a-116">Vous pouvez ajouter des gestionnaires personnalisés au pipeline.</span><span class="sxs-lookup"><span data-stu-id="57b5a-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="57b5a-117">Gestionnaires de messages conviennent pour les problèmes transversaux qui opèrent au niveau de HTTP messages (plutôt que les actions de contrôleur).</span><span class="sxs-lookup"><span data-stu-id="57b5a-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="57b5a-118">Par exemple, un gestionnaire de messages peut :</span><span class="sxs-lookup"><span data-stu-id="57b5a-118">For example, a message handler might:</span></span>

- <span data-ttu-id="57b5a-119">Lire ou modifier les en-têtes de demande.</span><span class="sxs-lookup"><span data-stu-id="57b5a-119">Read or modify request headers.</span></span>
- <span data-ttu-id="57b5a-120">Ajouter un en-tête de réponse aux réponses.</span><span class="sxs-lookup"><span data-stu-id="57b5a-120">Add a response header to responses.</span></span>
- <span data-ttu-id="57b5a-121">Valider les demandes avant qu’ils atteignent le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="57b5a-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="57b5a-122">Ce diagramme illustre deux gestionnaires personnalisés insérés dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="57b5a-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="57b5a-123">Du côté client, HttpClient utilise également des gestionnaires de messages.</span><span class="sxs-lookup"><span data-stu-id="57b5a-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="57b5a-124">Pour plus d’informations, consultez [gestionnaires de messages HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="57b5a-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="57b5a-125">Gestionnaires de messages personnalisés</span><span class="sxs-lookup"><span data-stu-id="57b5a-125">Custom Message Handlers</span></span>

<span data-ttu-id="57b5a-126">Pour écrire un gestionnaire de messages personnalisés, dérivez de **System.Net.Http.DelegatingHandler** et remplacer le **SendAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="57b5a-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="57b5a-127">Cette méthode a la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="57b5a-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="57b5a-128">La méthode accepte un **HttpRequestMessage** comme entrée et retourne de façon asynchrone un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="57b5a-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="57b5a-129">Une implémentation classique effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="57b5a-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="57b5a-130">Traiter le message de demande.</span><span class="sxs-lookup"><span data-stu-id="57b5a-130">Process the request message.</span></span>
2. <span data-ttu-id="57b5a-131">Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="57b5a-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="57b5a-132">Le gestionnaire interne retourne un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="57b5a-132">The inner handler returns a response message.</span></span> <span data-ttu-id="57b5a-133">(Cette étape est asynchrone).</span><span class="sxs-lookup"><span data-stu-id="57b5a-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="57b5a-134">Traiter la réponse et le renvoyer à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="57b5a-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="57b5a-135">Voici un exemple simple :</span><span class="sxs-lookup"><span data-stu-id="57b5a-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="57b5a-136">L’appel à `base.SendAsync` est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="57b5a-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="57b5a-137">Si le gestionnaire effectue tout travail après cet appel, utilisez le **await** mot clé, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="57b5a-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="57b5a-138">Un gestionnaire de délégation peut également ignorer le gestionnaire interne et créer directement la réponse :</span><span class="sxs-lookup"><span data-stu-id="57b5a-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="57b5a-139">Si une délégation de gestionnaire crée la réponse sans appeler `base.SendAsync`, la requête ignore le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="57b5a-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="57b5a-140">Cela peut être utile pour un gestionnaire qui valide la demande (création d’une réponse d’erreur).</span><span class="sxs-lookup"><span data-stu-id="57b5a-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="57b5a-141">Ajout d’un gestionnaire au Pipeline</span><span class="sxs-lookup"><span data-stu-id="57b5a-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="57b5a-142">Pour ajouter un gestionnaire de messages côté serveur, ajoutez le gestionnaire à la **HttpConfiguration.MessageHandlers** collection.</span><span class="sxs-lookup"><span data-stu-id="57b5a-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="57b5a-143">Si vous avez utilisé le modèle « Application Web ASP.NET MVC 4 » pour créer le projet, vous pouvez effectuer ceci dans le **WebApiConfig** classe :</span><span class="sxs-lookup"><span data-stu-id="57b5a-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="57b5a-144">Gestionnaires de messages sont appelés dans l’ordre dans lequel ils apparaissent dans **MessageHandlers** collection.</span><span class="sxs-lookup"><span data-stu-id="57b5a-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="57b5a-145">Car ils sont imbriqués, le message de réponse sont transmises dans l’autre direction.</span><span class="sxs-lookup"><span data-stu-id="57b5a-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="57b5a-146">Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="57b5a-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="57b5a-147">Notez que vous n’avez pas besoin de définir des gestionnaires internes ; l’infrastructure API Web connecte automatiquement les gestionnaires de messages.</span><span class="sxs-lookup"><span data-stu-id="57b5a-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="57b5a-148">Si vous êtes [auto-hébergement](../older-versions/self-host-a-web-api.md), créez une instance de la **HttpSelfHostConfiguration** classe et ajoutez les gestionnaires pour le **MessageHandlers** collection.</span><span class="sxs-lookup"><span data-stu-id="57b5a-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="57b5a-149">Maintenant nous allons examiner quelques exemples de gestionnaires de messages personnalisés.</span><span class="sxs-lookup"><span data-stu-id="57b5a-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="57b5a-150">Exemple : X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="57b5a-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="57b5a-151">X-HTTP-Method-Override est un en-tête HTTP non standard.</span><span class="sxs-lookup"><span data-stu-id="57b5a-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="57b5a-152">Il est conçu pour les clients qui ne peuvent pas envoyer de certains types de demande HTTP, telles que PUT ou DELETE.</span><span class="sxs-lookup"><span data-stu-id="57b5a-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="57b5a-153">Au lieu de cela, le client envoie une demande POST et définit l’en-tête X-HTTP-Method-Override à la méthode de votre choix.</span><span class="sxs-lookup"><span data-stu-id="57b5a-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="57b5a-154">Exemple :</span><span class="sxs-lookup"><span data-stu-id="57b5a-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="57b5a-155">Voici un gestionnaire de messages qui ajoute la prise en charge de X-HTTP-Method-Override :</span><span class="sxs-lookup"><span data-stu-id="57b5a-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="57b5a-156">Dans le **SendAsync** (méthode), le gestionnaire vérifie si le message de demande est une demande POST, et s’il contient l’en-tête X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="57b5a-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="57b5a-157">Dans ce cas, il valide la valeur d’en-tête, puis modifie la méthode de demande.</span><span class="sxs-lookup"><span data-stu-id="57b5a-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="57b5a-158">Enfin, le gestionnaire appelle `base.SendAsync` pour transférer le message au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="57b5a-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="57b5a-159">Lorsque la demande atteint le **HttpControllerDispatcher** (classe), **HttpControllerDispatcher** achemine la demande selon la méthode de demande de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="57b5a-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="57b5a-160">Exemple : Ajout d’un en-tête de réponse personnalisée</span><span class="sxs-lookup"><span data-stu-id="57b5a-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="57b5a-161">Voici un gestionnaire de messages qui ajoute un en-tête personnalisé à chaque message de réponse :</span><span class="sxs-lookup"><span data-stu-id="57b5a-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="57b5a-162">Tout d’abord, le gestionnaire appelle `base.SendAsync` de transmettre la demande au Gestionnaire de messages interne.</span><span class="sxs-lookup"><span data-stu-id="57b5a-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="57b5a-163">Le gestionnaire interne retourne un message de réponse, mais il le fait à l’aide de manière asynchrone un **tâche&lt;T&gt;**  objet.</span><span class="sxs-lookup"><span data-stu-id="57b5a-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="57b5a-164">Le message de réponse n’est pas disponible tant que `base.SendAsync` se termine de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="57b5a-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="57b5a-165">Cet exemple utilise le **await** mot clé pour effectuer le travail asynchrone après `SendAsync` se termine.</span><span class="sxs-lookup"><span data-stu-id="57b5a-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="57b5a-166">Si vous ciblez .NET Framework 4.0, utilisez le **tâche**&lt;T&gt;**. ContinueWith** méthode :</span><span class="sxs-lookup"><span data-stu-id="57b5a-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="57b5a-167">Exemple : Vérification pour obtenir une clé API</span><span class="sxs-lookup"><span data-stu-id="57b5a-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="57b5a-168">Certains services web nécessitent des clients à inclure une clé API dans leur demande.</span><span class="sxs-lookup"><span data-stu-id="57b5a-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="57b5a-169">L’exemple suivant montre comment un gestionnaire de messages peut vérifier les demandes pour une clé API valide :</span><span class="sxs-lookup"><span data-stu-id="57b5a-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="57b5a-170">Ce gestionnaire recherche la clé API dans la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="57b5a-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="57b5a-171">(Pour cet exemple, nous partons du principe que la clé est une chaîne statique.</span><span class="sxs-lookup"><span data-stu-id="57b5a-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="57b5a-172">Une implémentation réelle utiliseriez sans doute une validation plus complexe.) Si la chaîne de requête contient la clé, le gestionnaire transmet la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="57b5a-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="57b5a-173">Si la demande n’a pas d’une clé valide, le gestionnaire crée un message de réponse avec état 403, interdit.</span><span class="sxs-lookup"><span data-stu-id="57b5a-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="57b5a-174">Dans ce cas, le gestionnaire n’appelle pas `base.SendAsync`, par conséquent, le gestionnaire interne ne reçoit jamais de la demande, de même le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="57b5a-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="57b5a-175">Par conséquent, le contrôleur peut supposer que toutes les demandes entrantes ont une clé API valide.</span><span class="sxs-lookup"><span data-stu-id="57b5a-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="57b5a-176">Si la clé d’API s’applique uniquement à certaines actions de contrôleur, envisagez d’utiliser un filtre d’action au lieu d’un gestionnaire de messages.</span><span class="sxs-lookup"><span data-stu-id="57b5a-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="57b5a-177">Filtres d’action exécutent après l’exécution de routage de l’URI.</span><span class="sxs-lookup"><span data-stu-id="57b5a-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="57b5a-178">Gestionnaires de messages d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="57b5a-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="57b5a-179">Gestionnaires dans le **HttpConfiguration.MessageHandlers** collection s’appliquent globalement.</span><span class="sxs-lookup"><span data-stu-id="57b5a-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="57b5a-180">Ou bien, vous pouvez ajouter un gestionnaire de messages à un itinéraire spécifique lorsque vous définissez l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="57b5a-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="57b5a-181">Dans cet exemple, si l’URI de demande correspond à « Route2 », la demande est distribuée à `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="57b5a-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="57b5a-182">Le diagramme suivant illustre le pipeline pour ces deux itinéraires :</span><span class="sxs-lookup"><span data-stu-id="57b5a-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="57b5a-183">Notez que `MessageHandler2` remplace la valeur par défaut **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="57b5a-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="57b5a-184">Dans cet exemple, `MessageHandler2` crée la réponse, et les requêtes correspondant à « Route2 » accédez jamais à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="57b5a-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="57b5a-185">Cela vous permet de remplacer l’ensemble du mécanisme contrôleur API Web avec votre propre point de terminaison personnalisé.</span><span class="sxs-lookup"><span data-stu-id="57b5a-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="57b5a-186">Vous pouvez également un gestionnaire de messages par route peut déléguer au **HttpControllerDispatcher**, qui ensuite envoie à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="57b5a-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="57b5a-187">Le code suivant montre comment configurer cet itinéraire :</span><span class="sxs-lookup"><span data-stu-id="57b5a-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
