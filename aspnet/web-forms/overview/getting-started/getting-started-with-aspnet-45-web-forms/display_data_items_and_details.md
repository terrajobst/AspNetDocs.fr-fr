---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Afficher les éléments de données et les détails | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous montrera les bases de la création d’une application ASP.NET Web Forms avec ASP.NET 4,7 et Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641112"
---
# <a name="display-data-items-and-details"></a><span data-ttu-id="b04f9-103">Afficher les éléments de données et les détails</span><span class="sxs-lookup"><span data-stu-id="b04f9-103">Display data items and details</span></span>

<span data-ttu-id="b04f9-104">par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="b04f9-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="b04f9-105">Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms avec ASP.NET 4,7 et Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b04f9-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="b04f9-106">Dans ce didacticiel, vous allez apprendre à afficher les éléments de données et les détails de l’élément de données avec ASP.NET Web Forms et Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="b04f9-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="b04f9-107">Ce didacticiel s’appuie sur le didacticiel « UI and navigation » précédent dans le cadre de la série de didacticiels pour Wingtip Toys Store.</span><span class="sxs-lookup"><span data-stu-id="b04f9-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="b04f9-108">À l’issue de ce didacticiel, vous verrez des produits sur la page *ProductsList. aspx* et les détails d’un produit sur la page *ProductDetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="b04f9-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="b04f9-109">Vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="b04f9-109">You'll learn how to:</span></span>

- <span data-ttu-id="b04f9-110">Ajouter un contrôle de données pour afficher les produits de la base de données</span><span class="sxs-lookup"><span data-stu-id="b04f9-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="b04f9-111">Connecter un contrôle de données aux données sélectionnées</span><span class="sxs-lookup"><span data-stu-id="b04f9-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="b04f9-112">Ajouter un contrôle de données pour afficher les détails du produit à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="b04f9-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="b04f9-113">Récupérer une valeur à partir de la chaîne de requête et utiliser cette valeur pour limiter les données récupérées de la base de données</span><span class="sxs-lookup"><span data-stu-id="b04f9-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="b04f9-114">Fonctionnalités introduites dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="b04f9-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="b04f9-115">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="b04f9-115">Model binding</span></span>
- <span data-ttu-id="b04f9-116">Fournisseurs de valeurs</span><span class="sxs-lookup"><span data-stu-id="b04f9-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="b04f9-117">Ajouter un contrôle de données</span><span class="sxs-lookup"><span data-stu-id="b04f9-117">Add a data control</span></span>

<span data-ttu-id="b04f9-118">Vous pouvez utiliser différentes options pour lier des données à un contrôle serveur.</span><span class="sxs-lookup"><span data-stu-id="b04f9-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="b04f9-119">Les plus courants sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="b04f9-119">The most common include:</span></span>

* <span data-ttu-id="b04f9-120">Ajout d’un contrôle de source de données</span><span class="sxs-lookup"><span data-stu-id="b04f9-120">Adding a data source control</span></span>
* <span data-ttu-id="b04f9-121">Ajout de code à la main</span><span class="sxs-lookup"><span data-stu-id="b04f9-121">Adding code by hand</span></span>
* <span data-ttu-id="b04f9-122">Utilisation de la liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="b04f9-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="b04f9-123">Utiliser un contrôle de source de données pour lier des données</span><span class="sxs-lookup"><span data-stu-id="b04f9-123">Use a data source control to bind data</span></span>

<span data-ttu-id="b04f9-124">L’ajout d’un contrôle de source de données vous permet de lier le contrôle de source de données au contrôle qui affiche les données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="b04f9-125">Avec cette approche, vous pouvez, plutôt que par programmation, connecter des contrôles côté serveur à des sources de données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="b04f9-126">Coder à la main pour lier des données</span><span class="sxs-lookup"><span data-stu-id="b04f9-126">Code by hand to bind data</span></span>

