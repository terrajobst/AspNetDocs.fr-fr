---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Liaison de paramètre dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit comment les API Web lient les paramètres et comment personnaliser le processus de liaison dans ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 464cb9b45dc0b62c4da38b7cf612934808854d32
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074902"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="e7d83-103">Liaison de paramètre dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e7d83-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="e7d83-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e7d83-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="e7d83-105">Cet article décrit comment les API Web lient les paramètres et comment vous pouvez personnaliser le processus de liaison.</span><span class="sxs-lookup"><span data-stu-id="e7d83-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="e7d83-106">Lorsque l’API Web appelle une méthode sur un contrôleur, elle doit définir des valeurs pour les paramètres, un processus appelé *Binding*.</span><span class="sxs-lookup"><span data-stu-id="e7d83-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span>

<span data-ttu-id="e7d83-107">Par défaut, l’API Web utilise les règles suivantes pour lier les paramètres :</span><span class="sxs-lookup"><span data-stu-id="e7d83-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="e7d83-108">Si le paramètre est un type « simple », l’API Web tente d’obtenir la valeur de l’URI.</span><span class="sxs-lookup"><span data-stu-id="e7d83-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="e7d83-109">Les types simples incluent les [types primitifs](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .net (**int**, **bool**, **double**, etc.), plus **TimeSpan**, **DateTime**, **GUID**, **Decimal**et **String**, *ainsi* que tout type avec un convertisseur de type qui peut effectuer une conversion à partir d’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="e7d83-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="e7d83-110">(En savoir plus sur les convertisseurs de type ultérieurement.)</span><span class="sxs-lookup"><span data-stu-id="e7d83-110">(More about type converters later.)</span></span>
- <span data-ttu-id="e7d83-111">Pour les types complexes, l’API Web tente de lire la valeur à partir du corps du message, à l’aide d’un [formateur de type média](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="e7d83-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="e7d83-112">Par exemple, voici une méthode de contrôleur d’API Web typique :</span><span class="sxs-lookup"><span data-stu-id="e7d83-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e7d83-113">Le paramètre *ID* est un type de&quot; simple &quot;, de sorte que l’API Web tente d’obtenir la valeur à partir de l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="e7d83-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="e7d83-114">Le paramètre d' *élément* est un type complexe, de sorte que l’API Web utilise un formateur de type de média pour lire la valeur du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="e7d83-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="e7d83-115">Pour obtenir une valeur à partir de l’URI, l’API Web examine les données d’itinéraire et la chaîne de requête d’URI.</span><span class="sxs-lookup"><span data-stu-id="e7d83-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="e7d83-116">Les données d’itinéraire sont renseignées lorsque le système de routage analyse l’URI et le met en correspondance avec un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="e7d83-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="e7d83-117">Pour plus d’informations, consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="e7d83-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="e7d83-118">Dans le reste de cet article, je vais vous montrer comment vous pouvez personnaliser le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="e7d83-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="e7d83-119">Toutefois, pour les types complexes, envisagez d’utiliser des formateurs de type de média dans la mesure du possible.</span><span class="sxs-lookup"><span data-stu-id="e7d83-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="e7d83-120">L’un des principaux principes du protocole HTTP est que les ressources sont envoyées dans le corps du message, à l’aide de la négociation de contenu pour spécifier la représentation de la ressource.</span><span class="sxs-lookup"><span data-stu-id="e7d83-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="e7d83-121">Les formateurs de type de média ont été conçus à cet effet.</span><span class="sxs-lookup"><span data-stu-id="e7d83-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="e7d83-122">Utilisation de [FromUri]</span><span class="sxs-lookup"><span data-stu-id="e7d83-122">Using [FromUri]</span></span>

<span data-ttu-id="e7d83-123">Pour forcer l’API Web à lire un type complexe à partir de l’URI, ajoutez l’attribut **[FromUri]** au paramètre.</span><span class="sxs-lookup"><span data-stu-id="e7d83-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="e7d83-124">L’exemple suivant définit un type de `GeoPoint`, ainsi qu’une méthode de contrôleur qui obtient les `GeoPoint` à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="e7d83-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e7d83-125">Le client peut placer les valeurs de latitude et de longitude dans la chaîne de requête et l’API Web les utilisera pour construire un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="e7d83-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="e7d83-126">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e7d83-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="e7d83-127">Utilisation de [FromBody]</span><span class="sxs-lookup"><span data-stu-id="e7d83-127">Using [FromBody]</span></span>

<span data-ttu-id="e7d83-128">Pour forcer l’API Web à lire un type simple à partir du corps de la requête, ajoutez l’attribut **[FromBody]** au paramètre :</span><span class="sxs-lookup"><span data-stu-id="e7d83-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e7d83-129">Dans cet exemple, l’API Web utilise un formateur de type de média pour lire la valeur du *nom* dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="e7d83-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="e7d83-130">Voici un exemple de demande de client.</span><span class="sxs-lookup"><span data-stu-id="e7d83-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="e7d83-131">Lorsqu’un paramètre a la valeur [FromBody], l’API Web utilise l’en-tête Content-type pour sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="e7d83-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="e7d83-132">Dans cet exemple, le type de contenu est &quot;application/JSON&quot; et le corps de la demande est une chaîne JSON brute (pas un objet JSON).</span><span class="sxs-lookup"><span data-stu-id="e7d83-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="e7d83-133">Au plus un paramètre est autorisé à lire à partir du corps du message.</span><span class="sxs-lookup"><span data-stu-id="e7d83-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="e7d83-134">Cela ne fonctionnera donc pas :</span><span class="sxs-lookup"><span data-stu-id="e7d83-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e7d83-135">La raison de cette règle est que le corps de la demande peut être stocké dans un flux non mis en mémoire tampon qui ne peut être lu qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="e7d83-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="e7d83-136">Convertisseurs de type</span><span class="sxs-lookup"><span data-stu-id="e7d83-136">Type Converters</span></span>

<span data-ttu-id="e7d83-137">Vous pouvez faire en sorte que l’API Web traite une classe comme un type simple (afin que l’API Web tente de la lier à partir de l’URI) en créant un **TypeConverter** et en fournissant une conversion de chaîne.</span><span class="sxs-lookup"><span data-stu-id="e7d83-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="e7d83-138">Le code suivant montre une classe `GeoPoint` qui représente un point géographique, plus un **TypeConverter** qui convertit des chaînes en instances `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="e7d83-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="e7d83-139">La classe `GeoPoint` est décorée avec un attribut **[TypeConverter]** pour spécifier le convertisseur de type.</span><span class="sxs-lookup"><span data-stu-id="e7d83-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="e7d83-140">(Cet exemple s’est inspiré du billet de blog de Mike Cabinet [Comment lier des objets personnalisés dans des signatures d’action dans MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="e7d83-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e7d83-141">Désormais, l’API Web traitera `GeoPoint` comme un type simple, ce qui signifie qu’elle essaiera de lier les paramètres `GeoPoint` à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="e7d83-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="e7d83-142">Vous n’avez pas besoin d’inclure **[FromUri]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="e7d83-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="e7d83-143">Le client peut appeler la méthode avec un URI comme suit :</span><span class="sxs-lookup"><span data-stu-id="e7d83-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="e7d83-144">Classeurs de modèles</span><span class="sxs-lookup"><span data-stu-id="e7d83-144">Model Binders</span></span>

<span data-ttu-id="e7d83-145">Une option plus flexible qu’un convertisseur de type consiste à créer un classeur de modèles personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e7d83-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="e7d83-146">Avec un classeur de modèles, vous avez accès à des éléments tels que la requête HTTP, la description de l’action et les valeurs brutes des données d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="e7d83-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="e7d83-147">Pour créer un classeur de modèles, implémentez l’interface **IModelBinder** .</span><span class="sxs-lookup"><span data-stu-id="e7d83-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="e7d83-148">Cette interface définit une méthode unique, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="e7d83-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="e7d83-149">Voici un classeur de modèles pour les objets `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="e7d83-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="e7d83-150">Un classeur de modèles obtient des valeurs d’entrée brutes à partir d’un *fournisseur de valeurs*.</span><span class="sxs-lookup"><span data-stu-id="e7d83-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="e7d83-151">Cette conception sépare deux fonctions distinctes :</span><span class="sxs-lookup"><span data-stu-id="e7d83-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="e7d83-152">Le fournisseur de valeur prend la requête HTTP et remplit un dictionnaire de paires clé-valeur.</span><span class="sxs-lookup"><span data-stu-id="e7d83-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="e7d83-153">Le Binder de modèle utilise ce dictionnaire pour remplir le modèle.</span><span class="sxs-lookup"><span data-stu-id="e7d83-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="e7d83-154">Le fournisseur de valeurs par défaut dans l’API Web obtient des valeurs à partir des données d’itinéraire et de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="e7d83-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="e7d83-155">Par exemple, si l’URI est `http://localhost/api/values/1?location=48,-122`, le fournisseur de valeurs crée les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7d83-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="e7d83-156">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="e7d83-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="e7d83-157">emplacement = &quot;48 122&quot;</span><span class="sxs-lookup"><span data-stu-id="e7d83-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="e7d83-158">(Je suppose que le modèle de routage par défaut est &quot;API/{Controller}/{ID}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="e7d83-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="e7d83-159">Le nom du paramètre à lier est stocké dans la propriété **ModelBindingContext. ModelName** .</span><span class="sxs-lookup"><span data-stu-id="e7d83-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="e7d83-160">Le Binder de modèle recherche une clé avec cette valeur dans le dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="e7d83-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="e7d83-161">Si la valeur existe et peut être convertie en `GeoPoint`, le classeur de modèles affecte la valeur liée à la propriété **ModelBindingContext. Model** .</span><span class="sxs-lookup"><span data-stu-id="e7d83-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="e7d83-162">Notez que le classeur de modèles n’est pas limité à une conversion de type simple.</span><span class="sxs-lookup"><span data-stu-id="e7d83-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="e7d83-163">Dans cet exemple, le Binder de modèle commence par Rechercher dans une table d’emplacements connus et, en cas d’échec, il utilise la conversion de type.</span><span class="sxs-lookup"><span data-stu-id="e7d83-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="e7d83-164">**Définition du classeur de modèles**</span><span class="sxs-lookup"><span data-stu-id="e7d83-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="e7d83-165">Il existe plusieurs façons de définir un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="e7d83-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="e7d83-166">Tout d’abord, vous pouvez ajouter un attribut **[ModelBinder]** au paramètre.</span><span class="sxs-lookup"><span data-stu-id="e7d83-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="e7d83-167">Vous pouvez également ajouter un attribut **[ModelBinder]** au type.</span><span class="sxs-lookup"><span data-stu-id="e7d83-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="e7d83-168">L’API Web utilise le classeur de modèles spécifié pour tous les paramètres de ce type.</span><span class="sxs-lookup"><span data-stu-id="e7d83-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="e7d83-169">Enfin, vous pouvez ajouter un fournisseur de classeurs de modèles à **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="e7d83-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="e7d83-170">Un fournisseur de classeurs de modèles est simplement une classe de fabrique qui crée un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="e7d83-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="e7d83-171">Vous pouvez créer un fournisseur en dérivant de la classe [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) .</span><span class="sxs-lookup"><span data-stu-id="e7d83-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="e7d83-172">Toutefois, si votre classeur de modèles gère un type unique, il est plus facile d’utiliser le **SimpleModelBinderProvider**intégré, conçu à cet effet.</span><span class="sxs-lookup"><span data-stu-id="e7d83-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="e7d83-173">Le code suivant montre comment procéder.</span><span class="sxs-lookup"><span data-stu-id="e7d83-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="e7d83-174">Avec un fournisseur de liaison de modèle, vous devez toujours ajouter l’attribut **[ModelBinder]** au paramètre pour indiquer à l’API Web qu’il doit utiliser un classeur de modèles et non un formateur de type de média.</span><span class="sxs-lookup"><span data-stu-id="e7d83-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="e7d83-175">Mais maintenant, vous n’avez pas besoin de spécifier le type de Binder de modèle dans l’attribut :</span><span class="sxs-lookup"><span data-stu-id="e7d83-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="e7d83-176">Fournisseurs de valeurs</span><span class="sxs-lookup"><span data-stu-id="e7d83-176">Value Providers</span></span>

<span data-ttu-id="e7d83-177">J’ai indiqué qu’un Binder de modèle obtient des valeurs à partir d’un fournisseur de valeurs.</span><span class="sxs-lookup"><span data-stu-id="e7d83-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="e7d83-178">Pour écrire un fournisseur de valeurs personnalisées, implémentez l’interface **IValueProvider** .</span><span class="sxs-lookup"><span data-stu-id="e7d83-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="e7d83-179">Voici un exemple qui extrait les valeurs des cookies de la requête :</span><span class="sxs-lookup"><span data-stu-id="e7d83-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="e7d83-180">Vous devez également créer une fabrique de fournisseur de valeur en dérivant de la classe **ValueProviderFactory** .</span><span class="sxs-lookup"><span data-stu-id="e7d83-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="e7d83-181">Ajoutez la fabrique de fournisseur de valeur au **HttpConfiguration** comme suit.</span><span class="sxs-lookup"><span data-stu-id="e7d83-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="e7d83-182">L’API Web compose tous les fournisseurs de valeurs, donc lorsqu’un Binder de modèle appelle **ValueProvider. GetValue**, le Binder de modèle reçoit la valeur du premier fournisseur de valeur capable de le produire.</span><span class="sxs-lookup"><span data-stu-id="e7d83-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="e7d83-183">Vous pouvez également définir la fabrique de fournisseur de valeur au niveau du paramètre à l’aide de l’attribut **ValueProvider** , comme suit :</span><span class="sxs-lookup"><span data-stu-id="e7d83-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="e7d83-184">Cela indique à l’API Web d’utiliser la liaison de modèle avec la fabrique de fournisseur de valeur spécifiée, et de ne pas utiliser les autres fournisseurs de valeurs inscrits.</span><span class="sxs-lookup"><span data-stu-id="e7d83-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="e7d83-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="e7d83-185">HttpParameterBinding</span></span>

<span data-ttu-id="e7d83-186">Les classeurs de modèles sont une instance spécifique d’un mécanisme plus général.</span><span class="sxs-lookup"><span data-stu-id="e7d83-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="e7d83-187">Si vous examinez l’attribut **[ModelBinder]** , vous verrez qu’il dérive de la classe abstraite **ParameterBindingAttribute** .</span><span class="sxs-lookup"><span data-stu-id="e7d83-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="e7d83-188">Cette classe définit une méthode unique, **GetBinding**, qui retourne un objet **HttpParameterBinding** :</span><span class="sxs-lookup"><span data-stu-id="e7d83-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="e7d83-189">Un **HttpParameterBinding** est chargé de lier un paramètre à une valeur.</span><span class="sxs-lookup"><span data-stu-id="e7d83-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="e7d83-190">Dans le cas de **[ModelBinder]** , l’attribut retourne une implémentation **HttpParameterBinding** qui utilise un **IModelBinder** pour effectuer la liaison réelle.</span><span class="sxs-lookup"><span data-stu-id="e7d83-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="e7d83-191">Vous pouvez également implémenter votre propre **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="e7d83-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="e7d83-192">Par exemple, supposons que vous souhaitiez obtenir des ETags à partir des en-têtes `if-match` et `if-none-match` dans la demande.</span><span class="sxs-lookup"><span data-stu-id="e7d83-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="e7d83-193">Nous allons commencer par définir une classe pour représenter des ETags.</span><span class="sxs-lookup"><span data-stu-id="e7d83-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="e7d83-194">Nous allons également définir une énumération pour indiquer s’il faut récupérer l’ETag à partir de l’en-tête `if-match` ou de l’en-tête `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="e7d83-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="e7d83-195">Voici un **HttpParameterBinding** qui obtient l’ETag de l’en-tête souhaité et le lie à un paramètre de type ETag :</span><span class="sxs-lookup"><span data-stu-id="e7d83-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="e7d83-196">La méthode **ExecuteBindingAsync** effectue la liaison.</span><span class="sxs-lookup"><span data-stu-id="e7d83-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="e7d83-197">Dans cette méthode, ajoutez la valeur de paramètre lié au dictionnaire **ActionArgument** dans le **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="e7d83-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d83-198">Si votre méthode **ExecuteBindingAsync** lit le corps du message de demande, remplacez la valeur de la propriété **WillReadBody** pour retourner true.</span><span class="sxs-lookup"><span data-stu-id="e7d83-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="e7d83-199">Le corps de la demande peut être un flux non mis en mémoire tampon qui ne peut être lu qu’une seule fois. par conséquent, l’API Web applique une règle qui permet au plus à une liaison de lire le corps du message.</span><span class="sxs-lookup"><span data-stu-id="e7d83-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>

<span data-ttu-id="e7d83-200">Pour appliquer un **HttpParameterBinding**personnalisé, vous pouvez définir un attribut qui dérive de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="e7d83-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="e7d83-201">Pour `ETagParameterBinding`, nous allons définir deux attributs, l’un pour les en-têtes `if-match` et l’autre pour les en-têtes `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="e7d83-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="e7d83-202">Les deux dérivent d’une classe de base abstraite.</span><span class="sxs-lookup"><span data-stu-id="e7d83-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="e7d83-203">Voici une méthode de contrôleur qui utilise l’attribut `[IfNoneMatch]`.</span><span class="sxs-lookup"><span data-stu-id="e7d83-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="e7d83-204">Outre **ParameterBindingAttribute**, il existe un autre raccordement pour l’ajout d’un **HttpParameterBinding**personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e7d83-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="e7d83-205">Sur l’objet **HttpConfiguration** , la propriété **ParameterBindingRules** est une collection de fonctions anonymes de type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="e7d83-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="e7d83-206">Par exemple, vous pouvez ajouter une règle selon laquelle tout paramètre ETag sur une méthode obtenir utilise `ETagParameterBinding` avec `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="e7d83-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="e7d83-207">La fonction doit retourner `null` pour les paramètres où la liaison n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="e7d83-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="e7d83-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="e7d83-208">IActionValueBinder</span></span>

<span data-ttu-id="e7d83-209">L’ensemble du processus de liaison de paramètres est contrôlé par un service enfichable, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="e7d83-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="e7d83-210">L’implémentation par défaut de **IActionValueBinder** effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7d83-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="e7d83-211">Recherchez un **ParameterBindingAttribute** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="e7d83-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="e7d83-212">Cela comprend les attributs **[FromBody]** , **[FromUri]** et **[ModelBinder]** , ou les attributs personnalisés.</span><span class="sxs-lookup"><span data-stu-id="e7d83-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="e7d83-213">Sinon, recherchez dans **HttpConfiguration. ParameterBindingRules** une fonction qui retourne un **HttpParameterBinding**non null.</span><span class="sxs-lookup"><span data-stu-id="e7d83-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="e7d83-214">Dans le cas contraire, utilisez les règles par défaut que j’ai décrites précédemment.</span><span class="sxs-lookup"><span data-stu-id="e7d83-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="e7d83-215">Si le type de paramètre est « simple » ou a un convertisseur de type, établissez une liaison à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="e7d83-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="e7d83-216">Cela équivaut à placer l’attribut **[FromUri]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="e7d83-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="e7d83-217">Dans le cas contraire, essayez de lire le paramètre à partir du corps du message.</span><span class="sxs-lookup"><span data-stu-id="e7d83-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="e7d83-218">Cela équivaut à placer **[FromBody]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="e7d83-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="e7d83-219">Si vous le souhaitez, vous pouvez remplacer l’ensemble du service **IActionValueBinder** par une implémentation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="e7d83-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7d83-220">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e7d83-220">Additional Resources</span></span>

[<span data-ttu-id="e7d83-221">Exemple de liaison de paramètre personnalisé</span><span class="sxs-lookup"><span data-stu-id="e7d83-221">Custom Parameter Binding Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

<span data-ttu-id="e7d83-222">Mike Stall a écrit une bonne série de billets de blog sur la liaison de paramètres d’API Web :</span><span class="sxs-lookup"><span data-stu-id="e7d83-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="e7d83-223">Mode de liaison des paramètres par l’API Web</span><span class="sxs-lookup"><span data-stu-id="e7d83-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="e7d83-224">Liaison de paramètre de style MVC pour l’API Web</span><span class="sxs-lookup"><span data-stu-id="e7d83-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="e7d83-225">Comment lier des objets personnalisés dans des signatures d’action dans MVC/API Web</span><span class="sxs-lookup"><span data-stu-id="e7d83-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="e7d83-226">Comment créer un fournisseur de valeurs personnalisées dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="e7d83-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="e7d83-227">Liaison de paramètres d’API Web en coulisses</span><span class="sxs-lookup"><span data-stu-id="e7d83-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
