---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routage dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557609"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="c4828-102">Routing in ASP.NET Web API (Routage dans l’API Web ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="c4828-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="c4828-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4828-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c4828-104">Cet article explique comment API Web ASP.NET achemine les requêtes HTTP vers les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="c4828-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="c4828-105">Si vous êtes familiarisé avec ASP.NET MVC, le routage de l’API Web est très similaire au routage MVC.</span><span class="sxs-lookup"><span data-stu-id="c4828-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="c4828-106">La principale différence est que l’API Web utilise le verbe HTTP, et non le chemin d’accès de l’URI, pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="c4828-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="c4828-107">Vous pouvez également utiliser le routage de type MVC dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="c4828-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="c4828-108">Cet article ne suppose aucune connaissance de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c4828-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="c4828-109">Tables de routage</span><span class="sxs-lookup"><span data-stu-id="c4828-109">Routing Tables</span></span>

<span data-ttu-id="c4828-110">Dans API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes http.</span><span class="sxs-lookup"><span data-stu-id="c4828-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="c4828-111">Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement des *actions*.</span><span class="sxs-lookup"><span data-stu-id="c4828-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="c4828-112">Lorsque l’infrastructure de l’API Web reçoit une demande, elle route la demande vers une action.</span><span class="sxs-lookup"><span data-stu-id="c4828-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="c4828-113">Pour déterminer l’action à appeler, l’infrastructure utilise une *table de routage*.</span><span class="sxs-lookup"><span data-stu-id="c4828-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="c4828-114">Le modèle de projet Visual Studio pour l’API Web crée un itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="c4828-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="c4828-115">Cet itinéraire est défini dans le fichier *WebApiConfig.cs* , qui est placé dans l' *application\_* répertoire de démarrage :</span><span class="sxs-lookup"><span data-stu-id="c4828-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="c4828-116">Pour plus d’informations sur la classe `WebApiConfig`, consultez [Configuration des API Web ASP.net](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c4828-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="c4828-117">Si vous auto-hébergez une API Web, vous devez définir la table de routage directement sur l’objet `HttpSelfHostConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="c4828-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="c4828-118">Pour plus d’informations, consultez [auto-host a Web API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c4828-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="c4828-119">Chaque entrée de la table de routage contient un *modèle*de routage.</span><span class="sxs-lookup"><span data-stu-id="c4828-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="c4828-120">Le modèle de routage par défaut pour l’API Web est &quot;API/{Controller}/{ID}&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4828-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="c4828-121">Dans ce modèle, &quot;&quot; d’API est un segment de chemin d’accès littéral, et {Controller} et {ID} sont des variables d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="c4828-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="c4828-122">Lorsque l’infrastructure de l’API Web reçoit une requête HTTP, elle tente de faire correspondre l’URI à l’un des modèles de route de la table de routage.</span><span class="sxs-lookup"><span data-stu-id="c4828-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="c4828-123">Si aucun itinéraire ne correspond, le client reçoit une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="c4828-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="c4828-124">Par exemple, les URI suivants correspondent à l’itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="c4828-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="c4828-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="c4828-125">/api/contacts</span></span>
- <span data-ttu-id="c4828-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="c4828-126">/api/contacts/1</span></span>
- <span data-ttu-id="c4828-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="c4828-127">/api/products/gizmo1</span></span>

<span data-ttu-id="c4828-128">Toutefois, l’URI suivant ne correspond pas, car il ne dispose pas de l’API &quot;&quot; segment :</span><span class="sxs-lookup"><span data-stu-id="c4828-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="c4828-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="c4828-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="c4828-130">La raison de l’utilisation de « API » dans l’itinéraire consiste à éviter les collisions avec le routage ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c4828-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="c4828-131">De cette façon, vous pouvez avoir &quot;/contacts&quot; accéder à un contrôleur MVC et &quot;/API/contacts&quot; accéder à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="c4828-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="c4828-132">Bien entendu, si vous n’aimez pas cette Convention, vous pouvez modifier la table de routage par défaut.</span><span class="sxs-lookup"><span data-stu-id="c4828-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="c4828-133">Une fois qu’un itinéraire correspondant est trouvé, l’API Web sélectionne le contrôleur et l’action :</span><span class="sxs-lookup"><span data-stu-id="c4828-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="c4828-134">Pour trouver le contrôleur, l’API Web ajoute &quot;contrôleur&quot; à la valeur de la variable *{Controller}* .</span><span class="sxs-lookup"><span data-stu-id="c4828-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="c4828-135">Pour trouver l’action, l’API Web examine le verbe HTTP, puis recherche une action dont le nom commence par ce nom de verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4828-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="c4828-136">Par exemple, avec une demande d’accès, l’API Web recherche une action portant le préfixe &quot;obtenir&quot;, par exemple &quot;GetContact&quot; ou &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4828-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="c4828-137">Cette Convention s’applique uniquement aux verbes d’extraction, de publication, de placement, de suppression, d’en-tête, d’OPTIONS et de correctif.</span><span class="sxs-lookup"><span data-stu-id="c4828-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="c4828-138">Vous pouvez activer d’autres verbes HTTP à l’aide d’attributs sur votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c4828-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="c4828-139">Nous verrons un exemple de ce qui est plus tard.</span><span class="sxs-lookup"><span data-stu-id="c4828-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="c4828-140">Les autres variables d’espace réservé dans le modèle de routage, telles que *{ID},* sont mappées aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="c4828-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="c4828-141">Intéressons-nous à un exemple.</span><span class="sxs-lookup"><span data-stu-id="c4828-141">Let's look at an example.</span></span> <span data-ttu-id="c4828-142">Supposons que vous définissiez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="c4828-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="c4828-143">Voici quelques requêtes HTTP possibles, ainsi que l’action qui est appelée pour chaque :</span><span class="sxs-lookup"><span data-stu-id="c4828-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="c4828-144">Verbe HTTP</span><span class="sxs-lookup"><span data-stu-id="c4828-144">HTTP Verb</span></span> | <span data-ttu-id="c4828-145">Chemin de l’URI</span><span class="sxs-lookup"><span data-stu-id="c4828-145">URI Path</span></span> | <span data-ttu-id="c4828-146">Action</span><span class="sxs-lookup"><span data-stu-id="c4828-146">Action</span></span> | <span data-ttu-id="c4828-147">Paramètre</span><span class="sxs-lookup"><span data-stu-id="c4828-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c4828-148">GET</span><span class="sxs-lookup"><span data-stu-id="c4828-148">GET</span></span> | <span data-ttu-id="c4828-149">API/produits</span><span class="sxs-lookup"><span data-stu-id="c4828-149">api/products</span></span> | <span data-ttu-id="c4828-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="c4828-150">GetAllProducts</span></span> | <span data-ttu-id="c4828-151">*None*</span><span class="sxs-lookup"><span data-stu-id="c4828-151">*(none)*</span></span> |
| <span data-ttu-id="c4828-152">GET</span><span class="sxs-lookup"><span data-stu-id="c4828-152">GET</span></span> | <span data-ttu-id="c4828-153">API/produits/4</span><span class="sxs-lookup"><span data-stu-id="c4828-153">api/products/4</span></span> | <span data-ttu-id="c4828-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="c4828-154">GetProductById</span></span> | <span data-ttu-id="c4828-155">4</span><span class="sxs-lookup"><span data-stu-id="c4828-155">4</span></span> |
| <span data-ttu-id="c4828-156">Suppression</span><span class="sxs-lookup"><span data-stu-id="c4828-156">DELETE</span></span> | <span data-ttu-id="c4828-157">API/produits/4</span><span class="sxs-lookup"><span data-stu-id="c4828-157">api/products/4</span></span> | <span data-ttu-id="c4828-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="c4828-158">DeleteProduct</span></span> | <span data-ttu-id="c4828-159">4</span><span class="sxs-lookup"><span data-stu-id="c4828-159">4</span></span> |
| <span data-ttu-id="c4828-160">POST</span><span class="sxs-lookup"><span data-stu-id="c4828-160">POST</span></span> | <span data-ttu-id="c4828-161">API/produits</span><span class="sxs-lookup"><span data-stu-id="c4828-161">api/products</span></span> | <span data-ttu-id="c4828-162">*(aucune correspondance)*</span><span class="sxs-lookup"><span data-stu-id="c4828-162">*(no match)*</span></span> |  |

<span data-ttu-id="c4828-163">Notez que le segment *{ID}* de l’URI, s’il est présent, est mappé au paramètre *ID* de l’action.</span><span class="sxs-lookup"><span data-stu-id="c4828-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="c4828-164">Dans cet exemple, le contrôleur définit deux méthodes d’extraction, l’une avec un paramètre *ID* et l’autre sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="c4828-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="c4828-165">Notez également que la demande de publication échouera, car le contrôleur ne définit pas une méthode &quot;de publication...&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4828-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="c4828-166">Variations de routage</span><span class="sxs-lookup"><span data-stu-id="c4828-166">Routing Variations</span></span>

<span data-ttu-id="c4828-167">La section précédente a décrit le mécanisme de routage de base pour API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4828-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="c4828-168">Cette section décrit certaines variations.</span><span class="sxs-lookup"><span data-stu-id="c4828-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="c4828-169">verbes HTTP</span><span class="sxs-lookup"><span data-stu-id="c4828-169">HTTP verbs</span></span>

<span data-ttu-id="c4828-170">Au lieu d’utiliser la Convention d’affectation de noms pour les verbes HTTP, vous pouvez spécifier explicitement le verbe HTTP pour une action en décorant la méthode d’action avec l’un des attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="c4828-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="c4828-171">Dans l’exemple suivant, la méthode `FindProduct` est mappée aux demandes d’extraction :</span><span class="sxs-lookup"><span data-stu-id="c4828-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="c4828-172">Pour autoriser plusieurs verbes HTTP pour une action, ou pour autoriser les verbes HTTP autres que obtenir, PUT, poster, supprimer, HEAD, OPTIONS et PATCH, utilisez l’attribut `[AcceptVerbs]`, qui prend une liste de verbes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4828-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="c4828-173">Routage par nom d’action</span><span class="sxs-lookup"><span data-stu-id="c4828-173">Routing by Action Name</span></span>

<span data-ttu-id="c4828-174">Avec le modèle de routage par défaut, l’API Web utilise le verbe HTTP pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="c4828-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="c4828-175">Toutefois, vous pouvez également créer un itinéraire où le nom de l’action est inclus dans l’URI :</span><span class="sxs-lookup"><span data-stu-id="c4828-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="c4828-176">Dans ce modèle de routage, le paramètre *{action}* nomme la méthode d’action sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c4828-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="c4828-177">Avec ce style de routage, utilisez des attributs pour spécifier les verbes HTTP autorisés.</span><span class="sxs-lookup"><span data-stu-id="c4828-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="c4828-178">Par exemple, supposons que votre contrôleur ait la méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="c4828-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="c4828-179">Dans ce cas, une requête d’extraction pour « API/Products/Details/1 » est mappée à la méthode `Details`.</span><span class="sxs-lookup"><span data-stu-id="c4828-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="c4828-180">Ce style de routage est similaire à ASP.NET MVC et peut convenir à une API de style RPC.</span><span class="sxs-lookup"><span data-stu-id="c4828-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="c4828-181">Vous pouvez remplacer le nom de l’action à l’aide de l’attribut `[ActionName]`.</span><span class="sxs-lookup"><span data-stu-id="c4828-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="c4828-182">Dans l’exemple suivant, deux actions sont mappées à &quot;API/Products/thumbnail/*ID*. L’un prend en charge l’extraction et l’autre prend en charge la publication :</span><span class="sxs-lookup"><span data-stu-id="c4828-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="c4828-183">Non-actions</span><span class="sxs-lookup"><span data-stu-id="c4828-183">Non-Actions</span></span>

<span data-ttu-id="c4828-184">Pour empêcher une méthode d’être appelée en tant qu’action, utilisez l’attribut `[NonAction]`.</span><span class="sxs-lookup"><span data-stu-id="c4828-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="c4828-185">Cela signale à l’infrastructure que la méthode n’est pas une action, même si elle correspondrait autrement aux règles de routage.</span><span class="sxs-lookup"><span data-stu-id="c4828-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="c4828-186">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c4828-186">Further Reading</span></span>

<span data-ttu-id="c4828-187">Cette rubrique a fourni une vue d’ensemble du routage.</span><span class="sxs-lookup"><span data-stu-id="c4828-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="c4828-188">Pour plus d’informations, consultez [routage et sélection des actions](routing-and-action-selection.md), qui décrit exactement comment l’infrastructure correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à appeler.</span><span class="sxs-lookup"><span data-stu-id="c4828-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