<span data-ttu-id="b04f9-127">Le codage à la main implique :</span><span class="sxs-lookup"><span data-stu-id="b04f9-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="b04f9-128">Lecture d’une valeur</span><span class="sxs-lookup"><span data-stu-id="b04f9-128">Reading a value</span></span>
2. <span data-ttu-id="b04f9-129">Vérification de la présence de null</span><span class="sxs-lookup"><span data-stu-id="b04f9-129">Checking if it's null</span></span>
3. <span data-ttu-id="b04f9-130">Conversion en type approprié</span><span class="sxs-lookup"><span data-stu-id="b04f9-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="b04f9-131">Vérification réussie de la conversion</span><span class="sxs-lookup"><span data-stu-id="b04f9-131">Checking conversion success</span></span>
5. <span data-ttu-id="b04f9-132">Utilisation de la valeur dans la requête</span><span class="sxs-lookup"><span data-stu-id="b04f9-132">Using the value in the query</span></span> 

<span data-ttu-id="b04f9-133">Cette approche vous permet d’avoir un contrôle total sur votre logique d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="b04f9-134">Utiliser la liaison de modèle pour lier des données</span><span class="sxs-lookup"><span data-stu-id="b04f9-134">Use model binding to bind data</span></span>

<span data-ttu-id="b04f9-135">La liaison de modèle vous permet de lier des résultats avec beaucoup moins de code et vous donne la possibilité de réutiliser les fonctionnalités dans votre application.</span><span class="sxs-lookup"><span data-stu-id="b04f9-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="b04f9-136">Il simplifie l’utilisation de la logique d’accès aux données axée sur le code tout en fournissant une infrastructure de liaison de données riche.</span><span class="sxs-lookup"><span data-stu-id="b04f9-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="b04f9-137">Afficher les produits</span><span class="sxs-lookup"><span data-stu-id="b04f9-137">Display products</span></span>

<span data-ttu-id="b04f9-138">Dans ce didacticiel, vous allez utiliser la liaison de modèle pour lier des données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="b04f9-139">Pour configurer un contrôle de données afin d’utiliser la liaison de modèle pour sélectionner des données, vous affectez à la propriété `SelectMethod` du contrôle un nom de méthode dans le code de la page.</span><span class="sxs-lookup"><span data-stu-id="b04f9-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="b04f9-140">Le contrôle de données appelle la méthode au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées.</span><span class="sxs-lookup"><span data-stu-id="b04f9-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="b04f9-141">Il n’est pas nécessaire d’appeler explicitement la méthode `DataBind`.</span><span class="sxs-lookup"><span data-stu-id="b04f9-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="b04f9-142">Dans **Explorateur de solutions**, ouvrez *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="b04f9-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="b04f9-143">Remplacez le balisage existant par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="b04f9-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="b04f9-144">Ce code utilise un contrôle **ListView** nommé `productList` pour afficher les produits.</span><span class="sxs-lookup"><span data-stu-id="b04f9-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="b04f9-145">Avec les modèles et les styles, vous définissez la manière dont le contrôle **ListView** affiche des données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="b04f9-146">Elle est utile pour les données de toute structure répétée.</span><span class="sxs-lookup"><span data-stu-id="b04f9-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="b04f9-147">Bien que cet exemple de **ListView** affiche simplement des données de base de données, vous pouvez également, sans code, permettre aux utilisateurs de modifier, d’insérer et de supprimer des données, ainsi que de trier et de paginer des données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="b04f9-148">En définissant la propriété `ItemType` dans le contrôle **ListView** , l’expression de liaison de données `Item` est disponible et le contrôle est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="b04f9-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="b04f9-149">Comme mentionné dans le didacticiel précédent, vous pouvez sélectionner les détails de l’objet d’élément avec IntelliSense, par exemple en spécifiant les `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="b04f9-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Afficher les éléments de données et les détails-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="b04f9-151">Vous utilisez également la liaison de modèle pour spécifier une valeur de `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="b04f9-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="b04f9-152">Cette valeur (`GetProducts`) correspond à la méthode que vous ajouterez au code-behind pour afficher les produits à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="b04f9-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="b04f9-153">Ajouter du code pour afficher les produits</span><span class="sxs-lookup"><span data-stu-id="b04f9-153">Add code to display products</span></span>

