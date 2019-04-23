---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Appeler une API Web à partir d’un Client .NET (C#)-ASP.NET 4.x
author: MikeWasson
description: Ce didacticiel montre comment appeler une API web à partir d’une application .NET 4.x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 113600ca1e77ae9667465464da505478fc948c9b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59421106"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="dc18c-103">Appeler une API Web à partir d’un Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="dc18c-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="dc18c-104">par [Mike Wasson](https://github.com/MikeWasson) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc18c-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc18c-105">[Télécharger le projet terminé](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="dc18c-105">[Download Completed Project](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="dc18c-106">[Télécharger les instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="dc18c-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="dc18c-107">Ce didacticiel montre comment appeler une API web à partir d’une application .NET, à l’aide de [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="dc18c-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="dc18c-108">Dans ce didacticiel, une application cliente est écrit qui consomme l’API web suivante :</span><span class="sxs-lookup"><span data-stu-id="dc18c-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="dc18c-109">Action</span><span class="sxs-lookup"><span data-stu-id="dc18c-109">Action</span></span> | <span data-ttu-id="dc18c-110">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="dc18c-110">HTTP method</span></span> | <span data-ttu-id="dc18c-111">URI relatif</span><span class="sxs-lookup"><span data-stu-id="dc18c-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dc18c-112">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="dc18c-112">Get a product by ID</span></span> | <span data-ttu-id="dc18c-113">GET</span><span class="sxs-lookup"><span data-stu-id="dc18c-113">GET</span></span> | <span data-ttu-id="dc18c-114">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dc18c-114">/api/products/*id*</span></span> |
| <span data-ttu-id="dc18c-115">Création d’un produit</span><span class="sxs-lookup"><span data-stu-id="dc18c-115">Create a new product</span></span> | <span data-ttu-id="dc18c-116">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="dc18c-116">POST</span></span> | <span data-ttu-id="dc18c-117">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="dc18c-117">/api/products</span></span> |
| <span data-ttu-id="dc18c-118">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="dc18c-118">Update a product</span></span> | <span data-ttu-id="dc18c-119">PUT</span><span class="sxs-lookup"><span data-stu-id="dc18c-119">PUT</span></span> | <span data-ttu-id="dc18c-120">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dc18c-120">/api/products/*id*</span></span> |
| <span data-ttu-id="dc18c-121">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="dc18c-121">Delete a product</span></span> | <span data-ttu-id="dc18c-122">SUPPR</span><span class="sxs-lookup"><span data-stu-id="dc18c-122">DELETE</span></span> | <span data-ttu-id="dc18c-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="dc18c-123">/api/products/*id*</span></span> |

<span data-ttu-id="dc18c-124">Pour savoir comment implémenter cette API avec API Web ASP.NET, consultez [d’une API Web qui prend en charge les opérations CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="dc18c-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="dc18c-125">Par souci de simplicité, l’application cliente dans ce didacticiel est une application de console Windows.</span><span class="sxs-lookup"><span data-stu-id="dc18c-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="dc18c-126">**HttpClient** est également pris en charge pour les applications Windows Phone et Windows Store.</span><span class="sxs-lookup"><span data-stu-id="dc18c-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="dc18c-127">Pour plus d’informations, consultez [écriture Web API Client de Code pour plusieurs plateformes utilisant les bibliothèques portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="dc18c-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="dc18c-128">Créer l’Application de Console</span><span class="sxs-lookup"><span data-stu-id="dc18c-128">Create the Console Application</span></span>

<span data-ttu-id="dc18c-129">Dans Visual Studio, créez une nouvelle application de console Windows nommée **HttpClientSample** et collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dc18c-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="dc18c-130">Le code précédent est l’application cliente complète.</span><span class="sxs-lookup"><span data-stu-id="dc18c-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="dc18c-131">`RunAsync` exécutions et bloque jusqu'à ce qu’elle se termine.</span><span class="sxs-lookup"><span data-stu-id="dc18c-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="dc18c-132">La plupart des **HttpClient** méthodes sont asynchrones, car ils effectuent des e/s réseau.</span><span class="sxs-lookup"><span data-stu-id="dc18c-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="dc18c-133">Toutes les tâches async sont effectuées à l’intérieur de `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc18c-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="dc18c-134">Normalement une application ne bloque pas le thread principal, mais aucune interaction n’autorise pas cette application.</span><span class="sxs-lookup"><span data-stu-id="dc18c-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="dc18c-135">Installer les bibliothèques de Client de l’API Web</span><span class="sxs-lookup"><span data-stu-id="dc18c-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="dc18c-136">Utilisez le Gestionnaire de Package NuGet pour installer le package de bibliothèques de Client d’API Web.</span><span class="sxs-lookup"><span data-stu-id="dc18c-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="dc18c-137">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="dc18c-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="dc18c-138">Dans le Package Manager de la console, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="dc18c-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="dc18c-139">La commande précédente ajoute les packages NuGet suivants au projet :</span><span class="sxs-lookup"><span data-stu-id="dc18c-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="dc18c-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="dc18c-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="dc18c-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="dc18c-141">Newtonsoft.Json</span></span>

<span data-ttu-id="dc18c-142">Json.NET est une infrastructure JSON de hautes performances populaire pour .NET.</span><span class="sxs-lookup"><span data-stu-id="dc18c-142">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="dc18c-143">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="dc18c-143">Add a Model Class</span></span>

<span data-ttu-id="dc18c-144">Examiner la classe `Product` :</span><span class="sxs-lookup"><span data-stu-id="dc18c-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="dc18c-145">Cette classe correspond au modèle de données utilisé par l’API web.</span><span class="sxs-lookup"><span data-stu-id="dc18c-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="dc18c-146">Une application peut utiliser **HttpClient** pour lire un `Product` instance à partir d’une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="dc18c-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="dc18c-147">L’application n’a pas à écrire de code de la désérialisation.</span><span class="sxs-lookup"><span data-stu-id="dc18c-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="dc18c-148">Créer et initialiser HttpClient</span><span class="sxs-lookup"><span data-stu-id="dc18c-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="dc18c-149">Examinez la méthode statique **HttpClient** propriété :</span><span class="sxs-lookup"><span data-stu-id="dc18c-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="dc18c-150">**HttpClient** est destinée à être instancié une seule fois et réutilisées tout au long de la durée de vie d’une application.</span><span class="sxs-lookup"><span data-stu-id="dc18c-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="dc18c-151">Les conditions suivantes peuvent entraîner **SocketException** erreurs :</span><span class="sxs-lookup"><span data-stu-id="dc18c-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="dc18c-152">Création d’un nouveau **HttpClient** instance par demande.</span><span class="sxs-lookup"><span data-stu-id="dc18c-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="dc18c-153">Serveur sous une charge importante.</span><span class="sxs-lookup"><span data-stu-id="dc18c-153">Server under heavy load.</span></span>

<span data-ttu-id="dc18c-154">Création d’un nouveau **HttpClient** instance par demande peut épuiser les sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="dc18c-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="dc18c-155">Le code suivant initialise le **HttpClient** instance :</span><span class="sxs-lookup"><span data-stu-id="dc18c-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="dc18c-156">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="dc18c-156">The preceding code:</span></span>

* <span data-ttu-id="dc18c-157">Définit l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="dc18c-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="dc18c-158">Modifier le numéro de port au port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="dc18c-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="dc18c-159">L’application ne fonctionne pas si le port de l’application serveur est utilisé.</span><span class="sxs-lookup"><span data-stu-id="dc18c-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="dc18c-160">Définit l’en-tête Accept par « application/json ».</span><span class="sxs-lookup"><span data-stu-id="dc18c-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="dc18c-161">Définition de cet en-tête indique le serveur pour envoyer des données au format JSON.</span><span class="sxs-lookup"><span data-stu-id="dc18c-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="dc18c-162">Envoie une requête GET pour récupérer une ressource</span><span class="sxs-lookup"><span data-stu-id="dc18c-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="dc18c-163">Le code suivant envoie une demande GET pour un produit :</span><span class="sxs-lookup"><span data-stu-id="dc18c-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="dc18c-164">Le **GetAsync** méthode envoie la requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="dc18c-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="dc18c-165">Quand la méthode se termine, elle retourne un **HttpResponseMessage** qui contient la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="dc18c-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="dc18c-166">Si le code d’état dans la réponse est un code de réussite, le corps de réponse contient la représentation JSON d’un produit.</span><span class="sxs-lookup"><span data-stu-id="dc18c-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="dc18c-167">Appelez **ReadAsAsync** pour désérialiser la charge utile JSON pour un `Product` instance.</span><span class="sxs-lookup"><span data-stu-id="dc18c-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="dc18c-168">Le **ReadAsAsync** méthode est asynchrone, car le corps de réponse peut être arbitrairement grand.</span><span class="sxs-lookup"><span data-stu-id="dc18c-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="dc18c-169">**HttpClient** ne lève pas d’exception lors de la réponse HTTP contient un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="dc18c-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="dc18c-170">Au lieu de cela, le **IsSuccessStatusCode** propriété est **false** si l’état est un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="dc18c-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="dc18c-171">Si vous préférez traiter les codes d’erreur HTTP en tant qu’exceptions, appelez [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) sur l’objet de réponse.</span><span class="sxs-lookup"><span data-stu-id="dc18c-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="dc18c-172">`EnsureSuccessStatusCode` lève une exception si le code d’état se situe en dehors de la plage 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="dc18c-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="dc18c-173">Notez que **HttpClient** peut lever des exceptions pour d’autres raisons &mdash; par exemple, si la demande expire.</span><span class="sxs-lookup"><span data-stu-id="dc18c-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="dc18c-174">Formateurs de Type de média à désérialiser</span><span class="sxs-lookup"><span data-stu-id="dc18c-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="dc18c-175">Lorsque **ReadAsAsync** est appelée sans paramètres, il utilise un ensemble par défaut de *formateurs de médias* pour lire le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="dc18c-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="dc18c-176">Les formateurs par défaut prend en charge JSON, XML et codée en url de formulaire de données.</span><span class="sxs-lookup"><span data-stu-id="dc18c-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="dc18c-177">Au lieu d’utiliser les formateurs par défaut, vous pouvez fournir une liste de formateurs à la **ReadAsAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="dc18c-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="dc18c-178">À l’aide d’une liste de formateurs est utile si vous avez un formateur personnalisé de type de média :</span><span class="sxs-lookup"><span data-stu-id="dc18c-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="dc18c-179">Pour plus d’informations, consultez [formateurs de médias dans ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="dc18c-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="dc18c-180">Envoi d’une demande POST pour créer une ressource</span><span class="sxs-lookup"><span data-stu-id="dc18c-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="dc18c-181">Le code suivant envoie une demande POST qui contient un `Product` instance au format JSON :</span><span class="sxs-lookup"><span data-stu-id="dc18c-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="dc18c-182">Le **PostAsJsonAsync** méthode :</span><span class="sxs-lookup"><span data-stu-id="dc18c-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="dc18c-183">Sérialise un objet au format JSON.</span><span class="sxs-lookup"><span data-stu-id="dc18c-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="dc18c-184">Envoie la charge utile JSON dans une requête POST.</span><span class="sxs-lookup"><span data-stu-id="dc18c-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="dc18c-185">Si la demande réussit :</span><span class="sxs-lookup"><span data-stu-id="dc18c-185">If the request succeeds:</span></span>

* <span data-ttu-id="dc18c-186">Elle doit retourner la réponse 201 (créé).</span><span class="sxs-lookup"><span data-stu-id="dc18c-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="dc18c-187">La réponse doit inclure l’URL, les ressources créées dans l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="dc18c-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="dc18c-188">Envoi d’une demande PUT pour mettre à jour une ressource</span><span class="sxs-lookup"><span data-stu-id="dc18c-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="dc18c-189">Le code suivant envoie une demande PUT pour mettre à jour un produit :</span><span class="sxs-lookup"><span data-stu-id="dc18c-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="dc18c-190">Le **PutAsJsonAsync** méthode fonctionne comme **PostAsJsonAsync**, sauf qu’il envoie une demande PUT au lieu de POST.</span><span class="sxs-lookup"><span data-stu-id="dc18c-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="dc18c-191">Envoi d’une demande de suppression pour supprimer une ressource</span><span class="sxs-lookup"><span data-stu-id="dc18c-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="dc18c-192">Le code suivant envoie une demande de suppression pour supprimer un produit :</span><span class="sxs-lookup"><span data-stu-id="dc18c-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="dc18c-193">Comme GET, une demande de suppression n’a pas un corps de demande.</span><span class="sxs-lookup"><span data-stu-id="dc18c-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="dc18c-194">Vous n’avez pas besoin de spécifier le format JSON ou XML quand la suppression.</span><span class="sxs-lookup"><span data-stu-id="dc18c-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="dc18c-195">Tester l’exemple</span><span class="sxs-lookup"><span data-stu-id="dc18c-195">Test the sample</span></span>

<span data-ttu-id="dc18c-196">Pour tester l’application cliente :</span><span class="sxs-lookup"><span data-stu-id="dc18c-196">To test the client app:</span></span>

1. <span data-ttu-id="dc18c-197">[Télécharger](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) et exécuter l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="dc18c-197">[Download](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="dc18c-198">[Télécharger les instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="dc18c-198">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="dc18c-199">Vérifiez que l’application serveur fonctionne.</span><span class="sxs-lookup"><span data-stu-id="dc18c-199">Verify the server app is working.</span></span> <span data-ttu-id="dc18c-200">Par exemple, `http://localhost:64195/api/products` doit retourner une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="dc18c-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="dc18c-201">Définir l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="dc18c-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="dc18c-202">Modifier le numéro de port au port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="dc18c-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="dc18c-203">Exécutez l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="dc18c-203">Run the client app.</span></span> <span data-ttu-id="dc18c-204">La sortie suivante est produite :</span><span class="sxs-lookup"><span data-stu-id="dc18c-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
