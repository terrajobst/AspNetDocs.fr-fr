---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Activation des opérations CRUD dans ASP.NET Web API 1 - ASP.NET 4.x
author: MikeWasson
description: Didacticiel montre comment prendre en charge des opérations CRUD dans un service HTTP à l’aide des API Web ASP.NET pour ASP.NET 4.x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 855c3fa35d82173c87d13adb51e10fd13698ade5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381352"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="04990-103">Activation des opérations CRUD dans ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="04990-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="04990-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="04990-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="04990-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="04990-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="04990-106">Ce didacticiel montre comment prendre en charge des opérations CRUD dans un service HTTP à l’aide des API Web ASP.NET pour ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="04990-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="04990-107">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="04990-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="04990-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="04990-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="04990-109">Web API 1 (fonctionne également avec l’API Web 2)</span><span class="sxs-lookup"><span data-stu-id="04990-109">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="04990-110">CRUD est l’acronyme &quot;Create, Read, Update et Delete,&quot; qui sont les quatre opérations de base de données.</span><span class="sxs-lookup"><span data-stu-id="04990-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="04990-111">De nombreux services HTTP également les opérations CRUD via REST ou API REST de type de modèle.</span><span class="sxs-lookup"><span data-stu-id="04990-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="04990-112">Dans ce didacticiel, vous allez générer une API pour gérer une liste de produits de web très simple.</span><span class="sxs-lookup"><span data-stu-id="04990-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="04990-113">Chaque produit contient un nom, le prix et la catégorie (tel que &quot;toys&quot; ou &quot;matériel&quot;), ainsi que d’un ID de produit.</span><span class="sxs-lookup"><span data-stu-id="04990-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="04990-114">Les produits API exposera suivant des méthodes.</span><span class="sxs-lookup"><span data-stu-id="04990-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="04990-115">Action</span><span class="sxs-lookup"><span data-stu-id="04990-115">Action</span></span> | <span data-ttu-id="04990-116">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="04990-116">HTTP method</span></span> | <span data-ttu-id="04990-117">URI relatif</span><span class="sxs-lookup"><span data-stu-id="04990-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="04990-118">Obtenir une liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="04990-118">Get a list of all products</span></span> | <span data-ttu-id="04990-119">GET</span><span class="sxs-lookup"><span data-stu-id="04990-119">GET</span></span> | <span data-ttu-id="04990-120">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="04990-120">/api/products</span></span> |
| <span data-ttu-id="04990-121">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="04990-121">Get a product by ID</span></span> | <span data-ttu-id="04990-122">GET</span><span class="sxs-lookup"><span data-stu-id="04990-122">GET</span></span> | <span data-ttu-id="04990-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="04990-123">/api/products/*id*</span></span> |
| <span data-ttu-id="04990-124">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="04990-124">Get a product by category</span></span> | <span data-ttu-id="04990-125">GET</span><span class="sxs-lookup"><span data-stu-id="04990-125">GET</span></span> | <span data-ttu-id="04990-126">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="04990-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="04990-127">Création d’un produit</span><span class="sxs-lookup"><span data-stu-id="04990-127">Create a new product</span></span> | <span data-ttu-id="04990-128">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="04990-128">POST</span></span> | <span data-ttu-id="04990-129">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="04990-129">/api/products</span></span> |
| <span data-ttu-id="04990-130">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="04990-130">Update a product</span></span> | <span data-ttu-id="04990-131">PUT</span><span class="sxs-lookup"><span data-stu-id="04990-131">PUT</span></span> | <span data-ttu-id="04990-132">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="04990-132">/api/products/*id*</span></span> |
| <span data-ttu-id="04990-133">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="04990-133">Delete a product</span></span> | <span data-ttu-id="04990-134">SUPPR</span><span class="sxs-lookup"><span data-stu-id="04990-134">DELETE</span></span> | <span data-ttu-id="04990-135">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="04990-135">/api/products/*id*</span></span> |

<span data-ttu-id="04990-136">Notez que certains des URI comprennent l’ID de produit dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="04990-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="04990-137">Par exemple, pour obtenir le produit dont l’ID est 28, le client envoie une demande GET pour `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="04990-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="04990-138">Ressources</span><span class="sxs-lookup"><span data-stu-id="04990-138">Resources</span></span>

<span data-ttu-id="04990-139">Les produits API définit l’URI pour deux types de ressources :</span><span class="sxs-lookup"><span data-stu-id="04990-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="04990-140">Ressource</span><span class="sxs-lookup"><span data-stu-id="04990-140">Resource</span></span> | <span data-ttu-id="04990-141">URI</span><span class="sxs-lookup"><span data-stu-id="04990-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="04990-142">La liste de tous les produits.</span><span class="sxs-lookup"><span data-stu-id="04990-142">The list of all the products.</span></span> | <span data-ttu-id="04990-143">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="04990-143">/api/products</span></span> |
| <span data-ttu-id="04990-144">Un produit individuel.</span><span class="sxs-lookup"><span data-stu-id="04990-144">An individual product.</span></span> | <span data-ttu-id="04990-145">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="04990-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="04990-146">Méthodes</span><span class="sxs-lookup"><span data-stu-id="04990-146">Methods</span></span>

<span data-ttu-id="04990-147">Les quatre principales méthodes HTTP (GET, PUT, POST et DELETE) peuvent être mappés à des opérations CRUD comme suit :</span><span class="sxs-lookup"><span data-stu-id="04990-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="04990-148">GET récupère la représentation sous forme de la ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="04990-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="04990-149">GET ne doit avoir aucun effet secondaire sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="04990-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="04990-150">PUT met à jour une ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="04990-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="04990-151">PUT peut également servir à créer une nouvelle ressource à un URI spécifié, si le serveur autorise les clients à spécifier de nouveaux URI.</span><span class="sxs-lookup"><span data-stu-id="04990-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="04990-152">Pour ce didacticiel, l’API ne prendra pas en charge la création via PUT.</span><span class="sxs-lookup"><span data-stu-id="04990-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="04990-153">POST crée une nouvelle ressource.</span><span class="sxs-lookup"><span data-stu-id="04990-153">POST creates a new resource.</span></span> <span data-ttu-id="04990-154">Le serveur assigne l’URI pour le nouvel objet et retourne cet URI en tant que partie du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="04990-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="04990-155">DELETE supprime une ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="04990-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="04990-156">Remarque : La méthode PUT remplace l’entité de l’ensemble du produit.</span><span class="sxs-lookup"><span data-stu-id="04990-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="04990-157">Autrement dit, le client est censé envoyer une représentation complète du produit mis à jour.</span><span class="sxs-lookup"><span data-stu-id="04990-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="04990-158">Si vous souhaitez prendre en charge les mises à jour partielles, la méthode PATCH est préférée.</span><span class="sxs-lookup"><span data-stu-id="04990-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="04990-159">Ce didacticiel n’implémente pas de correctif.</span><span class="sxs-lookup"><span data-stu-id="04990-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="04990-160">Créer un nouveau projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="04990-160">Create a New Web API Project</span></span>

<span data-ttu-id="04990-161">Commencez par exécuter Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="04990-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="04990-162">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="04990-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="04990-163">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="04990-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="04990-164">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="04990-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="04990-165">Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="04990-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="04990-166">Nommez le projet &quot;ProductStore&quot; et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="04990-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="04990-167">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **API Web** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="04990-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="04990-168">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="04990-168">Adding a Model</span></span>

<span data-ttu-id="04990-169">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="04990-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="04990-170">Dans l’API Web ASP.NET, vous pouvez utiliser des objets CLR fortement typés en tant que modèles, et ils sont automatiquement sérialisés au format XML ou JSON pour le client.</span><span class="sxs-lookup"><span data-stu-id="04990-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="04990-171">L’API ProductStore, nos données se composent de produits, nous allons créer une nouvelle classe nommée `Product`.</span><span class="sxs-lookup"><span data-stu-id="04990-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="04990-172">Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le **vue** menu et sélectionnez **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="04990-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="04990-173">Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="04990-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="04990-174">Dans le menu contextuel, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="04990-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="04990-175">Nommez la classe &quot;produit&quot;.</span><span class="sxs-lookup"><span data-stu-id="04990-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="04990-176">Ajoutez les propriétés suivantes à la `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="04990-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="04990-177">Ajout d’un référentiel</span><span class="sxs-lookup"><span data-stu-id="04990-177">Adding a Repository</span></span>

<span data-ttu-id="04990-178">Nous avons besoin de stocker une collection de produits.</span><span class="sxs-lookup"><span data-stu-id="04990-178">We need to store a collection of products.</span></span> <span data-ttu-id="04990-179">Il est judicieux de séparer la collecte à partir de notre implémentation de service.</span><span class="sxs-lookup"><span data-stu-id="04990-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="04990-180">De cette façon, nous pouvons modifier le magasin de stockage sans réécrire la classe de service.</span><span class="sxs-lookup"><span data-stu-id="04990-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="04990-181">Ce type de conception est appelé le *référentiel* modèle.</span><span class="sxs-lookup"><span data-stu-id="04990-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="04990-182">Commencez par définir une interface générique pour le référentiel.</span><span class="sxs-lookup"><span data-stu-id="04990-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="04990-183">Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="04990-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="04990-184">Sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="04990-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="04990-185">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le nœud c#.</span><span class="sxs-lookup"><span data-stu-id="04990-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="04990-186">Dans c#, sélectionnez **Code**.</span><span class="sxs-lookup"><span data-stu-id="04990-186">Under C#, select **Code**.</span></span> <span data-ttu-id="04990-187">Dans la liste des modèles de code, sélectionnez **Interface**.</span><span class="sxs-lookup"><span data-stu-id="04990-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="04990-188">Nom de l’interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="04990-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="04990-189">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="04990-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="04990-190">Ajoutez maintenant une autre classe dans le dossier Modèles, nommé &quot;ProductRepository.&quot; Cette classe va implémenter l’interface `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="04990-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="04990-191">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="04990-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="04990-192">Le référentiel conserve la liste dans la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="04990-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="04990-193">Il s’agit OK pour obtenir un didacticiel, mais dans une application réelle, vous stockez les données de façon externe, une base de données ou dans le stockage cloud.</span><span class="sxs-lookup"><span data-stu-id="04990-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="04990-194">Le modèle de référentiel facilitera modifier l’implémentation par la suite.</span><span class="sxs-lookup"><span data-stu-id="04990-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="04990-195">Ajout d’un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="04990-195">Adding a Web API Controller</span></span>

<span data-ttu-id="04990-196">Si vous avez travaillé avec ASP.NET MVC, puis vous êtes déjà familiarisé avec les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="04990-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="04990-197">Dans l’API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes HTTP à partir du client.</span><span class="sxs-lookup"><span data-stu-id="04990-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="04990-198">L’Assistant Nouveau projet créé deux contrôleurs pour vous lors de la création du projet.</span><span class="sxs-lookup"><span data-stu-id="04990-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="04990-199">Pour les consulter, développez le dossier contrôleurs dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="04990-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="04990-200">HomeController est un contrôleur ASP.NET MVC traditionnel.</span><span class="sxs-lookup"><span data-stu-id="04990-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="04990-201">Il est responsable du traitement des pages HTML pour le site et n’est pas directement liée à notre API web.</span><span class="sxs-lookup"><span data-stu-id="04990-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="04990-202">ValuesController est un exemple de contrôleur WebAPI.</span><span class="sxs-lookup"><span data-stu-id="04990-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="04990-203">Continuez et supprimer ValuesController, en double-cliquant sur le fichier dans l’Explorateur de solutions et en sélectionnant **supprimer.**</span><span class="sxs-lookup"><span data-stu-id="04990-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="04990-204">Maintenant, ajoutez un nouveau contrôleur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="04990-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="04990-205">Dans **l’Explorateur de solutions**, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="04990-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="04990-206">Sélectionnez **ajouter** , puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="04990-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="04990-207">Dans le **ajouter un contrôleur** Assistant, nommez le contrôleur &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="04990-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="04990-208">Dans le **modèle** liste déroulante, sélectionnez **contrôleur d’API vide**.</span><span class="sxs-lookup"><span data-stu-id="04990-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="04990-209">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="04990-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="04990-210">Il n’est pas nécessaire de le placer vos contrôleurs dans un dossier nommé contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="04990-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="04990-211">Le nom du dossier n’est pas important ; Il est simplement un moyen pratique d’organiser vos fichiers sources.</span><span class="sxs-lookup"><span data-stu-id="04990-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="04990-212">Le **ajouter un contrôleur** Assistant va créer un fichier nommé ProductsController.cs dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="04990-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="04990-213">Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="04990-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="04990-214">Ajoutez le code suivant **à l’aide de** instruction :</span><span class="sxs-lookup"><span data-stu-id="04990-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="04990-215">Ajouter un champ qui contient un **IProductRepository** instance.</span><span class="sxs-lookup"><span data-stu-id="04990-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="04990-216">Appel `new ProductRepository()` dans le contrôleur n’est pas la meilleure conception, parce qu’elle lie le contrôleur à une implémentation particulière de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="04990-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="04990-217">Pour une meilleure approche, consultez [à l’aide du résolveur de dépendance d’API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="04990-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="04990-218">Obtention d’une ressource</span><span class="sxs-lookup"><span data-stu-id="04990-218">Getting a Resource</span></span>

<span data-ttu-id="04990-219">L’API ProductStore expose plusieurs &quot;lire&quot; actions en tant que méthodes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="04990-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="04990-220">Chaque action correspond à une méthode dans la `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="04990-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="04990-221">Action</span><span class="sxs-lookup"><span data-stu-id="04990-221">Action</span></span> | <span data-ttu-id="04990-222">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="04990-222">HTTP method</span></span> | <span data-ttu-id="04990-223">URI relatif</span><span class="sxs-lookup"><span data-stu-id="04990-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="04990-224">Obtenir une liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="04990-224">Get a list of all products</span></span> | <span data-ttu-id="04990-225">GET</span><span class="sxs-lookup"><span data-stu-id="04990-225">GET</span></span> | <span data-ttu-id="04990-226">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="04990-226">/api/products</span></span> |
| <span data-ttu-id="04990-227">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="04990-227">Get a product by ID</span></span> | <span data-ttu-id="04990-228">GET</span><span class="sxs-lookup"><span data-stu-id="04990-228">GET</span></span> | <span data-ttu-id="04990-229">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="04990-229">/api/products/*id*</span></span> |
| <span data-ttu-id="04990-230">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="04990-230">Get a product by category</span></span> | <span data-ttu-id="04990-231">GET</span><span class="sxs-lookup"><span data-stu-id="04990-231">GET</span></span> | <span data-ttu-id="04990-232">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="04990-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="04990-233">Pour obtenir la liste de tous les produits, ajoutez cette méthode à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="04990-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="04990-234">Le nom de la méthode commence par &quot;obtenir&quot;, par convention, il mappe aux demandes GET.</span><span class="sxs-lookup"><span data-stu-id="04990-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="04990-235">En outre, étant donné que la méthode n’a aucun paramètre, il est mappé à un URI qui ne contient-elle pas une *&quot;id&quot;* segment dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="04990-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="04990-236">Pour obtenir un produit par ID, ajoutez cette méthode à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="04990-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="04990-237">Nom de la méthode commence également par &quot;obtenir&quot;, mais la méthode a un paramètre nommé *id*. Ce paramètre est mappé à la &quot;id&quot; segment du chemin d’accès de l’URI.</span><span class="sxs-lookup"><span data-stu-id="04990-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="04990-238">L’infrastructure de l’API Web ASP.NET convertit automatiquement l’ID pour le type de données correct (**int**) pour le paramètre.</span><span class="sxs-lookup"><span data-stu-id="04990-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="04990-239">La méthode GetProduct lève une exception de type **HttpResponseException** si *id* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="04990-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="04990-240">Cette exception sera traduite par l’infrastructure en une erreur 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="04990-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="04990-241">Enfin, ajoutez une méthode pour rechercher des produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="04990-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="04990-242">Si l’URI de requête a une chaîne de requête, API Web tente de faire correspondre les paramètres de requête aux paramètres de la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="04990-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="04990-243">Par conséquent, un URI au format « api/products ? catégorie =*catégorie*» est mappé à cette méthode.</span><span class="sxs-lookup"><span data-stu-id="04990-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="04990-244">Création d’une ressource</span><span class="sxs-lookup"><span data-stu-id="04990-244">Creating a Resource</span></span>

<span data-ttu-id="04990-245">Ensuite, nous allons ajouter une méthode à la `ProductsController` classe pour créer un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="04990-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="04990-246">Voici une implémentation simple de la méthode :</span><span class="sxs-lookup"><span data-stu-id="04990-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="04990-247">Noter deux choses concernant cette méthode :</span><span class="sxs-lookup"><span data-stu-id="04990-247">Note two things about this method:</span></span>

- <span data-ttu-id="04990-248">Le nom de la méthode commence par &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="04990-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="04990-249">Pour créer un nouveau produit, le client envoie une requête HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="04990-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="04990-250">La méthode prend un paramètre de type de produit.</span><span class="sxs-lookup"><span data-stu-id="04990-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="04990-251">Dans l’API Web, les paramètres avec des types complexes sont désérialisés à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="04990-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="04990-252">Par conséquent, nous pensons que le client envoie une représentation sérialisée d’un objet de produit, au format XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="04990-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="04990-253">Cette implémentation fonctionnera, mais il n’est pas tout à fait complet.</span><span class="sxs-lookup"><span data-stu-id="04990-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="04990-254">Dans l’idéal, nous aimerions la réponse HTTP à inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="04990-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="04990-255">**Code de réponse :** Par défaut, l’infrastructure API Web définit le code d’état de réponse 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="04990-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="04990-256">Mais, conformément au protocole HTTP/1.1, quand une demande POST entraîne la création d’une ressource, le serveur doit répondre avec l’état 201 (créé).</span><span class="sxs-lookup"><span data-stu-id="04990-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="04990-257">**Emplacement :** Lorsque le serveur crée une ressource, il doit inclure l’URI de la nouvelle ressource dans l’en-tête Location de la réponse.</span><span class="sxs-lookup"><span data-stu-id="04990-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="04990-258">API Web ASP.NET rend plus facile manipuler le message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="04990-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="04990-259">Voici l’implémentation améliorée :</span><span class="sxs-lookup"><span data-stu-id="04990-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="04990-260">Notez que le type de retour de méthode est désormais **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="04990-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="04990-261">En retournant un **HttpResponseMessage** au lieu d’un produit, nous pouvons contrôler les détails du message de réponse HTTP, y compris le code d’état et l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="04990-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="04990-262">Le **CreateResponse** méthode crée un **HttpResponseMessage** et automatiquement écrit une représentation sérialisée de l’objet Product dans le corps de fo le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="04990-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="04990-263">Cet exemple ne valide pas le `Product`.</span><span class="sxs-lookup"><span data-stu-id="04990-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="04990-264">Pour plus d’informations sur la validation de modèle, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="04990-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="04990-265">La mise à jour une ressource</span><span class="sxs-lookup"><span data-stu-id="04990-265">Updating a Resource</span></span>

<span data-ttu-id="04990-266">La mise à jour d’un produit avec la méthode PUT est simple :</span><span class="sxs-lookup"><span data-stu-id="04990-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="04990-267">Le nom de la méthode commence par &quot;placer... &quot;, de sorte que l’API Web correspondant à des demandes PUT.</span><span class="sxs-lookup"><span data-stu-id="04990-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="04990-268">La méthode accepte deux paramètres, l’ID de produit et les produits mis à jour.</span><span class="sxs-lookup"><span data-stu-id="04990-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="04990-269">Le *id* paramètre est issu le chemin d’accès de l’URI et la *produit* paramètre est désérialisé à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="04990-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="04990-270">Par défaut, l’infrastructure de l’API Web ASP.NET effectue des types de paramètres simples à partir de l’itinéraire et les types complexes à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="04990-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="04990-271">Suppression d’une ressource</span><span class="sxs-lookup"><span data-stu-id="04990-271">Deleting a Resource</span></span>

<span data-ttu-id="04990-272">Pour supprimer une ressource, définir un paramètre « Supprimer... » méthode.</span><span class="sxs-lookup"><span data-stu-id="04990-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="04990-273">Si une demande de suppression réussit, elle peut retourner l’état 200 (OK) avec un corps d’entité qui décrit l’état ; état 202 (accepté) si la suppression est toujours en attente ; état ou 204 (aucun contenu) sans corps d’entité.</span><span class="sxs-lookup"><span data-stu-id="04990-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="04990-274">Dans ce cas, le `DeleteProduct` méthode a un `void` type de retour, afin de l’API Web ASP.NET traduit automatiquement cette situation en état code 204 (aucun contenu).</span><span class="sxs-lookup"><span data-stu-id="04990-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
