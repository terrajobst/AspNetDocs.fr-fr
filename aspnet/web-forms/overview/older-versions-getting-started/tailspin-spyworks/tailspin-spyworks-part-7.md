---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Partie 7 : ajout de fonctionnalités | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 7 ajoute des fonctionnalités supplémentaires, telles que le compte Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641994"
---
# <a name="part-7-adding-features"></a><span data-ttu-id="1a8eb-104">Partie 7 : ajout de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="1a8eb-104">Part 7: Adding Features</span></span>

<span data-ttu-id="1a8eb-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1a8eb-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="1a8eb-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="1a8eb-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="1a8eb-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="1a8eb-109">La partie 7 ajoute des fonctionnalités supplémentaires, telles que la révision de compte, les révisions de produits et les contrôles utilisateur « éléments populaires » et « également achetés ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>

## <a id="_Toc260221673"></a><span data-ttu-id="1a8eb-110">Ajout de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="1a8eb-110">Adding Features</span></span>

<span data-ttu-id="1a8eb-111">Bien que les utilisateurs puissent parcourir notre catalogue, placer des articles dans leur panier et terminer le processus de validation, il existe un certain nombre de fonctionnalités de prise en charge que nous inclurons pour améliorer notre site.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="1a8eb-112">Revue de compte (répertorier les commandes passées et afficher les détails.)</span><span class="sxs-lookup"><span data-stu-id="1a8eb-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="1a8eb-113">Ajoutez du contenu spécifique au contexte sur la page de premier plan.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="1a8eb-114">Ajoutez une fonctionnalité pour permettre aux utilisateurs de passer en revue les produits du catalogue.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="1a8eb-115">Créez un contrôle utilisateur pour afficher les éléments populaires et placez ce contrôle sur la première page.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="1a8eb-116">Créez un contrôle utilisateur « également acheté » et ajoutez-le à la page Détails du produit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="1a8eb-117">Ajoutez une page de contact.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="1a8eb-118">Ajoutez une page à propos de.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-118">Add an About Page.</span></span>
8. <span data-ttu-id="1a8eb-119">Erreur globale</span><span class="sxs-lookup"><span data-stu-id="1a8eb-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="1a8eb-120">Revue de compte</span><span class="sxs-lookup"><span data-stu-id="1a8eb-120">Account Review</span></span>

<span data-ttu-id="1a8eb-121">Dans le dossier « Account », créez deux pages. aspx, l’une nommée OrderList. aspx et l’autre nommée OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="1a8eb-122">OrderList. aspx tirera parti des contrôles GridView et EntityDataSource comme nous l’avons déjà fait.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-122">OrderList.aspx will leverage the GridView and EntityDataSource controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="1a8eb-123">La fonction EntityDataSource sélectionne les enregistrements de la table Orders filtrés sur le nom d’utilisateur (voir WhereParameter) que nous avons défini dans une variable de session lorsque le journal de l’utilisateur est dans.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-123">The EntityDataSource selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="1a8eb-124">Notez également ces paramètres dans le HyperlinkField du contrôle GridView :</span><span class="sxs-lookup"><span data-stu-id="1a8eb-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="1a8eb-125">Celles-ci spécifient le lien vers la vue des détails de la commande pour chaque produit en spécifiant le champ OrderID en tant que paramètre QueryString dans la page OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="1a8eb-126">OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="1a8eb-126">OrderDetails.aspx</span></span>

<span data-ttu-id="1a8eb-127">Nous allons utiliser un contrôle EntityDataSource pour accéder aux commandes et à un FormView pour afficher les données de commande et un autre EntityDataSource avec un GridView pour afficher tous les éléments de ligne de la commande.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="1a8eb-128">Dans le fichier code-behind (OrderDetails.aspx.cs), nous avons deux petits bits de maintenance.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="1a8eb-129">Tout d’abord, assurez-vous que OrderDetails obtient toujours un OrderId.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="1a8eb-130">Nous avons également besoin de calculer et d’afficher le total des commandes à partir des éléments de ligne.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="1a8eb-131">Page d’hébergement</span><span class="sxs-lookup"><span data-stu-id="1a8eb-131">The Home Page</span></span>

<span data-ttu-id="1a8eb-132">Nous allons ajouter du contenu statique à la page default. aspx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="1a8eb-133">Je vais tout d’abord créer un dossier « Content » (contenu) et, dans celui-ci, un dossier images (et je vais inclure une image à utiliser sur la page d’hébergement).</span><span class="sxs-lookup"><span data-stu-id="1a8eb-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="1a8eb-134">Dans l’espace réservé inférieur de la page default. aspx, ajoutez le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="1a8eb-135">Révisions de produits</span><span class="sxs-lookup"><span data-stu-id="1a8eb-135">Product Reviews</span></span>