<span data-ttu-id="b04f9-154">Dans cette étape, vous allez ajouter du code pour remplir le contrôle **ListView** avec les données de produit de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="b04f9-155">Le code prend en charge l’indication de tous les produits et de chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="b04f9-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="b04f9-156">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *ProductList. aspx* , puis sélectionnez **afficher le code**.</span><span class="sxs-lookup"><span data-stu-id="b04f9-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="b04f9-157">Remplacez le code existant dans le fichier *ProductList.aspx.cs* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="b04f9-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="b04f9-158">Ce code montre la méthode `GetProducts` que la propriété du contrôle **ListView** `ItemType` référence dans la page *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="b04f9-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="b04f9-159">Pour limiter les résultats à une catégorie de base de données spécifique, le code définit la valeur de la `categoryId` à partir de la valeur de la chaîne de requête transmise à la page *ProductList. aspx* lorsque la page *ProductList. aspx* est parcourue.</span><span class="sxs-lookup"><span data-stu-id="b04f9-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="b04f9-160">La classe `QueryStringAttribute` de l’espace de noms `System.Web.ModelBinding` est utilisée pour récupérer la valeur de la variable de chaîne de requête `id`.</span><span class="sxs-lookup"><span data-stu-id="b04f9-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="b04f9-161">Cela indique à la liaison de modèle d’essayer de lier une valeur de la chaîne de requête au paramètre `categoryId` au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b04f9-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="b04f9-162">Lorsqu’une catégorie valide est transmise en tant que chaîne de requête à la page, les résultats de la requête sont limités aux produits de la base de données qui correspondent à la valeur `categoryId`.</span><span class="sxs-lookup"><span data-stu-id="b04f9-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="b04f9-163">Par exemple, si l’URL de la page *ProductsList. aspx* est la suivante :</span><span class="sxs-lookup"><span data-stu-id="b04f9-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="b04f9-164">La page affiche uniquement les produits dont le `categoryId` est égal à `1`.</span><span class="sxs-lookup"><span data-stu-id="b04f9-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="b04f9-165">Tous les produits sont affichés si aucune chaîne de requête n’est incluse lorsque la page *ProductList. aspx* est appelée.</span><span class="sxs-lookup"><span data-stu-id="b04f9-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="b04f9-166">Les sources de valeurs pour ces méthodes sont appelées *fournisseurs* de valeurs (telles que *QueryString*) et les attributs de paramètres qui indiquent le fournisseur de valeur à utiliser sont appelés *attributs de fournisseur de valeur* (par exemple, `id`).</span><span class="sxs-lookup"><span data-stu-id="b04f9-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="b04f9-167">ASP.NET comprend des fournisseurs de valeurs et des attributs correspondants pour toutes les sources typiques de l’entrée utilisateur dans une application Web Forms, telle que la chaîne de requête, les cookies, les valeurs de formulaire, les contrôles, l’état d’affichage, l’état de session et les propriétés de profil.</span><span class="sxs-lookup"><span data-stu-id="b04f9-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="b04f9-168">Vous pouvez également écrire des fournisseurs de valeurs personnalisés.</span><span class="sxs-lookup"><span data-stu-id="b04f9-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="b04f9-169">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="b04f9-169">Run the application</span></span>

<span data-ttu-id="b04f9-170">Exécutez l’application maintenant pour afficher tous les produits ou les produits d’une catégorie.</span><span class="sxs-lookup"><span data-stu-id="b04f9-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="b04f9-171">Appuyez sur **F5** dans Visual Studio pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b04f9-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="b04f9-172">Le navigateur s’ouvre et affiche la page *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="b04f9-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="b04f9-173">Sélectionnez **Cars** dans le menu de navigation des catégories de produits.</span><span class="sxs-lookup"><span data-stu-id="b04f9-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="b04f9-174">La page *ProductList. aspx* affiche uniquement les produits de catégorie **Cars** .</span><span class="sxs-lookup"><span data-stu-id="b04f9-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="b04f9-175">Plus loin dans ce didacticiel, vous allez afficher les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="b04f9-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Afficher les éléments de données et les détails-voitures](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="b04f9-177">Sélectionnez **produits** dans le menu de navigation en haut.</span><span class="sxs-lookup"><span data-stu-id="b04f9-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="b04f9-178">Là encore, la page *ProductList. aspx* s’affiche, mais cette fois-ci, elle affiche la liste complète des produits.</span><span class="sxs-lookup"><span data-stu-id="b04f9-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Afficher les éléments de données et les détails-produits](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="b04f9-180">Fermez le navigateur et revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b04f9-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="b04f9-181">Ajouter un contrôle de données pour afficher les détails du produit</span><span class="sxs-lookup"><span data-stu-id="b04f9-181">Add a data control to display product details</span></span>

