---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Ajout d’une couche de logique métier à un projet qui utilise la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634833"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="4534a-104">Ajout d’une couche de logique métier à un projet qui utilise la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="4534a-104">Adding business logic layer to a project that uses model binding and web forms</span></span>

<span data-ttu-id="4534a-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4534a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4534a-106">Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4534a-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4534a-107">La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="4534a-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4534a-108">Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="4534a-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4534a-109">Ce didacticiel montre comment utiliser la liaison de modèle avec une couche de logique métier.</span><span class="sxs-lookup"><span data-stu-id="4534a-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="4534a-110">Vous allez définir le membre OnCallingDataMethods pour spécifier qu’un objet autre que la page actuelle est utilisé pour appeler les méthodes de données.</span><span class="sxs-lookup"><span data-stu-id="4534a-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="4534a-111">Ce didacticiel s’appuie sur le projet créé dans les parties [précédentes](retrieving-data.md) de la série.</span><span class="sxs-lookup"><span data-stu-id="4534a-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="4534a-112">Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB.</span><span class="sxs-lookup"><span data-stu-id="4534a-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4534a-113">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4534a-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4534a-114">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="4534a-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="4534a-115">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="4534a-115">What you'll build</span></span>

<span data-ttu-id="4534a-116">La liaison de modèle vous permet de placer votre code d’interaction de données dans le fichier code-behind d’une page Web ou dans une classe de logique métier distincte.</span><span class="sxs-lookup"><span data-stu-id="4534a-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="4534a-117">Les didacticiels précédents ont montré comment utiliser les fichiers code-behind pour le code d’interaction des données.</span><span class="sxs-lookup"><span data-stu-id="4534a-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="4534a-118">Cette approche fonctionne pour les petits sites, mais elle peut entraîner une répétition du code et une plus grande difficulté lors de la maintenance d’un site de grande taille.</span><span class="sxs-lookup"><span data-stu-id="4534a-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="4534a-119">Il peut également être très difficile de tester par programmation le code qui réside dans les fichiers code-behind, car il n’existe aucune couche d’abstraction.</span><span class="sxs-lookup"><span data-stu-id="4534a-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="4534a-120">Pour centraliser le code d’interaction des données, vous pouvez créer une couche de logique métier qui contient toute la logique permettant d’interagir avec les données.</span><span class="sxs-lookup"><span data-stu-id="4534a-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="4534a-121">Vous appelez ensuite la couche de logique métier à partir de vos pages Web.</span><span class="sxs-lookup"><span data-stu-id="4534a-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="4534a-122">Ce didacticiel montre comment déplacer tout le code que vous avez écrit dans les didacticiels précédents dans une couche de logique métier, puis comment utiliser ce code à partir des pages.</span><span class="sxs-lookup"><span data-stu-id="4534a-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="4534a-123">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="4534a-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4534a-124">Déplacer le code des fichiers code-behind vers une couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="4534a-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="4534a-125">Modifier vos contrôles liés aux données pour appeler les méthodes dans la couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="4534a-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="4534a-126">Créer une couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="4534a-126">Create business logic layer</span></span>

<span data-ttu-id="4534a-127">À présent, vous allez créer la classe qui est appelée à partir des pages Web.</span><span class="sxs-lookup"><span data-stu-id="4534a-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="4534a-128">Les méthodes de cette classe sont similaires aux méthodes que vous avez utilisées dans les didacticiels précédents et incluent les attributs du fournisseur de valeurs.</span><span class="sxs-lookup"><span data-stu-id="4534a-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="4534a-129">Tout d’abord, ajoutez un nouveau dossier appelé **BLL**.</span><span class="sxs-lookup"><span data-stu-id="4534a-129">First, add a new folder called **BLL**.</span></span>

