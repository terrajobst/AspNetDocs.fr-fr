---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework la génération de modèles automatique et les migrations | Microsoft Docs
author: rick-anderson
description: Si vous êtes familiarisé avec les méthodes du contrôleur ASP.NET MVC 4, ou si vous avez terminé les &quot;helpers, les formulaires et la validation&quot; laboratoire pratique, vous devez être conscient...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598902"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="b3610-103">Génération de modèles automatique et migrations d’ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b3610-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="b3610-104">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b3610-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b3610-105">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="b3610-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b3610-106">Si vous êtes familiarisé avec les méthodes du contrôleur ASP.NET MVC 4 ou si vous avez terminé les &quot;helpers, les formulaires et la validation&quot; laboratoire pratique, vous devez savoir que la logique de création, de mise à jour, de liste et de suppression d’une entité de données est répétée dans l’application.</span><span class="sxs-lookup"><span data-stu-id="b3610-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="b3610-107">Pour ne pas mentionner que, si votre modèle a plusieurs classes à manipuler, vous devrez probablement consacrer beaucoup de temps à écrire les méthodes de publication et d’action pour chaque opération d’entité, ainsi que chacune des vues.</span><span class="sxs-lookup"><span data-stu-id="b3610-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="b3610-108">Dans ce laboratoire, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 pour générer automatiquement la ligne de base du CRUD de votre application (créer, lire, mettre à jour et supprimer).</span><span class="sxs-lookup"><span data-stu-id="b3610-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="b3610-109">À partir d’une classe de modèle simple et, sans écrire une seule ligne de code, vous allez créer un contrôleur qui contiendra toutes les opérations CRUD, ainsi que toutes les vues nécessaires.</span><span class="sxs-lookup"><span data-stu-id="b3610-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="b3610-110">Après avoir généré et exécuté la solution simple, la base de données d’application est générée, ainsi que la logique MVC et les vues pour la manipulation des données.</span><span class="sxs-lookup"><span data-stu-id="b3610-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="b3610-111">En outre, vous allez apprendre combien il est facile d’utiliser Entity Framework migrations pour effectuer des mises à jour de modèle dans toute votre application.</span><span class="sxs-lookup"><span data-stu-id="b3610-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="b3610-112">Entity Framework migrations vous permet de modifier votre base de données une fois que le modèle a été modifié avec des étapes simples.</span><span class="sxs-lookup"><span data-stu-id="b3610-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="b3610-113">Avec tout cela à l’esprit, vous serez en mesure de créer et de gérer des applications Web plus efficacement, en tirant parti des fonctionnalités les plus récentes de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b3610-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="b3610-114">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b3610-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b3610-115">Le projet propre à ce laboratoire est disponible dans [ASP.NET MVC 4 Entity Framework la génération de modèles automatique et les migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="b3610-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b3610-116">Objectifs</span><span class="sxs-lookup"><span data-stu-id="b3610-116">Objectives</span></span>

<span data-ttu-id="b3610-117">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="b3610-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b3610-118">Utilisez la génération de modèles automatique ASP.NET pour les opérations CRUD dans les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b3610-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="b3610-119">Modifiez le modèle de base de données à l’aide de Entity Framework migrations.</span><span class="sxs-lookup"><span data-stu-id="b3610-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b3610-120">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="b3610-120">Prerequisites</span></span>

<span data-ttu-id="b3610-121">Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b3610-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b3610-122">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).</span><span class="sxs-lookup"><span data-stu-id="b3610-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b3610-123">Installation</span><span class="sxs-lookup"><span data-stu-id="b3610-123">Setup</span></span>

<span data-ttu-id="b3610-124">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="b3610-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="b3610-125">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3610-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b3610-126">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="b3610-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b3610-127">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe B : utilisation d’extraits de Code](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b3610-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b3610-128">Exercices</span><span class="sxs-lookup"><span data-stu-id="b3610-128">Exercises</span></span>

<span data-ttu-id="b3610-129">L’exercice suivant constitue ce laboratoire pratique :</span><span class="sxs-lookup"><span data-stu-id="b3610-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="b3610-130">Utilisation de la génération de modèles automatique ASP.NET MVC 4 avec Entity Framework migrations</span><span class="sxs-lookup"><span data-stu-id="b3610-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="b3610-131">Cet exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir à l’issue de l’exercice.</span><span class="sxs-lookup"><span data-stu-id="b3610-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="b3610-132">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire dans le cadre de l’exercice.</span><span class="sxs-lookup"><span data-stu-id="b3610-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>

<span data-ttu-id="b3610-133">Durée estimée pour effectuer ce laboratoire : **30 minutes**</span><span class="sxs-lookup"><span data-stu-id="b3610-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="b3610-134">Exercice 1 : utilisation de la génération de modèles automatique ASP.NET MVC 4 avec Entity Framework migrations</span><span class="sxs-lookup"><span data-stu-id="b3610-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="b3610-135">La génération de modèles automatique ASP.NET MVC offre un moyen rapide de générer les opérations CRUD de manière standardisée, créant ainsi la logique nécessaire qui permet à votre application d’interagir avec la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="b3610-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="b3610-136">Dans cet exercice, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 avec code First pour créer les méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="b3610-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="b3610-137">Vous allez ensuite apprendre à mettre à jour votre modèle en appliquant les modifications apportées à la base de données à l’aide de Entity Framework migrations.</span><span class="sxs-lookup"><span data-stu-id="b3610-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="b3610-138">Tâche 1 : création d’un projet ASP.NET MVC 4 à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="b3610-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="b3610-139">S’il n’est pas déjà ouvert, démarrez **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="b3610-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="b3610-140">Sélectionner un **fichier | Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="b3610-140">Select **File | New Project**.</span></span> <span data-ttu-id="b3610-141">Dans la boîte de dialogue Nouveau projet, sous le **visuel C# | Section Web** , sélectionnez **Application Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="b3610-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="b3610-142">Nommez le projet en **MVC4andEFMigrations** et définissez l’emplacement sur le dossier **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** de ce Lab.</span><span class="sxs-lookup"><span data-stu-id="b3610-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="b3610-143">Définissez le **nom** de la solution sur **Démarrer** et assurez-vous que l’option **créer le répertoire pour la solution** est cochée.</span><span class="sxs-lookup"><span data-stu-id="b3610-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="b3610-144">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3610-144">Click **OK**.</span></span>

    <span data-ttu-id="b3610-145">![Boîte de dialogue Nouveau projet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Boîte de dialogue Nouveau projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b3610-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="b3610-146">*Boîte de dialogue Nouveau projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b3610-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="b3610-147">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle **application Internet** et assurez-vous que **Razor** est le **moteur d’affichage**sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b3610-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="b3610-148">Cliquez sur **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="b3610-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="b3610-149">![Nouvelle application Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nouvelle application Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b3610-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="b3610-150">*Nouvelle application Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b3610-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="b3610-151">Dans le Explorateur de solutions, cliquez avec le bouton droit sur **modèles** , puis sélectionnez **Ajouter | Classe** pour créer un utilisateur de classe simple (POCO).</span><span class="sxs-lookup"><span data-stu-id="b3610-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="b3610-152">Nommez-le **Person** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3610-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="b3610-153">Ouvrez la classe Person et insérez les propriétés suivantes.</span><span class="sxs-lookup"><span data-stu-id="b3610-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="b3610-154">(Extrait de code- *ASP.NET MVC 4 et migrations de Entity Framework-propriétés de la personne EX1*)</span><span class="sxs-lookup"><span data-stu-id="b3610-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="b3610-155">Cliquez sur **générer | Générez la solution** pour enregistrer les modifications et générer le projet.</span><span class="sxs-lookup"><span data-stu-id="b3610-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="b3610-156">![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Génération de l’application")</span><span class="sxs-lookup"><span data-stu-id="b3610-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="b3610-157">*Génération de l’application*</span><span class="sxs-lookup"><span data-stu-id="b3610-157">*Building the Application*</span></span>
7. <span data-ttu-id="b3610-158">Dans le Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers et sélectionnez **Ajouter | Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="b3610-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="b3610-159">Nommez le contrôleur *PersonController* et complétez les **options de génération de modèles** automatique avec les valeurs suivantes.</span><span class="sxs-lookup"><span data-stu-id="b3610-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="b3610-160">Dans la liste déroulante **modèle** , sélectionnez le **contrôleur MVC avec des actions et des affichages en lecture/écriture, à l’aide de l’option Entity Framework** .</span><span class="sxs-lookup"><span data-stu-id="b3610-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="b3610-161">Dans la liste déroulante **classe de modèle** , sélectionnez la classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="b3610-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="b3610-162">Dans la liste **classe du contexte de données** , sélectionnez **&lt;nouveau contexte de données...&gt;** .</span><span class="sxs-lookup"><span data-stu-id="b3610-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="b3610-163">Choisissez un nom et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3610-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="b3610-164">Dans la liste déroulante **affichages** , assurez-vous que l’option **Razor** est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="b3610-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="b3610-165">![Ajout du contrôleur person avec la génération de modèles automatique](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Ajout du contrôleur person avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="b3610-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="b3610-166">*Ajout du contrôleur person avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="b3610-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="b3610-167">Cliquez sur **Ajouter** pour créer le contrôleur pour la personne avec génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="b3610-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="b3610-168">Vous avez maintenant généré les actions du contrôleur ainsi que les vues.</span><span class="sxs-lookup"><span data-stu-id="b3610-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="b3610-169">![Après avoir créé le contrôleur person avec la génération de modèles automatique](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Après avoir créé le contrôleur person avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="b3610-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="b3610-170">*Après avoir créé le contrôleur person avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="b3610-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="b3610-171">Ouvrez la classe **PersonController** .</span><span class="sxs-lookup"><span data-stu-id="b3610-171">Open **PersonController** class.</span></span> <span data-ttu-id="b3610-172">Notez que les méthodes d’action CRUD complètes ont été générées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b3610-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="b3610-173">![À l’intérieur du contrôleur person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "À l’intérieur du contrôleur person")</span><span class="sxs-lookup"><span data-stu-id="b3610-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="b3610-174">*À l’intérieur du contrôleur person*</span><span class="sxs-lookup"><span data-stu-id="b3610-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="b3610-175">Tâche 2 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="b3610-175">Task 2- Running the application</span></span>

<span data-ttu-id="b3610-176">À ce stade, la base de données n’est pas encore créée.</span><span class="sxs-lookup"><span data-stu-id="b3610-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="b3610-177">Dans cette tâche, vous allez exécuter l’application pour la première fois et tester les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="b3610-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="b3610-178">La base de données sera créée à la volée avec Code First.</span><span class="sxs-lookup"><span data-stu-id="b3610-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="b3610-179">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b3610-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b3610-180">Dans le navigateur, ajoutez **/Person** à l’URL pour ouvrir la page Person.</span><span class="sxs-lookup"><span data-stu-id="b3610-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="b3610-181">![Première exécution de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Première exécution de l’application")</span><span class="sxs-lookup"><span data-stu-id="b3610-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="b3610-182">*Application : première exécution*</span><span class="sxs-lookup"><span data-stu-id="b3610-182">*Application: first run*</span></span>
3. <span data-ttu-id="b3610-183">Vous allez maintenant explorer les pages Person et tester les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="b3610-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="b3610-184">Cliquez sur **créer** pour ajouter une nouvelle personne.</span><span class="sxs-lookup"><span data-stu-id="b3610-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="b3610-185">Entrez un prénom et un nom, puis cliquez sur **créer**.</span><span class="sxs-lookup"><span data-stu-id="b3610-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="b3610-186">![Ajout d’une nouvelle personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Ajout d’une nouvelle personne")</span><span class="sxs-lookup"><span data-stu-id="b3610-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="b3610-187">*Ajout d’une nouvelle personne*</span><span class="sxs-lookup"><span data-stu-id="b3610-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="b3610-188">Dans la liste des personnes, vous pouvez supprimer, modifier ou ajouter des éléments.</span><span class="sxs-lookup"><span data-stu-id="b3610-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="b3610-189">![Liste des personnes](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Liste des personnes")</span><span class="sxs-lookup"><span data-stu-id="b3610-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="b3610-190">*Liste des personnes*</span><span class="sxs-lookup"><span data-stu-id="b3610-190">*Person list*</span></span>
    3. <span data-ttu-id="b3610-191">Cliquez sur **Détails** pour ouvrir les détails de la personne.</span><span class="sxs-lookup"><span data-stu-id="b3610-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="b3610-192">![Détails de la personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Détails de la personne")</span><span class="sxs-lookup"><span data-stu-id="b3610-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="b3610-193">*Détails de la personne*</span><span class="sxs-lookup"><span data-stu-id="b3610-193">*Person's details*</span></span>
