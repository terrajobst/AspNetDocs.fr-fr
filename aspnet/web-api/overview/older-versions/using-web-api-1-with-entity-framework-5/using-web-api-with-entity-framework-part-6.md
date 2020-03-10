---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Partie 6 : création de contrôleurs de produit et de commandes | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78524989"
---
# <a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="30822-102">Partie 6 : création de contrôleurs de produit et de commandes</span><span class="sxs-lookup"><span data-stu-id="30822-102">Part 6: Creating Product and Order Controllers</span></span>

<span data-ttu-id="30822-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="30822-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="30822-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="30822-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="30822-105">Ajouter un contrôleur Products</span><span class="sxs-lookup"><span data-stu-id="30822-105">Add a Products Controller</span></span>

<span data-ttu-id="30822-106">Le contrôleur d’administration est destiné aux utilisateurs disposant de privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="30822-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="30822-107">En revanche, les clients peuvent afficher les produits, mais ils ne peuvent pas les créer, les mettre à jour ou les supprimer.</span><span class="sxs-lookup"><span data-stu-id="30822-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="30822-108">Nous pouvons facilement limiter l’accès aux méthodes de publication, put et Delete, tout en laissant les méthodes d’extraction ouvertes.</span><span class="sxs-lookup"><span data-stu-id="30822-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="30822-109">Examinez les données retournées pour un produit :</span><span class="sxs-lookup"><span data-stu-id="30822-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="30822-110">La propriété `ActualCost` ne doit pas être visible pour les clients.</span><span class="sxs-lookup"><span data-stu-id="30822-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="30822-111">La solution consiste à définir un *objet de transfert de données* (DTO) qui comprend un sous-ensemble de propriétés qui doivent être visibles par les clients.</span><span class="sxs-lookup"><span data-stu-id="30822-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="30822-112">Nous allons utiliser LINQ pour projeter des instances de `Product` pour `ProductDTO` instances.</span><span class="sxs-lookup"><span data-stu-id="30822-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="30822-113">Ajoutez une classe nommée `ProductDTO` au dossier Models.</span><span class="sxs-lookup"><span data-stu-id="30822-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="30822-114">Ajoutez maintenant le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="30822-114">Now add the controller.</span></span> <span data-ttu-id="30822-115">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers.</span><span class="sxs-lookup"><span data-stu-id="30822-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="30822-116">Sélectionnez **Ajouter**, puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="30822-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="30822-117">Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur &quot;&quot;ProductsController.</span><span class="sxs-lookup"><span data-stu-id="30822-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="30822-118">Sous **modèle**, sélectionnez **contrôleur d’API vide**.</span><span class="sxs-lookup"><span data-stu-id="30822-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="30822-119">Remplacez tout ce qui se trouve dans le fichier source par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="30822-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="30822-120">Le contrôleur utilise toujours le `OrdersContext` pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="30822-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="30822-121">Mais au lieu de retourner directement des instances de `Product`, nous appelons `MapProducts` pour les projeter sur `ProductDTO` instances :</span><span class="sxs-lookup"><span data-stu-id="30822-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="30822-122">La méthode `MapProducts` retourne un **IQueryable**, donc nous pouvons composer le résultat avec d’autres paramètres de requête.</span><span class="sxs-lookup"><span data-stu-id="30822-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="30822-123">Vous pouvez le voir dans la méthode `GetProduct`, qui ajoute une clause **Where** à la requête :</span><span class="sxs-lookup"><span data-stu-id="30822-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="30822-124">Ajouter un contrôleur de commandes</span><span class="sxs-lookup"><span data-stu-id="30822-124">Add an Orders Controller</span></span>

