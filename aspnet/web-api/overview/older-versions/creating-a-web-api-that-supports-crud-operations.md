---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Activation des opérations CRUD dans API Web ASP.NET 1-ASP.NET 4. x
author: MikeWasson
description: Le didacticiel montre comment prendre en charge les opérations CRUD dans un service HTTP à l’aide de API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600339"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="6e7cb-103">Activation des opérations CRUD dans API Web ASP.NET 1</span><span class="sxs-lookup"><span data-stu-id="6e7cb-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="6e7cb-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6e7cb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6e7cb-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="6e7cb-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="6e7cb-106">Ce didacticiel montre comment prendre en charge les opérations CRUD dans un service HTTP à l’aide de API Web ASP.NET pour ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6e7cb-107">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="6e7cb-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6e7cb-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6e7cb-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="6e7cb-109">API Web 1 (fonctionne également avec l’API Web 2)</span><span class="sxs-lookup"><span data-stu-id="6e7cb-109">Web API 1 (also works with Web API 2)</span></span>

<span data-ttu-id="6e7cb-110">CRUD est l’abréviation de &quot;Create, Read, Update et Delete,&quot; qui sont les quatre opérations de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="6e7cb-111">De nombreux services HTTP modélisent également les opérations CRUD via des API REST ou de type REST.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="6e7cb-112">Dans ce didacticiel, vous allez créer une API Web très simple pour gérer une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="6e7cb-113">Chaque produit contient un nom, un prix et une catégorie (par exemple, &quot;Toys&quot; ou &quot;matériel&quot;), ainsi qu’un ID de produit.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="6e7cb-114">L’API Products expose les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="6e7cb-115">Action</span><span class="sxs-lookup"><span data-stu-id="6e7cb-115">Action</span></span> | <span data-ttu-id="6e7cb-116">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="6e7cb-116">HTTP method</span></span> | <span data-ttu-id="6e7cb-117">URI relatif</span><span class="sxs-lookup"><span data-stu-id="6e7cb-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6e7cb-118">Obtenir la liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="6e7cb-118">Get a list of all products</span></span> | <span data-ttu-id="6e7cb-119">GET</span><span class="sxs-lookup"><span data-stu-id="6e7cb-119">GET</span></span> | <span data-ttu-id="6e7cb-120">/api/products</span><span class="sxs-lookup"><span data-stu-id="6e7cb-120">/api/products</span></span> |
| <span data-ttu-id="6e7cb-121">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="6e7cb-121">Get a product by ID</span></span> | <span data-ttu-id="6e7cb-122">GET</span><span class="sxs-lookup"><span data-stu-id="6e7cb-122">GET</span></span> | <span data-ttu-id="6e7cb-123">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="6e7cb-123">/api/products/*id*</span></span> |
| <span data-ttu-id="6e7cb-124">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="6e7cb-124">Get a product by category</span></span> | <span data-ttu-id="6e7cb-125">GET</span><span class="sxs-lookup"><span data-stu-id="6e7cb-125">GET</span></span> | <span data-ttu-id="6e7cb-126">/API/products ? category =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="6e7cb-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="6e7cb-127">Créer un produit</span><span class="sxs-lookup"><span data-stu-id="6e7cb-127">Create a new product</span></span> | <span data-ttu-id="6e7cb-128">Publier</span><span class="sxs-lookup"><span data-stu-id="6e7cb-128">POST</span></span> | <span data-ttu-id="6e7cb-129">/api/products</span><span class="sxs-lookup"><span data-stu-id="6e7cb-129">/api/products</span></span> |
| <span data-ttu-id="6e7cb-130">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="6e7cb-130">Update a product</span></span> | <span data-ttu-id="6e7cb-131">PUT</span><span class="sxs-lookup"><span data-stu-id="6e7cb-131">PUT</span></span> | <span data-ttu-id="6e7cb-132">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="6e7cb-132">/api/products/*id*</span></span> |
| <span data-ttu-id="6e7cb-133">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="6e7cb-133">Delete a product</span></span> | <span data-ttu-id="6e7cb-134">SUPPR</span><span class="sxs-lookup"><span data-stu-id="6e7cb-134">DELETE</span></span> | <span data-ttu-id="6e7cb-135">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="6e7cb-135">/api/products/*id*</span></span> |

<span data-ttu-id="6e7cb-136">Notez que certains des URI incluent l’ID de produit dans path.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="6e7cb-137">Par exemple, pour obtenir le produit dont l’ID est 28, le client envoie une demande d’extraction pour `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="6e7cb-138">Ressources</span><span class="sxs-lookup"><span data-stu-id="6e7cb-138">Resources</span></span>

<span data-ttu-id="6e7cb-139">L’API Products définit des URI pour deux types de ressources :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="6e7cb-140">Ressource</span><span class="sxs-lookup"><span data-stu-id="6e7cb-140">Resource</span></span> | <span data-ttu-id="6e7cb-141">URI</span><span class="sxs-lookup"><span data-stu-id="6e7cb-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="6e7cb-142">Liste de tous les produits.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-142">The list of all the products.</span></span> | <span data-ttu-id="6e7cb-143">/api/products</span><span class="sxs-lookup"><span data-stu-id="6e7cb-143">/api/products</span></span> |
| <span data-ttu-id="6e7cb-144">Un produit individuel.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-144">An individual product.</span></span> | <span data-ttu-id="6e7cb-145">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="6e7cb-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="6e7cb-146">Méthodes</span><span class="sxs-lookup"><span data-stu-id="6e7cb-146">Methods</span></span>

<span data-ttu-id="6e7cb-147">Les quatre méthodes HTTP principales (obtient, PUT, poster et supprimer) peuvent être mappées aux opérations CRUD comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="6e7cb-148">OBTENIR récupère la représentation de la ressource à l’URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="6e7cb-149">L’extraction ne doit pas avoir d’effets secondaires sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="6e7cb-150">PUT met à jour une ressource à l’URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="6e7cb-151">PUT peut également être utilisé pour créer une ressource à un URI spécifié, si le serveur permet aux clients de spécifier de nouveaux URI.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="6e7cb-152">Pour ce didacticiel, l’API ne prend pas en charge la création via PUT.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="6e7cb-153">La publication crée une ressource.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-153">POST creates a new resource.</span></span> <span data-ttu-id="6e7cb-154">Le serveur assigne l’URI pour le nouvel objet et retourne cet URI dans le cadre du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="6e7cb-155">Supprimer supprime une ressource à l’URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="6e7cb-156">Remarque : la méthode PUT remplace l’ensemble de l’entité Product.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="6e7cb-157">Autrement dit, le client est censé envoyer une représentation complète du produit mis à jour.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="6e7cb-158">Si vous souhaitez prendre en charge des mises à jour partielles, il est préférable d’utiliser la méthode PATCH.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="6e7cb-159">Ce didacticiel n’implémente pas les CORRECTIFs.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="6e7cb-160">Créer un projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="6e7cb-160">Create a New Web API Project</span></span>

<span data-ttu-id="6e7cb-161">Commencez par exécuter Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="6e7cb-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="6e7cb-162">Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="6e7cb-163">Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  .</span><span class="sxs-lookup"><span data-stu-id="6e7cb-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="6e7cb-164">Sous **visuel C#** , sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="6e7cb-165">Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="6e7cb-166">Nommez le projet &quot;&quot; ProductStore, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="6e7cb-167">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **API Web** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="6e7cb-168">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="6e7cb-168">Adding a Model</span></span>

<span data-ttu-id="6e7cb-169">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="6e7cb-170">Dans API Web ASP.NET, vous pouvez utiliser des objets CLR fortement typés en tant que modèles, et ils seront automatiquement sérialisés en XML ou JSON pour le client.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="6e7cb-171">Pour l’API ProductStore, nos données sont constituées de produits. nous allons donc créer une nouvelle classe nommée `Product`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="6e7cb-172">Si Explorateur de solutions n’est pas déjà visible, cliquez sur le menu **affichage** et sélectionnez **Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="6e7cb-173">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier **modèles** .</span><span class="sxs-lookup"><span data-stu-id="6e7cb-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="6e7cb-174">Dans le menu contextuel, sélectionnez **Ajouter**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="6e7cb-175">Nommez la classe &quot;&quot;de produit.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="6e7cb-176">Ajoutez les propriétés suivantes à la classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="6e7cb-177">Ajout d'une base de données de référentiel</span><span class="sxs-lookup"><span data-stu-id="6e7cb-177">Adding a Repository</span></span>

<span data-ttu-id="6e7cb-178">Nous devons stocker une collection de produits.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-178">We need to store a collection of products.</span></span> <span data-ttu-id="6e7cb-179">Il est judicieux de séparer la collection de notre implémentation de service.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="6e7cb-180">De cette façon, nous pouvons modifier le magasin de stockage sans réécrire la classe de service.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="6e7cb-181">Ce type de conception est appelé modèle de *référentiel* .</span><span class="sxs-lookup"><span data-stu-id="6e7cb-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="6e7cb-182">Commencez par définir une interface générique pour le référentiel.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="6e7cb-183">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier **modèles** .</span><span class="sxs-lookup"><span data-stu-id="6e7cb-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="6e7cb-184">Sélectionnez **Ajouter**, puis **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="6e7cb-185">Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le C# nœud.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="6e7cb-186">Sous C#, sélectionnez **code**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-186">Under C#, select **Code**.</span></span> <span data-ttu-id="6e7cb-187">Dans la liste des modèles de code, sélectionnez **interface**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="6e7cb-188">Nommez l’interface &quot;&quot;IProductRepository.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="6e7cb-189">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="6e7cb-190">À présent, ajoutez une autre classe au dossier Models, nommé &quot;ProductRepository.&quot; cette classe implémentera l’interface `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="6e7cb-191">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="6e7cb-192">Le référentiel conserve la liste dans la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="6e7cb-193">Cela est correct pour un didacticiel, mais dans une application réelle, vous stockez les données en externe, soit une base de données, soit dans un stockage cloud.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="6e7cb-194">Le modèle de référentiel facilite la modification ultérieure de l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="6e7cb-195">Ajout d’un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="6e7cb-195">Adding a Web API Controller</span></span>

<span data-ttu-id="6e7cb-196">Si vous avez travaillé avec ASP.NET MVC, vous êtes déjà familiarisé avec les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="6e7cb-197">Dans API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes http provenant du client.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="6e7cb-198">L’Assistant Nouveau projet a créé deux contrôleurs pour vous lors de la création du projet.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="6e7cb-199">Pour les afficher, développez le dossier Controllers dans Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="6e7cb-200">HomeController est un contrôleur MVC ASP.NET traditionnel.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="6e7cb-201">Il est responsable de la fourniture des pages HTML pour le site et n’est pas directement lié à notre API Web.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="6e7cb-202">ValuesController est un exemple de contrôleur WebAPI.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="6e7cb-203">Continuez et supprimez ValuesController, en cliquant avec le bouton droit sur le fichier dans Explorateur de solutions et en sélectionnant **Supprimer.**</span><span class="sxs-lookup"><span data-stu-id="6e7cb-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="6e7cb-204">Ajoutez maintenant un nouveau contrôleur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="6e7cb-205">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier Controllers.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="6e7cb-206">Sélectionnez **Ajouter** , puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="6e7cb-207">Dans l’Assistant Ajout d’un **contrôleur** , nommez le contrôleur &quot;&quot;ProductsController.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="6e7cb-208">Dans la liste déroulante **modèle** , sélectionnez **contrôleur d’API vide**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="6e7cb-209">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="6e7cb-210">Il n’est pas nécessaire de placer vos contrôleurs dans un dossier nommé contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="6e7cb-211">Le nom du dossier n’est pas important. Il s’agit simplement d’un moyen pratique d’organiser vos fichiers sources.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>

<span data-ttu-id="6e7cb-212">L’Assistant Ajout d’un **contrôleur** crée un fichier nommé ProductsController.cs dans le dossier Controllers.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="6e7cb-213">Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="6e7cb-214">Ajoutez l’instruction **using** suivante :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="6e7cb-215">Ajoutez un champ qui contient une instance de **IProductRepository** .</span><span class="sxs-lookup"><span data-stu-id="6e7cb-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="6e7cb-216">L’appel de `new ProductRepository()` dans le contrôleur n’est pas la meilleure conception, car il associe le contrôleur à une implémentation particulière de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="6e7cb-217">Pour une meilleure approche, consultez [utilisation du programme de résolution des dépendances de l’API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6e7cb-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>

## <a name="getting-a-resource"></a><span data-ttu-id="6e7cb-218">Obtention d’une ressource</span><span class="sxs-lookup"><span data-stu-id="6e7cb-218">Getting a Resource</span></span>

<span data-ttu-id="6e7cb-219">L’API ProductStore expose plusieurs &quot;actions de lecture&quot; en tant que méthodes HTTP d’extraction.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="6e7cb-220">Chaque action correspond à une méthode dans la classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="6e7cb-221">Action</span><span class="sxs-lookup"><span data-stu-id="6e7cb-221">Action</span></span> | <span data-ttu-id="6e7cb-222">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="6e7cb-222">HTTP method</span></span> | <span data-ttu-id="6e7cb-223">URI relatif</span><span class="sxs-lookup"><span data-stu-id="6e7cb-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6e7cb-224">Obtenir la liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="6e7cb-224">Get a list of all products</span></span> | <span data-ttu-id="6e7cb-225">GET</span><span class="sxs-lookup"><span data-stu-id="6e7cb-225">GET</span></span> | <span data-ttu-id="6e7cb-226">/api/products</span><span class="sxs-lookup"><span data-stu-id="6e7cb-226">/api/products</span></span> |
| <span data-ttu-id="6e7cb-227">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="6e7cb-227">Get a product by ID</span></span> | <span data-ttu-id="6e7cb-228">GET</span><span class="sxs-lookup"><span data-stu-id="6e7cb-228">GET</span></span> | <span data-ttu-id="6e7cb-229">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="6e7cb-229">/api/products/*id*</span></span> |
| <span data-ttu-id="6e7cb-230">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="6e7cb-230">Get a product by category</span></span> | <span data-ttu-id="6e7cb-231">GET</span><span class="sxs-lookup"><span data-stu-id="6e7cb-231">GET</span></span> | <span data-ttu-id="6e7cb-232">/API/products ? category =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="6e7cb-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="6e7cb-233">Pour afficher la liste de tous les produits, ajoutez cette méthode à la classe `ProductsController` :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="6e7cb-234">Le nom de la méthode commence par &quot;obtient&quot;, donc par convention il est mappé aux demandes d’extraction.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="6e7cb-235">En outre, étant donné que la méthode n’a aucun paramètre, elle est mappée à un URI qui ne contient pas d' *ID de&quot;&quot;* segment dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="6e7cb-236">Pour obtenir un produit par ID, ajoutez cette méthode à la classe `ProductsController` :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="6e7cb-237">Ce nom de méthode commence également par &quot;obtenir&quot;, mais la méthode a un paramètre nommé *ID*. Ce paramètre est mappé à l’ID de &quot;&quot; segment du chemin d’accès de l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="6e7cb-238">L’infrastructure de API Web ASP.NET convertit automatiquement l’ID en type de données correct (**int**) pour le paramètre.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="6e7cb-239">La méthode GetProduct lève une exception de type **HttpResponseException** si l' *ID* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="6e7cb-240">Cette exception sera traduite par l’infrastructure en erreur 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="6e7cb-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="6e7cb-241">Enfin, ajoutez une méthode pour rechercher les produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="6e7cb-242">Si l’URI de la demande a une chaîne de requête, l’API Web tente de faire correspondre les paramètres de requête aux paramètres de la méthode du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="6e7cb-243">Par conséquent, un URI de la forme « API/products ? category =*Category*» est mappé à cette méthode.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="6e7cb-244">Création d’une ressource</span><span class="sxs-lookup"><span data-stu-id="6e7cb-244">Creating a Resource</span></span>

<span data-ttu-id="6e7cb-245">Ensuite, nous allons ajouter une méthode à la classe `ProductsController` pour créer un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="6e7cb-246">Voici une implémentation simple de la méthode :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="6e7cb-247">Notez deux choses à propos de cette méthode :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-247">Note two things about this method:</span></span>

- <span data-ttu-id="6e7cb-248">Le nom de la méthode commence par &quot;billet...&quot;.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="6e7cb-249">Pour créer un nouveau produit, le client envoie une requête HTTP après.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="6e7cb-250">La méthode prend un paramètre de type Product.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="6e7cb-251">Dans l’API Web, les paramètres avec des types complexes sont désérialisés à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="6e7cb-252">Par conséquent, nous pensons que le client envoie une représentation sérialisée d’un objet de produit au format XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="6e7cb-253">Cette implémentation fonctionnera, mais elle n’est pas tout à fait complète.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="6e7cb-254">Dans l’idéal, nous aimerions que la réponse HTTP inclue les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="6e7cb-255">**Code de réponse :** Par défaut, l’infrastructure d’API Web affecte au code d’état de réponse la valeur 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="6e7cb-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="6e7cb-256">Toutefois, selon le protocole HTTP/1.1, lorsqu’une requête de publication entraîne la création d’une ressource, le serveur doit répondre avec l’État 201 (créé).</span><span class="sxs-lookup"><span data-stu-id="6e7cb-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="6e7cb-257">**Emplacement :** Lorsque le serveur crée une ressource, il doit inclure l’URI de la nouvelle ressource dans l’en-tête Location de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="6e7cb-258">API Web ASP.NET permet de manipuler facilement le message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="6e7cb-259">Voici l’implémentation améliorée :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="6e7cb-260">Notez que le type de retour de la méthode est maintenant **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="6e7cb-261">En retournant un **HttpResponseMessage** au lieu d’un produit, nous pouvons contrôler les détails du message de réponse http, notamment le code d’État et l’en-tête d’emplacement.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="6e7cb-262">La méthode **CreateResponse** crée un **HttpResponseMessage** et écrit automatiquement une représentation sérialisée de l’objet Product dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="6e7cb-263">Cet exemple ne valide pas la `Product`.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="6e7cb-264">Pour plus d’informations sur la validation de modèle, consultez [validation de modèle dans API Web ASP.net](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6e7cb-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

## <a name="updating-a-resource"></a><span data-ttu-id="6e7cb-265">Mise à jour d’une ressource</span><span class="sxs-lookup"><span data-stu-id="6e7cb-265">Updating a Resource</span></span>

<span data-ttu-id="6e7cb-266">La mise à jour d’un produit avec PUT est simple :</span><span class="sxs-lookup"><span data-stu-id="6e7cb-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="6e7cb-267">Le nom de la méthode commence par &quot;put...&quot;, de sorte que l’API Web la met en correspondance pour placer les demandes.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="6e7cb-268">La méthode accepte deux paramètres, l’ID de produit et le produit mis à jour.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="6e7cb-269">Le paramètre *ID* est extrait du chemin d’accès à l’URI et le paramètre *Product* est désérialisé à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="6e7cb-270">Par défaut, le API Web ASP.NET Framework prend des types de paramètres simples de l’itinéraire et des types complexes du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="6e7cb-271">Suppression d’une ressource</span><span class="sxs-lookup"><span data-stu-id="6e7cb-271">Deleting a Resource</span></span>

<span data-ttu-id="6e7cb-272">Pour supprimer une ressource, définissez une « suppression... » méthode.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="6e7cb-273">Si une demande de suppression est réussie, elle peut retourner l’état 200 (OK) avec un corps d’entité qui décrit l’État. État 202 (accepté) si la suppression est toujours en attente ; ou état 204 (aucun contenu) sans corps d’entité.</span><span class="sxs-lookup"><span data-stu-id="6e7cb-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="6e7cb-274">Dans ce cas, la méthode `DeleteProduct` a un type de retour `void`, donc API Web ASP.NET le convertit automatiquement en code d’État 204 (aucun contenu).</span><span class="sxs-lookup"><span data-stu-id="6e7cb-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
