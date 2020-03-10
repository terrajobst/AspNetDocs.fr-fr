---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Partie 4 : ajout d’une vue d’administration | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556013"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="2d3b0-102">Partie 4 : ajout d’une vue d’administration</span><span class="sxs-lookup"><span data-stu-id="2d3b0-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="2d3b0-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2d3b0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2d3b0-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="2d3b0-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="2d3b0-105">Ajouter une vue d’administration</span><span class="sxs-lookup"><span data-stu-id="2d3b0-105">Add an Admin View</span></span>

<span data-ttu-id="2d3b0-106">À présent, nous allons activer le côté client et ajouter une page qui peut consommer des données du contrôleur d’administration.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="2d3b0-107">La page permet aux utilisateurs de créer, modifier ou supprimer des produits, en envoyant des demandes AJAX au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="2d3b0-108">Dans Explorateur de solutions, développez le dossier Controllers et ouvrez le fichier nommé HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="2d3b0-109">Ce fichier contient un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-109">This file contains an MVC controller.</span></span> <span data-ttu-id="2d3b0-110">Ajoutez une méthode nommée `Admin`:</span><span class="sxs-lookup"><span data-stu-id="2d3b0-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="2d3b0-111">La méthode **HttpRouteUrl** crée l’URI vers l’API Web, et nous la stockons dans le sac d’affichage pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="2d3b0-112">Ensuite, placez le curseur de texte à l’intérieur de la méthode d’action `Admin`, puis cliquez avec le bouton droit et sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="2d3b0-113">La boîte de dialogue **Ajouter une vue s’affiche** .</span><span class="sxs-lookup"><span data-stu-id="2d3b0-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="2d3b0-114">Dans la boîte de dialogue **Ajouter une vue** , nommez la vue « admin ».</span><span class="sxs-lookup"><span data-stu-id="2d3b0-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="2d3b0-115">Activez la case à cocher **créer une vue fortement typée**.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="2d3b0-116">Sous **classe de modèle**, sélectionnez « produit (ProductStore. Models) ».</span><span class="sxs-lookup"><span data-stu-id="2d3b0-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="2d3b0-117">Laissez toutes les autres options en tant que valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="2d3b0-118">Cliquez sur **Ajouter** pour ajouter un fichier nommé Admin. cshtml sous affichages/page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="2d3b0-119">Ouvrez ce fichier et ajoutez le code HTML suivant.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="2d3b0-120">Ce code HTML définit la structure de la page, mais aucune fonctionnalité n’est encore connectée.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="2d3b0-121">Créer un lien vers la page d’administration</span><span class="sxs-lookup"><span data-stu-id="2d3b0-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="2d3b0-122">Dans Explorateur de solutions, développez le dossier views, puis développez le dossier Shared.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="2d3b0-123">Ouvrez le fichier nommé \_Layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="2d3b0-124">Recherchez l’élément **UL** avec ID = "menu" et un lien d’action pour la vue d’administration :</span><span class="sxs-lookup"><span data-stu-id="2d3b0-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="2d3b0-125">Dans l’exemple de projet, j’ai apporté quelques modifications esthétiques, telles que le remplacement de la chaîne « Your logo here ».</span><span class="sxs-lookup"><span data-stu-id="2d3b0-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="2d3b0-126">Celles-ci n’affectent pas les fonctionnalités de l’application.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="2d3b0-127">Vous pouvez télécharger le projet et comparer les fichiers.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-127">You can download the project and compare the files.</span></span>

<span data-ttu-id="2d3b0-128">Exécutez l’application et cliquez sur le lien « admin » qui apparaît en haut de la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="2d3b0-129">La page d’administration doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="2d3b0-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="2d3b0-130">Pour le moment, la page ne fait rien.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="2d3b0-131">Dans la section suivante, nous allons utiliser Knockout. js pour créer une interface utilisateur dynamique.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="2d3b0-132">Ajouter une autorisation</span><span class="sxs-lookup"><span data-stu-id="2d3b0-132">Add Authorization</span></span>

<span data-ttu-id="2d3b0-133">La page d’administration est actuellement accessible à toute personne visitant le site.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="2d3b0-134">Modifions ceci pour restreindre l’autorisation aux administrateurs.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="2d3b0-135">Commencez par ajouter un rôle « administrateur » et un utilisateur administrateur.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="2d3b0-136">Dans Explorateur de solutions, développez le dossier filtres et ouvrez le fichier nommé InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="2d3b0-137">Recherchez le constructeur `SimpleMembershipInitializer`.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="2d3b0-138">Après l’appel à **WebSecurity. InitializeDatabaseConnection**, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2d3b0-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="2d3b0-139">Il s’agit d’une méthode rapide et incorrecte pour ajouter le rôle « administrateur » et créer un utilisateur pour le rôle.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="2d3b0-140">Dans Explorateur de solutions, développez le dossier Controllers et ouvrez le fichier HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="2d3b0-141">Ajoutez l’attribut **Authorize** à la méthode `Admin`.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="2d3b0-142">Ouvrez le fichier AdminController.cs et ajoutez l’attribut **Authorize** à l’ensemble de la classe `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="2d3b0-143">MVC et l’API Web définissent tous deux des attributs d' **autorisation** , dans différents espaces de noms.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="2d3b0-144">MVC utilise **System. Web. Mvc. AuthorizeAttribute**, tandis que l’API Web utilise **System. Web. http. AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>

<span data-ttu-id="2d3b0-145">Désormais, seuls les administrateurs peuvent afficher la page d’administration.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="2d3b0-146">En outre, si vous envoyez une requête HTTP au contrôleur d’administration, la requête doit contenir un cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="2d3b0-147">Si ce n’est pas le cas, le serveur envoie une réponse HTTP 401 (non autorisée).</span><span class="sxs-lookup"><span data-stu-id="2d3b0-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="2d3b0-148">Vous pouvez le voir dans Fiddler en envoyant une demande de récupération à `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="2d3b0-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d3b0-149">[Précédent](using-web-api-with-entity-framework-part-3.md)
> [Suivant](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="2d3b0-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
