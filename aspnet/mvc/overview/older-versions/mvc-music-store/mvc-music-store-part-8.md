---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Partie 8 : panier d’achat avec mises à jour d’Ajax | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 8 couvre le panier d’achat avec les mises à jour Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539255"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="32b78-104">Partie 8 : panier d’achat avec mises à jour d’Ajax</span><span class="sxs-lookup"><span data-stu-id="32b78-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="32b78-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="32b78-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="32b78-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="32b78-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="32b78-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="32b78-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="32b78-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="32b78-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="32b78-109">La partie 8 couvre le panier d’achat avec les mises à jour Ajax.</span><span class="sxs-lookup"><span data-stu-id="32b78-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>

<span data-ttu-id="32b78-110">Nous autoriserons les utilisateurs à placer des albums dans leur panier sans les inscrire, mais ils devront s’inscrire en tant qu’invités pour finaliser l’extraction.</span><span class="sxs-lookup"><span data-stu-id="32b78-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="32b78-111">Le processus d’achat et de validation sera divisé en deux contrôleurs : un contrôleur ShoppingCart qui permet d’ajouter des éléments de manière anonyme à un panier et un contrôleur d’extraction qui gère le processus d’extraction.</span><span class="sxs-lookup"><span data-stu-id="32b78-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="32b78-112">Nous allons commencer par le panier dans cette section, puis créer le processus d’extraction dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="32b78-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="32b78-113">Ajout des classes de modèle cart, Order et OrderDetail</span><span class="sxs-lookup"><span data-stu-id="32b78-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="32b78-114">Nos processus de panier d’achat et de validation feront appel à certaines nouvelles classes.</span><span class="sxs-lookup"><span data-stu-id="32b78-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="32b78-115">Cliquez avec le bouton droit sur le dossier Models et ajoutez une classe de panier (Cart.cs) avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="32b78-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="32b78-116">Cette classe est assez similaire à celle que nous avons utilisée jusqu’à présent, à l’exception de l’attribut [Key] pour la propriété RecordId.</span><span class="sxs-lookup"><span data-stu-id="32b78-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="32b78-117">Nos éléments de panier auront un identificateur de chaîne nommé CartID pour autoriser les achats anonymes, mais la table contient une clé primaire entière nommée RecordId.</span><span class="sxs-lookup"><span data-stu-id="32b78-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="32b78-118">Par Convention, Entity Framework code First s’attend à ce que la clé primaire d’une table nommée Cart soit CartId ou ID, mais nous pouvons facilement la remplacer par des annotations ou du code, si nous le souhaitons.</span><span class="sxs-lookup"><span data-stu-id="32b78-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="32b78-119">Il s’agit d’un exemple de la façon dont nous pouvons utiliser les conventions simples dans Entity Framework code-tout d’abord lorsqu’ils nous intéressent, mais nous n’y sommes pas contraints.</span><span class="sxs-lookup"><span data-stu-id="32b78-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="32b78-120">Ensuite, ajoutez une classe Order (Order.cs) avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="32b78-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="32b78-121">Cette classe effectue le suivi des informations de synthèse et de livraison pour une commande.</span><span class="sxs-lookup"><span data-stu-id="32b78-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="32b78-122">**Elle n’est pas encore compilée**, car elle possède une propriété de navigation OrderDetails qui dépend d’une classe que nous n’avons pas encore créée.</span><span class="sxs-lookup"><span data-stu-id="32b78-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="32b78-123">Nous allons maintenant résoudre le problème en ajoutant une classe nommée OrderDetail.cs, en ajoutant le code suivant.</span><span class="sxs-lookup"><span data-stu-id="32b78-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="32b78-124">Nous allons effectuer une dernière mise à jour de notre classe MusicStoreEntities pour inclure des DbSets qui exposent ces nouvelles classes de modèle, y compris un DbSet&lt;Artist&gt;.</span><span class="sxs-lookup"><span data-stu-id="32b78-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="32b78-125">La classe MusicStoreEntities mise à jour apparaît comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="32b78-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="32b78-126">Gestion de la logique métier du panier d’achat</span><span class="sxs-lookup"><span data-stu-id="32b78-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="32b78-127">Ensuite, nous allons créer la classe ShoppingCart dans le dossier Models.</span><span class="sxs-lookup"><span data-stu-id="32b78-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="32b78-128">Le modèle ShoppingCart gère l’accès aux données de la table Cart.</span><span class="sxs-lookup"><span data-stu-id="32b78-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="32b78-129">En outre, il gère la logique métier pour l’ajout et la suppression d’articles dans le panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="32b78-130">Étant donné que nous ne souhaitons pas obliger les utilisateurs à s’inscrire à un compte simplement pour ajouter des éléments à leur panier d’achat, nous attribuons aux utilisateurs un identificateur unique temporaire (à l’aide d’un GUID ou d’un identificateur global unique) lorsqu’ils accèdent au panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="32b78-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="32b78-131">Nous allons stocker cet ID à l’aide de la classe de session ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32b78-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="32b78-132">*Remarque : la session ASP.NET est un emplacement pratique pour stocker des informations spécifiques à l’utilisateur, qui expireront une fois le site quitté. Bien qu’une mauvaise utilisation de l’état de session puisse avoir un impact sur les performances sur les sites plus importants, notre utilisation légère fonctionnera bien à des fins de démonstration.*</span><span class="sxs-lookup"><span data-stu-id="32b78-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="32b78-133">La classe ShoppingCart expose les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="32b78-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="32b78-134">**AddToCart** prend un album comme paramètre et l’ajoute au panier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32b78-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="32b78-135">Étant donné que la table Cart effectue le suivi de la quantité pour chaque album, elle inclut une logique pour créer une nouvelle ligne si nécessaire ou simplement incrémenter la quantité si l’utilisateur a déjà commandé une copie de l’album.</span><span class="sxs-lookup"><span data-stu-id="32b78-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="32b78-136">**RemoveFromCart** prend un ID d’album et le supprime du panier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32b78-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="32b78-137">Si l’utilisateur n’avait qu’une seule copie de l’album dans son panier, la ligne est supprimée.</span><span class="sxs-lookup"><span data-stu-id="32b78-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="32b78-138">**EmptyCart** supprime tous les éléments du panier d’achat d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32b78-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="32b78-139">**GetCartItems** récupère une liste de CartItems pour l’affichage ou le traitement.</span><span class="sxs-lookup"><span data-stu-id="32b78-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="32b78-140">**GetCount** récupère un nombre total d’albums dont dispose un utilisateur dans son panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="32b78-141">**GetTotal** calcule le coût total de tous les articles dans le panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="32b78-142">**CreateOrder** convertit le panier d’achat en commande au cours de la phase de validation.</span><span class="sxs-lookup"><span data-stu-id="32b78-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="32b78-143">**GetCart** est une méthode statique qui permet à nos contrôleurs d’obtenir un objet de panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="32b78-144">Elle utilise la méthode **GetCartId** pour gérer la lecture du CartId à partir de la session de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32b78-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="32b78-145">La méthode GetCartId nécessite la méthode HttpContextBase pour pouvoir lire le CartId de l’utilisateur à partir de la session de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32b78-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="32b78-146">Voici la **classe ShoppingCart**complète :</span><span class="sxs-lookup"><span data-stu-id="32b78-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="32b78-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="32b78-147">ViewModels</span></span>

