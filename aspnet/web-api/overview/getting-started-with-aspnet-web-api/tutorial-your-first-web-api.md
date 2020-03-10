---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Prise en main de API Web ASP.NET 2C#()-ASP.net 4. x
author: MikeWasson
description: Didacticiel avec code. Utilisez API Web ASP.NET pour créer une API Web qui retourne une liste de produits.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3e35c2bc0e46dfdb4544b772775eddd533f27be3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556797"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="28e1e-104">Prise en main de API Web ASP.NET 2C#()</span><span class="sxs-lookup"><span data-stu-id="28e1e-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="28e1e-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="28e1e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="28e1e-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="28e1e-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="28e1e-107">Dans ce didacticiel, vous allez utiliser API Web ASP.NET pour créer une API Web qui retourne une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="28e1e-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="28e1e-108">HTTP n’est pas seulement destiné au traitement des pages Web.</span><span class="sxs-lookup"><span data-stu-id="28e1e-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="28e1e-109">HTTP est également une plateforme puissante pour la création d’API qui exposent des services et des données.</span><span class="sxs-lookup"><span data-stu-id="28e1e-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="28e1e-110">HTTP est simple, flexible et omniprésent.</span><span class="sxs-lookup"><span data-stu-id="28e1e-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="28e1e-111">Presque toutes les plateformes que vous pouvez considérer ont une bibliothèque HTTP, de sorte que les services HTTP peuvent atteindre un large éventail de clients, notamment des navigateurs, des appareils mobiles et des applications de bureau traditionnelles.</span><span class="sxs-lookup"><span data-stu-id="28e1e-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="28e1e-112">API Web ASP.NET est une infrastructure permettant de créer des API Web en plus des .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="28e1e-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="28e1e-113">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="28e1e-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="28e1e-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="28e1e-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="28e1e-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="28e1e-115">Web API 2</span></span>

