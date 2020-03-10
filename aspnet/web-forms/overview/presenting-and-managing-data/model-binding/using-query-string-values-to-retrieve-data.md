---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilisation de valeurs de chaîne de requête pour filtrer des données avec une liaison de modèle et des Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639096"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="af97d-104">Utilisation de valeurs de chaîne de requête pour filtrer des données avec la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="af97d-104">Using query string values to filter data with model binding and web forms</span></span>

<span data-ttu-id="af97d-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="af97d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="af97d-106">Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="af97d-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="af97d-107">La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="af97d-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="af97d-108">Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="af97d-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="af97d-109">Ce didacticiel montre comment passer une valeur dans la chaîne de requête et comment utiliser cette valeur pour récupérer des données via une liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="af97d-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="af97d-110">Ce didacticiel s’appuie sur le projet créé dans les parties [précédentes](retrieving-data.md) de la série.</span><span class="sxs-lookup"><span data-stu-id="af97d-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="af97d-111">Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB.</span><span class="sxs-lookup"><span data-stu-id="af97d-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="af97d-112">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="af97d-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="af97d-113">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="af97d-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="af97d-114">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="af97d-114">What you'll build</span></span>

<span data-ttu-id="af97d-115">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="af97d-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="af97d-116">Ajouter une nouvelle page pour afficher les cours inscrits pour un étudiant</span><span class="sxs-lookup"><span data-stu-id="af97d-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="af97d-117">Récupérer les cours inscrits pour l’étudiant sélectionné en fonction d’une valeur dans la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="af97d-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="af97d-118">Ajouter un lien hypertexte avec une valeur de chaîne de requête de la vue grille à la nouvelle page</span><span class="sxs-lookup"><span data-stu-id="af97d-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="af97d-119">Les étapes de ce didacticiel sont assez similaires à celles que vous avez effectuées dans le [didacticiel](sorting-paging-and-filtering-data.md) précédent pour filtrer les étudiants affichés en fonction de la sélection de l’utilisateur dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="af97d-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="af97d-120">Dans ce didacticiel, vous avez utilisé l’attribut **Control** dans la méthode Select pour spécifier que la valeur du paramètre provient d’un contrôle.</span><span class="sxs-lookup"><span data-stu-id="af97d-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="af97d-121">Dans ce didacticiel, vous allez utiliser l’attribut **QueryString** dans la méthode Select pour spécifier que la valeur du paramètre provient de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="af97d-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="af97d-122">Ajouter une nouvelle page pour afficher les cours d’un étudiant</span><span class="sxs-lookup"><span data-stu-id="af97d-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="af97d-123">Ajoutez un nouveau formulaire Web qui utilise la page maître site. Master et nommez les **cours**de la page.</span><span class="sxs-lookup"><span data-stu-id="af97d-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="af97d-124">Dans le fichier **courses. aspx** , ajoutez une vue grille pour afficher les cours de l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="af97d-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="af97d-125">Définir la méthode Select</span><span class="sxs-lookup"><span data-stu-id="af97d-125">Define the select method</span></span>

<span data-ttu-id="af97d-126">Dans **courses.aspx.cs**, vous allez ajouter la méthode Select avec le nom que vous avez spécifié dans la propriété **SelectMethod** de la vue de grille.</span><span class="sxs-lookup"><span data-stu-id="af97d-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="af97d-127">Dans cette méthode, vous allez définir la requête de récupération des cours d’un étudiant et spécifier que le paramètre provient d’une valeur de chaîne de requête portant le même nom que le paramètre.</span><span class="sxs-lookup"><span data-stu-id="af97d-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="af97d-128">Tout d’abord, vous devez ajouter les instructions **using** suivantes.</span><span class="sxs-lookup"><span data-stu-id="af97d-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="af97d-129">Ensuite, ajoutez le code suivant à Courses.aspx.cs :</span><span class="sxs-lookup"><span data-stu-id="af97d-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="af97d-130">L’attribut QueryString signifie qu’une valeur de chaîne de requête nommée StudentID est automatiquement assignée au paramètre dans cette méthode.</span><span class="sxs-lookup"><span data-stu-id="af97d-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="af97d-131">Ajouter un lien hypertexte avec une valeur de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="af97d-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="af97d-132">Dans l’affichage de grille sur students. aspx, vous allez ajouter un champ de lien hypertexte qui lie à votre nouvelle page de cours.</span><span class="sxs-lookup"><span data-stu-id="af97d-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="af97d-133">Le lien hypertexte inclut une valeur de chaîne de requête avec l’ID de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="af97d-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="af97d-134">Dans students. aspx, ajoutez le champ suivant aux colonnes de la vue grille juste en dessous du champ pour le nombre total de crédits.</span><span class="sxs-lookup"><span data-stu-id="af97d-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="af97d-135">Exécutez l’application et notez que l’affichage de grille comprend désormais le lien cours.</span><span class="sxs-lookup"><span data-stu-id="af97d-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Ajouter un lien hypertexte](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="af97d-137">Lorsque vous cliquez sur l’un des liens, vous voyez les cours inscrits de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="af97d-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![afficher les cours](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="af97d-139">Conclusion</span><span class="sxs-lookup"><span data-stu-id="af97d-139">Conclusion</span></span>

<span data-ttu-id="af97d-140">Dans ce didacticiel, vous avez ajouté un lien avec une valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="af97d-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="af97d-141">Vous avez utilisé cette valeur de chaîne de requête pour la valeur de paramètre dans la méthode Select.</span><span class="sxs-lookup"><span data-stu-id="af97d-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="af97d-142">Dans le [didacticiel](adding-business-logic-layer.md)suivant, vous allez déplacer le code des fichiers code-behind dans une couche de logique métier et une couche d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="af97d-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="af97d-143">[Précédent](integrating-jquery-ui.md)
> [Suivant](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="af97d-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
