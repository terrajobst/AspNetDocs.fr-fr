---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Prise en charge des relations d’entité dans OData v3 avec l’API Web 2 | Microsoft Docs
author: MikeWasson
description: 'La plupart des jeux de données définissent les relations entre les entités : les clients ont des commandes. les ouvrages ont des auteurs ; les produits ont des fournisseurs. À l’aide d’OData, les clients peuvent naviguer...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598741"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="45593-104">Prise en charge des relations d’entité dans OData v3 avec l’API Web 2</span><span class="sxs-lookup"><span data-stu-id="45593-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>

<span data-ttu-id="45593-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="45593-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="45593-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="45593-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="45593-107">La plupart des jeux de données définissent les relations entre les entités : les clients ont des commandes. les ouvrages ont des auteurs ; les produits ont des fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="45593-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="45593-108">À l’aide d’OData, les clients peuvent naviguer sur les relations d’entité.</span><span class="sxs-lookup"><span data-stu-id="45593-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="45593-109">Pour un produit donné, vous pouvez trouver le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="45593-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="45593-110">Vous pouvez également créer ou supprimer des relations.</span><span class="sxs-lookup"><span data-stu-id="45593-110">You can also create or remove relationships.</span></span> <span data-ttu-id="45593-111">Par exemple, vous pouvez définir le fournisseur d’un produit.</span><span class="sxs-lookup"><span data-stu-id="45593-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="45593-112">Ce didacticiel montre comment prendre en charge ces opérations dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="45593-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="45593-113">Ce didacticiel s’appuie sur le didacticiel [création d’un point de terminaison OData v3 avec l’API Web 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="45593-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="45593-114">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="45593-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="45593-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="45593-115">Web API 2</span></span>
> - <span data-ttu-id="45593-116">OData version 3</span><span class="sxs-lookup"><span data-stu-id="45593-116">OData Version 3</span></span>
> - <span data-ttu-id="45593-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="45593-117">Entity Framework 6</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="45593-118">Ajouter une entité fournisseur</span><span class="sxs-lookup"><span data-stu-id="45593-118">Add a Supplier Entity</span></span>

<span data-ttu-id="45593-119">Tout d’abord, nous devons ajouter un nouveau type d’entité à notre flux OData.</span><span class="sxs-lookup"><span data-stu-id="45593-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="45593-120">Nous allons ajouter une classe `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="45593-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="45593-121">Cette classe utilise une chaîne pour la clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="45593-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="45593-122">Dans la pratique, cela peut être moins courant que l’utilisation d’une clé entière.</span><span class="sxs-lookup"><span data-stu-id="45593-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="45593-123">Toutefois, il est important de voir comment OData gère d’autres types de clés en plus des entiers.</span><span class="sxs-lookup"><span data-stu-id="45593-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="45593-124">Ensuite, nous allons créer une relation en ajoutant une propriété `Supplier` à la classe `Product` :</span><span class="sxs-lookup"><span data-stu-id="45593-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="45593-125">Ajoutez un nouveau **DbSet** à la classe `ProductServiceContext`, afin que Entity Framework inclue la table `Supplier` dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="45593-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="45593-126">Dans WebApiConfig.cs, ajoutez une entité « Suppliers » au modèle EDM :</span><span class="sxs-lookup"><span data-stu-id="45593-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="45593-127">Propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="45593-127">Navigation Properties</span></span>

<span data-ttu-id="45593-128">Pour obtenir le fournisseur d’un produit, le client envoie une demande d’extraction :</span><span class="sxs-lookup"><span data-stu-id="45593-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="45593-129">Ici, « Supplier » est une propriété de navigation sur le type de `Product`.</span><span class="sxs-lookup"><span data-stu-id="45593-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="45593-130">Dans ce cas, `Supplier` fait référence à un seul élément, mais une propriété de navigation peut également retourner une collection (relation un-à-plusieurs ou plusieurs-à-plusieurs).</span><span class="sxs-lookup"><span data-stu-id="45593-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="45593-131">Pour prendre en charge cette demande, ajoutez la méthode suivante à la classe `ProductsController` :</span><span class="sxs-lookup"><span data-stu-id="45593-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="45593-132">Le paramètre de *clé* est la clé du produit.</span><span class="sxs-lookup"><span data-stu-id="45593-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="45593-133">La méthode retourne l’entité&#8212;associée dans ce cas, une instance de `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="45593-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="45593-134">Le nom de la méthode et le nom du paramètre sont tous deux importants.</span><span class="sxs-lookup"><span data-stu-id="45593-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="45593-135">En général, si la propriété de navigation est nommée « X », vous devez ajouter une méthode nommée « GetX ».</span><span class="sxs-lookup"><span data-stu-id="45593-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="45593-136">La méthode doit prendre un paramètre nommé «*Key*» qui correspond au type de données de la clé du parent.</span><span class="sxs-lookup"><span data-stu-id="45593-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="45593-137">Il est également important d’inclure l’attribut **[FromOdataUri]** dans le paramètre de *clé* .</span><span class="sxs-lookup"><span data-stu-id="45593-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="45593-138">Cet attribut indique à l’API Web d’utiliser les règles de syntaxe OData lorsqu’elle analyse la clé à partir de l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="45593-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="45593-139">Création et suppression de liens</span><span class="sxs-lookup"><span data-stu-id="45593-139">Creating and Deleting Links</span></span>

