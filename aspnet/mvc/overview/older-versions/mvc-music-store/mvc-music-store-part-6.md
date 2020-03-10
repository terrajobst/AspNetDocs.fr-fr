---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Partie 6 : utilisation d’annotations de données pour la validation de modèle | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 6 couvre l’utilisation des annotations de données pour le modèle V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539276"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="33923-104">Partie 6 : utilisation d’annotations de données pour la validation de modèle</span><span class="sxs-lookup"><span data-stu-id="33923-104">Part 6: Using Data Annotations for Model Validation</span></span>

<span data-ttu-id="33923-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="33923-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="33923-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="33923-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="33923-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="33923-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="33923-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="33923-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="33923-109">La partie 6 couvre l’utilisation des annotations de données pour la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="33923-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>

<span data-ttu-id="33923-110">Nous avons un problème majeur avec nos formulaires de création et de modification : ils ne font pas de validation.</span><span class="sxs-lookup"><span data-stu-id="33923-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="33923-111">Nous pouvons effectuer des opérations telles que laisser les champs obligatoires vides ou taper des lettres dans le champ Price, et la première erreur que nous allons voir provient de la base de données.</span><span class="sxs-lookup"><span data-stu-id="33923-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="33923-112">Nous pouvons facilement ajouter la validation à notre application en ajoutant des annotations de données à nos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="33923-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="33923-113">Les annotations de données nous permettent de décrire les règles que nous voulons appliquer à nos propriétés de modèle, et ASP.NET MVC s’occupera de leur application et de l’affichage des messages appropriés à nos utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="33923-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="33923-114">Ajout de la validation à nos formulaires d’album</span><span class="sxs-lookup"><span data-stu-id="33923-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="33923-115">Nous allons utiliser les attributs d’annotation de données suivants :</span><span class="sxs-lookup"><span data-stu-id="33923-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="33923-116">**Required** : indique que la propriété est un champ obligatoire</span><span class="sxs-lookup"><span data-stu-id="33923-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="33923-117">**DisplayName** : définit le texte que nous souhaitons utiliser sur les champs de formulaire et les messages de validation</span><span class="sxs-lookup"><span data-stu-id="33923-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="33923-118">**StringLength** : définit une longueur maximale pour un champ de chaîne.</span><span class="sxs-lookup"><span data-stu-id="33923-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="33923-119">**Plage** : donne une valeur maximale et minimale pour un champ numérique</span><span class="sxs-lookup"><span data-stu-id="33923-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="33923-120">**Bind** : répertorie les champs à exclure ou inclure lors de la liaison de valeurs de paramètre ou de formulaire à des propriétés de modèle</span><span class="sxs-lookup"><span data-stu-id="33923-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="33923-121">**ScaffoldColumn** : permet de masquer les champs des formulaires de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="33923-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="33923-122">*Remarque : pour plus d’informations sur la validation de modèle à l’aide des attributs d’annotation de données, consultez la documentation MSDN à l’adresse* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="33923-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="33923-123">Ouvrez la classe album et ajoutez les instructions *using* suivantes en haut.</span><span class="sxs-lookup"><span data-stu-id="33923-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="33923-124">Ensuite, mettez à jour les propriétés pour ajouter des attributs d’affichage et de validation comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="33923-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="33923-125">Alors que nous y sommes, nous avons également modifié le genre et l’artiste en propriétés virtuelles.</span><span class="sxs-lookup"><span data-stu-id="33923-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="33923-126">Cela permet à Entity Framework de les charger en différé en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="33923-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="33923-127">Une fois que vous avez ajouté ces attributs à notre modèle d’album, notre écran de création et de modification commence immédiatement à valider les champs et à utiliser les noms d’affichage que nous avons choisis (par exemple, URL de la pochette de l’album au lieu de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="33923-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="33923-128">Exécutez l’application et accédez à/StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="33923-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="33923-129">Ensuite, nous allons rompre certaines règles de validation.</span><span class="sxs-lookup"><span data-stu-id="33923-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="33923-130">Entrez un prix de 0 et laissez le titre vide.</span><span class="sxs-lookup"><span data-stu-id="33923-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="33923-131">Quand vous cliquez sur le bouton créer, le formulaire s’affiche avec des messages d’erreur de validation indiquant les champs qui ne respectent pas les règles de validation que nous avons définies.</span><span class="sxs-lookup"><span data-stu-id="33923-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="33923-132">Test de la validation côté client</span><span class="sxs-lookup"><span data-stu-id="33923-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="33923-133">La validation côté serveur est très importante du point de vue de l’application, car les utilisateurs peuvent contourner la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="33923-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="33923-134">Toutefois, les formulaires de page Web qui implémentent uniquement la validation côté serveur présentent trois problèmes significatifs.</span><span class="sxs-lookup"><span data-stu-id="33923-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="33923-135">L’utilisateur doit attendre que le formulaire soit publié, validé sur le serveur et que la réponse soit envoyée à son navigateur.</span><span class="sxs-lookup"><span data-stu-id="33923-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="33923-136">L’utilisateur ne recevra pas de commentaires immédiats lorsqu’il corrigera un champ afin qu’il transmette maintenant les règles de validation.</span><span class="sxs-lookup"><span data-stu-id="33923-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="33923-137">Nous gaspillons les ressources du serveur pour exécuter la logique de validation au lieu de tirer parti du navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="33923-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="33923-138">Heureusement, les modèles de l’échafaudage ASP.NET MVC 3 intègrent la validation côté client, ce qui ne nécessite aucun travail supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="33923-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="33923-139">Si vous tapez une seule lettre dans le champ titre conforme aux exigences de validation, le message de validation est immédiatement supprimé.</span><span class="sxs-lookup"><span data-stu-id="33923-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> <span data-ttu-id="33923-140">[Précédent](mvc-music-store-part-5.md)
> [Suivant](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="33923-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
