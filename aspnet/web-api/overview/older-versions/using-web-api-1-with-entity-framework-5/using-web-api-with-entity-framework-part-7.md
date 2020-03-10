---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Partie 7 : création de la page principale | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598678"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="39a40-102">Partie 7 : création de la page principale</span><span class="sxs-lookup"><span data-stu-id="39a40-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="39a40-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="39a40-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="39a40-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="39a40-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="39a40-105">Création de la page principale</span><span class="sxs-lookup"><span data-stu-id="39a40-105">Creating the Main Page</span></span>

<span data-ttu-id="39a40-106">Dans cette section, vous allez créer la page principale de l’application.</span><span class="sxs-lookup"><span data-stu-id="39a40-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="39a40-107">Cette page est plus complexe que la page d’administration. nous allons donc l’aborder en plusieurs étapes.</span><span class="sxs-lookup"><span data-stu-id="39a40-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="39a40-108">En cours de route, vous verrez des techniques plus avancées de Knockout. js.</span><span class="sxs-lookup"><span data-stu-id="39a40-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="39a40-109">Voici la disposition de base de la page :</span><span class="sxs-lookup"><span data-stu-id="39a40-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="39a40-110">« Products » contient un tableau de produits.</span><span class="sxs-lookup"><span data-stu-id="39a40-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="39a40-111">« Cart » contient un tableau de produits avec des quantités.</span><span class="sxs-lookup"><span data-stu-id="39a40-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="39a40-112">Cliquez sur « Ajouter au panier » pour mettre à jour le panier.</span><span class="sxs-lookup"><span data-stu-id="39a40-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="39a40-113">« Orders » contient un tableau d’ID de commande.</span><span class="sxs-lookup"><span data-stu-id="39a40-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="39a40-114">« Détails » contient un détail de commande, qui est un tableau d’éléments (produits avec quantités)</span><span class="sxs-lookup"><span data-stu-id="39a40-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="39a40-115">Nous allons commencer par définir une mise en page de base en HTML, sans liaison de données ni script.</span><span class="sxs-lookup"><span data-stu-id="39a40-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="39a40-116">Ouvrez le fichier views/orig/index. cshtml et remplacez tout le contenu par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="39a40-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="39a40-117">Ensuite, ajoutez une section scripts et créez un modèle d’affichage vide :</span><span class="sxs-lookup"><span data-stu-id="39a40-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="39a40-118">Selon la conception esquissée précédemment, notre modèle de vue a besoin de observables pour les produits, les paniers, les commandes et les détails.</span><span class="sxs-lookup"><span data-stu-id="39a40-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="39a40-119">Ajoutez les variables suivantes à l’objet `AppViewModel` :</span><span class="sxs-lookup"><span data-stu-id="39a40-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="39a40-120">Les utilisateurs peuvent ajouter des éléments à partir de la liste des produits dans le panier et supprimer des éléments du panier.</span><span class="sxs-lookup"><span data-stu-id="39a40-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="39a40-121">Pour encapsuler ces fonctions, nous allons créer une autre classe de modèle d’affichage qui représente un produit.</span><span class="sxs-lookup"><span data-stu-id="39a40-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="39a40-122">Ajoutez le code suivant à `AppViewModel` :</span><span class="sxs-lookup"><span data-stu-id="39a40-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="39a40-123">La classe `ProductViewModel` contient deux fonctions qui permettent de déplacer le produit vers et à partir du panier : `addItemToCart` ajoute une unité du produit au panier et `removeAllFromCart` supprime toutes les quantités du produit.</span><span class="sxs-lookup"><span data-stu-id="39a40-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="39a40-124">Les utilisateurs peuvent sélectionner une commande existante et récupérer les détails de la commande.</span><span class="sxs-lookup"><span data-stu-id="39a40-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="39a40-125">Nous encapsulerons cette fonctionnalité dans un autre modèle d’affichage :</span><span class="sxs-lookup"><span data-stu-id="39a40-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="39a40-126">La `OrderDetailsViewModel` est initialisée avec une commande et extrait les détails de la commande en envoyant une demande AJAX au serveur.</span><span class="sxs-lookup"><span data-stu-id="39a40-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="39a40-127">Notez également la propriété `total` sur le `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="39a40-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="39a40-128">Cette propriété est un type spécial d’observable appelé [observable calculé](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="39a40-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="39a40-129">Comme son nom l’indique, un observable calculé vous permet de lier des données à une valeur&#8212;calculée dans ce cas, le coût total de la commande.</span><span class="sxs-lookup"><span data-stu-id="39a40-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="39a40-130">Ensuite, ajoutez ces fonctions à `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="39a40-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="39a40-131">`resetCart` supprime tous les éléments du panier.</span><span class="sxs-lookup"><span data-stu-id="39a40-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="39a40-132">`getDetails` obtient les détails d’une commande (en envoyant un nouvel `OrderDetailsViewModel` dans la liste `details`).</span><span class="sxs-lookup"><span data-stu-id="39a40-132">`getDetails` gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="39a40-133">`createOrder` crée une nouvelle commande et vide le panier.</span><span class="sxs-lookup"><span data-stu-id="39a40-133">`createOrder` creates a new order and empties the cart.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="39a40-134">Enfin, initialisez le modèle de vue en effectuant des demandes AJAX pour les produits et les commandes :</span><span class="sxs-lookup"><span data-stu-id="39a40-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="39a40-135">Bon, c’est beaucoup de code, mais nous l’avons créé pas à pas, donc j’espère que la conception est claire.</span><span class="sxs-lookup"><span data-stu-id="39a40-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="39a40-136">Nous pouvons à présent ajouter des liaisons Knockout. js au code HTML.</span><span class="sxs-lookup"><span data-stu-id="39a40-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="39a40-137">**Produits**</span><span class="sxs-lookup"><span data-stu-id="39a40-137">**Products**</span></span>

<span data-ttu-id="39a40-138">Voici les liaisons de la liste de produits :</span><span class="sxs-lookup"><span data-stu-id="39a40-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="39a40-139">Cela permet d’effectuer une itération sur le tableau Products et d’afficher le nom et le prix.</span><span class="sxs-lookup"><span data-stu-id="39a40-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="39a40-140">Le bouton « Ajouter à la commande » n’est visible que lorsque l’utilisateur a ouvert une session.</span><span class="sxs-lookup"><span data-stu-id="39a40-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="39a40-141">Le bouton « Ajouter à la commande » appelle `addItemToCart` sur l’instance `ProductViewModel` pour le produit.</span><span class="sxs-lookup"><span data-stu-id="39a40-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="39a40-142">Cela démontre une fonctionnalité intéressante de Knockout. js : quand un modèle de vue contient d’autres modèles d’affichage, vous pouvez appliquer les liaisons au modèle interne.</span><span class="sxs-lookup"><span data-stu-id="39a40-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="39a40-143">Dans cet exemple, les liaisons au sein du `foreach` sont appliquées à chacune des instances de `ProductViewModel`.</span><span class="sxs-lookup"><span data-stu-id="39a40-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="39a40-144">Cette approche est bien plus propre que de placer toutes les fonctionnalités dans un modèle de vue unique.</span><span class="sxs-lookup"><span data-stu-id="39a40-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="39a40-145">**Caddie**</span><span class="sxs-lookup"><span data-stu-id="39a40-145">**Cart**</span></span>

<span data-ttu-id="39a40-146">Voici les liaisons du panier :</span><span class="sxs-lookup"><span data-stu-id="39a40-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="39a40-147">Cela permet d’effectuer une itération sur le tableau Cart et d’afficher le nom, le prix et la quantité.</span><span class="sxs-lookup"><span data-stu-id="39a40-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="39a40-148">Notez que le lien « supprimer » et le bouton « créer une commande » sont liés aux fonctions de modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="39a40-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="39a40-149">**Commandes**</span><span class="sxs-lookup"><span data-stu-id="39a40-149">**Orders**</span></span>

<span data-ttu-id="39a40-150">Voici les liaisons de la liste Orders :</span><span class="sxs-lookup"><span data-stu-id="39a40-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="39a40-151">Cela permet d’effectuer une itération sur les commandes et d’afficher l’ID de commande.</span><span class="sxs-lookup"><span data-stu-id="39a40-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="39a40-152">L’événement Click sur le lien est lié à la fonction `getDetails`.</span><span class="sxs-lookup"><span data-stu-id="39a40-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="39a40-153">**Détails de la commande**</span><span class="sxs-lookup"><span data-stu-id="39a40-153">**Order Details**</span></span>

<span data-ttu-id="39a40-154">Voici les liaisons pour les détails de la commande :</span><span class="sxs-lookup"><span data-stu-id="39a40-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="39a40-155">Cela permet d’effectuer une itération sur les éléments dans l’ordre et d’afficher le produit, le prix et la quantité.</span><span class="sxs-lookup"><span data-stu-id="39a40-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="39a40-156">La balise div environnante est visible uniquement si le tableau de détails contient un ou plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="39a40-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="39a40-157">Conclusion</span><span class="sxs-lookup"><span data-stu-id="39a40-157">Conclusion</span></span>

<span data-ttu-id="39a40-158">Dans ce didacticiel, vous avez créé une application qui utilise Entity Framework pour communiquer avec la base de données, et API Web ASP.NET pour fournir une interface publique en plus de la couche de données.</span><span class="sxs-lookup"><span data-stu-id="39a40-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="39a40-159">Nous utilisons ASP.NET MVC 4 pour afficher les pages HTML et Knockout. js plus jQuery pour fournir des interactions dynamiques sans rechargements de pages.</span><span class="sxs-lookup"><span data-stu-id="39a40-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="39a40-160">Ressources supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="39a40-160">Additional resources:</span></span>

- [<span data-ttu-id="39a40-161">Plan de contenu d’accès aux données ASP.NET</span><span class="sxs-lookup"><span data-stu-id="39a40-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="39a40-162">Centre de développement Entity Framework</span><span class="sxs-lookup"><span data-stu-id="39a40-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="39a40-163">Précédent</span><span class="sxs-lookup"><span data-stu-id="39a40-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