<span data-ttu-id="45593-140">OData prend en charge la création ou la suppression de relations entre deux entités.</span><span class="sxs-lookup"><span data-stu-id="45593-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="45593-141">Dans la terminologie OData, la relation est un « lien ».</span><span class="sxs-lookup"><span data-stu-id="45593-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="45593-142">Chaque lien a un URI au format *entité*/$Links/*entité*.</span><span class="sxs-lookup"><span data-stu-id="45593-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="45593-143">Par exemple, le lien du produit au fournisseur ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="45593-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="45593-144">Pour créer un nouveau lien, le client envoie une demande de publication à l’URI de lien.</span><span class="sxs-lookup"><span data-stu-id="45593-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="45593-145">Le corps de la demande est l’URI de l’entité cible.</span><span class="sxs-lookup"><span data-stu-id="45593-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="45593-146">Par exemple, supposons qu’il existe un fournisseur avec la clé « CTSO ».</span><span class="sxs-lookup"><span data-stu-id="45593-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="45593-147">Pour créer un lien à partir de « Product (1) » vers « Supplier («CTSO »)», le client envoie une requête semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="45593-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="45593-148">Pour supprimer un lien, le client envoie une requête DELETE à l’URI du lien.</span><span class="sxs-lookup"><span data-stu-id="45593-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="45593-149">**Création de liens**</span><span class="sxs-lookup"><span data-stu-id="45593-149">**Creating Links**</span></span>

<span data-ttu-id="45593-150">Pour permettre à un client de créer des liens vers des fournisseurs de produits, ajoutez le code suivant à la classe `ProductsController` :</span><span class="sxs-lookup"><span data-stu-id="45593-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="45593-151">Cette méthode accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="45593-151">This method takes three parameters:</span></span>

- <span data-ttu-id="45593-152">*clé*: clé de l’entité parente (le produit)</span><span class="sxs-lookup"><span data-stu-id="45593-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="45593-153">*navigationProperty*: nom de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="45593-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="45593-154">Dans cet exemple, la seule propriété de navigation valide est « Supplier ».</span><span class="sxs-lookup"><span data-stu-id="45593-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="45593-155">*lien*: URI OData de l’entité associée.</span><span class="sxs-lookup"><span data-stu-id="45593-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="45593-156">Cette valeur est extraite du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="45593-156">This value is taken from the request body.</span></span> <span data-ttu-id="45593-157">Par exemple, l’URI de lien peut être «`http://localhost/odata/Suppliers('CTSO')`, ce qui signifie que le fournisseur avec ID = «CTSO ».</span><span class="sxs-lookup"><span data-stu-id="45593-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="45593-158">La méthode utilise le lien pour rechercher le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="45593-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="45593-159">Si le fournisseur correspondant est trouvé, la méthode définit la propriété `Product.Supplier` et enregistre le résultat dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="45593-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="45593-160">La partie la plus difficile est l’analyse de l’URI de lien.</span><span class="sxs-lookup"><span data-stu-id="45593-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="45593-161">Fondamentalement, vous devez simuler le résultat de l’envoi d’une demande d’extraction à cet URI.</span><span class="sxs-lookup"><span data-stu-id="45593-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="45593-162">La méthode d’assistance suivante indique comment procéder.</span><span class="sxs-lookup"><span data-stu-id="45593-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="45593-163">La méthode appelle le processus de routage de l’API Web et retourne une instance **ODataPath** qui représente le chemin d’accès OData analysé.</span><span class="sxs-lookup"><span data-stu-id="45593-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="45593-164">Pour un URI de lien, l’un des segments doit être la clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="45593-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="45593-165">(Dans le cas contraire, le client a envoyé un URI incorrect.)</span><span class="sxs-lookup"><span data-stu-id="45593-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="45593-166">**Suppression de liens**</span><span class="sxs-lookup"><span data-stu-id="45593-166">**Deleting Links**</span></span>

<span data-ttu-id="45593-167">Pour supprimer un lien, ajoutez le code suivant à la classe `ProductsController` :</span><span class="sxs-lookup"><span data-stu-id="45593-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="45593-168">Dans cet exemple, la propriété de navigation est une entité `Supplier` unique.</span><span class="sxs-lookup"><span data-stu-id="45593-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="45593-169">Si la propriété de navigation est une collection, l’URI pour supprimer un lien doit inclure une clé pour l’entité associée.</span><span class="sxs-lookup"><span data-stu-id="45593-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="45593-170">Exemple :</span><span class="sxs-lookup"><span data-stu-id="45593-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="45593-171">Cette demande supprime la commande 1 du client 1.</span><span class="sxs-lookup"><span data-stu-id="45593-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="45593-172">Dans ce cas, la méthode DeleteLink aura la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="45593-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="45593-173">Le paramètre *relatedKey* indique la clé de l’entité associée.</span><span class="sxs-lookup"><span data-stu-id="45593-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="45593-174">Ainsi, dans votre méthode `DeleteLink`, recherchez l’entité principale par le paramètre de *clé* , recherchez l’entité associée par le paramètre *relatedKey* , puis supprimez l’Association.</span><span class="sxs-lookup"><span data-stu-id="45593-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="45593-175">Selon votre modèle de données, vous devrez peut-être implémenter les deux versions de `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="45593-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="45593-176">L’API Web appellera la version appropriée en fonction de l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="45593-176">Web API will call the correct version based on the request URI.</span></span>
