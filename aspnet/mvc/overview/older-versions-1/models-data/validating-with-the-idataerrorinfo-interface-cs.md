---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validation avec l’interface IDataErrorInfo (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisés en implémentant l’interface IDataErrorInfo dans une classe de modèle.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542559"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="52236-103">Validation avec l’interface IDataErrorInfo (C#)</span><span class="sxs-lookup"><span data-stu-id="52236-103">Validating with the IDataErrorInfo Interface (C#)</span></span>

<span data-ttu-id="52236-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="52236-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="52236-105">Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisés en implémentant l’interface IDataErrorInfo dans une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="52236-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>

<span data-ttu-id="52236-106">L’objectif de ce didacticiel est d’expliquer une approche de l’exécution de la validation dans une application MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="52236-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="52236-107">Vous allez apprendre comment empêcher une personne de soumettre un formulaire HTML sans fournir de valeurs pour les champs de formulaire requis.</span><span class="sxs-lookup"><span data-stu-id="52236-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="52236-108">Dans ce didacticiel, vous allez apprendre à effectuer la validation à l’aide de l’interface IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="52236-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="52236-109">Assumptions (Hypothèses)</span><span class="sxs-lookup"><span data-stu-id="52236-109">Assumptions</span></span>

<span data-ttu-id="52236-110">Dans ce didacticiel, je vais utiliser la base de données MoviesDB et la table de base de données movies.</span><span class="sxs-lookup"><span data-stu-id="52236-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="52236-111">Cette table présente les colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="52236-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>

| <span data-ttu-id="52236-112">**Nom de la colonne**</span><span class="sxs-lookup"><span data-stu-id="52236-112">**Column Name**</span></span> | <span data-ttu-id="52236-113">**Type de données**</span><span class="sxs-lookup"><span data-stu-id="52236-113">**Data Type**</span></span> | <span data-ttu-id="52236-114">**Null autorisé**</span><span class="sxs-lookup"><span data-stu-id="52236-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52236-115">Id</span><span class="sxs-lookup"><span data-stu-id="52236-115">Id</span></span> | <span data-ttu-id="52236-116">Int</span><span class="sxs-lookup"><span data-stu-id="52236-116">Int</span></span> | <span data-ttu-id="52236-117">False</span><span class="sxs-lookup"><span data-stu-id="52236-117">False</span></span> |
| <span data-ttu-id="52236-118">Titre</span><span class="sxs-lookup"><span data-stu-id="52236-118">Title</span></span> | <span data-ttu-id="52236-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="52236-119">Nvarchar(100)</span></span> | <span data-ttu-id="52236-120">False</span><span class="sxs-lookup"><span data-stu-id="52236-120">False</span></span> |
| <span data-ttu-id="52236-121">Directeur</span><span class="sxs-lookup"><span data-stu-id="52236-121">Director</span></span> | <span data-ttu-id="52236-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="52236-122">Nvarchar(100)</span></span> | <span data-ttu-id="52236-123">False</span><span class="sxs-lookup"><span data-stu-id="52236-123">False</span></span> |
| <span data-ttu-id="52236-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="52236-124">DateReleased</span></span> | <span data-ttu-id="52236-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="52236-125">DateTime</span></span> | <span data-ttu-id="52236-126">False</span><span class="sxs-lookup"><span data-stu-id="52236-126">False</span></span> |

<span data-ttu-id="52236-127">Dans ce didacticiel, j’utilise le Entity Framework Microsoft pour générer mes classes de modèle de base de données.</span><span class="sxs-lookup"><span data-stu-id="52236-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="52236-128">La classe Movie générée par le Entity Framework est affichée à la figure 1.</span><span class="sxs-lookup"><span data-stu-id="52236-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>

<span data-ttu-id="52236-129">[![l’entité film](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="52236-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="52236-130">**Figure 01**: entité de film ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="52236-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="52236-131">Pour en savoir plus sur l’utilisation de la Entity Framework pour générer vos classes de modèle de base de données, consultez mon didacticiel intitulé Création de classes de modèle avec l’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52236-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>

## <a name="the-controller-class"></a><span data-ttu-id="52236-132">La classe Controller</span><span class="sxs-lookup"><span data-stu-id="52236-132">The Controller Class</span></span>

<span data-ttu-id="52236-133">Nous utilisons le contrôleur d’hébergement pour répertorier les films et créer de nouveaux films.</span><span class="sxs-lookup"><span data-stu-id="52236-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="52236-134">Le code de cette classe est contenu dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="52236-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="52236-135">**Liste 1-Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="52236-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="52236-136">La classe de contrôleur d’hébergement de la liste 1 contient deux actions de création ().</span><span class="sxs-lookup"><span data-stu-id="52236-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="52236-137">La première action affiche le formulaire HTML pour la création d’un nouveau film.</span><span class="sxs-lookup"><span data-stu-id="52236-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="52236-138">La deuxième action Create () effectue l’insertion réelle du nouveau film dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="52236-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="52236-139">La deuxième action Create () est appelée lorsque le formulaire affiché par la première action Create () est envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="52236-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="52236-140">Notez que la deuxième action Create () contient les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="52236-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="52236-141">La propriété IsValid retourne la valeur false en cas d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="52236-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="52236-142">Dans ce cas, la vue Create qui contient le formulaire HTML pour la création d’un film est réaffichée.</span><span class="sxs-lookup"><span data-stu-id="52236-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="52236-143">Création d’une classe partielle</span><span class="sxs-lookup"><span data-stu-id="52236-143">Creating a Partial Class</span></span>

<span data-ttu-id="52236-144">La classe Movie est générée par la Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52236-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="52236-145">Vous pouvez voir le code de la classe Movie si vous développez le fichier MoviesDBModel. edmx dans la fenêtre Explorateur de solutions et que vous ouvrez le fichier MoviesDBModel.Designer.cs dans l’éditeur de code (voir la figure 2).</span><span class="sxs-lookup"><span data-stu-id="52236-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>

<span data-ttu-id="52236-146">[![le code de l’entité film](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="52236-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="52236-147">**Figure 02**: code de l’entité Movie ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="52236-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>

<span data-ttu-id="52236-148">La classe Movie est une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="52236-148">The Movie class is a partial class.</span></span> <span data-ttu-id="52236-149">Cela signifie que nous pouvons ajouter une autre classe partielle portant le même nom pour étendre les fonctionnalités de la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="52236-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="52236-150">Nous allons ajouter notre logique de validation à la nouvelle classe partielle.</span><span class="sxs-lookup"><span data-stu-id="52236-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="52236-151">Ajoutez la classe dans Listing 2 au dossier Models.</span><span class="sxs-lookup"><span data-stu-id="52236-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="52236-152">**Liste 2-Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="52236-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="52236-153">Notez que la classe de la liste 2 comprend le modificateur *Partial* .</span><span class="sxs-lookup"><span data-stu-id="52236-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="52236-154">Toutes les méthodes ou propriétés que vous ajoutez à cette classe font partie de la classe Movie générée par l’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52236-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="52236-155">Ajout de méthodes partielles OnChanging et OnChanged</span><span class="sxs-lookup"><span data-stu-id="52236-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="52236-156">Lorsque l’Entity Framework génère une classe d’entité, le Entity Framework ajoute automatiquement des méthodes partielles à la classe.</span><span class="sxs-lookup"><span data-stu-id="52236-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="52236-157">Le Entity Framework génère des méthodes partielles OnChanging et OnChanged qui correspondent à chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="52236-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="52236-158">Dans le cas de la classe Movie, le Entity Framework crée les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="52236-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="52236-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="52236-159">OnIdChanging</span></span>
- <span data-ttu-id="52236-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="52236-160">OnIdChanged</span></span>
- <span data-ttu-id="52236-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="52236-161">OnTitleChanging</span></span>
- <span data-ttu-id="52236-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="52236-162">OnTitleChanged</span></span>
- <span data-ttu-id="52236-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="52236-163">OnDirectorChanging</span></span>
- <span data-ttu-id="52236-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="52236-164">OnDirectorChanged</span></span>
- <span data-ttu-id="52236-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="52236-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="52236-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="52236-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="52236-167">La méthode OnChanging est appelée juste avant la modification de la propriété correspondante.</span><span class="sxs-lookup"><span data-stu-id="52236-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="52236-168">La méthode OnChanged est appelée juste après la modification de la propriété.</span><span class="sxs-lookup"><span data-stu-id="52236-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="52236-169">Vous pouvez tirer parti de ces méthodes partielles pour ajouter une logique de validation à la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="52236-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="52236-170">La classe Update Movie de la liste 3 vérifie que les valeurs non vides des propriétés Title et Director sont affectées.</span><span class="sxs-lookup"><span data-stu-id="52236-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="52236-171">Une méthode partielle est une méthode définie dans une classe que vous n’êtes pas obligé d’implémenter.</span><span class="sxs-lookup"><span data-stu-id="52236-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="52236-172">Si vous n’implémentez pas de méthode partielle, le compilateur supprime la signature de méthode et tous les appels à la méthode afin qu’il n’y ait aucun coût d’exécution associé à la méthode partielle.</span><span class="sxs-lookup"><span data-stu-id="52236-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="52236-173">Dans l’éditeur de Visual Studio Code, vous pouvez ajouter une méthode partielle en tapant le mot clé *Partial* suivi d’un espace pour afficher la liste des partielles à implémenter.</span><span class="sxs-lookup"><span data-stu-id="52236-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>

<span data-ttu-id="52236-174">**Liste 3-Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="52236-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="52236-175">Par exemple, si vous tentez d’affecter une chaîne vide à la propriété Title, un message d’erreur est assigné à un dictionnaire nommé \_Errors.</span><span class="sxs-lookup"><span data-stu-id="52236-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="52236-176">À ce stade, rien ne se produit lorsque vous affectez une chaîne vide à la propriété Title et qu’une erreur est ajoutée au champ private \_Errors.</span><span class="sxs-lookup"><span data-stu-id="52236-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="52236-177">Nous devons implémenter l’interface IDataErrorInfo pour exposer ces erreurs de validation à l’infrastructure MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="52236-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="52236-178">Implémentation de l’interface IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="52236-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="52236-179">L’interface IDataErrorInfo fait partie du .NET Framework depuis la première version.</span><span class="sxs-lookup"><span data-stu-id="52236-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="52236-180">Cette interface est très simple :</span><span class="sxs-lookup"><span data-stu-id="52236-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="52236-181">Si une classe implémente l’interface IDataErrorInfo, l’infrastructure MVC ASP.NET utilise cette interface lors de la création d’une instance de la classe.</span><span class="sxs-lookup"><span data-stu-id="52236-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="52236-182">Par exemple, l’action créer () du contrôleur d’hébergement accepte une instance de la classe Movie :</span><span class="sxs-lookup"><span data-stu-id="52236-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="52236-183">L’infrastructure MVC ASP.NET crée l’instance du film passé à l’action Create () à l’aide d’un classeur de modèles (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="52236-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="52236-184">Le Binder de modèle est chargé de créer une instance de l’objet Movie en liant les champs de formulaire HTML à une instance de l’objet Movie.</span><span class="sxs-lookup"><span data-stu-id="52236-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="52236-185">Le DefaultModelBinder détecte si une classe implémente l’interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="52236-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="52236-186">Si une classe implémente cette interface, le Binder de modèle appelle IDataErrorInfo. cet indexeur pour chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="52236-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="52236-187">Si l’indexeur retourne un message d’erreur, le Binder de modèle ajoute automatiquement ce message d’erreur à l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="52236-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="52236-188">Le DefaultModelBinder vérifie également la propriété IDataErrorInfo. Error.</span><span class="sxs-lookup"><span data-stu-id="52236-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="52236-189">Cette propriété est destinée à représenter les erreurs de validation non spécifiques à une propriété associées à la classe.</span><span class="sxs-lookup"><span data-stu-id="52236-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="52236-190">Par exemple, vous souhaiterez peut-être appliquer une règle de validation qui dépend des valeurs de plusieurs propriétés de la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="52236-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="52236-191">Dans ce cas, vous retournez une erreur de validation à partir de la propriété Error.</span><span class="sxs-lookup"><span data-stu-id="52236-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="52236-192">La classe Movie mise à jour de la liste 4 implémente l’interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="52236-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="52236-193">**Liste 4-Models\Movie.cs (implémente IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="52236-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="52236-194">Dans la liste 4, la propriété d’indexeur vérifie la collection d’erreurs \_pour déterminer si elle contient une clé qui correspond au nom de propriété passé à l’indexeur.</span><span class="sxs-lookup"><span data-stu-id="52236-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="52236-195">Si aucune erreur de validation n’est associée à la propriété, une chaîne vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="52236-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="52236-196">Vous n’avez pas besoin de modifier le contrôleur d’hébergement de quelque façon que ce soit pour utiliser la classe Movie modifiée.</span><span class="sxs-lookup"><span data-stu-id="52236-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="52236-197">La page affichée à la figure 3 illustre ce qui se passe quand aucune valeur n’est entrée pour les champs de formulaire de titre ou de directeur.</span><span class="sxs-lookup"><span data-stu-id="52236-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>

<span data-ttu-id="52236-198">[![créer automatiquement des méthodes d’action](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="52236-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="52236-199">**Figure 03**: formulaire avec des valeurs manquantes ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="52236-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>

<span data-ttu-id="52236-200">Notez que la valeur DateReleased est validée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="52236-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="52236-201">Étant donné que la propriété DateReleased n’accepte pas les valeurs NULL, DefaultModelBinder génère une erreur de validation pour cette propriété automatiquement lorsqu’elle n’a pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="52236-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="52236-202">Si vous souhaitez modifier le message d’erreur pour la propriété DateReleased, vous devez créer un classeur de modèles personnalisé.</span><span class="sxs-lookup"><span data-stu-id="52236-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="52236-203">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="52236-203">Summary</span></span>

<span data-ttu-id="52236-204">Dans ce didacticiel, vous avez appris à utiliser l’interface IDataErrorInfo pour générer des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="52236-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="52236-205">Tout d’abord, nous avons créé une classe de film partielle qui étend les fonctionnalités de la classe de film partielle générée par la Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52236-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="52236-206">Ensuite, nous avons ajouté une logique de validation aux méthodes partielles de la classe Movie OnTitleChanging () et OnDirectorChanging ().</span><span class="sxs-lookup"><span data-stu-id="52236-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="52236-207">Enfin, nous avons implémenté l’interface IDataErrorInfo pour exposer ces messages de validation à l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="52236-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="52236-208">[Précédent](performing-simple-validation-cs.md)
> [Suivant](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="52236-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