<span data-ttu-id="1a8eb-136">Tout d’abord, nous allons ajouter un bouton avec un lien vers un formulaire que nous pouvons utiliser pour passer en revue un produit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="1a8eb-137">Notez que nous transmettons le ProductID dans la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="1a8eb-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="1a8eb-138">Nous allons maintenant ajouter une page nommée ReviewAdd. aspx</span><span class="sxs-lookup"><span data-stu-id="1a8eb-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="1a8eb-139">Cette page utilise ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="1a8eb-140">Si vous ne l’avez pas encore fait, vous pouvez le télécharger à partir de [devexpress](http://devexpress.com/act) . vous y trouverez des conseils sur la configuration de la boîte à outils pour une utilisation avec Visual Studio [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="1a8eb-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="1a8eb-141">En mode création, faites glisser les contrôles et les validateurs de la boîte à outils et créez un formulaire comme celui ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="1a8eb-142">La balise doit ressembler à ceci.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="1a8eb-143">Maintenant que nous pouvons entrer des révisions, permet d’afficher ces révisions sur la page du produit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="1a8eb-144">Ajoutez cette balise à la page ProductDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="1a8eb-145">L’exécution de notre application maintenant et la navigation vers un produit affichent les informations sur le produit, y compris les évaluations des clients.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="1a8eb-146">Contrôle des éléments populaires (création de contrôles utilisateur)</span><span class="sxs-lookup"><span data-stu-id="1a8eb-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="1a8eb-147">Afin d’augmenter les ventes sur votre site Web, nous allons ajouter quelques fonctionnalités à des produits populaires ou connexes.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="1a8eb-148">La première de ces fonctionnalités sera une liste des produits les plus populaires dans notre catalogue de produits.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="1a8eb-149">Nous allons créer un « contrôle utilisateur » pour afficher les éléments les plus vendus sur la page d’hébergement de notre application.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="1a8eb-150">Comme il s’agit d’un contrôle, nous pouvons l’utiliser sur n’importe quelle page en faisant simplement glisser-déplacer le contrôle dans le concepteur de Visual Studio vers une page de votre choix.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="1a8eb-151">Dans l’Explorateur de solutions de Visual Studio, cliquez avec le bouton droit sur le nom de la solution et créez un nouveau répertoire nommé « contrôles ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="1a8eb-152">Bien qu’il ne soit pas nécessaire de le faire, nous vous aidons à garder notre projet organisé en créant tous nos contrôles utilisateur dans le répertoire « contrôles ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="1a8eb-153">Cliquez avec le bouton droit sur le dossier contrôles, puis choisissez « nouvel élément » :</span><span class="sxs-lookup"><span data-stu-id="1a8eb-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="1a8eb-154">Spécifiez un nom pour notre contrôle de « PopularItems ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="1a8eb-155">Notez que l’extension de fichier pour les contrôles utilisateur est. ascx not. aspx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="1a8eb-156">Le contrôle utilisateur des éléments populaires est défini comme suit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="1a8eb-157">Ici, nous utilisons une méthode que nous n’avons pas encore utilisée dans cette application.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="1a8eb-158">Nous utilisons le contrôle Repeater et, au lieu d’utiliser un contrôle de source de données, nous créons la liaison entre le contrôle Repeater et les résultats d’une requête LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="1a8eb-159">Dans le code-behind de notre contrôle, procédez comme suit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="1a8eb-160">Notez également cette ligne importante en haut du balisage de votre contrôle.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="1a8eb-161">Étant donné que les éléments les plus populaires ne changent pas une minute à la minute, nous pouvons ajouter une directive Aching pour améliorer les performances de notre application.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="1a8eb-162">Cette directive entraîne l’exécution du code de contrôle uniquement lorsque la sortie mise en cache du contrôle expire.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="1a8eb-163">Dans le cas contraire, la version mise en cache de la sortie du contrôle sera utilisée.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="1a8eb-164">Nous devons maintenant inclure notre nouveau contrôle dans notre page default. aspx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-164">Now all we have to do is include our new control in our Default.aspx page.</span></span>

<span data-ttu-id="1a8eb-165">Utilisez l’opération glisser-déplacer pour placer une instance du contrôle dans la colonne ouverte de notre formulaire par défaut.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="1a8eb-166">Maintenant, lorsque nous exécutons notre application, la page d’hébergement affiche les éléments les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="1a8eb-167">Contrôle « également acheté » (contrôles utilisateur avec paramètres)</span><span class="sxs-lookup"><span data-stu-id="1a8eb-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="1a8eb-168">Le deuxième contrôle utilisateur que nous allons créer prendrait des ventes au niveau supérieur en ajoutant une spécificité du contexte.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="1a8eb-169">La logique de calcul des éléments les plus importants « également achetés » n’est pas triviale.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="1a8eb-170">Notre contrôle « également acheté » sélectionne les enregistrements OrderDetails (précédemment achetés) pour le ProductID actuellement sélectionné et récupère le OrderIDs pour chaque commande unique trouvée.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="1a8eb-171">Ensuite, nous allons sélectionner al les produits de toutes ces commandes et additionner les quantités achetées.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="1a8eb-172">Nous allons trier les produits par la somme de la quantité et afficher les cinq premiers éléments.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="1a8eb-173">Étant donné la complexité de cette logique, nous allons implémenter cet algorithme en tant que procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="1a8eb-174">Le code T-SQL de la procédure stockée est le suivant.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="1a8eb-175">Notez que cette procédure stockée (SelectPurchasedWithProducts) existait dans la base de données lorsque nous l’avons incluse dans notre application et que nous avons généré le Entity Data Model nous avons spécifié cela, en plus des tables et des vues dont nous avons besoin, le Entity Data Model doit inclure cette procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="1a8eb-176">Pour accéder à la procédure stockée à partir de la Entity Data Model nous avons besoin d’importer la fonction.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="1a8eb-177">Double-cliquez sur l’Entity Data Model dans l’Explorateur de solutions pour l’ouvrir dans le concepteur et ouvrez l’Explorateur de modèles, cliquez avec le bouton droit dans le concepteur et sélectionnez « Ajouter une importation de fonction ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="1a8eb-178">Cela ouvre cette boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="1a8eb-179">Renseignez les champs comme indiqué ci-dessus, en sélectionnant « SelectPurchasedWithProducts » et en utilisant le nom de la procédure pour le nom de notre fonction importée.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="1a8eb-180">Cliquez sur « OK ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-180">Click "Ok".</span></span>

<span data-ttu-id="1a8eb-181">Cela fait, nous pouvons simplement programmer par rapport à la procédure stockée, comme tout autre élément dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="1a8eb-182">Ainsi, dans notre dossier « contrôles », créez un nouveau contrôle utilisateur nommé AlsoPurchased. ascx.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="1a8eb-183">Le balisage de ce contrôle sera très familier au contrôle PopularItems.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="1a8eb-184">La différence notable est que ne met pas en cache la sortie, car le rendu de l’élément est différent selon le produit.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="1a8eb-185">ProductId sera une « propriété » pour le contrôle.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="1a8eb-186">Dans le gestionnaire d’événements PreRender du contrôle, nous seau à effectuer trois opérations.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="1a8eb-187">Assurez-vous que le ProductID est défini.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="1a8eb-188">Vérifiez si des produits ont été achetés avec le produit actuel.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="1a8eb-189">Sortie de certains éléments, comme déterminé dans #2.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="1a8eb-190">Notez combien il est facile d’appeler la procédure stockée par le biais du modèle.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="1a8eb-191">Après avoir déterminé qu’il y a « également acheté », nous pouvons simplement lier le répéteur aux résultats retournés par la requête.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="1a8eb-192">S’il n’y avait pas d’éléments « également achetés », nous affichons simplement d’autres éléments populaires à partir de notre catalogue.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="1a8eb-193">Pour afficher les éléments « également achetés », ouvrez la page ProductDetails. aspx, puis faites glisser le contrôle AlsoPurchased à partir de l’Explorateur de solutions afin qu’il apparaisse à cette position dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="1a8eb-194">Vous créerez ainsi une référence au contrôle en haut de la page ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="1a8eb-195">Étant donné que le contrôle utilisateur AlsoPurchased requiert un numéro ProductId, nous définissons la propriété ProductID de notre contrôle à l’aide d’une instruction eval sur l’élément de modèle de données actuel de la page.</span><span class="sxs-lookup"><span data-stu-id="1a8eb-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="1a8eb-196">Quand nous créons et exécutons maintenant et que vous accédez à un produit, nous voyons les éléments « également achetés ».</span><span class="sxs-lookup"><span data-stu-id="1a8eb-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="1a8eb-197">[Précédent](tailspin-spyworks-part-6.md)
> [Suivant](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="1a8eb-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