<span data-ttu-id="30822-125">Ensuite, ajoutez un contrôleur qui permet aux utilisateurs de créer et d’afficher des commandes.</span><span class="sxs-lookup"><span data-stu-id="30822-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="30822-126">Nous allons commencer par un autre DTO.</span><span class="sxs-lookup"><span data-stu-id="30822-126">We'll start with another DTO.</span></span> <span data-ttu-id="30822-127">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Models et ajoutez une classe nommée `OrderDTO` utilisez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="30822-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="30822-128">Ajoutez maintenant le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="30822-128">Now add the controller.</span></span> <span data-ttu-id="30822-129">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers.</span><span class="sxs-lookup"><span data-stu-id="30822-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="30822-130">Sélectionnez **Ajouter**, puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="30822-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="30822-131">Dans la boîte de dialogue **Ajouter un contrôleur** , définissez les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="30822-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="30822-132">Sous **nom du contrôleur**, entrez « OrdersController ».</span><span class="sxs-lookup"><span data-stu-id="30822-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="30822-133">Sous **modèle**, sélectionnez contrôleur d’API avec actions en lecture/écriture, à l’aide de Entity Framework».</span><span class="sxs-lookup"><span data-stu-id="30822-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="30822-134">Sous **classe de modèle**, sélectionnez &quot;commande (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="30822-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="30822-135">Sous **classe de contexte de données**, sélectionnez &quot;OrdersContext (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="30822-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="30822-136">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="30822-136">Click **Add**.</span></span> <span data-ttu-id="30822-137">Cela ajoute un fichier nommé OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="30822-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="30822-138">Ensuite, nous devons modifier l’implémentation par défaut du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="30822-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="30822-139">Tout d’abord, supprimez les méthodes `PutOrder` et `DeleteOrder`.</span><span class="sxs-lookup"><span data-stu-id="30822-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="30822-140">Pour cet exemple, les clients ne peuvent pas modifier ou supprimer des commandes existantes.</span><span class="sxs-lookup"><span data-stu-id="30822-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="30822-141">Dans une application réelle, vous auriez besoin d’un grand nombre de logiques principales pour gérer ces cas.</span><span class="sxs-lookup"><span data-stu-id="30822-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="30822-142">(Par exemple, la commande a-t-elle déjà été expédiée ?)</span><span class="sxs-lookup"><span data-stu-id="30822-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="30822-143">Modifiez la méthode `GetOrders` pour retourner uniquement les commandes qui appartiennent à l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="30822-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="30822-144">Modifiez la méthode `GetOrder` comme suit :</span><span class="sxs-lookup"><span data-stu-id="30822-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="30822-145">Voici les modifications que nous avons apportées à la méthode :</span><span class="sxs-lookup"><span data-stu-id="30822-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="30822-146">La valeur de retour est une instance de `OrderDTO`, au lieu d’une `Order`.</span><span class="sxs-lookup"><span data-stu-id="30822-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="30822-147">Lors de l’interrogation de la base de données pour la commande, nous utilisons la méthode [DbQuery. include](https://msdn.microsoft.com/library/gg696395) pour extraire les entités `OrderDetail` et `Product` associées.</span><span class="sxs-lookup"><span data-stu-id="30822-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="30822-148">Nous aplatirons le résultat à l’aide d’une projection.</span><span class="sxs-lookup"><span data-stu-id="30822-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="30822-149">La réponse HTTP contient un tableau de produits dont les quantités sont :</span><span class="sxs-lookup"><span data-stu-id="30822-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="30822-150">Ce format est plus facile à utiliser par les clients que le graphique d’objets d’origine, qui contient des entités imbriquées (Order, Details et Products).</span><span class="sxs-lookup"><span data-stu-id="30822-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="30822-151">Dernière méthode à prendre en compte `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="30822-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="30822-152">Pour le moment, cette méthode prend une instance de `Order`.</span><span class="sxs-lookup"><span data-stu-id="30822-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="30822-153">Mais réfléchissez à ce qui se passe si un client envoie un corps de requête comme suit :</span><span class="sxs-lookup"><span data-stu-id="30822-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="30822-154">Il s’agit d’un ordre bien structuré et Entity Framework l’insère dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="30822-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="30822-155">Mais il contient une entité Product qui n’existait pas précédemment.</span><span class="sxs-lookup"><span data-stu-id="30822-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="30822-156">Le client vient de créer un nouveau produit dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="30822-156">The client just created a new product in our database!</span></span> <span data-ttu-id="30822-157">Ce sera une surprise pour le service Order envois, lorsqu’ils verront une commande pour Koala.</span><span class="sxs-lookup"><span data-stu-id="30822-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="30822-158">Le moral est, soyez très prudent quant aux données que vous acceptez dans une demande de publication ou de placement.</span><span class="sxs-lookup"><span data-stu-id="30822-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="30822-159">Pour éviter ce problème, modifiez la méthode `PostOrder` pour prendre une `OrderDTO` instance.</span><span class="sxs-lookup"><span data-stu-id="30822-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="30822-160">Utilisez la `OrderDTO` pour créer l' `Order`.</span><span class="sxs-lookup"><span data-stu-id="30822-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="30822-161">Notez que nous utilisons les propriétés `ProductID` et `Quantity`, et que nous ignorons toutes les valeurs que le client a envoyées pour le nom ou le prix du produit.</span><span class="sxs-lookup"><span data-stu-id="30822-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="30822-162">Si l’ID de produit n’est pas valide, il violera la contrainte de clé étrangère dans la base de données, et l’insertion échouera, comme c’est le cas.</span><span class="sxs-lookup"><span data-stu-id="30822-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="30822-163">Voici la méthode `PostOrder` complète :</span><span class="sxs-lookup"><span data-stu-id="30822-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="30822-164">Enfin, ajoutez l’attribut **Authorize** au contrôleur :</span><span class="sxs-lookup"><span data-stu-id="30822-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="30822-165">Désormais, seuls les utilisateurs inscrits peuvent créer ou afficher des commandes.</span><span class="sxs-lookup"><span data-stu-id="30822-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="30822-166">[Précédent](using-web-api-with-entity-framework-part-5.md)
> [Suivant](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="30822-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