<span data-ttu-id="32b78-148">Notre contrôleur de panier d’achat doit communiquer des informations complexes à ses vues qui ne sont pas correctement mappées à nos objets de modèle.</span><span class="sxs-lookup"><span data-stu-id="32b78-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="32b78-149">Nous ne voulons pas modifier nos modèles pour les adapter à nos vues ; Les classes de modèle doivent représenter notre domaine, et non l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32b78-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="32b78-150">Une solution consisterait à transmettre les informations à nos vues à l’aide de la classe ViewBag, comme nous l’avons fait avec les informations de la liste déroulante du gestionnaire de magasin, mais le fait de passer beaucoup d’informations via ViewBag devient difficile à gérer.</span><span class="sxs-lookup"><span data-stu-id="32b78-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="32b78-151">Une solution consiste à utiliser le modèle *ViewModel* .</span><span class="sxs-lookup"><span data-stu-id="32b78-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="32b78-152">Lors de l’utilisation de ce modèle, nous créons des classes fortement typées qui sont optimisées pour nos scénarios d’affichage spécifiques, et qui exposent des propriétés pour les valeurs/contenus dynamiques requis par nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="32b78-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="32b78-153">Nos classes de contrôleur peuvent ensuite remplir et transmettre ces classes optimisées en vue à notre modèle de vue à utiliser.</span><span class="sxs-lookup"><span data-stu-id="32b78-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="32b78-154">Cela permet la sécurité des types, la vérification au moment de la compilation et l’IntelliSense de l’éditeur dans les modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="32b78-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="32b78-155">Nous allons créer deux modèles de vue à utiliser dans notre contrôleur de panier d’achat : le ShoppingCartViewModel contiendra le contenu du panier d’achat de l’utilisateur, et le ShoppingCartRemoveViewModel sera utilisé pour afficher les informations de confirmation lorsqu’un utilisateur supprime un événement à partir de leur panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="32b78-156">Nous allons créer un nouveau dossier ViewModels à la racine de notre projet pour que les choses restent organisées.</span><span class="sxs-lookup"><span data-stu-id="32b78-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="32b78-157">Cliquez avec le bouton droit sur le projet, puis sélectionnez Ajouter/nouveau dossier.</span><span class="sxs-lookup"><span data-stu-id="32b78-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="32b78-158">Nommez le dossier ViewModels.</span><span class="sxs-lookup"><span data-stu-id="32b78-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="32b78-159">Ensuite, ajoutez la classe ShoppingCartViewModel dans le dossier ViewModels.</span><span class="sxs-lookup"><span data-stu-id="32b78-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="32b78-160">Il a deux propriétés : une liste d’éléments de panier et une valeur décimale pour stocker le prix total de tous les articles dans le panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="32b78-161">Ajoutez maintenant ShoppingCartRemoveViewModel au dossier ViewModels, avec les quatre propriétés suivantes.</span><span class="sxs-lookup"><span data-stu-id="32b78-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="32b78-162">Le contrôleur de panier d’achat</span><span class="sxs-lookup"><span data-stu-id="32b78-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="32b78-163">Le contrôleur de panier d’achat a trois objectifs principaux : l’ajout d’éléments à un panier, la suppression d’éléments du panier et l’affichage des éléments dans le panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="32b78-164">Elle utilisera les trois classes que nous venons de créer : ShoppingCartViewModel, ShoppingCartRemoveViewModel et ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="32b78-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="32b78-165">Comme dans StoreController et StoreManagerController, nous allons ajouter un champ pour contenir une instance de MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="32b78-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="32b78-166">Ajoutez un nouveau contrôleur de panier d’achat au projet à l’aide du modèle de contrôleur vide.</span><span class="sxs-lookup"><span data-stu-id="32b78-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="32b78-167">Voici le contrôleur ShoppingCart complet.</span><span class="sxs-lookup"><span data-stu-id="32b78-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="32b78-168">Les actions d’index et d’ajout de contrôleur doivent paraître très familières.</span><span class="sxs-lookup"><span data-stu-id="32b78-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="32b78-169">Les actions du contrôleur Remove et CartSummary gèrent deux cas spéciaux, que nous aborderons dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="32b78-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="32b78-170">Mises à jour Ajax avec jQuery</span><span class="sxs-lookup"><span data-stu-id="32b78-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="32b78-171">Nous allons ensuite créer une page d’index de panier d’achat fortement typée en ShoppingCartViewModel et utilisant le modèle de vue liste en utilisant la même méthode que précédemment.</span><span class="sxs-lookup"><span data-stu-id="32b78-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="32b78-172">Toutefois, au lieu d’utiliser un fichier html. ActionLink pour supprimer des éléments du panier, nous allons utiliser jQuery pour « associer » l’événement Click pour tous les liens de cette vue qui ont la classe HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="32b78-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="32b78-173">Au lieu de publier le formulaire, ce gestionnaire d’événements Click effectue simplement un rappel AJAX à notre action de contrôleur RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="32b78-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="32b78-174">RemoveFromCart retourne un résultat sérialisé JSON, que notre rappel jQuery analyse ensuite et exécute quatre mises à jour rapides sur la page à l’aide de jQuery :</span><span class="sxs-lookup"><span data-stu-id="32b78-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="32b78-175">Supprime l’album supprimé de la liste</span><span class="sxs-lookup"><span data-stu-id="32b78-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="32b78-176">Met à jour le nombre de caddies dans l’en-tête</span><span class="sxs-lookup"><span data-stu-id="32b78-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="32b78-177">Affiche un message de mise à jour à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="32b78-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="32b78-178">Met à jour le prix total du panier</span><span class="sxs-lookup"><span data-stu-id="32b78-178">Updates the cart total price</span></span>

