---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Utilisation de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 3 : tri et filtrage | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework 4,0. ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631669"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="1dbfa-104">Utilisation de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 3 : tri et filtrage</span><span class="sxs-lookup"><span data-stu-id="1dbfa-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>

<span data-ttu-id="1dbfa-105">par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="1dbfa-106">Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels Entity Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="1dbfa-107">Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="1dbfa-108">Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="1dbfa-109">Si vous avez des questions sur les didacticiels, vous pouvez les poster sur le [Forum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dbfa-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>

<span data-ttu-id="1dbfa-110">Dans le didacticiel précédent, vous avez implémenté le modèle de référentiel dans une application Web multiniveau qui utilise les Entity Framework et le contrôle `ObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="1dbfa-111">Ce didacticiel montre comment trier et filtrer et gérer les scénarios maître/détail.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="1dbfa-112">Vous allez ajouter les améliorations suivantes à la page *Departments. aspx* :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="1dbfa-113">Une zone de texte pour permettre aux utilisateurs de sélectionner des services par nom.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="1dbfa-114">Liste des cours pour chaque service affiché dans la grille.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="1dbfa-115">Possibilité de trier en cliquant sur les en-têtes de colonne.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="1dbfa-116">[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="1dbfa-117">Ajout de la possibilité de trier des colonnes GridView</span><span class="sxs-lookup"><span data-stu-id="1dbfa-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="1dbfa-118">Ouvrez la page *Departments. aspx* et ajoutez un attribut `SortParameterName="sortExpression"` au contrôle `ObjectDataSource` nommé `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="1dbfa-119">(Par la suite, vous créerez une méthode `GetDepartments` qui accepte un paramètre nommé `sortExpression`.) Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="1dbfa-120">Ajoutez l’attribut `AllowSorting="true"` à la balise d’ouverture du contrôle `GridView`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="1dbfa-121">Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="1dbfa-122">Dans *Departments.aspx.cs*, définissez l’ordre de tri par défaut en appelant la méthode `Sort` du contrôle `GridView` à partir de la méthode `Page_Load` :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="1dbfa-123">Vous pouvez ajouter du code qui effectue un tri ou un filtrage dans la classe de logique métier ou le référentiel.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="1dbfa-124">Si vous le faites dans la classe de logique métier, le tri ou le filtrage est effectué une fois les données récupérées de la base de données, car la classe de logique métier utilise un objet `IEnumerable` retourné par le référentiel.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="1dbfa-125">Si vous ajoutez du code de tri et de filtrage dans la classe de référentiel et que vous le faites avant qu’une expression LINQ ou une requête d’objet ait été convertie en objet `IEnumerable`, vos commandes sont transmises à la base de données à des fins de traitement, ce qui est généralement plus efficace.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="1dbfa-126">Dans ce didacticiel, vous allez implémenter le tri et le filtrage de manière à ce que la base de données effectue le traitement, c’est-à-dire dans le référentiel.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="1dbfa-127">Pour ajouter une fonctionnalité de tri, vous devez ajouter une nouvelle méthode à l’interface de référentiel et aux classes de référentiel, ainsi qu’à la classe de logique métier.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="1dbfa-128">Dans le fichier *ISchoolRepository.cs* , ajoutez une nouvelle méthode `GetDepartments` qui accepte un paramètre `sortExpression` qui sera utilisé pour trier la liste des services renvoyés :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="1dbfa-129">Le paramètre `sortExpression` spécifie la colonne à utiliser pour le tri et le sens du tri.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="1dbfa-130">Ajoutez le code de la nouvelle méthode au fichier *SchoolRepository.cs* :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="1dbfa-131">Modifiez la méthode `GetDepartments` sans paramètre existante pour appeler la nouvelle méthode :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="1dbfa-132">Dans le projet de test, ajoutez la nouvelle méthode suivante à *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="1dbfa-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="1dbfa-133">Si vous souhaitez créer des tests unitaires qui dépendent de cette méthode retournant une liste triée, vous devez trier la liste avant de la retourner.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="1dbfa-134">Comme vous ne créez pas de tests comme celui de ce didacticiel, la méthode peut simplement retourner la liste non triée des services.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="1dbfa-135">Dans le fichier *SchoolBL.cs* , ajoutez la nouvelle méthode suivante à la classe de logique métier :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="1dbfa-136">Ce code transmet le paramètre de tri à la méthode de référentiel.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="1dbfa-137">Exécutez la page *Departments. aspx* .</span><span class="sxs-lookup"><span data-stu-id="1dbfa-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="1dbfa-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="1dbfa-139">Vous pouvez maintenant cliquer sur n’importe quel en-tête de colonne pour effectuer un tri sur cette colonne.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="1dbfa-140">Si la colonne est déjà triée, le fait de cliquer sur l’en-tête inverse le sens de tri.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="1dbfa-141">Ajout d’une zone de recherche</span><span class="sxs-lookup"><span data-stu-id="1dbfa-141">Adding a Search Box</span></span>

<span data-ttu-id="1dbfa-142">Dans cette section, vous allez ajouter une zone de texte de recherche, la lier au contrôle `ObjectDataSource` à l’aide d’un paramètre de contrôle et ajouter une méthode à la classe de logique métier pour prendre en charge le filtrage.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="1dbfa-143">Ouvrez la page *Departments. aspx* et ajoutez le balisage suivant entre le titre et le premier contrôle `ObjectDataSource` :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="1dbfa-144">Dans le contrôle `ObjectDataSource` nommé `DepartmentsObjectDataSource`, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="1dbfa-145">Ajoutez un élément `SelectParameters` pour un paramètre nommé `nameSearchString` qui obtient la valeur entrée dans le contrôle `SearchTextBox`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="1dbfa-146">Remplacez la valeur de l’attribut `SelectMethod` par `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="1dbfa-147">(Vous allez créer cette méthode ultérieurement.)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-147">(You'll create this method later.)</span></span>

<span data-ttu-id="1dbfa-148">Le balisage du contrôle `ObjectDataSource` ressemble maintenant à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="1dbfa-149">Dans *ISchoolRepository.cs*, ajoutez une méthode `GetDepartmentsByName` qui accepte à la fois les paramètres `sortExpression` et `nameSearchString` :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="1dbfa-150">Dans *SchoolRepository.cs*, ajoutez la nouvelle méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="1dbfa-151">Ce code utilise une méthode `Where` pour sélectionner les éléments qui contiennent la chaîne recherchée.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="1dbfa-152">Si la chaîne de recherche est vide, tous les enregistrements sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="1dbfa-153">Notez que lorsque vous spécifiez des appels de méthode ensemble dans une instruction comme celle-ci (`Include`, `OrderBy`, puis `Where`), la méthode `Where` doit toujours être la dernière.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="1dbfa-154">Modifiez la méthode `GetDepartments` existante qui accepte un paramètre `sortExpression` pour appeler la nouvelle méthode :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="1dbfa-155">Dans *MockSchoolRepository.cs* dans le projet de test, ajoutez la nouvelle méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="1dbfa-156">Dans *SchoolBL.cs*, ajoutez la nouvelle méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="1dbfa-157">Exécutez la page *Departments. aspx* et entrez une chaîne de recherche pour vous assurer que la logique de sélection fonctionne.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="1dbfa-158">Laissez la zone de texte vide et essayez d’effectuer une recherche pour vous assurer que tous les enregistrements sont retournés.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="1dbfa-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="1dbfa-160">Ajout d’une colonne de détails pour chaque ligne de grille</span><span class="sxs-lookup"><span data-stu-id="1dbfa-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="1dbfa-161">Ensuite, vous souhaitez afficher tous les cours pour chaque département affiché dans la cellule de droite de la grille.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="1dbfa-162">Pour ce faire, vous allez utiliser un contrôle de `GridView` imbriqué et le lier aux données à partir de la propriété de navigation `Courses` de l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="1dbfa-163">Ouvrez *Departments. aspx* et dans le balisage pour le contrôle `GridView`, spécifiez un gestionnaire pour l’événement `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="1dbfa-164">Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="1dbfa-165">Ajoutez un nouvel élément `TemplateField` après le champ modèle `Administrator` :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="1dbfa-166">Ce balisage crée un contrôle de `GridView` imbriqué qui affiche le numéro de cours et le titre d’une liste de cours.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="1dbfa-167">Elle ne spécifie pas de source de données, car vous allez la lier au code dans le gestionnaire de `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="1dbfa-168">Ouvrez *Departments.aspx.cs* et ajoutez le gestionnaire suivant pour l’événement `RowDataBound` :</span><span class="sxs-lookup"><span data-stu-id="1dbfa-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="1dbfa-169">Ce code obtient l’entité `Department` à partir des arguments d’événement, convertit la propriété de navigation `Courses` en collection `List` et établit le `GridView` imbriqué à la collection.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="1dbfa-170">Ouvrez le fichier *SchoolRepository.cs* et spécifiez le chargement hâtif pour la propriété de navigation `Courses` en appelant la méthode `Include` dans la requête d’objet que vous créez dans la méthode `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="1dbfa-171">L’instruction `return` dans la méthode `GetDepartmentsByName` ressemble désormais à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="1dbfa-172">Exécutez la page.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-172">Run the page.</span></span> <span data-ttu-id="1dbfa-173">En plus de la fonctionnalité de tri et de filtrage que vous avez ajoutée précédemment, le contrôle GridView affiche désormais des détails de cours imbriqués pour chaque service.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="1dbfa-174">[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="1dbfa-175">Cela termine l’introduction aux scénarios de tri, de filtrage et de maître/détail.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="1dbfa-176">Dans le didacticiel suivant, vous verrez comment gérer l’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="1dbfa-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1dbfa-177">[Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="1dbfa-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
