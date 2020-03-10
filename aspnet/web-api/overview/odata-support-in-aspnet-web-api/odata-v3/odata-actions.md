---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Prise en charge des actions OData dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'Dans OData, les actions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités. Certaines utilisations des actions incluent : implémenter...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556356"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="c24ee-104">Prise en charge des actions OData dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="c24ee-104">Supporting OData Actions in ASP.NET Web API 2</span></span>

<span data-ttu-id="c24ee-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c24ee-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c24ee-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="c24ee-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="c24ee-107">Dans OData, les *actions* sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités.</span><span class="sxs-lookup"><span data-stu-id="c24ee-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="c24ee-108">Certaines utilisations des actions sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="c24ee-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="c24ee-109">Implémentation de transactions complexes.</span><span class="sxs-lookup"><span data-stu-id="c24ee-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="c24ee-110">Manipulation de plusieurs entités à la fois.</span><span class="sxs-lookup"><span data-stu-id="c24ee-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="c24ee-111">Autorisation des mises à jour uniquement pour certaines propriétés d’une entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="c24ee-112">Envoi d’informations au serveur qui n’est pas défini dans une entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c24ee-113">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="c24ee-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c24ee-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="c24ee-114">Web API 2</span></span>
> - <span data-ttu-id="c24ee-115">OData version 3</span><span class="sxs-lookup"><span data-stu-id="c24ee-115">OData Version 3</span></span>
> - <span data-ttu-id="c24ee-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c24ee-116">Entity Framework 6</span></span>

## <a name="example-rating-a-product"></a><span data-ttu-id="c24ee-117">Exemple : évaluation d’un produit</span><span class="sxs-lookup"><span data-stu-id="c24ee-117">Example: Rating a Product</span></span>

<span data-ttu-id="c24ee-118">Dans cet exemple, nous souhaitons permettre aux utilisateurs de évaluer les produits, puis d’exposer les évaluations moyennes pour chaque produit.</span><span class="sxs-lookup"><span data-stu-id="c24ee-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="c24ee-119">Sur la base de données, nous stockerons une liste de classifications, indexées sur les produits.</span><span class="sxs-lookup"><span data-stu-id="c24ee-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="c24ee-120">Voici le modèle que nous pouvons utiliser pour représenter les évaluations dans Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="c24ee-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="c24ee-121">Mais nous ne voulons pas que les clients PUBLIEnt un objet `ProductRating` dans une collection « Ratings ».</span><span class="sxs-lookup"><span data-stu-id="c24ee-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="c24ee-122">Intuitivement, l’évaluation est associée à la collection Products et le client doit uniquement poster la valeur d’évaluation.</span><span class="sxs-lookup"><span data-stu-id="c24ee-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="c24ee-123">Par conséquent, au lieu d’utiliser les opérations CRUD normales, nous définissons une action qu’un client peut appeler sur un produit.</span><span class="sxs-lookup"><span data-stu-id="c24ee-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="c24ee-124">Dans la terminologie OData, l’action est *liée* aux entités Product.</span><span class="sxs-lookup"><span data-stu-id="c24ee-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="c24ee-125">Les actions ont des effets secondaires sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c24ee-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="c24ee-126">Pour cette raison, elles sont appelées à l’aide de requêtes HTTP HTTP.</span><span class="sxs-lookup"><span data-stu-id="c24ee-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="c24ee-127">Les actions peuvent avoir des paramètres et des types de retour, qui sont décrits dans les métadonnées du service.</span><span class="sxs-lookup"><span data-stu-id="c24ee-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="c24ee-128">Le client envoie les paramètres dans le corps de la demande, et le serveur envoie la valeur de retour dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="c24ee-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="c24ee-129">Pour appeler l’action « taux de produit », le client envoie un billet à un URI comme suit :</span><span class="sxs-lookup"><span data-stu-id="c24ee-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="c24ee-130">Les données de la demande de publication sont simplement le classement du produit :</span><span class="sxs-lookup"><span data-stu-id="c24ee-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="c24ee-131">Déclarez l’action dans le Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="c24ee-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="c24ee-132">Dans la configuration de votre API Web, ajoutez l’action à l’EDM (Entity Data Model) :</span><span class="sxs-lookup"><span data-stu-id="c24ee-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="c24ee-133">Ce code définit « RateProduct » en tant qu’action qui peut être effectuée sur les entités Product.</span><span class="sxs-lookup"><span data-stu-id="c24ee-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="c24ee-134">Elle déclare également que l’action prend un paramètre **int** nommé « Rating » et retourne une valeur **int** .</span><span class="sxs-lookup"><span data-stu-id="c24ee-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="c24ee-135">Ajouter l’action au contrôleur</span><span class="sxs-lookup"><span data-stu-id="c24ee-135">Add the Action to the Controller</span></span>

