---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Contrôleurs de test unitaire dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Cette rubrique décrit des techniques spécifiques pour les contrôleurs de test unitaire dans Web API 2. Avant de lire cette rubrique, vous souhaiterez peut-être lire l’unité du didacticiel...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554991"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="ea5eb-104">Tests unitaires des contrôleurs dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ea5eb-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>

<span data-ttu-id="ea5eb-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ea5eb-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="ea5eb-106">Cette rubrique décrit des techniques spécifiques pour les contrôleurs de test unitaire dans Web API 2.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="ea5eb-107">Avant de lire cette rubrique, vous souhaiterez peut-être lire le didacticiel sur les [tests unitaires API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), qui montre comment ajouter un projet de test unitaire à votre solution.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ea5eb-108">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="ea5eb-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="ea5eb-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ea5eb-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="ea5eb-110">API Web 2</span><span class="sxs-lookup"><span data-stu-id="ea5eb-110">Web API 2</span></span>
> - <span data-ttu-id="ea5eb-111">[MOQ](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="ea5eb-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="ea5eb-112">J’ai utilisé MOQ, mais la même idée s’applique à n’importe quel Framework fictif.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="ea5eb-113">MOQ 4.5.30 (et versions ultérieures) prend en charge Visual Studio 2017, Roslyn et .NET 4,5 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="ea5eb-114">Un modèle courant de tests unitaires est &quot;les&quot;arrange-Act-Assert :</span><span class="sxs-lookup"><span data-stu-id="ea5eb-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="ea5eb-115">Réorganisation : configurez les composants requis pour l’exécution du test.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="ea5eb-116">Act : effectuez le test.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-116">Act: Perform the test.</span></span>
- <span data-ttu-id="ea5eb-117">Assertion : Vérifiez que le test a réussi.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="ea5eb-118">Dans l’étape de réorganisation, vous utilisez souvent des objets fictifs ou stub.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="ea5eb-119">Qui réduit le nombre de dépendances, le test est donc axé sur le test d’une seule chose.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="ea5eb-120">Voici quelques éléments que vous devez tester par unité dans vos contrôleurs d’API Web :</span><span class="sxs-lookup"><span data-stu-id="ea5eb-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="ea5eb-121">L’action retourne le type de réponse correct.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="ea5eb-122">Les paramètres non valides renvoient la réponse d’erreur correcte.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="ea5eb-123">L’action appelle la méthode correcte sur le référentiel ou la couche de service.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="ea5eb-124">Si la réponse comprend un modèle de domaine, vérifiez le type de modèle.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="ea5eb-125">Voici quelques-uns des éléments généraux à tester, mais les spécificités dépendent de l’implémentation de votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="ea5eb-126">En particulier, il est important de savoir si vos actions de contrôleur retournent **HttpResponseMessage** ou **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="ea5eb-127">Pour plus d’informations sur ces types de résultats, consultez [résultats des actions dans l’API Web 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="ea5eb-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="ea5eb-128">Test des actions qui retournent HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="ea5eb-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="ea5eb-129">Voici un exemple de contrôleur dont les actions retournent **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="ea5eb-130">Notez que le contrôleur utilise l’injection de dépendances pour injecter une `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="ea5eb-131">Cela rend le contrôleur plus testable, car vous pouvez injecter un référentiel fictif.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="ea5eb-132">Le test unitaire suivant vérifie que la méthode `Get` écrit un `Product` dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="ea5eb-133">Supposons que `repository` est un simulacre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="ea5eb-134">Il est important de définir la **demande** et la **configuration** sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="ea5eb-135">Dans le cas contraire, le test échoue avec **ArgumentNullException** ou **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="ea5eb-136">Test de la génération de liens</span><span class="sxs-lookup"><span data-stu-id="ea5eb-136">Testing Link Generation</span></span>

<span data-ttu-id="ea5eb-137">La méthode `Post` appelle **UrlHelper. Link** pour créer des liens dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="ea5eb-138">Cela nécessite un peu plus d’installation dans le test unitaire :</span><span class="sxs-lookup"><span data-stu-id="ea5eb-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="ea5eb-139">La classe **UrlHelper** a besoin de l’URL de la requête et des données d’itinéraire, de sorte que le test doit définir des valeurs pour celles-ci.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="ea5eb-140">Une autre option est un simulacre ou un stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="ea5eb-141">Avec cette approche, vous remplacez la valeur par défaut de [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) par une version factice ou stub qui retourne une valeur fixe.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="ea5eb-142">Nous allons réécrire le test à l’aide de l’infrastructure [MOQ](https://github.com/Moq) .</span><span class="sxs-lookup"><span data-stu-id="ea5eb-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="ea5eb-143">Installez le package NuGet `Moq` dans le projet de test.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="ea5eb-144">Dans cette version, vous n’avez pas besoin de configurer de données d’itinéraire, car le simulacre **UrlHelper** retourne une chaîne constante.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>

## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="ea5eb-145">Test des actions qui retournent IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="ea5eb-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="ea5eb-146">Dans l’API Web 2, une action de contrôleur peut retourner **IHttpActionResult**, ce qui est analogue à **ACTIONRESULT** dans ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="ea5eb-147">L’interface **IHttpActionResult** définit un modèle de commande pour la création de réponses http.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="ea5eb-148">Au lieu de créer la réponse directement, le contrôleur retourne un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="ea5eb-149">Plus tard, le pipeline appelle **IHttpActionResult** pour créer la réponse.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="ea5eb-150">Cette approche facilite l’écriture de tests unitaires, car vous pouvez ignorer un grand nombre de configurations nécessaires pour **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="ea5eb-151">Voici un exemple de contrôleur dont les actions retournent **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="ea5eb-152">Cet exemple montre des modèles courants à l’aide de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="ea5eb-153">Voyons comment effectuer des tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="ea5eb-154">L’action renvoie 200 (OK) avec un corps de réponse</span><span class="sxs-lookup"><span data-stu-id="ea5eb-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="ea5eb-155">La méthode `Get` appelle `Ok(product)` si le produit est trouvé.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="ea5eb-156">Dans le test unitaire, assurez-vous que le type de retour est **OkNegotiatedContentResult** et que le produit retourné a l’ID correct.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="ea5eb-157">Notez que le test unitaire n’exécute pas le résultat de l’action.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="ea5eb-158">Vous pouvez supposer que le résultat de l’action crée la réponse HTTP correctement.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="ea5eb-159">(C’est la raison pour laquelle l’infrastructure de l’API Web a ses propres tests unitaires !)</span><span class="sxs-lookup"><span data-stu-id="ea5eb-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="ea5eb-160">L’action renvoie 404 (introuvable)</span><span class="sxs-lookup"><span data-stu-id="ea5eb-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="ea5eb-161">La méthode `Get` appelle `NotFound()` si le produit est introuvable.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="ea5eb-162">Dans ce cas, le test unitaire vérifie uniquement si le type de retour est **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="ea5eb-163">L’action renvoie 200 (OK) sans corps de réponse</span><span class="sxs-lookup"><span data-stu-id="ea5eb-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="ea5eb-164">La méthode `Delete` appelle `Ok()` pour retourner une réponse HTTP 200 vide.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="ea5eb-165">Comme dans l’exemple précédent, le test unitaire vérifie le type de retour, dans ce cas **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="ea5eb-166">L’action renvoie 201 (créé) avec un en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="ea5eb-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="ea5eb-167">La méthode `Post` appelle `CreatedAtRoute` pour retourner une réponse HTTP 201 avec un URI dans l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="ea5eb-168">Dans le test unitaire, vérifiez que l’action définit les valeurs de routage correctes.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="ea5eb-169">L’action renvoie un autre 2XX avec un corps de réponse</span><span class="sxs-lookup"><span data-stu-id="ea5eb-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="ea5eb-170">La méthode `Put` appelle `Content` pour retourner une réponse HTTP 202 (acceptée) avec un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="ea5eb-171">Ce cas de figure est semblable au retour de 200 (OK), mais le test unitaire doit également vérifier le code d’État.</span><span class="sxs-lookup"><span data-stu-id="ea5eb-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="ea5eb-172">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ea5eb-172">Additional Resources</span></span>

- [<span data-ttu-id="ea5eb-173">Simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="ea5eb-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="ea5eb-174">[Écriture de tests pour un service API Web ASP.net](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (billet de blog de youssef moussaoui).</span><span class="sxs-lookup"><span data-stu-id="ea5eb-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="ea5eb-175">Débogage de API Web ASP.NET avec le débogueur de routage</span><span class="sxs-lookup"><span data-stu-id="ea5eb-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
