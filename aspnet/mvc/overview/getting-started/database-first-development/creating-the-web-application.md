---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Didacticiel : créer l’application Web et les modèles de données pour EF Database First avec ASP.NET MVC'
description: Ce didacticiel se concentre sur la création de l’application Web et la génération des modèles de données basés sur vos tables de base de données.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616269"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="bfdb3-103">Didacticiel : créer l’application Web et les modèles de données pour EF Database First avec ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bfdb3-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="bfdb3-104">À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="bfdb3-105">Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="bfdb3-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="bfdb3-107">Ce didacticiel se concentre sur la création de l’application Web et la génération des modèles de données basés sur vos tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="bfdb3-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="bfdb3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bfdb3-109">Créer une application web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bfdb3-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="bfdb3-110">Générer les modèles</span><span class="sxs-lookup"><span data-stu-id="bfdb3-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfdb3-111">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="bfdb3-111">Prerequisites</span></span>

* [<span data-ttu-id="bfdb3-112">Prise en main de Entity Framework 6 Database First à l’aide de MVC 5</span><span class="sxs-lookup"><span data-stu-id="bfdb3-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="bfdb3-113">Créer une application web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bfdb3-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="bfdb3-114">Dans une nouvelle solution ou dans la même solution que le projet de base de données, créez un nouveau projet dans Visual Studio et sélectionnez le modèle **application Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="bfdb3-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="bfdb3-115">Nommez le projet **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-115">Name the project **ContosoSite**.</span></span>

![créer un projet](creating-the-web-application/_static/image1.png)

<span data-ttu-id="bfdb3-117">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-117">Click **OK**.</span></span>

<span data-ttu-id="bfdb3-118">Dans la fenêtre nouveau projet ASP.NET, sélectionnez le modèle **MVC** .</span><span class="sxs-lookup"><span data-stu-id="bfdb3-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="bfdb3-119">Vous pouvez désactiver l' **ordinateur hôte dans l’option Cloud** pour le moment, car vous déploierez l’application dans le Cloud ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="bfdb3-120">Cliquez sur **OK** pour créer l’application.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="bfdb3-121">Le projet est créé avec les fichiers et dossiers par défaut.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="bfdb3-122">Dans ce didacticiel, vous allez utiliser Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="bfdb3-123">Vous pouvez double-vérifier la version de Entity Framework dans votre projet via la fenêtre gérer les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="bfdb3-124">Si nécessaire, mettez à jour votre version de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-124">If necessary, update your version of Entity Framework.</span></span>

![afficher la version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="bfdb3-126">Générer les modèles</span><span class="sxs-lookup"><span data-stu-id="bfdb3-126">Generate the models</span></span>

<span data-ttu-id="bfdb3-127">Vous allez maintenant créer Entity Framework des modèles à partir des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="bfdb3-128">Ces modèles sont des classes que vous allez utiliser pour travailler avec les données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="bfdb3-129">Chaque modèle reflète une table dans la base de données et contient des propriétés qui correspondent aux colonnes de la table.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="bfdb3-130">Cliquez avec le bouton droit sur le dossier **modèles** , puis sélectionnez **Ajouter** et **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="bfdb3-131">Dans la fenêtre Ajouter un nouvel élément, sélectionnez **données** dans le volet gauche et **ADO.NET Entity Data Model** dans les options du volet central.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="bfdb3-132">Nommez le nouveau fichier de modèle **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="bfdb3-133">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-133">Click **Add**.</span></span>

<span data-ttu-id="bfdb3-134">Dans l’Assistant Entity Data Model, sélectionnez le **Concepteur EF à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="bfdb3-135">Cliquez sur **Next**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-135">Click **Next**.</span></span>

<span data-ttu-id="bfdb3-136">Si vous avez défini des connexions de base de données dans votre environnement de développement, vous pouvez voir une de ces connexions présélectionnée.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="bfdb3-137">Toutefois, vous souhaitez créer une nouvelle connexion à la base de données que vous avez créée dans la première partie de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="bfdb3-138">Cliquez sur le bouton **nouvelle connexion** .</span><span class="sxs-lookup"><span data-stu-id="bfdb3-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="bfdb3-139">Dans la Fenêtre Propriétés de connexion, indiquez le nom du serveur local sur lequel votre base de données a été créée (dans le cas présent, **\ProjectsV13**).</span><span class="sxs-lookup"><span data-stu-id="bfdb3-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="bfdb3-140">Après avoir fourni le nom du serveur, sélectionnez ContosoUniversityData dans les bases de données disponibles.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![définir les propriétés de connexion](creating-the-web-application/_static/image8.png)

<span data-ttu-id="bfdb3-142">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-142">Click **OK**.</span></span>

<span data-ttu-id="bfdb3-143">Les propriétés de connexion correctes sont désormais affichées.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="bfdb3-144">Vous pouvez utiliser le nom par défaut pour la connexion dans le fichier Web. config.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="bfdb3-145">Cliquez sur **Next**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-145">Click **Next**.</span></span>

<span data-ttu-id="bfdb3-146">Sélectionnez la dernière version de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="bfdb3-147">Cliquez sur **Next**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-147">Click **Next**.</span></span>

<span data-ttu-id="bfdb3-148">Sélectionnez **tables** pour générer des modèles pour les trois tables.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="bfdb3-149">Cliquez sur **Finish**.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-149">Click **Finish**.</span></span>

<span data-ttu-id="bfdb3-150">Si vous recevez un avertissement de sécurité, sélectionnez **OK** pour continuer à exécuter le modèle.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="bfdb3-151">Les modèles sont générés à partir des tables de base de données, et un diagramme s’affiche pour afficher les propriétés et les relations entre les tables.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagramme du modèle](creating-the-web-application/_static/image11.png)

<span data-ttu-id="bfdb3-153">Le dossier Models comprend désormais de nombreux nouveaux fichiers associés aux modèles qui ont été générés à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="bfdb3-154">Le fichier **ContosoModel.Context.cs** contient une classe qui dérive de la classe **DbContext** et fournit une propriété pour chaque classe de modèle qui correspond à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="bfdb3-155">Les fichiers **course.cs**, **Enrollment.cs**et **Student.cs** contiennent les classes de modèle qui représentent les tables de bases de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="bfdb3-156">Vous allez utiliser la classe de contexte et les classes de modèle lors de l’utilisation de la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="bfdb3-157">Avant de poursuivre ce didacticiel, générez le projet.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="bfdb3-158">Dans la section suivante, vous allez générer du code basé sur les modèles de données, mais cette section ne fonctionnera pas si le projet n’a pas été généré.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfdb3-159">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="bfdb3-159">Next steps</span></span>

<span data-ttu-id="bfdb3-160">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="bfdb3-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bfdb3-161">Création d’une application Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bfdb3-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="bfdb3-162">Modèles générés</span><span class="sxs-lookup"><span data-stu-id="bfdb3-162">Generated the models</span></span>

<span data-ttu-id="bfdb3-163">Passez au didacticiel suivant pour apprendre à créer du code basé sur les modèles de données.</span><span class="sxs-lookup"><span data-stu-id="bfdb3-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="bfdb3-164">Génération de vues</span><span class="sxs-lookup"><span data-stu-id="bfdb3-164">Generating views</span></span>](generating-views.md)