<span data-ttu-id="b04f9-182">Ensuite, vous allez modifier le balisage de la page *ProductDetails. aspx* que vous avez ajoutée dans le didacticiel précédent pour afficher des informations spécifiques sur le produit.</span><span class="sxs-lookup"><span data-stu-id="b04f9-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="b04f9-183">Dans **Explorateur de solutions**, ouvrez *ProductDetails. aspx*.</span><span class="sxs-lookup"><span data-stu-id="b04f9-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="b04f9-184">Remplacez le balisage existant par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="b04f9-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="b04f9-185">Ce code utilise un contrôle **FormView** pour afficher des détails spécifiques sur un produit.</span><span class="sxs-lookup"><span data-stu-id="b04f9-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="b04f9-186">Ce balisage utilise des méthodes comme les méthodes utilisées pour afficher des données dans la page *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="b04f9-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="b04f9-187">Le contrôle **FormView** est utilisé pour afficher un seul enregistrement à la fois à partir d’une source de données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="b04f9-188">Quand vous utilisez le contrôle **FormView** , vous créez des modèles pour afficher et modifier des valeurs liées aux données.</span><span class="sxs-lookup"><span data-stu-id="b04f9-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="b04f9-189">Ces modèles contiennent des contrôles, des expressions de liaison et une mise en forme qui définissent l’apparence et les fonctionnalités du formulaire.</span><span class="sxs-lookup"><span data-stu-id="b04f9-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="b04f9-190">La connexion du balisage précédent à la base de données nécessite du code supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="b04f9-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="b04f9-191">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *ProductDetails. aspx* , puis cliquez sur **afficher le code**.</span><span class="sxs-lookup"><span data-stu-id="b04f9-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="b04f9-192">Le fichier *ProductDetails.aspx.cs* est affiché.</span><span class="sxs-lookup"><span data-stu-id="b04f9-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="b04f9-193">Remplacez le code existant par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b04f9-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="b04f9-194">Ce code recherche une valeur de chaîne de requête «`productID`».</span><span class="sxs-lookup"><span data-stu-id="b04f9-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="b04f9-195">Si une valeur de chaîne de requête valide est trouvée, le produit correspondant est affiché.</span><span class="sxs-lookup"><span data-stu-id="b04f9-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="b04f9-196">Si la chaîne de requête est introuvable ou si sa valeur n’est pas valide, aucun produit n’est affiché.</span><span class="sxs-lookup"><span data-stu-id="b04f9-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="b04f9-197">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="b04f9-197">Run the application</span></span>

<span data-ttu-id="b04f9-198">Vous pouvez maintenant exécuter l’application pour voir un produit individuel affiché en fonction de l’ID de produit.</span><span class="sxs-lookup"><span data-stu-id="b04f9-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="b04f9-199">Appuyez sur **F5** dans Visual Studio pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b04f9-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="b04f9-200">Le navigateur s’ouvre et affiche la page *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="b04f9-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="b04f9-201">Sélectionnez **bateaux** dans le menu de navigation de la catégorie.</span><span class="sxs-lookup"><span data-stu-id="b04f9-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="b04f9-202">La page *ProductList. aspx* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b04f9-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="b04f9-203">Sélectionnez **livre papier** dans la liste des produits.</span><span class="sxs-lookup"><span data-stu-id="b04f9-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="b04f9-204">La page *ProductDetails. aspx* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b04f9-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Afficher les éléments de données et les détails-produits](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="b04f9-206">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b04f9-206">Close the browser.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b04f9-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b04f9-207">Additional resources</span></span>

[<span data-ttu-id="b04f9-208">Récupération et affichage de données avec la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="b04f9-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="b04f9-209">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="b04f9-209">Next steps</span></span>

<span data-ttu-id="b04f9-210">Dans ce didacticiel, vous avez ajouté le balisage et le code pour afficher les produits et les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="b04f9-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="b04f9-211">Vous avez appris à propos des contrôles de données fortement typés, la liaison de modèle et les fournisseurs de valeurs.</span><span class="sxs-lookup"><span data-stu-id="b04f9-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="b04f9-212">Dans le didacticiel suivant, vous allez ajouter un panier à l’exemple d’application Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="b04f9-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="b04f9-213">[Précédent](ui_and_navigation.md)
> [Suivant](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="b04f9-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
