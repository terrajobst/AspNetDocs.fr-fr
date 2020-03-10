---
uid: web-api/overview/error-handling/exception-handling
title: Gestion des exceptions dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622317"
---
# <a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="b4e42-102">Gestion des exceptions dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b4e42-102">Exception Handling in ASP.NET Web API</span></span>

<span data-ttu-id="b4e42-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b4e42-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b4e42-104">Cet article décrit la gestion des erreurs et des exceptions dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4e42-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="b4e42-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="b4e42-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="b4e42-106">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="b4e42-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="b4e42-107">Inscription des filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="b4e42-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="b4e42-108">Erreur http</span><span class="sxs-lookup"><span data-stu-id="b4e42-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="b4e42-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="b4e42-109">HttpResponseException</span></span>

<span data-ttu-id="b4e42-110">Que se passe-t-il si un contrôleur d’API Web lève une exception non interceptée ?</span><span class="sxs-lookup"><span data-stu-id="b4e42-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="b4e42-111">Par défaut, la plupart des exceptions sont converties en réponse HTTP avec le code d’état 500, erreur de serveur interne.</span><span class="sxs-lookup"><span data-stu-id="b4e42-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="b4e42-112">Le type **HttpResponseException** est un cas spécial.</span><span class="sxs-lookup"><span data-stu-id="b4e42-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="b4e42-113">Cette exception retourne le code d’état HTTP que vous spécifiez dans le constructeur d’exception.</span><span class="sxs-lookup"><span data-stu-id="b4e42-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="b4e42-114">Par exemple, la méthode suivante retourne 404, introuvable, si le paramètre *ID* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="b4e42-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="b4e42-115">Pour mieux contrôler la réponse, vous pouvez également construire l’intégralité du message de réponse et l’inclure dans le **HttpResponseException :**</span><span class="sxs-lookup"><span data-stu-id="b4e42-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="b4e42-116">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="b4e42-116">Exception Filters</span></span>

<span data-ttu-id="b4e42-117">Vous pouvez personnaliser la façon dont l’API Web gère les exceptions en écrivant un *filtre d’exception*.</span><span class="sxs-lookup"><span data-stu-id="b4e42-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="b4e42-118">Un filtre d’exception est exécuté lorsqu’une méthode de contrôleur lève une exception non gérée qui n’est *pas* une exception **HttpResponseException** .</span><span class="sxs-lookup"><span data-stu-id="b4e42-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="b4e42-119">Le type **HttpResponseException** est un cas particulier, car il est conçu spécifiquement pour retourner une réponse http.</span><span class="sxs-lookup"><span data-stu-id="b4e42-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="b4e42-120">Les filtres d’exceptions implémentent l’interface **System. Web. http. filters. IExceptionFilter** .</span><span class="sxs-lookup"><span data-stu-id="b4e42-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="b4e42-121">La façon la plus simple d’écrire un filtre d’exception consiste à dériver de la classe **System. Web. http. filters. ExceptionFilterAttribute** et à substituer la méthode **OnException** .</span><span class="sxs-lookup"><span data-stu-id="b4e42-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="b4e42-122">Les filtres d’exception dans API Web ASP.NET sont similaires à ceux de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e42-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="b4e42-123">Toutefois, ils sont déclarés dans un espace de noms et une fonction séparés séparément.</span><span class="sxs-lookup"><span data-stu-id="b4e42-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="b4e42-124">En particulier, la classe **HandleErrorAttribute** utilisée dans MVC ne gère pas les exceptions levées par les contrôleurs d’API Web.</span><span class="sxs-lookup"><span data-stu-id="b4e42-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>

<span data-ttu-id="b4e42-125">Voici un filtre qui convertit les exceptions **NotImplementedException** en code d’état http 501, non implémenté :</span><span class="sxs-lookup"><span data-stu-id="b4e42-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="b4e42-126">La propriété **Response** de l’objet **HttpActionExecutedContext** contient le message de réponse HTTP qui sera envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="b4e42-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="b4e42-127">Inscription des filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="b4e42-127">Registering Exception Filters</span></span>