4. <span data-ttu-id="b3610-194">Fermez le navigateur et revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3610-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="b3610-195">Notez que vous avez créé l’ensemble de la CRUD pour l’entité Person dans votre application, du modèle aux vues, sans avoir à écrire une seule ligne de code !</span><span class="sxs-lookup"><span data-stu-id="b3610-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="b3610-196">Tâche 3 : mise à jour de la base de données à l’aide de Entity Framework migrations</span><span class="sxs-lookup"><span data-stu-id="b3610-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="b3610-197">Au cours de cette tâche, vous allez mettre à jour la base de données à l’aide de Entity Framework migrations.</span><span class="sxs-lookup"><span data-stu-id="b3610-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="b3610-198">Vous allez découvrir combien il est facile de modifier le modèle et de refléter les modifications apportées à vos bases de données à l’aide de la fonctionnalité de migration Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3610-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="b3610-199">Ouvrez la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="b3610-199">Open the Package Manager Console.</span></span> <span data-ttu-id="b3610-200">Cliquez sur **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="b3610-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="b3610-201">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b3610-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="b3610-202">PMC</span><span class="sxs-lookup"><span data-stu-id="b3610-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="b3610-203">![Activation des migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Activation des migrations")</span><span class="sxs-lookup"><span data-stu-id="b3610-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="b3610-204">*Activation des migrations*</span><span class="sxs-lookup"><span data-stu-id="b3610-204">*Enabling migrations*</span></span>

    <span data-ttu-id="b3610-205">La commande Enable-migration crée le dossier **migrations** , qui contient un script pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3610-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="b3610-206">![Dossier migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Dossier migrations")</span><span class="sxs-lookup"><span data-stu-id="b3610-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="b3610-207">*Dossier migrations*</span><span class="sxs-lookup"><span data-stu-id="b3610-207">*Migrations folder*</span></span>
3. <span data-ttu-id="b3610-208">Ouvrez le fichier **Configuration.cs** dans le dossier migrations.</span><span class="sxs-lookup"><span data-stu-id="b3610-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="b3610-209">Recherchez le constructeur de classe et remplacez la valeur **AutomaticMigrationsEnabled** par *true*.</span><span class="sxs-lookup"><span data-stu-id="b3610-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="b3610-210">Ouvrez la classe Person et ajoutez un attribut pour le deuxième prénom de la personne.</span><span class="sxs-lookup"><span data-stu-id="b3610-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="b3610-211">Avec ce nouvel attribut, vous modifiez le modèle.</span><span class="sxs-lookup"><span data-stu-id="b3610-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="b3610-212">Sélectionner une **Build | Générez la solution** dans le menu pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="b3610-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="b3610-213">![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Génération de l’application")</span><span class="sxs-lookup"><span data-stu-id="b3610-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="b3610-214">*Génération de l’application*</span><span class="sxs-lookup"><span data-stu-id="b3610-214">*Building the application*</span></span>
6. <span data-ttu-id="b3610-215">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b3610-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="b3610-216">PMC</span><span class="sxs-lookup"><span data-stu-id="b3610-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="b3610-217">Cette commande recherche les modifications dans les objets de données, puis ajoute les commandes nécessaires pour modifier la base de données en conséquence.</span><span class="sxs-lookup"><span data-stu-id="b3610-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="b3610-218">![Ajout d’un deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Ajout d’un deuxième prénom")</span><span class="sxs-lookup"><span data-stu-id="b3610-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="b3610-219">*Ajout d’un deuxième prénom*</span><span class="sxs-lookup"><span data-stu-id="b3610-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="b3610-220">Facultatif Vous pouvez exécuter la commande suivante pour générer un script SQL avec la mise à jour différentielle.</span><span class="sxs-lookup"><span data-stu-id="b3610-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="b3610-221">Cela vous permet de mettre à jour la base de données manuellement (dans ce cas, il n’est pas nécessaire) ou d’appliquer les modifications dans d’autres bases de données :</span><span class="sxs-lookup"><span data-stu-id="b3610-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="b3610-222">PMC</span><span class="sxs-lookup"><span data-stu-id="b3610-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="b3610-223">![Génération d’un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Génération d’un script SQL")</span><span class="sxs-lookup"><span data-stu-id="b3610-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="b3610-224">*Génération d’un script SQL*</span><span class="sxs-lookup"><span data-stu-id="b3610-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="b3610-225">![Mise à jour des scripts SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Mise à jour des scripts SQL")</span><span class="sxs-lookup"><span data-stu-id="b3610-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="b3610-226">*Mise à jour des scripts SQL*</span><span class="sxs-lookup"><span data-stu-id="b3610-226">*SQL Script update*</span></span>
8. <span data-ttu-id="b3610-227">Dans la console du gestionnaire de package, entrez la commande suivante pour mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="b3610-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="b3610-228">PMC</span><span class="sxs-lookup"><span data-stu-id="b3610-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="b3610-229">![Mise à jour de la base de données](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Mise à jour de la base de données")</span><span class="sxs-lookup"><span data-stu-id="b3610-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="b3610-230">*Mise à jour de la base de données*</span><span class="sxs-lookup"><span data-stu-id="b3610-230">*Updating the Database*</span></span>

    <span data-ttu-id="b3610-231">Cette opération ajoute la colonne **MiddleName** dans la table **People** pour correspondre à la définition actuelle de la classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="b3610-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="b3610-232">Une fois la base de données mise à jour, cliquez avec le bouton droit sur le dossier du contrôleur et sélectionnez **Ajouter | Pour rajouter le contrôleur de** personne (avec les mêmes valeurs).</span><span class="sxs-lookup"><span data-stu-id="b3610-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="b3610-233">Cela permet de mettre à jour les méthodes et les vues existantes qui ajoutent le nouvel attribut.</span><span class="sxs-lookup"><span data-stu-id="b3610-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="b3610-234">![Ajout d’une mise à jour du contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Ajout d’une mise à jour du contrôleur")</span><span class="sxs-lookup"><span data-stu-id="b3610-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="b3610-235">*Mise à jour du contrôleur*</span><span class="sxs-lookup"><span data-stu-id="b3610-235">*Updating the controller*</span></span>
10. <span data-ttu-id="b3610-236">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3610-236">Click **Add**.</span></span> <span data-ttu-id="b3610-237">Ensuite, sélectionnez les valeurs **overwrite PersonController.cs** et remplacer les **affichages associés** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3610-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Ajout d’un remplacement de contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="b3610-239">*Mise à jour du contrôleur*</span><span class="sxs-lookup"><span data-stu-id="b3610-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="b3610-240">Task4-exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="b3610-240">Task4- Running the application</span></span>

1. <span data-ttu-id="b3610-241">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b3610-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b3610-242">Ouvrez **/Person**.</span><span class="sxs-lookup"><span data-stu-id="b3610-242">Open **/Person**.</span></span> <span data-ttu-id="b3610-243">Notez que les données ont été conservées, tandis que la colonne de deuxième prénom a été ajoutée.</span><span class="sxs-lookup"><span data-stu-id="b3610-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="b3610-244">![Deuxième prénom ajouté](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Deuxième prénom ajouté")</span><span class="sxs-lookup"><span data-stu-id="b3610-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="b3610-245">*Deuxième prénom ajouté*</span><span class="sxs-lookup"><span data-stu-id="b3610-245">*Middle Name added*</span></span>
3. <span data-ttu-id="b3610-246">Si vous cliquez sur **modifier**, vous pouvez ajouter un deuxième prénom à la personne en cours.</span><span class="sxs-lookup"><span data-stu-id="b3610-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="b3610-247">![Édition du deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Édition du deuxième prénom")</span><span class="sxs-lookup"><span data-stu-id="b3610-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b3610-248">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="b3610-248">Summary</span></span>

<span data-ttu-id="b3610-249">Dans ce laboratoire pratique, vous avez appris des étapes simples pour créer des opérations CRUD avec la génération de modèles automatique ASP.NET MVC 4 à l’aide de n’importe quelle classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="b3610-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="b3610-250">Ensuite, vous avez appris à effectuer une mise à jour de bout en bout dans votre application, de la base de données aux vues, à l’aide de Entity Framework migrations.</span><span class="sxs-lookup"><span data-stu-id="b3610-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b3610-251">Annexe A : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="b3610-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b3610-252">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="b3610-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b3610-253">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b3610-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b3610-254">Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b3610-254">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b3610-255">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b3610-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b3610-256">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="b3610-256">Click on **Install Now**.</span></span> <span data-ttu-id="b3610-257">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="b3610-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b3610-258">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="b3610-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b3610-259">![Installer Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b3610-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b3610-260">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b3610-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b3610-261">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="b3610-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="b3610-263">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="b3610-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b3610-264">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="b3610-264">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="b3610-266">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="b3610-266">*Installation progress*</span></span>
6. <span data-ttu-id="b3610-267">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="b3610-267">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="b3610-269">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="b3610-269">*Installation completed*</span></span>
7. <span data-ttu-id="b3610-270">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="b3610-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b3610-271">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="b3610-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="b3610-273">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="b3610-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b3610-274">Annexe B : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="b3610-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b3610-275">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="b3610-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b3610-276">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="b3610-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b3610-277">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="b3610-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b3610-278">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="b3610-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b3610-279">***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***</span><span class="sxs-lookup"><span data-stu-id="b3610-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b3610-280">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="b3610-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b3610-281">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="b3610-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b3610-282">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="b3610-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b3610-283">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="b3610-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b3610-284">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="b3610-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b3610-285">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="b3610-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b3610-286">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="b3610-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="b3610-287">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="b3610-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b3610-288">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="b3610-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b3610-289">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="b3610-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b3610-290">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="b3610-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b3610-291">***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0.</span><span class="sxs-lookup"><span data-stu-id="b3610-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b3610-292">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="b3610-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b3610-293">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="b3610-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b3610-294">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="b3610-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b3610-295">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="b3610-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b3610-296">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="b3610-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b3610-297">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="b3610-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b3610-298">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="b3610-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
