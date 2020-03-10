---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Tri, pagination et filtrage des données avec la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548061"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="fb984-104">Tri, pagination et filtrage des données avec la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="fb984-104">Sorting, paging, and filtering data with model binding and web forms</span></span>

<span data-ttu-id="fb984-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fb984-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fb984-106">Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb984-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="fb984-107">La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="fb984-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="fb984-108">Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="fb984-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="fb984-109">Ce didacticiel montre comment ajouter le tri, la pagination et le filtrage des données par le biais de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="fb984-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="fb984-110">Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.</span><span class="sxs-lookup"><span data-stu-id="fb984-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="fb984-111">Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB.</span><span class="sxs-lookup"><span data-stu-id="fb984-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="fb984-112">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="fb984-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="fb984-113">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="fb984-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="fb984-114">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="fb984-114">What you'll build</span></span>

<span data-ttu-id="fb984-115">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="fb984-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="fb984-116">Activer le tri et la pagination des données</span><span class="sxs-lookup"><span data-stu-id="fb984-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="fb984-117">Activer le filtrage des données en fonction d’une sélection par l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="fb984-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="fb984-118">Ajouter la fonctionnalité de tri</span><span class="sxs-lookup"><span data-stu-id="fb984-118">Add sorting</span></span>

<span data-ttu-id="fb984-119">L’activation du tri dans le GridView est très facile.</span><span class="sxs-lookup"><span data-stu-id="fb984-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="fb984-120">Dans le fichier Student. aspx, affectez simplement la **valeur true** à **AllowSorting** dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="fb984-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="fb984-121">Vous n’avez pas besoin de définir une valeur **SortExpression** pour chaque colonne, car DataField est utilisé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="fb984-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="fb984-122">Le GridView modifie la requête pour inclure le classement des données par la valeur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="fb984-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="fb984-123">Le code en surbrillance ci-dessous montre l’ajout que vous devez effectuer pour activer le tri.</span><span class="sxs-lookup"><span data-stu-id="fb984-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="fb984-124">Exécutez l’application Web, puis testez les enregistrements d’étudiants de tri en fonction des valeurs figurant dans des colonnes différentes.</span><span class="sxs-lookup"><span data-stu-id="fb984-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Trier les élèves](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="fb984-126">Ajouter la fonctionnalité de pagination</span><span class="sxs-lookup"><span data-stu-id="fb984-126">Add paging</span></span>

<span data-ttu-id="fb984-127">L’activation de la pagination est également très facile.</span><span class="sxs-lookup"><span data-stu-id="fb984-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="fb984-128">Dans le GridView, affectez la valeur **true** à la propriété **AllowPaging** et définissez la propriété **pageSize** sur le nombre d’enregistrements que vous souhaitez afficher sur chaque page.</span><span class="sxs-lookup"><span data-stu-id="fb984-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="fb984-129">Dans ce didacticiel, vous pouvez lui affecter la valeur 4.</span><span class="sxs-lookup"><span data-stu-id="fb984-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="fb984-130">Exécutez l’application Web et remarquez que les enregistrements sont désormais répartis sur plusieurs pages, avec un maximum de 4 enregistrements affichés sur une seule page.</span><span class="sxs-lookup"><span data-stu-id="fb984-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Ajouter la pagination](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="fb984-132">L’exécution différée des requêtes améliore l’efficacité de l’application.</span><span class="sxs-lookup"><span data-stu-id="fb984-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="fb984-133">Au lieu de récupérer l’ensemble du jeu de données, le contrôle GridView modifie la requête pour récupérer uniquement les enregistrements de la page actuelle.</span><span class="sxs-lookup"><span data-stu-id="fb984-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="fb984-134">Filtrer les enregistrements par sélection d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="fb984-134">Filter records by user selection</span></span>

<span data-ttu-id="fb984-135">La liaison de modèle ajoute plusieurs attributs qui vous permettent de spécifier comment définir la valeur d’un paramètre dans une méthode de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="fb984-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="fb984-136">Ces attributs se trouvent dans l’espace de noms **System. Web. ModelBinding** .</span><span class="sxs-lookup"><span data-stu-id="fb984-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="fb984-137">Ils comprennent :</span><span class="sxs-lookup"><span data-stu-id="fb984-137">They include:</span></span>

- <span data-ttu-id="fb984-138">Contrôle</span><span class="sxs-lookup"><span data-stu-id="fb984-138">Control</span></span>
- <span data-ttu-id="fb984-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="fb984-139">Cookie</span></span>
- <span data-ttu-id="fb984-140">Formulaire</span><span class="sxs-lookup"><span data-stu-id="fb984-140">Form</span></span>
- <span data-ttu-id="fb984-141">Profil</span><span class="sxs-lookup"><span data-stu-id="fb984-141">Profile</span></span>
- <span data-ttu-id="fb984-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="fb984-142">QueryString</span></span>
- <span data-ttu-id="fb984-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="fb984-143">RouteData</span></span>
- <span data-ttu-id="fb984-144">Session</span><span class="sxs-lookup"><span data-stu-id="fb984-144">Session</span></span>
- <span data-ttu-id="fb984-145">Profils</span><span class="sxs-lookup"><span data-stu-id="fb984-145">UserProfile</span></span>
- <span data-ttu-id="fb984-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="fb984-146">ViewState</span></span>

<span data-ttu-id="fb984-147">Dans ce didacticiel, vous allez utiliser la valeur d’un contrôle pour filtrer les enregistrements affichés dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="fb984-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="fb984-148">Vous allez ajouter l’attribut de **contrôle** à la méthode de requête que vous avez créée précédemment.</span><span class="sxs-lookup"><span data-stu-id="fb984-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="fb984-149">Dans un didacticiel [ultérieur](using-query-string-values-to-retrieve-data.md) , vous allez appliquer l’attribut **QueryString** à un paramètre pour spécifier que la valeur du paramètre provient d’une valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="fb984-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="fb984-150">Tout d’abord, au-dessus du ValidationSummary, ajoutez une liste déroulante pour filtrer les élèves affichés.</span><span class="sxs-lookup"><span data-stu-id="fb984-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="fb984-151">Dans le fichier code-behind, modifiez la méthode Select pour recevoir une valeur du contrôle, puis définissez le nom du paramètre sur le nom du contrôle qui fournit la valeur.</span><span class="sxs-lookup"><span data-stu-id="fb984-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="fb984-152">Vous devez ajouter une instruction **using** pour l’espace de noms **System. Web. ModelBinding** pour résoudre l’attribut de contrôle.</span><span class="sxs-lookup"><span data-stu-id="fb984-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="fb984-153">Le code suivant montre que la méthode Select a été retravaillée pour filtrer les données retournées en fonction de la valeur de la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="fb984-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="fb984-154">L’ajout d’un attribut de contrôle avant un paramètre spécifie que la valeur de ce paramètre provient d’un contrôle portant le même nom.</span><span class="sxs-lookup"><span data-stu-id="fb984-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="fb984-155">Exécutez l’application Web et sélectionnez des valeurs différentes dans la liste déroulante pour filtrer la liste des étudiants.</span><span class="sxs-lookup"><span data-stu-id="fb984-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![filtrer les élèves](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="fb984-157">Conclusion</span><span class="sxs-lookup"><span data-stu-id="fb984-157">Conclusion</span></span>

<span data-ttu-id="fb984-158">Dans ce didacticiel, vous avez activé le tri et la pagination des données.</span><span class="sxs-lookup"><span data-stu-id="fb984-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="fb984-159">Vous avez également activé le filtrage des données par la valeur d’un contrôle.</span><span class="sxs-lookup"><span data-stu-id="fb984-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="fb984-160">Dans le [didacticiel](integrating-jquery-ui.md) suivant, vous allez améliorer l’interface utilisateur en intégrant un widget d’interface utilisateur jQuery dans le modèle Dynamic Data.</span><span class="sxs-lookup"><span data-stu-id="fb984-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb984-161">[Précédent](updating-deleting-and-creating-data.md)
> [Suivant](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="fb984-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
