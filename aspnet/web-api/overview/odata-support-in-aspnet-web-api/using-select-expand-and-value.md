---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: À l’aide de $select, $expand et $value dans ASP.NET Web API 2 OData - ASP.NET 4.x
author: MikeWasson
description: Exemples de code et de la vue d’ensemble pour le $expand, $select, et les options de $value dans les API Web OData 2 d’ASP.NET 4.x.
ms.author: riande
ms.date: 10/11/2013
ms.custom: seoapril2019
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: 8b5d3e87c679a31f1908aa648219ae5c6b701a1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400696"
---
# <a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="aa7bc-103">À l’aide de $select, $expand et $value dans ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="aa7bc-103">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="aa7bc-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aa7bc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="aa7bc-105">Exemples de code et de la vue d’ensemble pour le $expand, $select, et les options de $value dans les API Web OData 2 d’ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-105">Overview and code samples for the $expand, $select, and $value options in OData Web API 2 for ASP.NET 4.x.</span></span> <span data-ttu-id="aa7bc-106">Ces options permettent à un client contrôler la représentation, il récupère à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-106">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="aa7bc-107">**$expand** provoque des entités connexes doivent être incluses inline dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-107">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="aa7bc-108">**$select** sélectionne un sous-ensemble de propriétés à inclure dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-108">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="aa7bc-109">**$value** Obtient la valeur brute d’une propriété.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-109">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="aa7bc-110">Exemple de schéma</span><span class="sxs-lookup"><span data-stu-id="aa7bc-110">Example Schema</span></span>

<span data-ttu-id="aa7bc-111">Pour cet article, je vais utiliser un service OData qui définit trois entités : Produit, fournisseur et la catégorie.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-111">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="aa7bc-112">Chaque produit est associé à une catégorie et un seul fournisseur.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-112">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="aa7bc-113">Voici les classes c# qui définissent les modèles d’entité :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-113">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="aa7bc-114">Notez que le `Product` classe définit les propriétés de navigation pour le `Supplier` et `Category`.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-114">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="aa7bc-115">Le `Category` classe définit une propriété de navigation pour les produits dans chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-115">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="aa7bc-116">Pour créer un point de terminaison OData pour ce schéma, utilisez la génération de modèles Visual Studio 2013, comme décrit dans [création d’un point de terminaison OData dans ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="aa7bc-116">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="aa7bc-117">Ajouter des contrôleurs distincts pour les produits, catégories et de fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-117">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="aa7bc-118">L’activation de $développez et $select</span><span class="sxs-lookup"><span data-stu-id="aa7bc-118">Enabling $expand and $select</span></span>