<span data-ttu-id="c24ee-136">L’action « RateProduct » est liée aux entités Product.</span><span class="sxs-lookup"><span data-stu-id="c24ee-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="c24ee-137">Pour implémenter l’action, ajoutez une méthode nommée `RateProduct` au contrôleur Products :</span><span class="sxs-lookup"><span data-stu-id="c24ee-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="c24ee-138">Notez que le nom de la méthode correspond au nom de l’action dans le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="c24ee-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="c24ee-139">La méthode a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="c24ee-139">The method has two parameters:</span></span>

- <span data-ttu-id="c24ee-140">*clé*: clé du produit à évaluer.</span><span class="sxs-lookup"><span data-stu-id="c24ee-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="c24ee-141">*Parameters*: dictionnaire de valeurs de paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="c24ee-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="c24ee-142">Si vous utilisez les conventions de routage par défaut, le paramètre de clé doit être nommé « Key ».</span><span class="sxs-lookup"><span data-stu-id="c24ee-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="c24ee-143">Il est également important d’inclure l’attribut **[FromOdataUri]** , comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="c24ee-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="c24ee-144">Cet attribut indique à l’API Web d’utiliser les règles de syntaxe OData lorsqu’elle analyse la clé à partir de l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="c24ee-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="c24ee-145">Utilisez le dictionnaire de *paramètres* pour récupérer les paramètres d’action :</span><span class="sxs-lookup"><span data-stu-id="c24ee-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="c24ee-146">Si le client envoie les paramètres d’action dans le format correct, la valeur de **ModelState. IsValid** est true.</span><span class="sxs-lookup"><span data-stu-id="c24ee-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="c24ee-147">Dans ce cas, vous pouvez utiliser le dictionnaire **ODataActionParameters** pour récupérer les valeurs des paramètres.</span><span class="sxs-lookup"><span data-stu-id="c24ee-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="c24ee-148">Dans cet exemple, l’action `RateProduct` accepte un seul paramètre nommé « Rating ».</span><span class="sxs-lookup"><span data-stu-id="c24ee-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="c24ee-149">Métadonnées d’action</span><span class="sxs-lookup"><span data-stu-id="c24ee-149">Action Metadata</span></span>

<span data-ttu-id="c24ee-150">Pour afficher les métadonnées du service, envoyez une demande de récupération à/OData/$metadata.</span><span class="sxs-lookup"><span data-stu-id="c24ee-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="c24ee-151">Voici la partie des métadonnées qui déclare l’action `RateProduct` :</span><span class="sxs-lookup"><span data-stu-id="c24ee-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="c24ee-152">L’élément **FunctionImport** déclare l’action.</span><span class="sxs-lookup"><span data-stu-id="c24ee-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="c24ee-153">La plupart des champs sont explicites, mais deux à noter :</span><span class="sxs-lookup"><span data-stu-id="c24ee-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="c24ee-154">**IsBindable** signifie que l’action peut être appelée sur l’entité cible, au moins une partie du temps.</span><span class="sxs-lookup"><span data-stu-id="c24ee-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="c24ee-155">**IsAlwaysBindable** signifie que l’action peut toujours être appelée sur l’entité cible.</span><span class="sxs-lookup"><span data-stu-id="c24ee-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="c24ee-156">La différence est que certaines actions sont toujours disponibles pour les clients, mais d’autres peuvent dépendre de l’état de l’entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="c24ee-157">Supposons, par exemple, que vous définissiez une action « acheter ».</span><span class="sxs-lookup"><span data-stu-id="c24ee-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="c24ee-158">Vous ne pouvez acheter qu’un article en stock.</span><span class="sxs-lookup"><span data-stu-id="c24ee-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="c24ee-159">Si l’élément est en rupture de stock, un client ne peut pas appeler cette action.</span><span class="sxs-lookup"><span data-stu-id="c24ee-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="c24ee-160">Lorsque vous définissez le modèle EDM, la méthode d' **action** crée une action toujours pouvant être liée :</span><span class="sxs-lookup"><span data-stu-id="c24ee-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="c24ee-161">Je parlerai des actions non toujours liées (également appelées actions *transitoires* ) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="c24ee-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="c24ee-162">Appel de l’action</span><span class="sxs-lookup"><span data-stu-id="c24ee-162">Invoking the Action</span></span>

<span data-ttu-id="c24ee-163">Voyons maintenant comment un client appelle cette action.</span><span class="sxs-lookup"><span data-stu-id="c24ee-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="c24ee-164">Supposons que le client souhaite attribuer une évaluation de 2 au produit avec ID = 4.</span><span class="sxs-lookup"><span data-stu-id="c24ee-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="c24ee-165">Voici un exemple de message de demande, à l’aide du format JSON pour le corps de la demande :</span><span class="sxs-lookup"><span data-stu-id="c24ee-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="c24ee-166">Voici le message de réponse :</span><span class="sxs-lookup"><span data-stu-id="c24ee-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="c24ee-167">Liaison d’une action à un jeu d’entités</span><span class="sxs-lookup"><span data-stu-id="c24ee-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="c24ee-168">Dans l’exemple précédent, l’action est liée à une seule entité : le client évalue un produit unique.</span><span class="sxs-lookup"><span data-stu-id="c24ee-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="c24ee-169">Vous pouvez également lier une action à une collection d’entités.</span><span class="sxs-lookup"><span data-stu-id="c24ee-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="c24ee-170">Effectuez simplement les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="c24ee-170">Just make the following changes:</span></span>

