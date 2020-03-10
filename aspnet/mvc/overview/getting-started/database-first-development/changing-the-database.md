---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Didacticiel : modifier la base de données pour le Database First EF avec l’application MVC ASP.NET'
description: Ce didacticiel se concentre sur la mise à jour de la structure de la base de données et sur la propagation de cette modification dans l’application Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616262"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="5798e-103">Didacticiel : modifier la base de données pour le Database First EF avec l’application MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5798e-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="5798e-104">À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="5798e-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="5798e-105">Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="5798e-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="5798e-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="5798e-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="5798e-107">Ce didacticiel se concentre sur la mise à jour de la structure de la base de données et sur la propagation de cette modification dans l’application Web.</span><span class="sxs-lookup"><span data-stu-id="5798e-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="5798e-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="5798e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5798e-109">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="5798e-109">Add a column</span></span>
> * <span data-ttu-id="5798e-110">Ajouter la propriété aux vues</span><span class="sxs-lookup"><span data-stu-id="5798e-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5798e-111">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="5798e-111">Prerequisites</span></span>

* [<span data-ttu-id="5798e-112">Génération de vues</span><span class="sxs-lookup"><span data-stu-id="5798e-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="5798e-113">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="5798e-113">Add a column</span></span>

<span data-ttu-id="5798e-114">Si vous mettez à jour la structure d’une table dans votre base de données, vous devez vous assurer que votre modification est propagée vers le modèle de données, les vues et le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="5798e-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="5798e-115">Pour ce didacticiel, vous allez ajouter une nouvelle colonne à la table Student pour enregistrer le deuxième prénom de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="5798e-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="5798e-116">Pour ajouter cette colonne, ouvrez le projet de base de données, puis ouvrez le fichier Student. Sql.</span><span class="sxs-lookup"><span data-stu-id="5798e-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="5798e-117">Par le biais du concepteur ou du code T-SQL, ajoutez une colonne nommée **MiddleName** qui est de type nvarchar (50) et qui autorise les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="5798e-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="5798e-118">Déployez cette modification dans votre base de données locale en démarrant votre projet de base de données (ou F5).</span><span class="sxs-lookup"><span data-stu-id="5798e-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="5798e-119">Le nouveau champ est ajouté à la table.</span><span class="sxs-lookup"><span data-stu-id="5798e-119">The new field is added to the table.</span></span> <span data-ttu-id="5798e-120">Si vous ne le voyez pas dans le Explorateur d’objets SQL Server, cliquez sur le bouton Actualiser dans le volet.</span><span class="sxs-lookup"><span data-stu-id="5798e-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![afficher la nouvelle colonne](changing-the-database/_static/image2.png)

<span data-ttu-id="5798e-122">La nouvelle colonne existe dans la table de base de données, mais elle n’existe pas actuellement dans la classe de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="5798e-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="5798e-123">Vous devez mettre à jour le modèle pour inclure votre nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="5798e-123">You must update the model to include your new column.</span></span> <span data-ttu-id="5798e-124">Dans le dossier **Models** , ouvrez le fichier **ContosoModel. edmx** pour afficher le diagramme de modèle.</span><span class="sxs-lookup"><span data-stu-id="5798e-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="5798e-125">Notez que le modèle Student ne contient pas la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="5798e-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="5798e-126">Cliquez avec le bouton droit n’importe où sur l’aire de conception, puis sélectionnez **mettre à jour le modèle à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="5798e-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="5798e-127">Dans l’Assistant Mise à jour, sélectionnez l’onglet **Actualiser** , puis sélectionnez **Tables** > **dbo** > **Student**.</span><span class="sxs-lookup"><span data-stu-id="5798e-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="5798e-128">Cliquez sur **Finish**.</span><span class="sxs-lookup"><span data-stu-id="5798e-128">Click **Finish**.</span></span>

<span data-ttu-id="5798e-129">Une fois le processus de mise à jour terminé, le schéma de base de données comprend la nouvelle propriété **MiddleName** .</span><span class="sxs-lookup"><span data-stu-id="5798e-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="5798e-130">Enregistrez le fichier **ContosoModel. edmx** .</span><span class="sxs-lookup"><span data-stu-id="5798e-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="5798e-131">Vous devez enregistrer ce fichier pour la nouvelle propriété à propager vers la classe **Student.cs** .</span><span class="sxs-lookup"><span data-stu-id="5798e-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="5798e-132">Vous avez maintenant mis à jour la base de données et le modèle.</span><span class="sxs-lookup"><span data-stu-id="5798e-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="5798e-133">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="5798e-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="5798e-134">Ajouter la propriété aux vues</span><span class="sxs-lookup"><span data-stu-id="5798e-134">Add the property to the views</span></span>

<span data-ttu-id="5798e-135">Malheureusement, les vues ne contiennent toujours pas la nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="5798e-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="5798e-136">Pour mettre à jour les vues, vous avez deux options : vous pouvez soit régénérer les vues en ajoutant une nouvelle fois la génération de modèles automatique pour la classe Student, soit ajouter manuellement la nouvelle propriété à vos vues existantes.</span><span class="sxs-lookup"><span data-stu-id="5798e-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="5798e-137">Dans ce didacticiel, vous allez rajouter la génération de modèles automatique, car vous n’avez pas apporté de modifications personnalisées aux vues générées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="5798e-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="5798e-138">Vous pouvez envisager d’ajouter manuellement la propriété lorsque vous avez apporté des modifications aux vues et que vous ne souhaitez pas perdre ces modifications.</span><span class="sxs-lookup"><span data-stu-id="5798e-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="5798e-139">Pour vous assurer **que les vues** sont recréées, supprimez le dossier Student sous **views**, puis supprimez **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="5798e-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="5798e-140">Ensuite, cliquez avec le bouton droit sur le dossier **Controllers** et ajoutez la génération de modèles automatique pour le modèle **Student** .</span><span class="sxs-lookup"><span data-stu-id="5798e-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="5798e-141">Là encore, nommez le contrôleur **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="5798e-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="5798e-142">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="5798e-142">Select **Add**.</span></span>

<span data-ttu-id="5798e-143">Regénérez la solution.</span><span class="sxs-lookup"><span data-stu-id="5798e-143">Build the solution again.</span></span> <span data-ttu-id="5798e-144">Les vues contiennent maintenant la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="5798e-144">The views now contain the MiddleName property.</span></span>

![afficher le deuxième prénom](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="5798e-146">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="5798e-146">Next steps</span></span>

<span data-ttu-id="5798e-147">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="5798e-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5798e-148">Ajout d’une colonne</span><span class="sxs-lookup"><span data-stu-id="5798e-148">Added a column</span></span>
> * <span data-ttu-id="5798e-149">Ajout de la propriété aux vues</span><span class="sxs-lookup"><span data-stu-id="5798e-149">Added the property to the views</span></span>

<span data-ttu-id="5798e-150">Passez au didacticiel suivant pour découvrir comment personnaliser l’affichage pour afficher des détails sur un enregistrement d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="5798e-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="5798e-151">Personnaliser un affichage</span><span class="sxs-lookup"><span data-stu-id="5798e-151">Customize a view</span></span>](customizing-a-view.md)