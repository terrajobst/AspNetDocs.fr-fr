---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routage des attributs dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554984"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="71cba-102">Routage des attributs dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="71cba-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="71cba-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="71cba-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="71cba-104">Le *routage* correspond à la façon dont l’API Web correspond à un URI à une action.</span><span class="sxs-lookup"><span data-stu-id="71cba-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="71cba-105">L’API Web 2 prend en charge un nouveau type de routage, appelé *routage d’attribut*.</span><span class="sxs-lookup"><span data-stu-id="71cba-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="71cba-106">Comme son nom l’indique, le routage d’attributs utilise des attributs pour définir des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="71cba-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="71cba-107">Le routage des attributs vous donne davantage de contrôle sur les URI de votre API Web.</span><span class="sxs-lookup"><span data-stu-id="71cba-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="71cba-108">Par exemple, vous pouvez facilement créer des URI qui décrivent des hiérarchies de ressources.</span><span class="sxs-lookup"><span data-stu-id="71cba-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="71cba-109">Le style de routage précédent, appelé routage basé sur des conventions, est toujours entièrement pris en charge.</span><span class="sxs-lookup"><span data-stu-id="71cba-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="71cba-110">En fait, vous pouvez combiner les deux techniques dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="71cba-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="71cba-111">Cette rubrique montre comment activer le routage d’attributs et décrit les différentes options de routage des attributs.</span><span class="sxs-lookup"><span data-stu-id="71cba-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="71cba-112">Pour obtenir un didacticiel de bout en bout qui utilise le routage d’attributs, consultez [créer une API REST avec routage d’attributs dans Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="71cba-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71cba-113">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="71cba-113">Prerequisites</span></span>

<span data-ttu-id="71cba-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Édition Community, Professional ou Enterprise</span><span class="sxs-lookup"><span data-stu-id="71cba-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="71cba-115">Vous pouvez également utiliser le gestionnaire de package NuGet pour installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="71cba-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="71cba-116">Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="71cba-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="71cba-117">Entrez la commande suivante dans la fenêtre console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="71cba-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="71cba-118">Pourquoi le routage des attributs ?</span><span class="sxs-lookup"><span data-stu-id="71cba-118">Why Attribute Routing?</span></span>

<span data-ttu-id="71cba-119">La première version de l’API Web a utilisé le routage *basé sur des conventions* .</span><span class="sxs-lookup"><span data-stu-id="71cba-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="71cba-120">Dans ce type de routage, vous définissez un ou plusieurs modèles de routage, qui sont fondamentalement des chaînes paramétrables.</span><span class="sxs-lookup"><span data-stu-id="71cba-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="71cba-121">Lorsque l’infrastructure reçoit une demande, elle correspond à l’URI par rapport au modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="71cba-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="71cba-122">(Pour plus d’informations sur le routage basé sur une convention, consultez [routage dans API Web ASP.net](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="71cba-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="71cba-123">L’un des avantages du routage basé sur des conventions est que les modèles sont définis dans un emplacement unique et que les règles de routage sont appliquées de manière cohérente sur tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="71cba-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="71cba-124">Malheureusement, le routage basé sur des conventions rend difficile la prise en charge de certains modèles d’URI qui sont courants dans les API RESTful.</span><span class="sxs-lookup"><span data-stu-id="71cba-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="71cba-125">Par exemple, les ressources contiennent souvent des ressources enfants : les clients ont des commandes, des films ont des acteurs, des livres ont des auteurs, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="71cba-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="71cba-126">Il est naturel de créer des URI qui reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="71cba-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="71cba-127">Ce type d’URI est difficile à créer à l’aide du routage basé sur des conventions.</span><span class="sxs-lookup"><span data-stu-id="71cba-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="71cba-128">Bien que cela puisse être fait, les résultats ne sont pas correctement mis à l’échelle si vous avez plusieurs contrôleurs ou types de ressources.</span><span class="sxs-lookup"><span data-stu-id="71cba-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="71cba-129">Avec le routage d’attributs, il est trivial de définir un itinéraire pour cet URI.</span><span class="sxs-lookup"><span data-stu-id="71cba-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="71cba-130">Il vous suffit d’ajouter un attribut à l’action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="71cba-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="71cba-131">Voici d’autres modèles qui facilitent le routage d’attributs.</span><span class="sxs-lookup"><span data-stu-id="71cba-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="71cba-132">**Contrôle de version des API**</span><span class="sxs-lookup"><span data-stu-id="71cba-132">**API versioning**</span></span>

<span data-ttu-id="71cba-133">Dans cet exemple, « /API/v1/Products » est routé vers un autre contrôleur que « /API/v2/Products ».</span><span class="sxs-lookup"><span data-stu-id="71cba-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="71cba-134">**Segments d’URI surchargés**</span><span class="sxs-lookup"><span data-stu-id="71cba-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="71cba-135">Dans cet exemple, « 1 » est un numéro d’ordre, mais « Pending » est mappé à une collection.</span><span class="sxs-lookup"><span data-stu-id="71cba-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="71cba-136">**Types de paramètres multiples**</span><span class="sxs-lookup"><span data-stu-id="71cba-136">**Multiple parameter types**</span></span>

<span data-ttu-id="71cba-137">Dans cet exemple, « 1 » est un numéro d’ordre, mais « 2013/06/16 » spécifie une date.</span><span class="sxs-lookup"><span data-stu-id="71cba-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="71cba-138">Activation du routage des attributs</span><span class="sxs-lookup"><span data-stu-id="71cba-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="71cba-139">Pour activer le routage d’attribut, appelez **MapHttpAttributeRoutes** pendant la configuration.</span><span class="sxs-lookup"><span data-stu-id="71cba-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="71cba-140">Cette méthode d’extension est définie dans la classe **System. Web. http. HttpConfigurationExtensions** .</span><span class="sxs-lookup"><span data-stu-id="71cba-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="71cba-141">Le routage d’attribut peut être combiné avec [le routage basé sur des conventions](routing-in-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="71cba-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="71cba-142">Pour définir des itinéraires basés sur des conventions, appelez la méthode **MapHttpRoute** .</span><span class="sxs-lookup"><span data-stu-id="71cba-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="71cba-143">Pour plus d’informations sur la configuration de l’API Web, consultez [configuration de API Web ASP.NET 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="71cba-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="71cba-144">Remarque : migration à partir de l’API Web 1</span><span class="sxs-lookup"><span data-stu-id="71cba-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="71cba-145">Avant l’API Web 2, les modèles de projet d’API Web généraient du code comme celui-ci :</span><span class="sxs-lookup"><span data-stu-id="71cba-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="71cba-146">Si le routage des attributs est activé, ce code lèvera une exception.</span><span class="sxs-lookup"><span data-stu-id="71cba-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="71cba-147">Si vous mettez à niveau un projet d’API Web existant pour utiliser le routage d’attributs, veillez à mettre à jour ce code de configuration comme suit :</span><span class="sxs-lookup"><span data-stu-id="71cba-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="71cba-148">Pour plus d’informations, consultez Configuration de l' [API Web avec l’hébergement ASP.net](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="71cba-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="71cba-149">Ajouter des attributs de route</span><span class="sxs-lookup"><span data-stu-id="71cba-149">Adding Route Attributes</span></span>

<span data-ttu-id="71cba-150">Voici un exemple d’itinéraire défini à l’aide d’un attribut :</span><span class="sxs-lookup"><span data-stu-id="71cba-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="71cba-151">La chaîne &quot;clients/{customerId}/Orders&quot; est le modèle d’URI pour l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="71cba-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="71cba-152">L’API Web tente de faire correspondre l’URI de la demande au modèle.</span><span class="sxs-lookup"><span data-stu-id="71cba-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="71cba-153">Dans cet exemple, « Customers » et « Orders » sont des segments littéraux et « {customerId} » est un paramètre de variable.</span><span class="sxs-lookup"><span data-stu-id="71cba-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="71cba-154">Les URI suivants correspondent à ce modèle :</span><span class="sxs-lookup"><span data-stu-id="71cba-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="71cba-155">Vous pouvez restreindre la correspondance à l’aide de [contraintes](#constraints), décrites plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="71cba-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="71cba-156">Notez que le paramètre &quot;{customerId}&quot; dans le modèle de routage correspond au nom du paramètre *CustomerID* dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="71cba-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="71cba-157">Lorsque l’API Web appelle l’action du contrôleur, elle tente de lier les paramètres d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="71cba-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="71cba-158">Par exemple, si l’URI est `http://example.com/customers/1/orders`, l’API Web tente de lier la valeur « 1 » au paramètre *CustomerID* dans l’action.</span><span class="sxs-lookup"><span data-stu-id="71cba-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="71cba-159">Un modèle d’URI peut avoir plusieurs paramètres :</span><span class="sxs-lookup"><span data-stu-id="71cba-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="71cba-160">Toutes les méthodes de contrôleur qui n’ont pas d’attribut de routage utilisent le routage basé sur des conventions.</span><span class="sxs-lookup"><span data-stu-id="71cba-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="71cba-161">De cette façon, vous pouvez combiner les deux types de routage dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="71cba-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="71cba-162">HTTP Methods</span><span class="sxs-lookup"><span data-stu-id="71cba-162">HTTP Methods</span></span>

<span data-ttu-id="71cba-163">L’API Web sélectionne également des actions basées sur la méthode HTTP de la demande (obtenir, poster, etc.).</span><span class="sxs-lookup"><span data-stu-id="71cba-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="71cba-164">Par défaut, l’API Web recherche une correspondance ne respectant pas la casse avec le début du nom de la méthode du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="71cba-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="71cba-165">Par exemple, une méthode de contrôleur nommée `PutCustomers` correspond à une requête HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="71cba-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="71cba-166">Vous pouvez substituer cette Convention en décorant la méthode avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="71cba-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="71cba-167">**HttpDelete**</span><span class="sxs-lookup"><span data-stu-id="71cba-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="71cba-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="71cba-168">**[HttpGet]**</span></span>
- <span data-ttu-id="71cba-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="71cba-169">**[HttpHead]**</span></span>
- <span data-ttu-id="71cba-170">**HttpOptions**</span><span class="sxs-lookup"><span data-stu-id="71cba-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="71cba-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="71cba-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="71cba-172">**HttpPost**</span><span class="sxs-lookup"><span data-stu-id="71cba-172">**[HttpPost]**</span></span>
- <span data-ttu-id="71cba-173">**HttpPut**</span><span class="sxs-lookup"><span data-stu-id="71cba-173">**[HttpPut]**</span></span>

<span data-ttu-id="71cba-174">L’exemple suivant mappe la méthode CreateBook aux requêtes HTTP HTTP.</span><span class="sxs-lookup"><span data-stu-id="71cba-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="71cba-175">Pour toutes les autres méthodes HTTP, y compris les méthodes non standard, utilisez l’attribut **AcceptVerbs** , qui prend une liste de méthodes http.</span><span class="sxs-lookup"><span data-stu-id="71cba-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="71cba-176">Préfixes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="71cba-176">Route Prefixes</span></span>

<span data-ttu-id="71cba-177">Souvent, les routes d’un contrôleur commencent par le même préfixe.</span><span class="sxs-lookup"><span data-stu-id="71cba-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="71cba-178">Exemple :</span><span class="sxs-lookup"><span data-stu-id="71cba-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="71cba-179">Vous pouvez définir un préfixe commun pour un contrôleur entier à l’aide de l’attribut **[RoutePrefix]** :</span><span class="sxs-lookup"><span data-stu-id="71cba-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="71cba-180">Utilisez un tilde (~) sur l’attribut de méthode pour remplacer le préfixe d’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="71cba-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="71cba-181">Le préfixe d’itinéraire peut inclure des paramètres :</span><span class="sxs-lookup"><span data-stu-id="71cba-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="71cba-182">Contraintes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="71cba-182">Route Constraints</span></span>

<span data-ttu-id="71cba-183">Les contraintes de routage vous permettent de limiter la correspondance des paramètres dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="71cba-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="71cba-184">La syntaxe générale est &quot;{Parameter : Constraint}&quot;.</span><span class="sxs-lookup"><span data-stu-id="71cba-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="71cba-185">Exemple :</span><span class="sxs-lookup"><span data-stu-id="71cba-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="71cba-186">Ici, la première route est sélectionnée uniquement si l’ID de &quot;&quot; segment de l’URI est un entier.</span><span class="sxs-lookup"><span data-stu-id="71cba-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="71cba-187">Dans le cas contraire, le deuxième itinéraire est choisi.</span><span class="sxs-lookup"><span data-stu-id="71cba-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="71cba-188">Le tableau suivant répertorie les contraintes prises en charge.</span><span class="sxs-lookup"><span data-stu-id="71cba-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="71cba-189">Contrainte</span><span class="sxs-lookup"><span data-stu-id="71cba-189">Constraint</span></span> | <span data-ttu-id="71cba-190">Description</span><span class="sxs-lookup"><span data-stu-id="71cba-190">Description</span></span> | <span data-ttu-id="71cba-191">Exemple</span><span class="sxs-lookup"><span data-stu-id="71cba-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="71cba-192">alpha</span><span class="sxs-lookup"><span data-stu-id="71cba-192">alpha</span></span> | <span data-ttu-id="71cba-193">Correspond aux caractères de l’alphabet latin en majuscules ou en minuscules (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="71cba-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="71cba-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="71cba-194">{x:alpha}</span></span> |
| <span data-ttu-id="71cba-195">bool</span><span class="sxs-lookup"><span data-stu-id="71cba-195">bool</span></span> | <span data-ttu-id="71cba-196">Correspond à une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="71cba-196">Matches a Boolean value.</span></span> | <span data-ttu-id="71cba-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="71cba-197">{x:bool}</span></span> |
| <span data-ttu-id="71cba-198">datetime</span><span class="sxs-lookup"><span data-stu-id="71cba-198">datetime</span></span> | <span data-ttu-id="71cba-199">Correspond à une valeur **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="71cba-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="71cba-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="71cba-200">{x:datetime}</span></span> |
| <span data-ttu-id="71cba-201">decimal</span><span class="sxs-lookup"><span data-stu-id="71cba-201">decimal</span></span> | <span data-ttu-id="71cba-202">Correspond à une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="71cba-202">Matches a decimal value.</span></span> | <span data-ttu-id="71cba-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="71cba-203">{x:decimal}</span></span> |
| <span data-ttu-id="71cba-204">double</span><span class="sxs-lookup"><span data-stu-id="71cba-204">double</span></span> | <span data-ttu-id="71cba-205">Correspond à une valeur à virgule flottante 64 bits.</span><span class="sxs-lookup"><span data-stu-id="71cba-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="71cba-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="71cba-206">{x:double}</span></span> |
| <span data-ttu-id="71cba-207">float</span><span class="sxs-lookup"><span data-stu-id="71cba-207">float</span></span> | <span data-ttu-id="71cba-208">Correspond à une valeur à virgule flottante 32 bits.</span><span class="sxs-lookup"><span data-stu-id="71cba-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="71cba-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="71cba-209">{x:float}</span></span> |
| <span data-ttu-id="71cba-210">guid</span><span class="sxs-lookup"><span data-stu-id="71cba-210">guid</span></span> | <span data-ttu-id="71cba-211">Correspond à une valeur GUID.</span><span class="sxs-lookup"><span data-stu-id="71cba-211">Matches a GUID value.</span></span> | <span data-ttu-id="71cba-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="71cba-212">{x:guid}</span></span> |
| <span data-ttu-id="71cba-213">int</span><span class="sxs-lookup"><span data-stu-id="71cba-213">int</span></span> | <span data-ttu-id="71cba-214">Correspond à une valeur entière de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="71cba-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="71cba-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="71cba-215">{x:int}</span></span> |
| <span data-ttu-id="71cba-216">length</span><span class="sxs-lookup"><span data-stu-id="71cba-216">length</span></span> | <span data-ttu-id="71cba-217">Correspond à une chaîne avec la longueur spécifiée ou dans une plage de longueurs spécifiée.</span><span class="sxs-lookup"><span data-stu-id="71cba-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="71cba-218">{x :length (6)} {x :length (1, 20)}</span><span class="sxs-lookup"><span data-stu-id="71cba-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="71cba-219">long</span><span class="sxs-lookup"><span data-stu-id="71cba-219">long</span></span> | <span data-ttu-id="71cba-220">Correspond à une valeur entière de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="71cba-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="71cba-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="71cba-221">{x:long}</span></span> |
| <span data-ttu-id="71cba-222">max</span><span class="sxs-lookup"><span data-stu-id="71cba-222">max</span></span> | <span data-ttu-id="71cba-223">Correspond à un entier avec une valeur maximale.</span><span class="sxs-lookup"><span data-stu-id="71cba-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="71cba-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="71cba-224">{x:max(10)}</span></span> |
| <span data-ttu-id="71cba-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="71cba-225">maxlength</span></span> | <span data-ttu-id="71cba-226">Correspond à une chaîne d’une longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="71cba-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="71cba-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="71cba-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="71cba-228">min</span><span class="sxs-lookup"><span data-stu-id="71cba-228">min</span></span> | <span data-ttu-id="71cba-229">Correspond à un entier avec une valeur minimale.</span><span class="sxs-lookup"><span data-stu-id="71cba-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="71cba-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="71cba-230">{x:min(10)}</span></span> |
| <span data-ttu-id="71cba-231">minLength</span><span class="sxs-lookup"><span data-stu-id="71cba-231">minlength</span></span> | <span data-ttu-id="71cba-232">Correspond à une chaîne d’une longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="71cba-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="71cba-233">{x :minLength (10)}</span><span class="sxs-lookup"><span data-stu-id="71cba-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="71cba-234">range</span><span class="sxs-lookup"><span data-stu-id="71cba-234">range</span></span> | <span data-ttu-id="71cba-235">Correspond à un entier dans une plage de valeurs.</span><span class="sxs-lookup"><span data-stu-id="71cba-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="71cba-236">{x :Range (10, 50)}</span><span class="sxs-lookup"><span data-stu-id="71cba-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="71cba-237">regex</span><span class="sxs-lookup"><span data-stu-id="71cba-237">regex</span></span> | <span data-ttu-id="71cba-238">Correspond à une expression régulière.</span><span class="sxs-lookup"><span data-stu-id="71cba-238">Matches a regular expression.</span></span> | <span data-ttu-id="71cba-239">{x :Regex (^ \d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="71cba-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="71cba-240">Notez que certaines des contraintes, telles que &quot;min&quot;, prennent des arguments entre parenthèses.</span><span class="sxs-lookup"><span data-stu-id="71cba-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="71cba-241">Vous pouvez appliquer plusieurs contraintes à un paramètre, séparées par un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="71cba-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="71cba-242">Contraintes d’itinéraire personnalisé</span><span class="sxs-lookup"><span data-stu-id="71cba-242">Custom Route Constraints</span></span>

<span data-ttu-id="71cba-243">Vous pouvez créer des contraintes d’itinéraire personnalisées en implémentant l’interface **IHttpRouteConstraint** .</span><span class="sxs-lookup"><span data-stu-id="71cba-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="71cba-244">Par exemple, la contrainte suivante restreint un paramètre à une valeur entière différente de zéro.</span><span class="sxs-lookup"><span data-stu-id="71cba-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="71cba-245">Le code suivant montre comment enregistrer la contrainte :</span><span class="sxs-lookup"><span data-stu-id="71cba-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="71cba-246">Vous pouvez maintenant appliquer la contrainte dans vos itinéraires :</span><span class="sxs-lookup"><span data-stu-id="71cba-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="71cba-247">Vous pouvez également remplacer la classe **DefaultInlineConstraintResolver** entière en implémentant l’interface **IInlineConstraintResolver** .</span><span class="sxs-lookup"><span data-stu-id="71cba-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="71cba-248">Cela va remplacer toutes les contraintes intégrées, sauf si votre implémentation de **IInlineConstraintResolver** les ajoute spécifiquement.</span><span class="sxs-lookup"><span data-stu-id="71cba-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="71cba-249">Paramètres d’URI facultatifs et valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="71cba-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="71cba-250">Vous pouvez rendre un paramètre URI facultatif en ajoutant un point d’interrogation au paramètre d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="71cba-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="71cba-251">Si un paramètre d’itinéraire est facultatif, vous devez définir une valeur par défaut pour le paramètre de la méthode.</span><span class="sxs-lookup"><span data-stu-id="71cba-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="71cba-252">Dans cet exemple, `/api/books/locale/1033` et `/api/books/locale` retourner la même ressource.</span><span class="sxs-lookup"><span data-stu-id="71cba-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="71cba-253">Vous pouvez également spécifier une valeur par défaut dans le modèle de routage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="71cba-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="71cba-254">C’est presque identique à l’exemple précédent, mais il existe une légère différence de comportement lorsque la valeur par défaut est appliquée.</span><span class="sxs-lookup"><span data-stu-id="71cba-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="71cba-255">Dans le premier exemple (« {LCID : int ?} »), la valeur par défaut de 1033 est assignée directement au paramètre de la méthode, de sorte que le paramètre aura cette valeur exacte.</span><span class="sxs-lookup"><span data-stu-id="71cba-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="71cba-256">Dans le deuxième exemple (« {LCID : int = 1033} »), la valeur par défaut « 1033 » passe par le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="71cba-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="71cba-257">Le Binder de modèle par défaut convertit « 1033 » en la valeur numérique 1033.</span><span class="sxs-lookup"><span data-stu-id="71cba-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="71cba-258">Toutefois, vous pouvez intégrer un classeur de modèles personnalisé, ce qui peut être différent.</span><span class="sxs-lookup"><span data-stu-id="71cba-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="71cba-259">(Dans la plupart des cas, sauf si vous avez des classeurs de modèles personnalisés dans votre pipeline, les deux formulaires sont équivalents.)</span><span class="sxs-lookup"><span data-stu-id="71cba-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="71cba-260">Noms d’itinéraires</span><span class="sxs-lookup"><span data-stu-id="71cba-260">Route Names</span></span>

<span data-ttu-id="71cba-261">Dans l’API Web, chaque itinéraire a un nom.</span><span class="sxs-lookup"><span data-stu-id="71cba-261">In Web API, every route has a name.</span></span> <span data-ttu-id="71cba-262">Les noms de routes sont utiles pour générer des liens, afin que vous puissiez inclure un lien dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="71cba-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="71cba-263">Pour spécifier le nom de l’itinéraire, définissez la propriété **Name** sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="71cba-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="71cba-264">L’exemple suivant montre comment définir le nom de l’itinéraire et comment utiliser le nom de l’itinéraire lors de la génération d’un lien.</span><span class="sxs-lookup"><span data-stu-id="71cba-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="71cba-265">Ordre de routage</span><span class="sxs-lookup"><span data-stu-id="71cba-265">Route Order</span></span>

<span data-ttu-id="71cba-266">Lorsque le Framework tente de faire correspondre un URI avec un itinéraire, il évalue les itinéraires dans un ordre particulier.</span><span class="sxs-lookup"><span data-stu-id="71cba-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="71cba-267">Pour spécifier l’ordre, définissez la propriété **Order** sur l’attribut route.</span><span class="sxs-lookup"><span data-stu-id="71cba-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="71cba-268">Les valeurs inférieures sont évaluées en premier.</span><span class="sxs-lookup"><span data-stu-id="71cba-268">Lower values are evaluated first.</span></span> <span data-ttu-id="71cba-269">La valeur d’ordre par défaut est zéro.</span><span class="sxs-lookup"><span data-stu-id="71cba-269">The default order value is zero.</span></span>

<span data-ttu-id="71cba-270">Voici comment déterminer le classement total :</span><span class="sxs-lookup"><span data-stu-id="71cba-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="71cba-271">Comparez la propriété **Order** de l’attribut route.</span><span class="sxs-lookup"><span data-stu-id="71cba-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="71cba-272">Examinez chaque segment d’URI dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="71cba-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="71cba-273">Pour chaque segment, l’ordre est le suivant :</span><span class="sxs-lookup"><span data-stu-id="71cba-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="71cba-274">Segments littéraux.</span><span class="sxs-lookup"><span data-stu-id="71cba-274">Literal segments.</span></span>
    2. <span data-ttu-id="71cba-275">Acheminer les paramètres avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="71cba-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="71cba-276">Acheminer les paramètres sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="71cba-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="71cba-277">Segments de paramètres génériques avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="71cba-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="71cba-278">Segments de paramètres génériques sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="71cba-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="71cba-279">Dans le cas d’un lien, les itinéraires sont classés par une comparaison de chaînes ordinale ne respectant pas la casse ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) du modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="71cba-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="71cba-280">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="71cba-280">Here is an example.</span></span> <span data-ttu-id="71cba-281">Supposons que vous définissiez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="71cba-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="71cba-282">Ces itinéraires sont triés comme suit.</span><span class="sxs-lookup"><span data-stu-id="71cba-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="71cba-283">commandes/détails</span><span class="sxs-lookup"><span data-stu-id="71cba-283">orders/details</span></span>
2. <span data-ttu-id="71cba-284">commandes/{ID}</span><span class="sxs-lookup"><span data-stu-id="71cba-284">orders/{id}</span></span>
3. <span data-ttu-id="71cba-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="71cba-285">orders/{customerName}</span></span>
4. <span data-ttu-id="71cba-286">commandes/{date de\*}</span><span class="sxs-lookup"><span data-stu-id="71cba-286">orders/{\*date}</span></span>
5. <span data-ttu-id="71cba-287">commandes/en attente</span><span class="sxs-lookup"><span data-stu-id="71cba-287">orders/pending</span></span>

<span data-ttu-id="71cba-288">Notez que « Details » est un segment littéral qui apparaît avant « {ID} », mais « Pending » apparaît en dernier, car la propriété **Order** est 1.</span><span class="sxs-lookup"><span data-stu-id="71cba-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="71cba-289">(Cet exemple suppose qu’il n’existe aucun client nommé « Details » ou « pending ».</span><span class="sxs-lookup"><span data-stu-id="71cba-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="71cba-290">En général, essayez d’éviter les itinéraires ambigus.</span><span class="sxs-lookup"><span data-stu-id="71cba-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="71cba-291">Dans cet exemple, un meilleur modèle de routage pour `GetByCustomer` est « clients/{customerName} »)</span><span class="sxs-lookup"><span data-stu-id="71cba-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
