---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Résultats des actions dans l’API Web 2-ASP.NET 4. x
author: MikeWasson
description: Décrit comment API Web ASP.NET convertit la valeur de retour d’une action de contrôleur en message de réponse HTTP dans ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985835"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="4fde3-103">Résultats des actions dans l’API Web 2</span><span class="sxs-lookup"><span data-stu-id="4fde3-103">Action Results in Web API 2</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="4fde3-104">Cette rubrique décrit comment API Web ASP.NET convertit la valeur de retour d’une action de contrôleur en message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="4fde3-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="4fde3-105">Une action du contrôleur d’API Web peut retourner l’un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4fde3-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="4fde3-106">void</span><span class="sxs-lookup"><span data-stu-id="4fde3-106">void</span></span>
2. <span data-ttu-id="4fde3-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="4fde3-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="4fde3-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="4fde3-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="4fde3-109">Autre type</span><span class="sxs-lookup"><span data-stu-id="4fde3-109">Some other type</span></span>

<span data-ttu-id="4fde3-110">En fonction de la valeur renvoyée, l’API Web utilise un mécanisme différent pour créer la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="4fde3-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="4fde3-111">Type de retour</span><span class="sxs-lookup"><span data-stu-id="4fde3-111">Return type</span></span> | <span data-ttu-id="4fde3-112">Comment l’API Web crée la réponse</span><span class="sxs-lookup"><span data-stu-id="4fde3-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="4fde3-113">void</span><span class="sxs-lookup"><span data-stu-id="4fde3-113">void</span></span> | <span data-ttu-id="4fde3-114">Retourne un 204 vide (aucun contenu)</span><span class="sxs-lookup"><span data-stu-id="4fde3-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="4fde3-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="4fde3-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="4fde3-116">Convertissez directement en message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="4fde3-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="4fde3-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="4fde3-117">**IHttpActionResult**</span></span> | <span data-ttu-id="4fde3-118">Appelez **ExecuteAsync** pour créer un **HttpResponseMessage**, puis convertissez-le en message de réponse http.</span><span class="sxs-lookup"><span data-stu-id="4fde3-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="4fde3-119">Autre type</span><span class="sxs-lookup"><span data-stu-id="4fde3-119">Other type</span></span> | <span data-ttu-id="4fde3-120">Écrivez la valeur de retour sérialisée dans le corps de la réponse ; retourne 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="4fde3-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="4fde3-121">Le reste de cette rubrique décrit chaque option plus en détail.</span><span class="sxs-lookup"><span data-stu-id="4fde3-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="4fde3-122">void</span><span class="sxs-lookup"><span data-stu-id="4fde3-122">void</span></span>

<span data-ttu-id="4fde3-123">Si le type de retour `void`est, l’API Web retourne simplement une réponse http vide avec le code d’État 204 (aucun contenu).</span><span class="sxs-lookup"><span data-stu-id="4fde3-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="4fde3-124">Exemple de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="4fde3-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="4fde3-125">Réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="4fde3-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="4fde3-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="4fde3-126">HttpResponseMessage</span></span>

