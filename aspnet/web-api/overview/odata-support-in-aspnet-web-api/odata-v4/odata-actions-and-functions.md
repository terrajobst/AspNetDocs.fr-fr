---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Actions et fonctions dans OData v4 à l’aide de API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Dans OData, les actions et les fonctions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités. Ce didacticiel montre comment...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556223"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="5b2f7-104">Actions et fonctions dans OData v4 à l’aide de API Web ASP.NET 2,2</span><span class="sxs-lookup"><span data-stu-id="5b2f7-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="5b2f7-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5b2f7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5b2f7-106">Dans OData, les actions et les fonctions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="5b2f7-107">Ce didacticiel montre comment ajouter des actions et des fonctions à un point de terminaison OData v4, à l’aide de l’API Web 2,2.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="5b2f7-108">Le didacticiel s’appuie sur le didacticiel [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="5b2f7-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5b2f7-109">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5b2f7-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="5b2f7-110">API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="5b2f7-110">Web API 2.2</span></span>
> - <span data-ttu-id="5b2f7-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="5b2f7-111">OData v4</span></span>
> - <span data-ttu-id="5b2f7-112">Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="5b2f7-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="5b2f7-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5b2f7-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="5b2f7-114">Versions du didacticiel</span><span class="sxs-lookup"><span data-stu-id="5b2f7-114">Tutorial versions</span></span>
>
> <span data-ttu-id="5b2f7-115">Pour OData version 3, consultez [actions OData dans API Web ASP.NET 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="5b2f7-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="5b2f7-116">La différence entre les *actions* et les *fonctions* est que les actions peuvent avoir des effets secondaires, et non des fonctions.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="5b2f7-117">Les actions et les fonctions peuvent retourner des données.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-117">Both actions and functions can return data.</span></span> <span data-ttu-id="5b2f7-118">Certaines utilisations des actions sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-118">Some uses for actions include:</span></span>

- <span data-ttu-id="5b2f7-119">Transactions complexes.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-119">Complex transactions.</span></span>
- <span data-ttu-id="5b2f7-120">Manipulation de plusieurs entités à la fois.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="5b2f7-121">Autorisation des mises à jour uniquement pour certaines propriétés d’une entité.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="5b2f7-122">Envoi de données qui ne sont pas une entité.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="5b2f7-123">Les fonctions sont utiles pour retourner des informations qui ne correspondent pas directement à une entité ou à une collection.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="5b2f7-124">Une action (ou fonction) peut cibler une entité ou une collection unique.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="5b2f7-125">Dans la terminologie OData, il s’agit de la *liaison*.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="5b2f7-126">Vous pouvez également disposer d' &quot;&quot; actions/fonctions indépendantes, qui sont appelées comme des opérations statiques sur le service.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="5b2f7-127">Exemple : ajout d’une action</span><span class="sxs-lookup"><span data-stu-id="5b2f7-127">Example: Adding an Action</span></span>

<span data-ttu-id="5b2f7-128">Nous allons définir une action pour évaluer un produit.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="5b2f7-129">Ce didacticiel s’appuie sur le didacticiel [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="5b2f7-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="5b2f7-130">Tout d’abord, ajoutez un modèle de `ProductRating` pour représenter les évaluations.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="5b2f7-131">Ajoutez également un **DbSet** à la classe `ProductsContext`, afin que EF crée une table d’évaluation dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="5b2f7-132">Ajouter l’action à l’EDM</span><span class="sxs-lookup"><span data-stu-id="5b2f7-132">Add the Action to the EDM</span></span>

<span data-ttu-id="5b2f7-133">Dans WebApiConfig.cs, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="5b2f7-134">La méthode **EntityTypeConfiguration. action** ajoute une action au modèle EDM (Entity Data Model).</span><span class="sxs-lookup"><span data-stu-id="5b2f7-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="5b2f7-135">La méthode de **paramètre** spécifie un paramètre typé pour l’action.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="5b2f7-136">Ce code définit également l’espace de noms pour le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="5b2f7-137">L’espace de noms est important car l’URI de l’action comprend le nom d’action complet :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="5b2f7-138">Dans une configuration IIS standard, le point dans cette URL entraîne le renvoi de l’erreur 404 par IIS.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="5b2f7-139">Pour résoudre ce risque, ajoutez la section suivante à votre fichier Web. config :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="5b2f7-140">Ajouter une méthode de contrôleur pour l’action</span><span class="sxs-lookup"><span data-stu-id="5b2f7-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="5b2f7-141">Pour activer l’action&quot; rate &quot;, ajoutez la méthode suivante à `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="5b2f7-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="5b2f7-142">Notez que le nom de la méthode correspond au nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="5b2f7-143">L’attribut **[HttpPost]** spécifie que la méthode est une méthode http postale.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="5b2f7-144">Pour appeler l’action, le client envoie une requête HTTP HTTP semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="5b2f7-145">L’action &quot;rate&quot; est liée aux instances de produit. l’URI de l’action est donc le nom d’action complet ajouté à l’URI de l’entité.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="5b2f7-146">(Rappelez-vous que nous définissons l’espace de noms EDM sur &quot;ProductService&quot;, le nom d’action complet est donc &quot;ProductService. rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="5b2f7-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="5b2f7-147">Le corps de la requête contient les paramètres d’action sous la forme d’une charge utile JSON.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="5b2f7-148">L’API Web convertit automatiquement la charge utile JSON en objet **ODataActionParameters** , qui est simplement un dictionnaire de valeurs de paramètre.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="5b2f7-149">Utilisez ce dictionnaire pour accéder aux paramètres de votre méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="5b2f7-150">Si le client envoie les paramètres d’action dans un format incorrect, la valeur de **ModelState. IsValid** est false.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="5b2f7-151">Cochez cet indicateur dans votre méthode de contrôleur et retournez une erreur si **IsValid** a la valeur false.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="5b2f7-152">Exemple : ajout d’une fonction</span><span class="sxs-lookup"><span data-stu-id="5b2f7-152">Example: Adding a Function</span></span>

<span data-ttu-id="5b2f7-153">À présent, nous allons ajouter une fonction OData qui retourne le produit le plus onéreux.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="5b2f7-154">Comme précédemment, la première étape consiste à ajouter la fonction à l’EDM.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="5b2f7-155">Dans WebApiConfig.cs, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="5b2f7-156">Dans ce cas, la fonction est liée à la collection Products, plutôt qu’à des instances de produit individuelles.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="5b2f7-157">Les clients appellent la fonction en envoyant une requête d’extraction :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="5b2f7-158">Voici la méthode de contrôleur pour cette fonction :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="5b2f7-159">Notez que le nom de la méthode correspond au nom de la fonction.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="5b2f7-160">L’attribut **[HttpGet]** spécifie que la méthode est une méthode http d’extraction.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="5b2f7-161">Voici la réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="5b2f7-162">Exemple : ajout d’une fonction indépendante</span><span class="sxs-lookup"><span data-stu-id="5b2f7-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="5b2f7-163">L’exemple précédent était une fonction liée à une collection.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="5b2f7-164">Dans l’exemple suivant, nous allons créer une fonction *indépendante* .</span><span class="sxs-lookup"><span data-stu-id="5b2f7-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="5b2f7-165">Les fonctions indépendantes sont appelées en tant qu’opérations statiques sur le service.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="5b2f7-166">Dans cet exemple, la fonction retourne les taxes de vente pour un code postal donné.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="5b2f7-167">Dans le fichier WebApiConfig, ajoutez la fonction au modèle EDM :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="5b2f7-168">Notez que nous appelons la **fonction** directement sur le **ODataModelBuilder**, au lieu du type d’entité ou de la collection.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="5b2f7-169">Cela indique au générateur de modèles que la fonction est détachée.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="5b2f7-170">Voici la méthode de contrôleur qui implémente la fonction :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="5b2f7-171">Peu importe le contrôleur d’API Web dans lequel vous placez cette méthode.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="5b2f7-172">Vous pouvez le placer dans `ProductsController`ou définir un contrôleur séparé.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="5b2f7-173">L’attribut **[ODataRoute]** définit le modèle d’URI pour la fonction.</span><span class="sxs-lookup"><span data-stu-id="5b2f7-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="5b2f7-174">Voici un exemple de demande de client :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="5b2f7-175">Réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="5b2f7-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