<span data-ttu-id="c24ee-171">Dans le modèle EDM, ajoutez l’action à la propriété de **collection** de l’entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="c24ee-172">Dans la méthode Controller, omettez le paramètre *Key* .</span><span class="sxs-lookup"><span data-stu-id="c24ee-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="c24ee-173">À présent, le client appelle l’action sur le jeu d’entités Products :</span><span class="sxs-lookup"><span data-stu-id="c24ee-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="c24ee-174">Actions avec paramètres de regroupement</span><span class="sxs-lookup"><span data-stu-id="c24ee-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="c24ee-175">Les actions peuvent avoir des paramètres qui acceptent une collection de valeurs.</span><span class="sxs-lookup"><span data-stu-id="c24ee-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="c24ee-176">Dans le modèle EDM, utilisez **CollectionParameter&lt;t&gt;** pour déclarer le paramètre.</span><span class="sxs-lookup"><span data-stu-id="c24ee-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="c24ee-177">Cela déclare un paramètre nommé « ratings » qui prend une collection de valeurs **int** .</span><span class="sxs-lookup"><span data-stu-id="c24ee-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="c24ee-178">Dans la méthode Controller, vous recevez toujours la valeur de paramètre de l’objet **ODataActionParameters** , mais maintenant la valeur est une valeur **ICollection&lt;int&gt;** :</span><span class="sxs-lookup"><span data-stu-id="c24ee-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="c24ee-179">Actions transitoires</span><span class="sxs-lookup"><span data-stu-id="c24ee-179">Transient Actions</span></span>

<span data-ttu-id="c24ee-180">Dans l’exemple « RateProduct », les utilisateurs peuvent toujours évaluer un produit, de sorte que l’action est toujours disponible.</span><span class="sxs-lookup"><span data-stu-id="c24ee-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="c24ee-181">Toutefois, certaines actions dépendent de l’état de l’entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="c24ee-182">Par exemple, dans un service de location vidéo, l’action « CheckOut » n’est pas toujours disponible.</span><span class="sxs-lookup"><span data-stu-id="c24ee-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="c24ee-183">(Cela dépend de la disponibilité d’une copie de cette vidéo.) Ce type d’action est appelé action *transitoire* .</span><span class="sxs-lookup"><span data-stu-id="c24ee-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="c24ee-184">Dans les métadonnées du service, une action temporaire a **IsAlwaysBindable** égal à false.</span><span class="sxs-lookup"><span data-stu-id="c24ee-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="c24ee-185">Il s’agit en fait de la valeur par défaut, de sorte que les métadonnées ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="c24ee-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="c24ee-186">Voici pourquoi cela est important : si une action est temporaire, le serveur doit indiquer au client quand l’action est disponible.</span><span class="sxs-lookup"><span data-stu-id="c24ee-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="c24ee-187">Pour ce faire, il inclut un lien vers l’action dans l’entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="c24ee-188">Voici un exemple pour une entité de film :</span><span class="sxs-lookup"><span data-stu-id="c24ee-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="c24ee-189">La propriété « #CheckOut » contient un lien vers l’action d’extraction.</span><span class="sxs-lookup"><span data-stu-id="c24ee-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="c24ee-190">Si l’action n’est pas disponible, le serveur omet le lien.</span><span class="sxs-lookup"><span data-stu-id="c24ee-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="c24ee-191">Pour déclarer une action transitoire dans le modèle EDM, appelez la méthode **TransientAction** :</span><span class="sxs-lookup"><span data-stu-id="c24ee-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="c24ee-192">Vous devez également fournir une fonction qui retourne un lien d’action pour une entité donnée.</span><span class="sxs-lookup"><span data-stu-id="c24ee-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="c24ee-193">Définissez cette fonction en appelant **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="c24ee-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="c24ee-194">Vous pouvez écrire la fonction en tant qu’expression lambda :</span><span class="sxs-lookup"><span data-stu-id="c24ee-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="c24ee-195">Si l’action est disponible, l’expression lambda retourne un lien vers l’action.</span><span class="sxs-lookup"><span data-stu-id="c24ee-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="c24ee-196">Le sérialiseur OData comprend ce lien lorsqu’il sérialise l’entité.</span><span class="sxs-lookup"><span data-stu-id="c24ee-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="c24ee-197">Lorsque l’action n’est pas disponible, la fonction retourne `null`.</span><span class="sxs-lookup"><span data-stu-id="c24ee-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c24ee-198">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c24ee-198">Additional Resources</span></span>

[<span data-ttu-id="c24ee-199">Exemple d’actions OData</span><span class="sxs-lookup"><span data-stu-id="c24ee-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