<span data-ttu-id="28e1e-116">Pour obtenir une version plus récente de ce didacticiel [, consultez créer une API Web avec ASP.net Core et Visual Studio pour Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .</span><span class="sxs-lookup"><span data-stu-id="28e1e-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="28e1e-117">Créer un projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="28e1e-117">Create a Web API Project</span></span>

<span data-ttu-id="28e1e-118">Dans ce didacticiel, vous allez utiliser API Web ASP.NET pour créer une API Web qui retourne une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="28e1e-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="28e1e-119">La page Web frontale utilise jQuery pour afficher les résultats.</span><span class="sxs-lookup"><span data-stu-id="28e1e-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="28e1e-120">Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="28e1e-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="28e1e-121">Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="28e1e-122">Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  .</span><span class="sxs-lookup"><span data-stu-id="28e1e-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="28e1e-123">Sous **visuel C#** , sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="28e1e-124">Dans la liste des modèles de projet, sélectionnez **application Web ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="28e1e-125">Nommez le projet « ProductsApp », puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="28e1e-126">Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **vide** .</span><span class="sxs-lookup"><span data-stu-id="28e1e-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="28e1e-127">Sous &quot;ajouter des dossiers et des références principales pour&quot;, vérifiez **API Web**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="28e1e-128">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="28e1e-129">Vous pouvez également créer un projet d’API Web à l’aide du modèle de&quot; d’API Web &quot;.</span><span class="sxs-lookup"><span data-stu-id="28e1e-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="28e1e-130">Le modèle d’API Web utilise ASP.NET MVC pour fournir les pages d’aide de l’API.</span><span class="sxs-lookup"><span data-stu-id="28e1e-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="28e1e-131">J’utilise le modèle vide pour ce didacticiel, car je souhaite afficher l’API Web sans MVC.</span><span class="sxs-lookup"><span data-stu-id="28e1e-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="28e1e-132">En général, vous n’avez pas besoin de connaître ASP.NET MVC pour utiliser l’API Web.</span><span class="sxs-lookup"><span data-stu-id="28e1e-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="28e1e-133">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="28e1e-133">Adding a Model</span></span>

<span data-ttu-id="28e1e-134">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="28e1e-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="28e1e-135">API Web ASP.NET pouvez sérialiser automatiquement votre modèle en JSON, XML ou un autre format, puis écrire les données sérialisées dans le corps du message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="28e1e-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="28e1e-136">Tant qu’un client peut lire le format de sérialisation, il peut désérialiser l’objet.</span><span class="sxs-lookup"><span data-stu-id="28e1e-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="28e1e-137">La plupart des clients peuvent analyser XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="28e1e-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="28e1e-138">En outre, le client peut indiquer le format souhaité en définissant l’en-tête Accept dans le message de requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="28e1e-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="28e1e-139">Commençons par créer un modèle simple qui représente un produit.</span><span class="sxs-lookup"><span data-stu-id="28e1e-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="28e1e-140">Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le menu **Vue** et sélectionnez **Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="28e1e-141">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles.</span><span class="sxs-lookup"><span data-stu-id="28e1e-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="28e1e-142">Dans le menu contextuel, sélectionnez **Ajouter**, puis **Classe**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="28e1e-143">Nommez la classe &quot;&quot;de produit.</span><span class="sxs-lookup"><span data-stu-id="28e1e-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="28e1e-144">Ajoutez les propriétés suivantes à la classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="28e1e-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="28e1e-145">Ajour d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="28e1e-145">Adding a Controller</span></span>

<span data-ttu-id="28e1e-146">Dans l’API web, un *contrôleur* est un objet qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="28e1e-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="28e1e-147">Nous allons ajouter un contrôleur qui peut retourner une liste de produits ou un seul produit spécifié par ID.</span><span class="sxs-lookup"><span data-stu-id="28e1e-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="28e1e-148">Si vous avez utilisé ASP.NET MVC, vous êtes déjà familiarisé avec les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="28e1e-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="28e1e-149">Les contrôleurs d’API Web sont similaires aux contrôleurs MVC, mais héritent de la classe **ApiController** au lieu de la classe **Controller** .</span><span class="sxs-lookup"><span data-stu-id="28e1e-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="28e1e-150">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier Contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="28e1e-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="28e1e-151">Sélectionnez **Ajouter**, puis **Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="28e1e-152">Dans la boîte de dialogue **Ajouter une structure**, cliquez sur **Web API Controller - Empty** (Contrôleur d’API web - Vide).</span><span class="sxs-lookup"><span data-stu-id="28e1e-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="28e1e-153">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="28e1e-154">Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur &quot;&quot;ProductsController.</span><span class="sxs-lookup"><span data-stu-id="28e1e-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="28e1e-155">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="28e1e-156">La génération de modèles automatique crée un fichier nommé ProductsController.cs dans le dossier Controllers.</span><span class="sxs-lookup"><span data-stu-id="28e1e-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="28e1e-157">Vous n’avez pas besoin de placer vos contrôleurs dans un dossier nommé contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="28e1e-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="28e1e-158">Le nom du dossier est simplement un moyen pratique d’organiser vos fichiers sources.</span><span class="sxs-lookup"><span data-stu-id="28e1e-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="28e1e-159">Si ce fichier n’est pas déjà ouvert, double-cliquez dessus pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="28e1e-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="28e1e-160">Remplacez le code de ce fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="28e1e-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="28e1e-161">Pour que l’exemple reste simple, les produits sont stockés dans un tableau fixe à l’intérieur de la classe Controller.</span><span class="sxs-lookup"><span data-stu-id="28e1e-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="28e1e-162">Bien sûr, dans une application réelle, vous interrogez une base de données ou vous utilisez une autre source de données externe.</span><span class="sxs-lookup"><span data-stu-id="28e1e-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="28e1e-163">Le contrôleur définit deux méthodes qui retournent des produits :</span><span class="sxs-lookup"><span data-stu-id="28e1e-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="28e1e-164">La méthode `GetAllProducts` retourne l’intégralité de la liste de produits sous la forme d’un type de **&gt;de produit IEnumerable&lt;** .</span><span class="sxs-lookup"><span data-stu-id="28e1e-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="28e1e-165">La méthode `GetProduct` recherche un seul produit par son ID.</span><span class="sxs-lookup"><span data-stu-id="28e1e-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="28e1e-166">C’est tout !</span><span class="sxs-lookup"><span data-stu-id="28e1e-166">That's it!</span></span> <span data-ttu-id="28e1e-167">Vous disposez d’une API Web opérationnelle.</span><span class="sxs-lookup"><span data-stu-id="28e1e-167">You have a working web API.</span></span> <span data-ttu-id="28e1e-168">Chaque méthode sur le contrôleur correspond à un ou plusieurs URI :</span><span class="sxs-lookup"><span data-stu-id="28e1e-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="28e1e-169">Méthode du contrôleur</span><span class="sxs-lookup"><span data-stu-id="28e1e-169">Controller Method</span></span> | <span data-ttu-id="28e1e-170">URI</span><span class="sxs-lookup"><span data-stu-id="28e1e-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="28e1e-171">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="28e1e-171">GetAllProducts</span></span> | <span data-ttu-id="28e1e-172">/api/products</span><span class="sxs-lookup"><span data-stu-id="28e1e-172">/api/products</span></span> |
| <span data-ttu-id="28e1e-173">GetProduct</span><span class="sxs-lookup"><span data-stu-id="28e1e-173">GetProduct</span></span> | <span data-ttu-id="28e1e-174">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="28e1e-174">/api/products/*id*</span></span> |

<span data-ttu-id="28e1e-175">Pour la méthode `GetProduct`, l' *ID* de l’URI est un espace réservé.</span><span class="sxs-lookup"><span data-stu-id="28e1e-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="28e1e-176">Par exemple, pour obtenir le produit avec l’ID 5, l’URI est `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="28e1e-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="28e1e-177">Pour plus d’informations sur la façon dont l’API Web achemine les requêtes HTTP vers les méthodes de contrôleur, consultez [routage dans API Web ASP.net](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="28e1e-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="28e1e-178">Appel de l’API Web avec JavaScript et jQuery</span><span class="sxs-lookup"><span data-stu-id="28e1e-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="28e1e-179">Dans cette section, nous allons ajouter une page HTML qui utilise AJAX pour appeler l’API Web.</span><span class="sxs-lookup"><span data-stu-id="28e1e-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="28e1e-180">Nous allons utiliser jQuery pour effectuer les appels AJAX et également pour mettre à jour la page avec les résultats.</span><span class="sxs-lookup"><span data-stu-id="28e1e-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="28e1e-181">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter**, puis sélectionnez **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="28e1e-182">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le nœud **Web** sous **visuel C#** , puis sélectionnez l’élément de **page HTML** .</span><span class="sxs-lookup"><span data-stu-id="28e1e-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="28e1e-183">Nommez la page &quot;&quot;index. html.</span><span class="sxs-lookup"><span data-stu-id="28e1e-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="28e1e-184">Remplacez tout ce qui suit dans ce fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="28e1e-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="28e1e-185">Il existe plusieurs façons d’obtenir jQuery.</span><span class="sxs-lookup"><span data-stu-id="28e1e-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="28e1e-186">Dans cet exemple, j’ai utilisé le [CDN Microsoft Ajax](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="28e1e-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="28e1e-187">Vous pouvez également le télécharger à partir de [http://jquery.com/](http://jquery.com/), et le modèle de projet ASP.NET « API Web » comprend également jQuery.</span><span class="sxs-lookup"><span data-stu-id="28e1e-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="28e1e-188">Obtention d’une liste de produits</span><span class="sxs-lookup"><span data-stu-id="28e1e-188">Getting a List of Products</span></span>

<span data-ttu-id="28e1e-189">Pour obtenir la liste des produits, envoyez une requête HTTP-r à &quot;&quot;/API/Products.</span><span class="sxs-lookup"><span data-stu-id="28e1e-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="28e1e-190">La fonction jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) envoie une requête Ajax.</span><span class="sxs-lookup"><span data-stu-id="28e1e-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="28e1e-191">La réponse contient un tableau d’objets JSON.</span><span class="sxs-lookup"><span data-stu-id="28e1e-191">For response contains array of JSON objects.</span></span> <span data-ttu-id="28e1e-192">La fonction `done` spécifie un rappel qui est appelé si la demande réussit.</span><span class="sxs-lookup"><span data-stu-id="28e1e-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="28e1e-193">Dans le rappel, nous mettons à jour le DOM avec les informations sur le produit.</span><span class="sxs-lookup"><span data-stu-id="28e1e-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="28e1e-194">Obtention d’un produit par ID</span><span class="sxs-lookup"><span data-stu-id="28e1e-194">Getting a Product By ID</span></span>

<span data-ttu-id="28e1e-195">Pour obtenir un produit par ID, envoyez une requête HTTP-to à &quot;*ID* /API/Products/&quot;, où *ID* est l’ID du produit.</span><span class="sxs-lookup"><span data-stu-id="28e1e-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="28e1e-196">Nous appelons toujours `getJSON` pour envoyer la demande AJAX, mais cette fois-ci, nous plaçons l’ID dans l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="28e1e-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="28e1e-197">La réponse de cette demande est une représentation JSON d’un produit unique.</span><span class="sxs-lookup"><span data-stu-id="28e1e-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="28e1e-198">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="28e1e-198">Running the Application</span></span>

<span data-ttu-id="28e1e-199">Appuyez sur F5 pour lancer le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="28e1e-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="28e1e-200">La page Web doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="28e1e-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="28e1e-201">Pour obtenir un produit par ID, entrez l’ID, puis cliquez sur Rechercher :</span><span class="sxs-lookup"><span data-stu-id="28e1e-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="28e1e-202">Si vous entrez un ID non valide, le serveur renvoie une erreur HTTP :</span><span class="sxs-lookup"><span data-stu-id="28e1e-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="28e1e-203">Utilisation de la touche F12 pour afficher la requête et la réponse HTTP</span><span class="sxs-lookup"><span data-stu-id="28e1e-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="28e1e-204">Lorsque vous utilisez un service HTTP, il peut être très utile de voir les messages de requête et de demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="28e1e-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="28e1e-205">Pour ce faire, vous pouvez utiliser les outils de développement F12 dans Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="28e1e-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="28e1e-206">Dans Internet Explorer 9, appuyez sur **F12** pour ouvrir les outils.</span><span class="sxs-lookup"><span data-stu-id="28e1e-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="28e1e-207">Cliquez sur l’onglet **réseau** , puis appuyez sur **Démarrer la capture**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="28e1e-208">Revenez maintenant à la page Web et appuyez sur **F5** pour recharger la page Web.</span><span class="sxs-lookup"><span data-stu-id="28e1e-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="28e1e-209">Internet Explorer capture le trafic HTTP entre le navigateur et le serveur Web.</span><span class="sxs-lookup"><span data-stu-id="28e1e-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="28e1e-210">La vue Résumé affiche tout le trafic réseau d’une page :</span><span class="sxs-lookup"><span data-stu-id="28e1e-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="28e1e-211">Recherchez l’entrée de l’URI relatif « API/Products/ ».</span><span class="sxs-lookup"><span data-stu-id="28e1e-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="28e1e-212">Sélectionnez cette entrée, puis cliquez sur **accéder à la vue détaillée**.</span><span class="sxs-lookup"><span data-stu-id="28e1e-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="28e1e-213">Dans la vue détaillée, il existe des onglets pour afficher les en-têtes et les corps de la demande et de la réponse.</span><span class="sxs-lookup"><span data-stu-id="28e1e-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="28e1e-214">Par exemple, si vous cliquez sur l’onglet **en-têtes de demande** , vous pouvez voir que le client a demandé &quot;application/JSON&quot; dans l’en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="28e1e-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="28e1e-215">Si vous cliquez sur l’onglet corps de la réponse, vous pouvez voir comment la liste de produits a été sérialisée au format JSON.</span><span class="sxs-lookup"><span data-stu-id="28e1e-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="28e1e-216">D’autres navigateurs ont des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="28e1e-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="28e1e-217">[Fiddler](http://www.fiddler2.com/fiddler2/)est un autre outil utile : le proxy de débogage Web.</span><span class="sxs-lookup"><span data-stu-id="28e1e-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="28e1e-218">Vous pouvez utiliser Fiddler pour afficher votre trafic HTTP, ainsi que pour composer des requêtes HTTP, ce qui vous donne un contrôle total sur les en-têtes HTTP de la requête.</span><span class="sxs-lookup"><span data-stu-id="28e1e-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="28e1e-219">Voir cette application en cours d’exécution sur Azure</span><span class="sxs-lookup"><span data-stu-id="28e1e-219">See this App Running on Azure</span></span>

<span data-ttu-id="28e1e-220">Voulez-vous que le site terminé s’exécute en tant qu’application Web en direct ?</span><span class="sxs-lookup"><span data-stu-id="28e1e-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="28e1e-221">Vous pouvez déployer une version complète de l’application sur votre compte Azure en cliquant simplement sur le bouton suivant.</span><span class="sxs-lookup"><span data-stu-id="28e1e-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="28e1e-222">Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure.</span><span class="sxs-lookup"><span data-stu-id="28e1e-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="28e1e-223">Si vous n’avez pas encore de compte, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="28e1e-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="28e1e-224">[Ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : vous recevez des crédits que vous pouvez utiliser pour tester les services Azure payants, et même après leur utilisation, vous pouvez conserver le compte et utiliser les services Azure gratuits.</span><span class="sxs-lookup"><span data-stu-id="28e1e-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="28e1e-225">[Activer les avantages des abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="28e1e-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28e1e-226">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="28e1e-226">Next Steps</span></span>

- <span data-ttu-id="28e1e-227">Pour obtenir un exemple plus complet d’un service HTTP qui prend en charge les actions de publication, PUT et suppression et écrit dans une base de données, consultez [utilisation de l’API Web 2 avec Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="28e1e-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="28e1e-228">Pour plus d’informations sur la création d’applications Web fluides et réactives au-dessus d’un service HTTP, consultez [application à page unique ASP.net](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="28e1e-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="28e1e-229">Pour plus d’informations sur le déploiement d’un projet Web Visual Studio sur Azure App Service, consultez [créer une application web ASP.net dans Azure App service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="28e1e-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