<span data-ttu-id="4fde3-127">Si l’action retourne un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), l’API Web convertit la valeur de retour directement en message de réponse http, à l’aide des propriétés de l’objet **HttpResponseMessage** pour remplir la réponse.</span><span class="sxs-lookup"><span data-stu-id="4fde3-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="4fde3-128">Cette option vous donne un grand contrôle sur le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="4fde3-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="4fde3-129">Par exemple, l’action de contrôleur suivante définit l’en-tête Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="4fde3-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="4fde3-130">Réponse :</span><span class="sxs-lookup"><span data-stu-id="4fde3-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="4fde3-131">Si vous transmettez un modèle de domaine à la méthode **CreateResponse** , l’API Web utilise un [formateur multimédia](../formats-and-model-binding/media-formatters.md) pour écrire le modèle sérialisé dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="4fde3-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="4fde3-132">L’API Web utilise l’en-tête Accept dans la demande pour choisir le formateur.</span><span class="sxs-lookup"><span data-stu-id="4fde3-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="4fde3-133">Pour plus d’informations, consultez [négociation de contenu](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="4fde3-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="4fde3-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="4fde3-134">IHttpActionResult</span></span>

<span data-ttu-id="4fde3-135">L’interface **IHttpActionResult** a été introduite dans l’API Web 2.</span><span class="sxs-lookup"><span data-stu-id="4fde3-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="4fde3-136">Fondamentalement, il définit une fabrique **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="4fde3-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="4fde3-137">Voici quelques avantages de l’utilisation de l’interface **IHttpActionResult** :</span><span class="sxs-lookup"><span data-stu-id="4fde3-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="4fde3-138">Simplifie le [test unitaire](../testing-and-debugging/unit-testing-controllers-in-web-api.md) de vos contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="4fde3-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="4fde3-139">Déplace la logique commune pour créer des réponses HTTP dans des classes distinctes.</span><span class="sxs-lookup"><span data-stu-id="4fde3-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="4fde3-140">Rend l’objectif de l’action du contrôleur plus clair, en masquant les détails de bas niveau de la construction de la réponse.</span><span class="sxs-lookup"><span data-stu-id="4fde3-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="4fde3-141">**IHttpActionResult** contient une méthode unique, **ExecuteAsync**, qui crée de manière asynchrone une instance **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="4fde3-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="4fde3-142">Si une action de contrôleur retourne un **IHttpActionResult**, l’API Web appelle la méthode **ExecuteAsync** pour créer un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="4fde3-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="4fde3-143">Il convertit ensuite le **HttpResponseMessage** en message de réponse http.</span><span class="sxs-lookup"><span data-stu-id="4fde3-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="4fde3-144">Voici une implémentation simple de **IHttpActionResult** qui crée une réponse en texte brut :</span><span class="sxs-lookup"><span data-stu-id="4fde3-144">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="4fde3-145">Exemple d’action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="4fde3-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="4fde3-146">Réponse :</span><span class="sxs-lookup"><span data-stu-id="4fde3-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="4fde3-147">Plus souvent, vous utilisez les implémentations **IHttpActionResult** définies dans l’espace de noms **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="4fde3-147">More often, you use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="4fde3-148">La classe **ApiController** définit des méthodes d’assistance qui retournent ces résultats d’action intégrés.</span><span class="sxs-lookup"><span data-stu-id="4fde3-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="4fde3-149">Dans l’exemple suivant, si la demande ne correspond pas à un ID de produit existant, le contrôleur appelle [ApiController. NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) pour créer une réponse 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="4fde3-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="4fde3-150">Dans le cas contraire, le contrôleur appelle [ApiController. OK](https://msdn.microsoft.com/library/dn314591.aspx), qui crée une réponse 200 (OK) qui contient le produit.</span><span class="sxs-lookup"><span data-stu-id="4fde3-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="4fde3-151">Autres types de retour</span><span class="sxs-lookup"><span data-stu-id="4fde3-151">Other Return Types</span></span>

<span data-ttu-id="4fde3-152">Pour tous les autres types de retour, l’API Web utilise un [formateur multimédia](../formats-and-model-binding/media-formatters.md) pour sérialiser la valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="4fde3-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="4fde3-153">L’API Web écrit la valeur sérialisée dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="4fde3-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="4fde3-154">Le code d’état de la réponse est 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="4fde3-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="4fde3-155">L’inconvénient de cette approche est que vous ne pouvez pas retourner directement un code d’erreur, tel que 404.</span><span class="sxs-lookup"><span data-stu-id="4fde3-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="4fde3-156">Toutefois, vous pouvez lever un **HttpResponseException** pour les codes d’erreur.</span><span class="sxs-lookup"><span data-stu-id="4fde3-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="4fde3-157">Pour plus d’informations, consultez [gestion des exceptions dans API Web ASP.net](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="4fde3-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="4fde3-158">L’API Web utilise l’en-tête Accept dans la demande pour choisir le formateur.</span><span class="sxs-lookup"><span data-stu-id="4fde3-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="4fde3-159">Pour plus d’informations, consultez [négociation de contenu](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="4fde3-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="4fde3-160">Exemple de requête</span><span class="sxs-lookup"><span data-stu-id="4fde3-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="4fde3-161">Exemple de réponse</span><span class="sxs-lookup"><span data-stu-id="4fde3-161">Example response</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
