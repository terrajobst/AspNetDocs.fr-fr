---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Didacticiel : personnaliser l’affichage des Database First EF avec l’application MVC ASP.NET'
description: Ce didacticiel se concentre sur la modification des vues générées automatiquement pour améliorer la présentation.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583593"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="8dfd1-103">Didacticiel : personnaliser l’affichage des Database First EF avec l’application MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8dfd1-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="8dfd1-104">À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8dfd1-105">Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8dfd1-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="8dfd1-107">Ce didacticiel se concentre sur la modification des vues générées automatiquement pour améliorer la présentation.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="8dfd1-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="8dfd1-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8dfd1-109">Ajouter des cours à la page de détails de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="8dfd1-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="8dfd1-110">Confirmer l’ajout des cours à la page</span><span class="sxs-lookup"><span data-stu-id="8dfd1-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dfd1-111">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="8dfd1-111">Prerequisites</span></span>

* [<span data-ttu-id="8dfd1-112">Modifier la base de données</span><span class="sxs-lookup"><span data-stu-id="8dfd1-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="8dfd1-113">Ajouter des cours aux détails de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="8dfd1-113">Add courses to student detail</span></span>

<span data-ttu-id="8dfd1-114">Le code généré fournit un bon point de départ pour votre application, mais elle ne fournit pas nécessairement toutes les fonctionnalités dont vous avez besoin dans votre application.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="8dfd1-115">Vous pouvez personnaliser le code pour répondre aux besoins particuliers de votre application.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="8dfd1-116">Actuellement, votre application n’affiche pas les cours inscrits pour l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="8dfd1-117">Dans cette section, vous allez ajouter les cours inscrits pour chaque étudiant à la vue **Détails** de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="8dfd1-118">Ouvrez **vues** > **étudiants** > *Details. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="8dfd1-119">Sous la dernière balise &lt;/DL&gt;, mais avant la balise de fin &lt;/div&gt;, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="8dfd1-120">Ce code crée une table qui affiche une ligne pour chaque enregistrement de la table d’inscription pour l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="8dfd1-121">La méthode **Display** restitue le code HTML de l’objet (ModelItem) qui représente l’expression.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="8dfd1-122">Vous utilisez la méthode d’affichage (au lieu d’incorporer simplement la valeur de la propriété dans le code) pour vous assurer que la valeur est correctement mise en forme en fonction de son type et du modèle pour ce type.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="8dfd1-123">Dans cet exemple, chaque expression retourne une propriété unique à partir de l’enregistrement en cours dans la boucle, et les valeurs sont des types primitifs qui sont rendus sous forme de texte.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="8dfd1-124">Confirmer l’ajout des cours</span><span class="sxs-lookup"><span data-stu-id="8dfd1-124">Confirm courses are added</span></span>

<span data-ttu-id="8dfd1-125">Exécutez la solution.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-125">Run the solution.</span></span> <span data-ttu-id="8dfd1-126">Cliquez sur **liste des élèves** et sélectionnez les **Détails** de l’un des étudiants.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="8dfd1-127">Vous verrez que les cours inscrits ont été inclus dans la vue.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-127">You will see the enrolled courses have been included in the view.</span></span>

![étudiant avec inscription](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="8dfd1-129">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="8dfd1-129">Next steps</span></span>
<span data-ttu-id="8dfd1-130">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="8dfd1-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8dfd1-131">Ajout de cours à la page de détails de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="8dfd1-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="8dfd1-132">Confirmation de l’ajout des cours à la page</span><span class="sxs-lookup"><span data-stu-id="8dfd1-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="8dfd1-133">Passez au didacticiel suivant pour apprendre à ajouter des annotations de données pour spécifier les exigences de validation et la mise en forme d’affichage.</span><span class="sxs-lookup"><span data-stu-id="8dfd1-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8dfd1-134">Améliorer la validation des données</span><span class="sxs-lookup"><span data-stu-id="8dfd1-134">Enhance data validation</span></span>](enhancing-data-validation.md)