<span data-ttu-id="b4e42-128">Plusieurs méthodes pour enregistrer un filtre d’exception d’API web sont possibles :</span><span class="sxs-lookup"><span data-stu-id="b4e42-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="b4e42-129">Par action</span><span class="sxs-lookup"><span data-stu-id="b4e42-129">By action</span></span>
- <span data-ttu-id="b4e42-130">Par contrôleur</span><span class="sxs-lookup"><span data-stu-id="b4e42-130">By controller</span></span>
- <span data-ttu-id="b4e42-131">Globalement</span><span class="sxs-lookup"><span data-stu-id="b4e42-131">Globally</span></span>

<span data-ttu-id="b4e42-132">Pour appliquer le filtre à une action spécifique, ajoutez le filtre en tant qu’attribut à l’action :</span><span class="sxs-lookup"><span data-stu-id="b4e42-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="b4e42-133">Pour appliquer le filtre à toutes les actions sur un contrôleur, ajoutez le filtre en tant qu’attribut à la classe de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="b4e42-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="b4e42-134">Pour appliquer le filtre globalement à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la collection **GlobalConfiguration. Configuration. filters** .</span><span class="sxs-lookup"><span data-stu-id="b4e42-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="b4e42-135">Les filtres d’exception de cette collection s’appliquent à n’importe quelle action de contrôleur d’API web.</span><span class="sxs-lookup"><span data-stu-id="b4e42-135">Exception filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="b4e42-136">Si vous utilisez le modèle de projet « application Web ASP.NET MVC 4 » pour créer votre projet, placez le code de configuration de votre API Web à l’intérieur de la classe `WebApiConfig`, qui se trouve dans le dossier de démarrage de l’application\_:</span><span class="sxs-lookup"><span data-stu-id="b4e42-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="b4e42-137">Erreur http</span><span class="sxs-lookup"><span data-stu-id="b4e42-137">HttpError</span></span>

<span data-ttu-id="b4e42-138">L’objet **erreur http** offre un moyen cohérent de retourner des informations d’erreur dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="b4e42-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="b4e42-139">L’exemple suivant montre comment renvoyer le code d’état HTTP 404 (introuvable) avec un **erreur http** dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="b4e42-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="b4e42-140">**CreateErrorResponse** est une méthode d’extension définie dans la classe **System .net. http. HttpRequestMessageExtensions** .</span><span class="sxs-lookup"><span data-stu-id="b4e42-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="b4e42-141">En interne, **CreateErrorResponse** crée une instance **erreur http** , puis crée un **HttpResponseMessage** qui contient le **erreur http**.</span><span class="sxs-lookup"><span data-stu-id="b4e42-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="b4e42-142">Dans cet exemple, si la méthode réussit, elle retourne le produit dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4e42-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="b4e42-143">Toutefois, si le produit demandé est introuvable, la réponse HTTP contient un **erreur http** dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="b4e42-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="b4e42-144">La réponse peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="b4e42-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="b4e42-145">Notez que **erreur http** a été SÉRIALISÉ en JSON dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="b4e42-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="b4e42-146">L’un des avantages de l’utilisation de **erreur http** est qu’elle passe par le même processus de [négociation de contenu](../formats-and-model-binding/content-negotiation.md) et de sérialisation que tout autre modèle fortement typé.</span><span class="sxs-lookup"><span data-stu-id="b4e42-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="b4e42-147">Erreur http et validation de modèle</span><span class="sxs-lookup"><span data-stu-id="b4e42-147">HttpError and Model Validation</span></span>

<span data-ttu-id="b4e42-148">Pour la validation de modèle, vous pouvez passer l’état du modèle à **CreateErrorResponse**pour inclure les erreurs de validation dans la réponse :</span><span class="sxs-lookup"><span data-stu-id="b4e42-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="b4e42-149">Cet exemple peut retourner la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="b4e42-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="b4e42-150">Pour plus d’informations sur la validation de modèle, consultez [validation de modèle dans API Web ASP.net](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b4e42-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="b4e42-151">Utilisation de erreur http avec HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="b4e42-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="b4e42-152">Les exemples précédents renvoient un message **HttpResponseMessage** à partir de l’action du contrôleur, mais vous pouvez également utiliser **HttpResponseException** pour retourner un **erreur http**.</span><span class="sxs-lookup"><span data-stu-id="b4e42-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="b4e42-153">Cela vous permet de retourner un modèle fortement typé dans le cas d’une réussite normale, tout en retournant **erreur http** en cas d’erreur :</span><span class="sxs-lookup"><span data-stu-id="b4e42-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