<span data-ttu-id="aa7bc-119">Dans Visual Studio 2013, la génération de modèles automatique Web API OData crée un contrôleur qui prend automatiquement en charge $expand et $select.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-119">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="aa7bc-120">Pour référence, voici la configuration requise pour prendre en charge $développez et $select dans un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-120">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="aa7bc-121">Pour les de collections, le contrôleur `Get` méthode doit retourner un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-121">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="aa7bc-122">Pour les entités uniques, retourner un **SingleResult&lt;T&gt;**, où T est un **IQueryable** qui contient des entités zéro ou un.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-122">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="aa7bc-123">En outre, décorer votre `Get` méthodes avec la **[Queryable]** attribut, comme indiqué dans les extraits de code précédent.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-123">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="aa7bc-124">Vous pouvez également appeler **EnableQuerySupport** sur le **HttpConfiguration** objet au démarrage.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-124">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="aa7bc-125">(Pour plus d’informations, consultez [l’activation des Options de requête OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="aa7bc-125">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="aa7bc-126">À l’aide de $développez</span><span class="sxs-lookup"><span data-stu-id="aa7bc-126">Using $expand</span></span>

<span data-ttu-id="aa7bc-127">Lorsque vous interrogez une entité OData ou une collection, la réponse par défaut n’inclut pas les entités associées.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-127">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="aa7bc-128">Par exemple, voici la réponse par défaut pour le jeu d’entités de catégories :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-128">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="aa7bc-129">Comme vous pouvez le voir, la réponse n’inclut pas tous les produits, même si l’entité de catégorie a un lien de navigation de produits.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-129">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="aa7bc-130">Toutefois, le client peut utiliser $développer pour obtenir la liste des produits pour chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-130">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="aa7bc-131">Le $expand option est placé dans la chaîne de requête de la demande :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-131">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="aa7bc-132">Le serveur inclut désormais les produits pour chaque catégorie, inline avec les catégories.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-132">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="aa7bc-133">Voici la charge utile de réponse :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-133">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="aa7bc-134">Notez que chaque entrée dans le tableau « value » contient une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-134">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="aa7bc-135">Le $expand option prend séparées par des virgules liste de propriétés de navigation à développer.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-135">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="aa7bc-136">La requête suivante étend la catégorie et le fournisseur pour un produit.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-136">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="aa7bc-137">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-137">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="aa7bc-138">Vous pouvez développer plusieurs niveaux de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-138">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="aa7bc-139">L’exemple suivant inclut tous les produits pour une catégorie, ainsi que le fournisseur pour chaque produit.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-139">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="aa7bc-140">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-140">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="aa7bc-141">Par défaut, les API Web limite la profondeur d’expansion maximale à 2.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-141">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="aa7bc-142">Le client qui empêche l’envoi des demandes complexes telles que `$expand=Orders/OrderDetails/Product/Supplier/Region`, qui peut être inefficace pour interroger et créer les réponses volumineuses.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-142">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="aa7bc-143">Pour remplacer la valeur par défaut, affectez à la **MaxExpansionDepth** propriété sur le **[Queryable]** attribut.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-143">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="aa7bc-144">Pour plus d’informations sur le $développez option, consultez [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) dans la documentation officielle de OData.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-144">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="aa7bc-145">À l’aide de $select</span><span class="sxs-lookup"><span data-stu-id="aa7bc-145">Using $select</span></span>

<span data-ttu-id="aa7bc-146">L’option $select spécifie un sous-ensemble de propriétés à inclure dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-146">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="aa7bc-147">Par exemple, pour obtenir uniquement le nom et le prix de chaque produit, utilisez la requête suivante :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-147">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="aa7bc-148">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-148">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="aa7bc-149">Vous pouvez combiner $select et $expand dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-149">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="aa7bc-150">Veillez à inclure la propriété développée dans l’option $select.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-150">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="aa7bc-151">Par exemple, la requête suivante obtient le nom de produit et le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-151">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="aa7bc-152">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-152">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="aa7bc-153">Vous pouvez également sélectionner les propriétés dans une propriété développée.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-153">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="aa7bc-154">La requête suivante développe des produits et sélectionne le nom de catégorie et nom de produit.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-154">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="aa7bc-155">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-155">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="aa7bc-156">Pour plus d’informations sur l’option $select, consultez [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) dans la documentation officielle de OData.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-156">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="aa7bc-157">Obtention des propriétés individuelles d’une entité ($value)</span><span class="sxs-lookup"><span data-stu-id="aa7bc-157">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="aa7bc-158">Il existe deux façons pour un client OData obtenir une propriété individuelle d’une entité.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-158">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="aa7bc-159">Le client peut obtenir la valeur dans le format OData, soit obtenir la valeur brute de la propriété.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-159">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="aa7bc-160">La requête suivante obtient une propriété dans le format OData.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-160">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="aa7bc-161">Voici un exemple de réponse au format JSON :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-161">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="aa7bc-162">Pour obtenir la valeur brute de la propriété, ajoutez $value à l’URI :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-162">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="aa7bc-163">Voici la réponse.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-163">Here is the response.</span></span> <span data-ttu-id="aa7bc-164">Notez que le type de contenu est « text/plain », pas un JSON.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-164">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="aa7bc-165">Pour prendre en charge ces requêtes dans votre contrôleur OData, ajoutez une méthode nommée `GetProperty`, où `Property` est le nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-165">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="aa7bc-166">Par exemple, la méthode à obtenir la propriété de nom serait nommée `GetName`.</span><span class="sxs-lookup"><span data-stu-id="aa7bc-166">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="aa7bc-167">La méthode doit retourner la valeur de cette propriété :</span><span class="sxs-lookup"><span data-stu-id="aa7bc-167">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
