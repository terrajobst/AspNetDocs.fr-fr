---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Partie 5 : logique métier | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 5 ajoute une certaine logique métier.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630304"
---
# <a name="part-5-business-logic"></a><span data-ttu-id="c6109-104">Partie 5 : logique métier</span><span class="sxs-lookup"><span data-stu-id="c6109-104">Part 5: Business Logic</span></span>

<span data-ttu-id="c6109-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="c6109-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="c6109-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="c6109-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="c6109-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="c6109-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="c6109-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="c6109-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="c6109-109">La partie 5 ajoute une certaine logique métier.</span><span class="sxs-lookup"><span data-stu-id="c6109-109">Part 5 adds some business logic.</span></span>

## <a id="_Toc260221671"></a><span data-ttu-id="c6109-110">Ajout d’une logique métier</span><span class="sxs-lookup"><span data-stu-id="c6109-110">Adding Some Business Logic</span></span>

<span data-ttu-id="c6109-111">Nous souhaitons que notre expérience d’achat soit disponible chaque fois que quelqu’un visite notre site Web.</span><span class="sxs-lookup"><span data-stu-id="c6109-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="c6109-112">Les visiteurs seront en mesure de parcourir et d’ajouter des éléments au panier d’achat même s’ils ne sont pas enregistrés ou connectés.</span><span class="sxs-lookup"><span data-stu-id="c6109-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="c6109-113">Lorsqu’ils sont prêts à vérifier, ils peuvent s’authentifier et, s’ils ne sont pas encore membres, ils peuvent créer un compte.</span><span class="sxs-lookup"><span data-stu-id="c6109-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="c6109-114">Cela signifie que nous aurons besoin d’implémenter la logique pour convertir le panier d’achat d’un État anonyme en état « utilisateur inscrit ».</span><span class="sxs-lookup"><span data-stu-id="c6109-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="c6109-115">Créons un répertoire nommé « classes », puis cliquons avec le bouton droit sur le dossier et créons un nouveau fichier de « classe » nommé MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="c6109-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="c6109-116">Comme nous l’avons vu précédemment, nous allons étendre la classe qui implémente la page MyShoppingCart. aspx et nous allons utiliser. Construction « partielle de classe » puissante de .net.</span><span class="sxs-lookup"><span data-stu-id="c6109-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="c6109-117">L’appel généré pour notre fichier MyShoppingCart.aspx.cf ressemble à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="c6109-118">Notez l’utilisation du mot clé « Partial ».</span><span class="sxs-lookup"><span data-stu-id="c6109-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="c6109-119">Le fichier de classe que nous venons de générer ressemble à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="c6109-120">Nous fusionnerons nos implémentations en ajoutant également le mot clé Partial à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="c6109-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="c6109-121">Notre nouveau fichier de classe ressemble maintenant à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="c6109-122">La première méthode que nous ajouterons à notre classe est la méthode « AddItem ».</span><span class="sxs-lookup"><span data-stu-id="c6109-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="c6109-123">Il s’agit de la méthode qui sera finalement appelée lorsque l’utilisateur cliquera sur les liens « ajouter à l’art » dans les pages liste des produits et Détails du produit.</span><span class="sxs-lookup"><span data-stu-id="c6109-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="c6109-124">Ajoutez ce qui suit aux instructions using en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="c6109-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="c6109-125">Et ajoutez cette méthode à la classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="c6109-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="c6109-126">Nous utilisons LINQ to Entities pour voir si l’élément est déjà dans le panier.</span><span class="sxs-lookup"><span data-stu-id="c6109-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="c6109-127">Dans ce cas, nous mettons à jour la quantité commandée de l’article. dans le cas contraire, nous créons une nouvelle entrée pour l’élément sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c6109-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="c6109-128">Pour appeler cette méthode, nous allons implémenter une page AddToCart. aspx qui non seulement classe cette méthode, mais qui affiche ensuite le panier en cours a = Cart après l’ajout de l’élément.</span><span class="sxs-lookup"><span data-stu-id="c6109-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="c6109-129">Cliquez avec le bouton droit sur le nom de la solution dans l’Explorateur de solutions et ajoutez une nouvelle page nommée AddToCart. aspx, comme nous l’avons fait précédemment.</span><span class="sxs-lookup"><span data-stu-id="c6109-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="c6109-130">Bien que nous puissions utiliser cette page pour afficher des résultats intermédiaires comme des problèmes de stock faible, etc., dans notre implémentation, la page ne s’affiche pas réellement, mais elle appelle plutôt la logique « ajouter » et la redirection.</span><span class="sxs-lookup"><span data-stu-id="c6109-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="c6109-131">Pour ce faire, nous allons ajouter le code suivant à la page\_événement Load.</span><span class="sxs-lookup"><span data-stu-id="c6109-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="c6109-132">Notez que nous récupérons le produit à ajouter au panier d’achat à partir d’un paramètre QueryString et que j’appelle la méthode AddItem de notre classe.</span><span class="sxs-lookup"><span data-stu-id="c6109-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="c6109-133">En supposant qu’aucune erreur ne se produise, le contrôle est passé à la page SHoppingCart. aspx que nous allons implémenter entièrement.</span><span class="sxs-lookup"><span data-stu-id="c6109-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="c6109-134">S’il doit y avoir une erreur, nous levez une exception.</span><span class="sxs-lookup"><span data-stu-id="c6109-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="c6109-135">Actuellement, nous n’avons pas encore implémenté de gestionnaire d’erreurs global. par conséquent, cette exception n’est pas gérée par notre application, mais nous y remédierons prochainement.</span><span class="sxs-lookup"><span data-stu-id="c6109-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="c6109-136">Notez également l’utilisation de l’instruction Debug. Fail () (disponible par le biais de `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="c6109-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="c6109-137">Si l’application s’exécute dans le débogueur, cette méthode affiche une boîte de dialogue détaillée avec les informations relatives à l’état des applications, ainsi que le message d’erreur que nous spécifions.</span><span class="sxs-lookup"><span data-stu-id="c6109-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="c6109-138">En cas d’exécution en production, l’instruction Debug. Fail () est ignorée.</span><span class="sxs-lookup"><span data-stu-id="c6109-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="c6109-139">Vous noterez dans le code ci-dessus un appel à une méthode dans nos noms de classe de panier d’achat « GetShoppingCartId ».</span><span class="sxs-lookup"><span data-stu-id="c6109-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="c6109-140">Ajoutez le code pour implémenter la méthode comme suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="c6109-141">Notez que nous avons également ajouté les boutons de mise à jour et d’extraction et une étiquette dans laquelle vous pouvez afficher le « total » du panier.</span><span class="sxs-lookup"><span data-stu-id="c6109-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="c6109-142">Nous pouvons maintenant ajouter des articles à notre panier, mais nous n’avons pas implémenté la logique permettant d’afficher le panier après l’ajout d’un produit.</span><span class="sxs-lookup"><span data-stu-id="c6109-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="c6109-143">Ainsi, dans la page MyShoppingCart. aspx, nous allons ajouter un contrôle EntityDataSource et un contrôle GridVire comme suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="c6109-144">Appelez le formulaire dans le concepteur afin de pouvoir double-cliquer sur le bouton mettre à jour le panier et générer le gestionnaire d’événements Click qui est spécifié dans la déclaration dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="c6109-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="c6109-145">Nous allons implémenter les détails ultérieurement, mais cela nous permettra de générer et d’exécuter notre application sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="c6109-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="c6109-146">Lorsque vous exécutez l’application et ajoutez un élément au panier d’achat, vous le verrez.</span><span class="sxs-lookup"><span data-stu-id="c6109-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="c6109-147">Notez que nous avons utilisé l’affichage de grille « par défaut » en implémentant trois colonnes personnalisées.</span><span class="sxs-lookup"><span data-stu-id="c6109-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="c6109-148">Le premier est un champ modifiable « lié » pour la quantité :</span><span class="sxs-lookup"><span data-stu-id="c6109-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="c6109-149">La suivante est une colonne « calculée » qui affiche le total de la ligne (le coût de l’élément multiplié par la quantité à commander) :</span><span class="sxs-lookup"><span data-stu-id="c6109-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="c6109-150">Enfin, nous avons une colonne personnalisée qui contient un contrôle CheckBox que l’utilisateur utilisera pour indiquer que l’élément doit être supprimé du graphique d’achat.</span><span class="sxs-lookup"><span data-stu-id="c6109-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="c6109-151">Comme vous pouvez le voir, la ligne de total de commande est vide. nous allons donc ajouter une logique pour calculer le total de la commande.</span><span class="sxs-lookup"><span data-stu-id="c6109-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="c6109-152">Nous allons tout d’abord implémenter une méthode « GetTotal » pour notre classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="c6109-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="c6109-153">Dans le fichier MyShoppingCart.cs, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="c6109-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="c6109-154">Ensuite, dans la page\_gestionnaire d’événements Load, nous pouvons appeler notre méthode GetTotal.</span><span class="sxs-lookup"><span data-stu-id="c6109-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="c6109-155">En même temps, nous allons ajouter un test pour voir si le panier d’achat est vide et ajuster l’affichage en conséquence, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="c6109-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="c6109-156">Maintenant, si le panier est vide, nous obtenons ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="c6109-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="c6109-157">Si ce n’est pas le cas, nous voyons notre total.</span><span class="sxs-lookup"><span data-stu-id="c6109-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="c6109-158">Toutefois, cette page n’est pas encore terminée.</span><span class="sxs-lookup"><span data-stu-id="c6109-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="c6109-159">Nous aurons besoin d’une logique supplémentaire pour recalculer le panier d’achat en supprimant les éléments marqués pour suppression et en déterminant les nouvelles valeurs de quantité, car certaines peuvent avoir été modifiées dans la grille par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c6109-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="c6109-160">Permet d’ajouter une méthode « RemoveItem » à notre classe de panier d’achat dans MyShoppingCart.cs pour gérer le cas où un utilisateur marque un élément en vue de sa suppression.</span><span class="sxs-lookup"><span data-stu-id="c6109-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="c6109-161">À présent, nous allons utiliser une méthode pour gérer le cas où un utilisateur modifie simplement la qualité à trier dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="c6109-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="c6109-162">Avec les fonctionnalités de base de suppression et de mise à jour en place, nous pouvons implémenter la logique qui met réellement à jour le panier d’achat dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6109-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="c6109-163">(Dans MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="c6109-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="c6109-164">Vous noterez que cette méthode attend deux paramètres.</span><span class="sxs-lookup"><span data-stu-id="c6109-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="c6109-165">L’un est l’ID du panier d’achat et l’autre est un tableau d’objets de type défini par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c6109-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="c6109-166">Par conséquent, pour réduire la dépendance de notre logique sur les caractéristiques de l’interface utilisateur, nous avons défini une structure de données que nous pouvons utiliser pour transmettre les éléments du panier d’achat à notre code sans que notre méthode doive accéder directement au contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="c6109-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="c6109-167">Dans notre fichier MyShoppingCart.aspx.cs, nous pouvons utiliser cette structure dans notre gestionnaire d’événements de clic du bouton mettre à jour comme suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="c6109-168">Notez qu’en plus de la mise à jour du panier, nous mettrons également à jour le total du panier.</span><span class="sxs-lookup"><span data-stu-id="c6109-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="c6109-169">Notez, avec un intérêt particulier, cette ligne de code :</span><span class="sxs-lookup"><span data-stu-id="c6109-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="c6109-170">GetValues () est une fonction d’assistance spéciale que nous allons implémenter dans MyShoppingCart.aspx.cs comme suit.</span><span class="sxs-lookup"><span data-stu-id="c6109-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="c6109-171">Cela offre un moyen propre pour accéder aux valeurs des éléments liés dans notre contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="c6109-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="c6109-172">Étant donné que notre contrôle de case à cocher « Supprimer l’élément » n’est pas lié, nous y accéderons via la méthode FindControl ().</span><span class="sxs-lookup"><span data-stu-id="c6109-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="c6109-173">À ce niveau du développement de votre projet, nous sommes prêts à implémenter le processus d’extraction.</span><span class="sxs-lookup"><span data-stu-id="c6109-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="c6109-174">Avant cela, nous allons utiliser Visual Studio pour générer la base de données d’appartenance et ajouter un utilisateur au référentiel d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="c6109-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c6109-175">[Précédent](tailspin-spyworks-part-4.md)
> [Suivant](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c6109-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
