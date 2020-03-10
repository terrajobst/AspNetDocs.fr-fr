---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Partie 4 : liste des produits | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 4 couvre la liste des produits avec le contrôle GridView contr...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566982"
---
# <a name="part-4-listing-products"></a><span data-ttu-id="4d0b2-104">Partie 4 : liste des produits</span><span class="sxs-lookup"><span data-stu-id="4d0b2-104">Part 4: Listing Products</span></span>

<span data-ttu-id="4d0b2-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4d0b2-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4d0b2-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4d0b2-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4d0b2-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4d0b2-109">La partie 4 couvre la liste des produits avec le contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-109">Part 4 covers listing products with the GridView control.</span></span>

## <a id="_Toc260221670"></a><span data-ttu-id="4d0b2-110">Liste des produits avec le contrôle GridView</span><span class="sxs-lookup"><span data-stu-id="4d0b2-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="4d0b2-111">Commençons à implémenter notre page ProductsList. aspx en cliquant avec le bouton droit sur notre solution et en sélectionnant « Ajouter » et « nouvel élément ».</span><span class="sxs-lookup"><span data-stu-id="4d0b2-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="4d0b2-112">Choisissez « Web Form using Master page » et entrez le nom de la page ProductsList. aspx».</span><span class="sxs-lookup"><span data-stu-id="4d0b2-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="4d0b2-113">Cliquez sur « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="4d0b2-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="4d0b2-114">Ensuite, choisissez le dossier « styles » dans lequel nous avons placé la page site. Master et sélectionnez-la dans la fenêtre « contenu du dossier ».</span><span class="sxs-lookup"><span data-stu-id="4d0b2-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="4d0b2-115">Cliquez sur OK pour créer la page.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="4d0b2-116">Notre base de données est remplie avec les données du produit, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="4d0b2-117">Une fois la page créée, nous allons utiliser une source de données d’entité pour accéder aux données du produit, mais dans cette instance, nous devons sélectionner les entités Product et nous devons restreindre les éléments qui sont renvoyés uniquement à ceux de la catégorie sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="4d0b2-118">Pour ce faire, nous indiquons à EntityDataSource de générer automatiquement la clause WHERE et nous allons spécifier WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="4d0b2-119">Vous vous souviendrez que lorsque nous avons créé les éléments de menu dans notre « menu de catégorie de produits », nous avons créé le lien de manière dynamique en ajoutant CategoryID à la chaîne de chaîne de chaque lien.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CategoryID to the QueryString for each link.</span></span> <span data-ttu-id="4d0b2-120">Nous indiquons à la source de données d’entité de dériver le paramètre WHERE de ce paramètre QueryString.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="4d0b2-121">Ensuite, nous allons configurer le contrôle ListView pour afficher une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="4d0b2-122">Pour créer une expérience d’achat optimale, nous allons compacter plusieurs fonctionnalités concises dans chaque produit affiché dans notre ListVew.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="4d0b2-123">Le nom du produit est un lien vers l’affichage détaillé du produit.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="4d0b2-124">Le prix du produit s’affiche.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="4d0b2-125">Une image du produit s’affiche et l’image est sélectionnée de manière dynamique dans un répertoire d’images de catalogue dans notre application.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="4d0b2-126">Nous allons inclure un lien pour ajouter immédiatement le produit spécifique au panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="4d0b2-127">Voici le balisage de notre instance de contrôle ListView.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="4d0b2-128">Nous créons de manière dynamique plusieurs liens pour chaque produit affiché.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="4d0b2-129">En outre, avant de tester une nouvelle page, nous devons créer la structure de répertoires pour les images du catalogue de produits comme suit.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="4d0b2-130">Une fois nos images de produit accessibles, nous pouvons tester notre page de liste de produits.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="4d0b2-131">Sur la page d’hébergement du site, cliquez sur l’un des liens de la liste des catégories.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="4d0b2-132">Nous devons à présent implémenter la page ProductDetails. aspx et la fonctionnalité AddToCart.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-132">Now we need to implement the ProductDetails.aspx page and the AddToCart functionality.</span></span>

<span data-ttu-id="4d0b2-133">Utilisez fichier-&gt;nouveau pour créer un nom de page ProductDetails. aspx à l’aide de la page maître de site comme nous l’avons fait précédemment.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="4d0b2-134">Nous utiliserons à nouveau un contrôle EntityDataSource pour accéder à l’enregistrement de produit spécifique dans la base de données et nous utiliserons un contrôle FormView ASP.NET pour afficher les données du produit comme suit.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="4d0b2-135">Ne vous inquiétez pas si la mise en forme semble un peu amusante pour vous.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="4d0b2-136">Le balisage ci-dessus laisse de l’espace dans la disposition d’affichage pour quelques fonctionnalités que nous allons implémenter ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="4d0b2-137">Le panier représente la logique la plus complexe de notre application.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="4d0b2-138">Pour commencer, utilisez fichier-&gt;nouveau pour créer une page nommée MyShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="4d0b2-139">Notez que nous n’avons pas choisi le nom ShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="4d0b2-140">Notre base de données contient une table nommée « ShoppingCart ».</span><span class="sxs-lookup"><span data-stu-id="4d0b2-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="4d0b2-141">Lorsque nous avons généré une Entity Data Model une classe a été créée pour chaque table de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="4d0b2-142">Par conséquent, le Entity Data Model généré une classe d’entité nommée « ShoppingCart ».</span><span class="sxs-lookup"><span data-stu-id="4d0b2-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="4d0b2-143">Nous pouvons modifier le modèle afin de pouvoir utiliser ce nom pour notre implémentation de panier d’achat ou l’étendre pour nos besoins, mais nous allons choisir de simplement sélectionner un nom qui évitera le conflit.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply select a name that will avoid the conflict.</span></span>

<span data-ttu-id="4d0b2-144">Il est également intéressant de noter que nous allons créer un panier d’achat simple et incorporer la logique du panier d’achat avec l’affichage du panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="4d0b2-145">Nous pouvons également choisir d’implémenter notre panier d’achat dans une couche métier complètement distincte.</span><span class="sxs-lookup"><span data-stu-id="4d0b2-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4d0b2-146">[Précédent](tailspin-spyworks-part-3.md)
> [Suivant](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="4d0b2-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
