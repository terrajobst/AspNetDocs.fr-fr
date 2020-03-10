---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routage et sélection des actions dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554886"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="4e819-102">Routage et sélection des actions dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4e819-102">Routing and Action Selection in ASP.NET Web API</span></span>

<span data-ttu-id="4e819-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4e819-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4e819-104">Cet article explique comment API Web ASP.NET achemine une requête HTTP vers une action particulière sur un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="4e819-105">Pour obtenir une vue d’ensemble générale du routage, consultez [routage dans API Web ASP.net](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4e819-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="4e819-106">Cet article examine en détail le processus de routage.</span><span class="sxs-lookup"><span data-stu-id="4e819-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="4e819-107">Si vous créez un projet d’API Web et que certaines demandes ne sont pas acheminées comme prévu, cet article vous aidera.</span><span class="sxs-lookup"><span data-stu-id="4e819-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="4e819-108">Le routage comporte trois phases principales :</span><span class="sxs-lookup"><span data-stu-id="4e819-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="4e819-109">Correspondance de l’URI avec un modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="4e819-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="4e819-110">Sélection d’un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-110">Selecting a controller.</span></span>
3. <span data-ttu-id="4e819-111">Sélection d’une action.</span><span class="sxs-lookup"><span data-stu-id="4e819-111">Selecting an action.</span></span>

<span data-ttu-id="4e819-112">Vous pouvez remplacer certaines parties du processus par vos propres comportements personnalisés.</span><span class="sxs-lookup"><span data-stu-id="4e819-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="4e819-113">Dans cet article, je décrirai le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="4e819-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="4e819-114">À la fin, je remarque les endroits où vous pouvez personnaliser le comportement.</span><span class="sxs-lookup"><span data-stu-id="4e819-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="4e819-115">Modèles de routage</span><span class="sxs-lookup"><span data-stu-id="4e819-115">Route Templates</span></span>

<span data-ttu-id="4e819-116">Un modèle de routage ressemble à un chemin d’accès d’URI, mais il peut avoir des valeurs d’espace réservé, indiquées par des accolades :</span><span class="sxs-lookup"><span data-stu-id="4e819-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="4e819-117">Lorsque vous créez un itinéraire, vous pouvez fournir des valeurs par défaut pour certains ou tous les espaces réservés :</span><span class="sxs-lookup"><span data-stu-id="4e819-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="4e819-118">Vous pouvez également fournir des contraintes, qui restreignent la manière dont un segment d’URI peut correspondre à un espace réservé :</span><span class="sxs-lookup"><span data-stu-id="4e819-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="4e819-119">L’infrastructure tente de faire correspondre les segments dans le chemin d’accès de l’URI au modèle.</span><span class="sxs-lookup"><span data-stu-id="4e819-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="4e819-120">Les littéraux dans le modèle doivent correspondre exactement.</span><span class="sxs-lookup"><span data-stu-id="4e819-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="4e819-121">Un espace réservé correspond à n’importe quelle valeur, sauf si vous spécifiez des contraintes.</span><span class="sxs-lookup"><span data-stu-id="4e819-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="4e819-122">Le Framework ne correspond pas à d’autres parties de l’URI, telles que le nom d’hôte ou les paramètres de requête.</span><span class="sxs-lookup"><span data-stu-id="4e819-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="4e819-123">L’infrastructure sélectionne le premier itinéraire dans la table de routage qui correspond à l’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="4e819-124">Il existe deux espaces réservés spéciaux : « {Controller} » et « {action} ».</span><span class="sxs-lookup"><span data-stu-id="4e819-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="4e819-125">« {Controller} » fournit le nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="4e819-126">« {action} » fournit le nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="4e819-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="4e819-127">Dans l’API Web, la Convention habituelle consiste à omettre « {action} ».</span><span class="sxs-lookup"><span data-stu-id="4e819-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="4e819-128">Valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="4e819-128">Defaults</span></span>

<span data-ttu-id="4e819-129">Si vous fournissez des valeurs par défaut, l’itinéraire correspond à un URI qui ne contient pas ces segments.</span><span class="sxs-lookup"><span data-stu-id="4e819-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="4e819-130">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4e819-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="4e819-131">Les URI `http://localhost/api/products/all` et `http://localhost/api/products` correspondent à l’itinéraire précédent.</span><span class="sxs-lookup"><span data-stu-id="4e819-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="4e819-132">Dans ce dernier URI, la valeur par défaut `all`est assignée au segment de `{category}` manquant.</span><span class="sxs-lookup"><span data-stu-id="4e819-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="4e819-133">Dictionnaire de routage</span><span class="sxs-lookup"><span data-stu-id="4e819-133">Route Dictionary</span></span>

<span data-ttu-id="4e819-134">Si le Framework trouve une correspondance pour un URI, il crée un dictionnaire qui contient la valeur de chaque espace réservé.</span><span class="sxs-lookup"><span data-stu-id="4e819-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="4e819-135">Les clés sont les noms des espaces réservés, à l’exclusion des accolades.</span><span class="sxs-lookup"><span data-stu-id="4e819-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="4e819-136">Les valeurs sont extraites du chemin d’accès de l’URI ou des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="4e819-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="4e819-137">Le dictionnaire est stocké dans l’objet **IHttpRouteData** .</span><span class="sxs-lookup"><span data-stu-id="4e819-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="4e819-138">Pendant cette phase de correspondance de routage, les espaces réservés « {Controller} » et « {action} » spéciaux sont traités comme les autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="4e819-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="4e819-139">Elles sont simplement stockées dans le dictionnaire avec les autres valeurs.</span><span class="sxs-lookup"><span data-stu-id="4e819-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="4e819-140">Une valeur par défaut peut avoir la valeur spéciale **RouteParameter. optional**.</span><span class="sxs-lookup"><span data-stu-id="4e819-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="4e819-141">Si cette valeur est assignée à un espace réservé, la valeur n’est pas ajoutée au dictionnaire de routage.</span><span class="sxs-lookup"><span data-stu-id="4e819-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="4e819-142">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4e819-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="4e819-143">Pour le chemin d’accès URI « API/Products », le dictionnaire de routage contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4e819-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="4e819-144">contrôleur : « Products »</span><span class="sxs-lookup"><span data-stu-id="4e819-144">controller: "products"</span></span>
- <span data-ttu-id="4e819-145">Catégorie : « tous »</span><span class="sxs-lookup"><span data-stu-id="4e819-145">category: "all"</span></span>

<span data-ttu-id="4e819-146">Toutefois, pour « API/Products/Toys/123 », le dictionnaire de routes contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4e819-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="4e819-147">contrôleur : « Products »</span><span class="sxs-lookup"><span data-stu-id="4e819-147">controller: "products"</span></span>
- <span data-ttu-id="4e819-148">Catégorie : « jouets »</span><span class="sxs-lookup"><span data-stu-id="4e819-148">category: "toys"</span></span>
- <span data-ttu-id="4e819-149">ID : « 123 »</span><span class="sxs-lookup"><span data-stu-id="4e819-149">id: "123"</span></span>

<span data-ttu-id="4e819-150">Les valeurs par défaut peuvent également inclure une valeur qui n’apparaît nulle part dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="4e819-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="4e819-151">Si l’itinéraire correspond, cette valeur est stockée dans le dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="4e819-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="4e819-152">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4e819-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="4e819-153">Si le chemin d’accès de l’URI est « API/root/8 », le dictionnaire contiendra deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="4e819-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="4e819-154">contrôleur : « clients »</span><span class="sxs-lookup"><span data-stu-id="4e819-154">controller: "customers"</span></span>
- <span data-ttu-id="4e819-155">ID : « 8 »</span><span class="sxs-lookup"><span data-stu-id="4e819-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="4e819-156">Sélection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="4e819-156">Selecting a Controller</span></span>

<span data-ttu-id="4e819-157">La sélection du contrôleur est gérée par la méthode **IHttpControllerSelector. SelectController** .</span><span class="sxs-lookup"><span data-stu-id="4e819-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="4e819-158">Cette méthode prend une instance **HttpRequestMessage** et retourne un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="4e819-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="4e819-159">L’implémentation par défaut est fournie par la classe **DefaultHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="4e819-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="4e819-160">Cette classe utilise un algorithme simple :</span><span class="sxs-lookup"><span data-stu-id="4e819-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="4e819-161">Recherchez la clé « Controller » dans le dictionnaire de routage.</span><span class="sxs-lookup"><span data-stu-id="4e819-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="4e819-162">Prenez la valeur de cette clé et ajoutez la chaîne « Controller » pour obtenir le nom du type de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="4e819-163">Recherchez un contrôleur d’API Web avec ce nom de type.</span><span class="sxs-lookup"><span data-stu-id="4e819-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="4e819-164">Par exemple, si le dictionnaire de routage contient la paire clé-valeur « Controller » = « Products », le type de contrôleur est « ProductsController ».</span><span class="sxs-lookup"><span data-stu-id="4e819-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="4e819-165">S’il n’y a aucun type correspondant, ou plusieurs correspondances, le Framework retourne une erreur au client.</span><span class="sxs-lookup"><span data-stu-id="4e819-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="4e819-166">Pour l’étape 3, **DefaultHttpControllerSelector** utilise l’interface **IHttpControllerTypeResolver** pour obtenir la liste des types de contrôleurs d’API Web.</span><span class="sxs-lookup"><span data-stu-id="4e819-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="4e819-167">L’implémentation par défaut de **IHttpControllerTypeResolver** retourne toutes les classes publiques qui (a) implémentent **IHttpController**, (b) ne sont pas abstraites et (c) ont un nom qui se termine par « Controller ».</span><span class="sxs-lookup"><span data-stu-id="4e819-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="4e819-168">Sélection de l’action</span><span class="sxs-lookup"><span data-stu-id="4e819-168">Action Selection</span></span>

<span data-ttu-id="4e819-169">Après avoir sélectionné le contrôleur, le Framework sélectionne l’action en appelant la méthode **IHttpActionSelector. SelectAction** .</span><span class="sxs-lookup"><span data-stu-id="4e819-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="4e819-170">Cette méthode prend un **HttpControllerContext** et retourne un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="4e819-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="4e819-171">L’implémentation par défaut est fournie par la classe **ApiControllerActionSelector** .</span><span class="sxs-lookup"><span data-stu-id="4e819-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="4e819-172">Pour sélectionner une action, elle se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="4e819-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="4e819-173">Méthode HTTP de la demande.</span><span class="sxs-lookup"><span data-stu-id="4e819-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="4e819-174">Espace réservé « {action} » dans le modèle de routage, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="4e819-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="4e819-175">Paramètres des actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="4e819-176">Avant d’examiner l’algorithme de sélection, nous devons comprendre certains aspects des actions du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="4e819-177">**Quelles méthodes sur le contrôleur sont considérées comme « actions » ?**</span><span class="sxs-lookup"><span data-stu-id="4e819-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="4e819-178">Lors de la sélection d’une action, l’infrastructure examine uniquement les méthodes d’instance publiques sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="4e819-179">En outre, elle exclut les méthodes de [« nom spécial »](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (constructeurs, événements, surcharges d’opérateur, etc.) et les méthodes héritées de la classe **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="4e819-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="4e819-180">**Méthodes HTTP.**</span><span class="sxs-lookup"><span data-stu-id="4e819-180">**HTTP Methods.**</span></span> <span data-ttu-id="4e819-181">L’infrastructure choisit uniquement les actions qui correspondent à la méthode HTTP de la requête, déterminée comme suit :</span><span class="sxs-lookup"><span data-stu-id="4e819-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="4e819-182">Vous pouvez spécifier la méthode HTTP avec un attribut : **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**ou **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="4e819-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="4e819-183">Dans le cas contraire, si le nom de la méthode de contrôleur commence par « obtient », « après », « put », « Delete », « Head », « options » ou « patch », alors que l’action prend en charge cette méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="4e819-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="4e819-184">Si aucun des éléments ci-dessus n’est présent, la méthode prend en charge l’envoi.</span><span class="sxs-lookup"><span data-stu-id="4e819-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="4e819-185">**Liaisons de paramètres.**</span><span class="sxs-lookup"><span data-stu-id="4e819-185">**Parameter Bindings.**</span></span> <span data-ttu-id="4e819-186">Une liaison de paramètre indique comment l’API Web crée une valeur pour un paramètre.</span><span class="sxs-lookup"><span data-stu-id="4e819-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="4e819-187">Voici la règle par défaut pour la liaison de paramètre :</span><span class="sxs-lookup"><span data-stu-id="4e819-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="4e819-188">Les types simples sont extraits de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="4e819-189">Les types complexes sont extraits du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="4e819-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="4e819-190">Les types simples incluent tous les [.NET Framework types primitifs](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **GUID**, **String**et **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="4e819-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="4e819-191">Pour chaque action, au plus un paramètre peut lire le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="4e819-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="4e819-192">Il est possible de remplacer les règles de liaison par défaut.</span><span class="sxs-lookup"><span data-stu-id="4e819-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="4e819-193">Consultez [liaison de paramètre WebAPI sous le capot](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e819-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>

<span data-ttu-id="4e819-194">Avec cet arrière-plan, voici l’algorithme de sélection d’action.</span><span class="sxs-lookup"><span data-stu-id="4e819-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="4e819-195">Créez une liste de toutes les actions sur le contrôleur qui correspondent à la méthode de requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="4e819-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="4e819-196">Si le dictionnaire de routage a une entrée « action », supprimez les actions dont le nom ne correspond pas à cette valeur.</span><span class="sxs-lookup"><span data-stu-id="4e819-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="4e819-197">Essayez de faire correspondre les paramètres d’action à l’URI, comme suit :</span><span class="sxs-lookup"><span data-stu-id="4e819-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="4e819-198">Pour chaque action, récupérez la liste des paramètres qui sont de type simple, où la liaison obtient le paramètre à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="4e819-199">Excluez les paramètres facultatifs.</span><span class="sxs-lookup"><span data-stu-id="4e819-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="4e819-200">Dans cette liste, essayez de trouver une correspondance pour chaque nom de paramètre, soit dans le dictionnaire d’itinéraires, soit dans la chaîne de requête d’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="4e819-201">Les correspondances ne respectent pas la casse et ne dépendent pas de l’ordre des paramètres.</span><span class="sxs-lookup"><span data-stu-id="4e819-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="4e819-202">Sélectionnez une action où chaque paramètre de la liste a une correspondance dans l’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="4e819-203">Si plus d’une action répond à ces critères, choisissez celle dont les paramètres correspondent le plus.</span><span class="sxs-lookup"><span data-stu-id="4e819-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="4e819-204">Ignorez les actions avec l’attribut **[not action]** .</span><span class="sxs-lookup"><span data-stu-id="4e819-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="4e819-205">L’étape #3 est probablement le plus confus.</span><span class="sxs-lookup"><span data-stu-id="4e819-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="4e819-206">L’idée de base est qu’un paramètre peut obtenir sa valeur à partir de l’URI, du corps de la demande ou d’une liaison personnalisée.</span><span class="sxs-lookup"><span data-stu-id="4e819-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="4e819-207">Pour les paramètres provenant de l’URI, nous voulons nous assurer que l’URI contient effectivement une valeur pour ce paramètre, soit dans le chemin d’accès (via le dictionnaire d’itinéraires), soit dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="4e819-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="4e819-208">Par exemple, considérez l’action suivante :</span><span class="sxs-lookup"><span data-stu-id="4e819-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="4e819-209">Le paramètre *ID* est lié à l’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="4e819-210">Par conséquent, cette action ne peut correspondre qu’à un URI qui contient une valeur pour « ID », soit dans le dictionnaire d’itinéraires, soit dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="4e819-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="4e819-211">Les paramètres facultatifs sont une exception, car ils sont facultatifs.</span><span class="sxs-lookup"><span data-stu-id="4e819-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="4e819-212">Pour un paramètre facultatif, il est OK si la liaison ne peut pas obtenir la valeur de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="4e819-213">Les types complexes sont une exception pour une raison différente.</span><span class="sxs-lookup"><span data-stu-id="4e819-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="4e819-214">Un type complexe ne peut être lié qu’à l’URI par le biais d’une liaison personnalisée.</span><span class="sxs-lookup"><span data-stu-id="4e819-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="4e819-215">Mais dans ce cas, l’infrastructure ne peut pas savoir à l’avance si le paramètre doit être lié à un URI particulier.</span><span class="sxs-lookup"><span data-stu-id="4e819-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="4e819-216">Pour déterminer, il faut appeler la liaison.</span><span class="sxs-lookup"><span data-stu-id="4e819-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="4e819-217">L’objectif de l’algorithme de sélection consiste à sélectionner une action à partir de la description statique, avant d’appeler des liaisons.</span><span class="sxs-lookup"><span data-stu-id="4e819-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="4e819-218">Par conséquent, les types complexes sont exclus de l’algorithme de correspondance.</span><span class="sxs-lookup"><span data-stu-id="4e819-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="4e819-219">Une fois l’action sélectionnée, toutes les liaisons de paramètres sont appelées.</span><span class="sxs-lookup"><span data-stu-id="4e819-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="4e819-220">Résumé :</span><span class="sxs-lookup"><span data-stu-id="4e819-220">Summary:</span></span>

- <span data-ttu-id="4e819-221">L’action doit correspondre à la méthode HTTP de la requête.</span><span class="sxs-lookup"><span data-stu-id="4e819-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="4e819-222">Le nom de l’action doit correspondre à l’entrée « action » dans le dictionnaire de routage, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="4e819-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="4e819-223">Pour chaque paramètre de l’action, si le paramètre est extrait de l’URI, le nom du paramètre doit être trouvé dans le dictionnaire d’itinéraires ou dans la chaîne de requête d’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="4e819-224">(Les paramètres facultatifs et les paramètres avec des types complexes sont exclus.)</span><span class="sxs-lookup"><span data-stu-id="4e819-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="4e819-225">Essayez de faire correspondre le plus grand nombre de paramètres.</span><span class="sxs-lookup"><span data-stu-id="4e819-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="4e819-226">La meilleure correspondance peut être une méthode sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="4e819-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="4e819-227">Exemple étendu</span><span class="sxs-lookup"><span data-stu-id="4e819-227">Extended Example</span></span>

<span data-ttu-id="4e819-228">Poursuit</span><span class="sxs-lookup"><span data-stu-id="4e819-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="4e819-229">Contrôleur :</span><span class="sxs-lookup"><span data-stu-id="4e819-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="4e819-230">Requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="4e819-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="4e819-231">Correspondance d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="4e819-231">Route Matching</span></span>

<span data-ttu-id="4e819-232">L’URI correspond à l’itinéraire nommé « DefaultApi ».</span><span class="sxs-lookup"><span data-stu-id="4e819-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="4e819-233">Le dictionnaire de routage contient les entrées suivantes :</span><span class="sxs-lookup"><span data-stu-id="4e819-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="4e819-234">contrôleur : « Products »</span><span class="sxs-lookup"><span data-stu-id="4e819-234">controller: "products"</span></span>
- <span data-ttu-id="4e819-235">ID : « 1 »</span><span class="sxs-lookup"><span data-stu-id="4e819-235">id: "1"</span></span>

<span data-ttu-id="4e819-236">Le dictionnaire de routage ne contient pas les paramètres de chaîne de requête « version » et « détails », mais ils seront toujours pris en compte lors de la sélection de l’action.</span><span class="sxs-lookup"><span data-stu-id="4e819-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="4e819-237">Sélection du contrôleur</span><span class="sxs-lookup"><span data-stu-id="4e819-237">Controller Selection</span></span>

<span data-ttu-id="4e819-238">À partir de l’entrée « Controller » dans le dictionnaire de routage, le type de contrôleur est `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="4e819-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="4e819-239">Sélection de l’action</span><span class="sxs-lookup"><span data-stu-id="4e819-239">Action Selection</span></span>

<span data-ttu-id="4e819-240">La requête HTTP est une requête d’extraction.</span><span class="sxs-lookup"><span data-stu-id="4e819-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="4e819-241">Les actions de contrôleur qui prennent en charge sont `GetAll`, `GetById`et `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="4e819-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="4e819-242">Le dictionnaire de routage ne contient pas d’entrée pour « action », nous n’avons donc pas besoin de faire correspondre le nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="4e819-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="4e819-243">Ensuite, nous essayons de faire correspondre les noms de paramètres pour les actions, en regardant uniquement les actions obtenir.</span><span class="sxs-lookup"><span data-stu-id="4e819-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="4e819-244">Action</span><span class="sxs-lookup"><span data-stu-id="4e819-244">Action</span></span> | <span data-ttu-id="4e819-245">Paramètres à faire correspondre</span><span class="sxs-lookup"><span data-stu-id="4e819-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="4e819-246">aucun</span><span class="sxs-lookup"><span data-stu-id="4e819-246">none</span></span> |
| `GetById` | <span data-ttu-id="4e819-247">« ID »</span><span class="sxs-lookup"><span data-stu-id="4e819-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="4e819-248">nomme</span><span class="sxs-lookup"><span data-stu-id="4e819-248">"name"</span></span> |

<span data-ttu-id="4e819-249">Notez que le paramètre de *version* de `GetById` n’est pas pris en compte, car il s’agit d’un paramètre facultatif.</span><span class="sxs-lookup"><span data-stu-id="4e819-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="4e819-250">La méthode `GetAll` correspond à triflaconly.</span><span class="sxs-lookup"><span data-stu-id="4e819-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="4e819-251">La méthode `GetById` correspond également, car le dictionnaire de routes contient « ID ».</span><span class="sxs-lookup"><span data-stu-id="4e819-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="4e819-252">La méthode `FindProductsByName` ne correspond pas.</span><span class="sxs-lookup"><span data-stu-id="4e819-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="4e819-253">La méthode `GetById` gagne, car elle correspond à un paramètre, et non pas aux paramètres de `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="4e819-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="4e819-254">La méthode est appelée avec les valeurs de paramètre suivantes :</span><span class="sxs-lookup"><span data-stu-id="4e819-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="4e819-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="4e819-255">*id* = 1</span></span>
- <span data-ttu-id="4e819-256">*version* = 1,5</span><span class="sxs-lookup"><span data-stu-id="4e819-256">*version* = 1.5</span></span>

<span data-ttu-id="4e819-257">Notez que même si la *version* n’a pas été utilisée dans l’algorithme de sélection, la valeur du paramètre provient de la chaîne de requête d’URI.</span><span class="sxs-lookup"><span data-stu-id="4e819-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="4e819-258">Points d’extension</span><span class="sxs-lookup"><span data-stu-id="4e819-258">Extension Points</span></span>

<span data-ttu-id="4e819-259">L’API Web fournit des points d’extension pour certaines parties du processus de routage.</span><span class="sxs-lookup"><span data-stu-id="4e819-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="4e819-260">Interface</span><span class="sxs-lookup"><span data-stu-id="4e819-260">Interface</span></span> | <span data-ttu-id="4e819-261">Description</span><span class="sxs-lookup"><span data-stu-id="4e819-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4e819-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="4e819-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="4e819-263">Sélectionne le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-263">Selects the controller.</span></span> |
| <span data-ttu-id="4e819-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="4e819-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="4e819-265">Obtient la liste des types de contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="4e819-265">Gets the list of controller types.</span></span> <span data-ttu-id="4e819-266">Le **DefaultHttpControllerSelector** choisit le type de contrôleur dans cette liste.</span><span class="sxs-lookup"><span data-stu-id="4e819-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="4e819-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="4e819-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="4e819-268">Obtient la liste des assemblys de projet.</span><span class="sxs-lookup"><span data-stu-id="4e819-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="4e819-269">L’interface **IHttpControllerTypeResolver** utilise cette liste pour rechercher les types de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="4e819-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="4e819-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="4e819-271">Crée de nouvelles instances de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4e819-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="4e819-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="4e819-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="4e819-273">Sélectionne l'action.</span><span class="sxs-lookup"><span data-stu-id="4e819-273">Selects the action.</span></span> |
| <span data-ttu-id="4e819-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="4e819-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="4e819-275">Appelle l’action.</span><span class="sxs-lookup"><span data-stu-id="4e819-275">Invokes the action.</span></span> |

<span data-ttu-id="4e819-276">Pour fournir votre propre implémentation pour l’une de ces interfaces, utilisez la collection **services** sur l’objet **HttpConfiguration** :</span><span class="sxs-lookup"><span data-stu-id="4e819-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