<span data-ttu-id="32b78-179">Étant donné que le scénario de suppression est géré par un rappel Ajax au sein de la vue index, nous n’avons pas besoin d’une vue supplémentaire pour l’action RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="32b78-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="32b78-180">Voici le code complet de la vue/ShoppingCart/Index :</span><span class="sxs-lookup"><span data-stu-id="32b78-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="32b78-181">Pour le tester, nous devons être en mesure d’ajouter des éléments à notre panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="32b78-182">Nous allons mettre à jour notre vue des **Détails du magasin** pour inclure un bouton « Ajouter au panier ».</span><span class="sxs-lookup"><span data-stu-id="32b78-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="32b78-183">Pendant que nous sommes là, nous pouvons inclure certaines informations supplémentaires sur l’album, que nous avons ajoutées depuis la dernière mise à jour de cette vue : genre, artiste, prix et pochette de l’album.</span><span class="sxs-lookup"><span data-stu-id="32b78-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="32b78-184">Le code de vue des détails du magasin mis à jour s’affiche comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="32b78-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="32b78-185">À présent, nous pouvons cliquer sur le Store et tester l’ajout et la suppression d’albums vers et depuis notre panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="32b78-186">Exécutez l’application et accédez à l’index Store.</span><span class="sxs-lookup"><span data-stu-id="32b78-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="32b78-187">Ensuite, cliquez sur un genre pour afficher une liste d’albums.</span><span class="sxs-lookup"><span data-stu-id="32b78-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="32b78-188">En cliquant sur le titre d’un album, vous affichez la vue des détails de l’album mis à jour, y compris le bouton « Ajouter au panier ».</span><span class="sxs-lookup"><span data-stu-id="32b78-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="32b78-189">Si vous cliquez sur le bouton « Ajouter au panier », notre vue d’index de panier d’achat s’affiche avec la liste Résumé du panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="32b78-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="32b78-190">Après avoir chargé votre panier, vous pouvez cliquer sur le lien supprimer du panier pour afficher la mise à jour Ajax de votre panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="32b78-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="32b78-191">Nous avons créé un panier d’achat qui permet aux utilisateurs non inscrits d’ajouter des éléments à leur panier.</span><span class="sxs-lookup"><span data-stu-id="32b78-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="32b78-192">Dans la section suivante, nous allons les autoriser à s’inscrire et à terminer le processus d’extraction.</span><span class="sxs-lookup"><span data-stu-id="32b78-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32b78-193">[Précédent](mvc-music-store-part-7.md)
> [Suivant](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="32b78-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
