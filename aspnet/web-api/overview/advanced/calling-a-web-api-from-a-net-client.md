---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Appeler une API Web à partir d’un clientC#.net ()-ASP.net 4. x
author: MikeWasson
description: Ce didacticiel montre comment appeler une API Web à partir d’une application .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ab3ba71839123e848dffaa59871f9dac8c1a88d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622618"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="dd9e2-103">Appeler une API Web à partir d’un clientC#.net ()</span><span class="sxs-lookup"><span data-stu-id="dd9e2-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="dd9e2-104">par [Mike Wasson](https://github.com/MikeWasson) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dd9e2-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dd9e2-105">[Télécharger le projet terminé](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="dd9e2-105">[Download Completed Project](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="dd9e2-106">[Télécharger les instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="dd9e2-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="dd9e2-107">Ce didacticiel montre comment appeler une API Web à partir d’une application .NET, à l’aide de [System .net. http. httpclient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="dd9e2-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="dd9e2-108">Dans ce didacticiel, une application cliente est écrite et utilise l’API Web suivante :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="dd9e2-109">Action</span><span class="sxs-lookup"><span data-stu-id="dd9e2-109">Action</span></span> | <span data-ttu-id="dd9e2-110">HTTP method</span><span class="sxs-lookup"><span data-stu-id="dd9e2-110">HTTP method</span></span> | <span data-ttu-id="dd9e2-111">URI relatif</span><span class="sxs-lookup"><span data-stu-id="dd9e2-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dd9e2-112">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="dd9e2-112">Get a product by ID</span></span> | <span data-ttu-id="dd9e2-113">GET</span><span class="sxs-lookup"><span data-stu-id="dd9e2-113">GET</span></span> | <span data-ttu-id="dd9e2-114">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="dd9e2-114">/api/products/*id*</span></span> |
| <span data-ttu-id="dd9e2-115">Créer un produit</span><span class="sxs-lookup"><span data-stu-id="dd9e2-115">Create a new product</span></span> | <span data-ttu-id="dd9e2-116">POST</span><span class="sxs-lookup"><span data-stu-id="dd9e2-116">POST</span></span> | <span data-ttu-id="dd9e2-117">/api/products</span><span class="sxs-lookup"><span data-stu-id="dd9e2-117">/api/products</span></span> |
| <span data-ttu-id="dd9e2-118">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="dd9e2-118">Update a product</span></span> | <span data-ttu-id="dd9e2-119">PUT</span><span class="sxs-lookup"><span data-stu-id="dd9e2-119">PUT</span></span> | <span data-ttu-id="dd9e2-120">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="dd9e2-120">/api/products/*id*</span></span> |
| <span data-ttu-id="dd9e2-121">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="dd9e2-121">Delete a product</span></span> | <span data-ttu-id="dd9e2-122">Suppression</span><span class="sxs-lookup"><span data-stu-id="dd9e2-122">DELETE</span></span> | <span data-ttu-id="dd9e2-123">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="dd9e2-123">/api/products/*id*</span></span> |

<span data-ttu-id="dd9e2-124">Pour savoir comment implémenter cette API avec API Web ASP.NET, consultez [création d’une API Web qui prend en charge les opérations CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="dd9e2-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="dd9e2-125">Pour plus de simplicité, l’application cliente dans ce didacticiel est une application console Windows.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="dd9e2-126">**Httpclient** est également pris en charge pour les applications Windows Phone et Windows Store.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="dd9e2-127">Pour plus d’informations, consultez [écriture de code client d’API Web pour plusieurs plateformes à l’aide de bibliothèques portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="dd9e2-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="dd9e2-128">Création d'application console</span><span class="sxs-lookup"><span data-stu-id="dd9e2-128">Create the Console Application</span></span>

<span data-ttu-id="dd9e2-129">Dans Visual Studio, créez une nouvelle application console Windows nommée **HttpClientSample** et collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="dd9e2-130">Le code précédent est l’application cliente complète.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="dd9e2-131">`RunAsync` s’exécute et se bloque jusqu’à ce qu’il se termine.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="dd9e2-132">La plupart des méthodes **httpclient** sont asynchrones, car elles exécutent des e/s réseau.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="dd9e2-133">Toutes les tâches asynchrones sont effectuées dans `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="dd9e2-134">Normalement, une application ne bloque pas le thread principal, mais cette application n’autorise aucune interaction.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="dd9e2-135">Installer les bibliothèques clientes de l’API Web</span><span class="sxs-lookup"><span data-stu-id="dd9e2-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="dd9e2-136">Utilisez le gestionnaire de package NuGet pour installer le package de bibliothèques clientes de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="dd9e2-137">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="dd9e2-138">Dans la console du gestionnaire de package (PMC), tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="dd9e2-139">La commande précédente ajoute les packages NuGet suivants au projet :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="dd9e2-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="dd9e2-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="dd9e2-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="dd9e2-141">Newtonsoft.Json</span></span>

<span data-ttu-id="dd9e2-142">Netwonsoft. JSON (également appelé Json.NET) est une infrastructure JSON très performante pour .NET.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-142">Netwonsoft.Json (also known as Json.NET) is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="dd9e2-143">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="dd9e2-143">Add a Model Class</span></span>

<span data-ttu-id="dd9e2-144">Examiner la classe `Product` :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="dd9e2-145">Cette classe correspond au modèle de données utilisé par l’API Web.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="dd9e2-146">Une application peut utiliser **httpclient** pour lire une instance de `Product` à partir d’une réponse http.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="dd9e2-147">L’application n’a pas besoin d’écrire de code de désérialisation.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="dd9e2-148">Créer et initialiser HttpClient</span><span class="sxs-lookup"><span data-stu-id="dd9e2-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="dd9e2-149">Examinez la propriété **httpclient** statique :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="dd9e2-150">**Httpclient** est conçu pour être instancié une fois et réutilisé tout au long de la vie d’une application.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="dd9e2-151">Les conditions suivantes peuvent entraîner des erreurs **SocketException** :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="dd9e2-152">Création d’une nouvelle instance **httpclient** par demande.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="dd9e2-153">Serveur soumis à une charge importante.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-153">Server under heavy load.</span></span>

<span data-ttu-id="dd9e2-154">La création d’une nouvelle instance **httpclient** par demande peut épuiser les sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="dd9e2-155">Le code suivant initialise l’instance **httpclient** :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="dd9e2-156">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-156">The preceding code:</span></span>

* <span data-ttu-id="dd9e2-157">Définit l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="dd9e2-158">Remplacez le numéro de port par le port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="dd9e2-159">L’application ne fonctionne pas, sauf si le port de l’application serveur est utilisé.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="dd9e2-160">Définit l’en-tête Accept sur « application/JSON ».</span><span class="sxs-lookup"><span data-stu-id="dd9e2-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="dd9e2-161">La définition de cet en-tête indique au serveur d’envoyer des données au format JSON.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="dd9e2-162">Envoyer une demande d’extraction pour récupérer une ressource</span><span class="sxs-lookup"><span data-stu-id="dd9e2-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="dd9e2-163">Le code suivant envoie une requête d’extraction pour un produit :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="dd9e2-164">La méthode **GetAsync** envoie la requête http.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="dd9e2-165">Lorsque la méthode se termine, elle retourne un **HttpResponseMessage** qui contient la réponse http.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="dd9e2-166">Si le code d’État dans la réponse est un code de réussite, le corps de la réponse contient la représentation JSON d’un produit.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="dd9e2-167">Appelez **ReadAsAsync** pour désérialiser la charge utile JSON en une instance `Product`.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="dd9e2-168">La méthode **ReadAsAsync** est asynchrone, car le corps de la réponse peut être arbitrairement grand.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="dd9e2-169">**Httpclient** ne lève pas d’exception lorsque la réponse http contient un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="dd9e2-170">Au lieu de cela, la propriété **IsSuccessStatusCode** a la **valeur false** si l’État est un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="dd9e2-171">Si vous préférez traiter les codes d’erreur HTTP comme des exceptions, appelez [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) sur l’objet Response.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="dd9e2-172">`EnsureSuccessStatusCode` lève une exception si le code d’État se trouve en dehors de la plage 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="dd9e2-173">Notez que **httpclient** peut lever des exceptions pour d’autres raisons &mdash; par exemple, si la demande expire.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="dd9e2-174">Formateurs de type média à désérialiser</span><span class="sxs-lookup"><span data-stu-id="dd9e2-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="dd9e2-175">Quand **ReadAsAsync** est appelé sans paramètre, il utilise un jeu de *formateurs de médias* par défaut pour lire le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="dd9e2-176">Les formateurs par défaut prennent en charge les données JSON, XML et de type URL de formulaire.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="dd9e2-177">Au lieu d’utiliser les formateurs par défaut, vous pouvez fournir une liste de formateurs à la méthode **ReadAsAsync** .</span><span class="sxs-lookup"><span data-stu-id="dd9e2-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="dd9e2-178">L’utilisation d’une liste de formateurs est utile si vous avez un formateur de type de média personnalisé :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="dd9e2-179">Pour plus d’informations, consultez [formateurs de médias dans API Web ASP.NET 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="dd9e2-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="dd9e2-180">Envoi d’une demande de publication pour créer une ressource</span><span class="sxs-lookup"><span data-stu-id="dd9e2-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="dd9e2-181">Le code suivant envoie une demande de publication contenant une `Product` instance au format JSON :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="dd9e2-182">Méthode **PostAsJsonAsync** :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="dd9e2-183">Sérialise un objet au format JSON.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="dd9e2-184">Envoie la charge utile JSON dans une demande de publication.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="dd9e2-185">Si la demande est réussie :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-185">If the request succeeds:</span></span>

* <span data-ttu-id="dd9e2-186">Elle doit retourner une réponse 201 (créée).</span><span class="sxs-lookup"><span data-stu-id="dd9e2-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="dd9e2-187">La réponse doit inclure l’URL des ressources créées dans l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="dd9e2-188">Envoi d’une demande PUT pour mettre à jour une ressource</span><span class="sxs-lookup"><span data-stu-id="dd9e2-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="dd9e2-189">Le code suivant envoie une demande PUT pour mettre à jour un produit :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="dd9e2-190">La méthode **PutAsJsonAsync** fonctionne comme **PostAsJsonAsync**, à ceci près qu’elle envoie une demande put au lieu de la publication.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="dd9e2-191">Envoi d’une demande DELETE pour supprimer une ressource</span><span class="sxs-lookup"><span data-stu-id="dd9e2-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="dd9e2-192">Le code suivant envoie une demande DELETE pour supprimer un produit :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="dd9e2-193">Comme par exemple, une demande DELETE n’a pas de corps de demande.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="dd9e2-194">Vous n’avez pas besoin de spécifier le format JSON ou XML avec DELETE.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="dd9e2-195">Tester l’exemple</span><span class="sxs-lookup"><span data-stu-id="dd9e2-195">Test the sample</span></span>

<span data-ttu-id="dd9e2-196">Pour tester l’application cliente :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-196">To test the client app:</span></span>

1. <span data-ttu-id="dd9e2-197">[Téléchargez](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) et exécutez l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-197">[Download](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="dd9e2-198">[Télécharger les instructions](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="dd9e2-198">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="dd9e2-199">Vérifiez que l’application serveur fonctionne.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-199">Verify the server app is working.</span></span> <span data-ttu-id="dd9e2-200">Par exemple, `http://localhost:64195/api/products` doit retourner une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="dd9e2-201">Définissez l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="dd9e2-202">Remplacez le numéro de port par le port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="dd9e2-203">Exécutez l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="dd9e2-203">Run the client app.</span></span> <span data-ttu-id="dd9e2-204">La sortie suivante est produite :</span><span class="sxs-lookup"><span data-stu-id="dd9e2-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