![Ajouter un dossier](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="4534a-131">Dans le dossier BLL, créez une nouvelle classe nommée **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="4534a-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="4534a-132">Elle contient toutes les opérations de données qui se trouvaient à l’origine dans les fichiers code-behind.</span><span class="sxs-lookup"><span data-stu-id="4534a-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="4534a-133">Les méthodes sont presque les mêmes que celles du fichier code-behind, mais elles incluent des modifications.</span><span class="sxs-lookup"><span data-stu-id="4534a-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="4534a-134">La modification la plus importante à noter est que vous n’exécutez plus le code à partir d’une instance de la classe **page** .</span><span class="sxs-lookup"><span data-stu-id="4534a-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="4534a-135">La classe page contient la méthode **TryUpdateModel** et la propriété **ModelState** .</span><span class="sxs-lookup"><span data-stu-id="4534a-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="4534a-136">Quand ce code est déplacé vers une couche de logique métier, vous n’avez plus d’instance de la classe page pour appeler ces membres.</span><span class="sxs-lookup"><span data-stu-id="4534a-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="4534a-137">Pour contourner ce problème, vous devez ajouter un paramètre **ModelMethodContext** à toute méthode qui accède à TryUpdateModel ou ModelState.</span><span class="sxs-lookup"><span data-stu-id="4534a-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="4534a-138">Vous utilisez ce paramètre ModelMethodContext pour appeler TryUpdateModel ou récupérer ModelState.</span><span class="sxs-lookup"><span data-stu-id="4534a-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="4534a-139">Vous n’avez pas besoin de modifier quoi que ce soit dans la page Web pour tenir compte de ce nouveau paramètre.</span><span class="sxs-lookup"><span data-stu-id="4534a-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="4534a-140">Remplacez le code dans SchoolBL.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="4534a-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="4534a-141">Réviser les pages existantes pour récupérer des données de la couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="4534a-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="4534a-142">Enfin, vous allez convertir les pages students. aspx, AddStudent. aspx et courses. aspx en utilisant des requêtes dans le fichier code-behind pour utiliser la couche de logique métier.</span><span class="sxs-lookup"><span data-stu-id="4534a-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="4534a-143">Dans les fichiers code-behind pour les élèves, les AddStudent et les cours, supprimez ou commentez les méthodes de requête suivantes :</span><span class="sxs-lookup"><span data-stu-id="4534a-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="4534a-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="4534a-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="4534a-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="4534a-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="4534a-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="4534a-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="4534a-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="4534a-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="4534a-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="4534a-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="4534a-149">Vous ne devriez à présent avoir aucun code dans le fichier code-behind qui se rapporte aux opérations de données.</span><span class="sxs-lookup"><span data-stu-id="4534a-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="4534a-150">Le gestionnaire d’événements **OnCallingDataMethods** vous permet de spécifier un objet à utiliser pour les méthodes de données.</span><span class="sxs-lookup"><span data-stu-id="4534a-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="4534a-151">Dans students. aspx, ajoutez une valeur pour ce gestionnaire d’événements et remplacez les noms des méthodes de données par les noms des méthodes de la classe de logique métier.</span><span class="sxs-lookup"><span data-stu-id="4534a-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="4534a-152">Dans le fichier code-behind pour students. aspx, définissez le gestionnaire d’événements pour l’événement CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="4534a-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="4534a-153">Dans ce gestionnaire d’événements, vous spécifiez la classe de logique métier pour les opérations de données.</span><span class="sxs-lookup"><span data-stu-id="4534a-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="4534a-154">Dans AddStudent. aspx, apportez des modifications similaires.</span><span class="sxs-lookup"><span data-stu-id="4534a-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="4534a-155">Dans courses. aspx, apportez des modifications similaires.</span><span class="sxs-lookup"><span data-stu-id="4534a-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="4534a-156">Exécutez l’application et remarquez que toutes les pages fonctionnent comme auparavant.</span><span class="sxs-lookup"><span data-stu-id="4534a-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="4534a-157">La logique de validation fonctionne également correctement.</span><span class="sxs-lookup"><span data-stu-id="4534a-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4534a-158">Conclusion</span><span class="sxs-lookup"><span data-stu-id="4534a-158">Conclusion</span></span>

<span data-ttu-id="4534a-159">Dans ce didacticiel, vous avez restructuré votre application pour qu’elle utilise une couche d’accès aux données et une couche de logique métier.</span><span class="sxs-lookup"><span data-stu-id="4534a-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="4534a-160">Vous avez spécifié que les contrôles de données utilisent un objet qui n’est pas la page actuelle pour les opérations de données.</span><span class="sxs-lookup"><span data-stu-id="4534a-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4534a-161">Précédent</span><span class="sxs-lookup"><span data-stu-id="4534a-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
