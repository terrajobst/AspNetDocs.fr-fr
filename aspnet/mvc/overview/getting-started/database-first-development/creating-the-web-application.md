---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutoriel : Créer l’Application Web et les modèles de données pour EF Database First avec ASP.NET MVC'
description: Ce didacticiel se concentre sur la création de l’application web et la génération de modèles de données basés sur vos tables de base de données.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59404518"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="25c5a-103">Tutoriel : Créer l’Application Web et les modèles de données pour EF Database First avec ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="25c5a-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="25c5a-104">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="25c5a-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="25c5a-105">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="25c5a-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="25c5a-107">Ce didacticiel se concentre sur la création de l’application web et la génération de modèles de données basés sur vos tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="25c5a-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="25c5a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="25c5a-109">Créer une application web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25c5a-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="25c5a-110">Générer les modèles</span><span class="sxs-lookup"><span data-stu-id="25c5a-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25c5a-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="25c5a-111">Prerequisites</span></span>

* [<span data-ttu-id="25c5a-112">Bien démarrer avec Entity Framework 6 Database First avec MVC 5</span><span class="sxs-lookup"><span data-stu-id="25c5a-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="25c5a-113">Créer une application web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25c5a-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="25c5a-114">Dans une nouvelle solution ou la même solution que le projet de base de données, créez un nouveau projet dans Visual Studio et sélectionnez le **Application Web ASP.NET** modèle.</span><span class="sxs-lookup"><span data-stu-id="25c5a-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="25c5a-115">Nommez le projet **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-115">Name the project **ContosoSite**.</span></span>

![créer le projet](creating-the-web-application/_static/image1.png)

<span data-ttu-id="25c5a-117">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-117">Click **OK**.</span></span>

<span data-ttu-id="25c5a-118">Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **MVC** modèle.</span><span class="sxs-lookup"><span data-stu-id="25c5a-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="25c5a-119">Vous pouvez effacer le **hôte dans le cloud** option pour l’instant, car vous allez déployer l’application vers le cloud plus tard.</span><span class="sxs-lookup"><span data-stu-id="25c5a-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="25c5a-120">Cliquez sur **OK** pour créer l’application.</span><span class="sxs-lookup"><span data-stu-id="25c5a-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="25c5a-121">Le projet est créé avec les fichiers par défaut et les dossiers.</span><span class="sxs-lookup"><span data-stu-id="25c5a-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="25c5a-122">Dans ce didacticiel, vous allez utiliser Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="25c5a-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="25c5a-123">Vous pouvez vérifier la version d’Entity Framework dans votre projet via la fenêtre Gérer les Packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="25c5a-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="25c5a-124">Si nécessaire, mettez à jour votre version d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25c5a-124">If necessary, update your version of Entity Framework.</span></span>

![Affiche la version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="25c5a-126">Générer les modèles</span><span class="sxs-lookup"><span data-stu-id="25c5a-126">Generate the models</span></span>

<span data-ttu-id="25c5a-127">Vous allez maintenant créer des modèles Entity Framework à partir des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="25c5a-128">Ces modèles sont des classes que vous allez utiliser pour travailler avec les données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="25c5a-129">Chaque modèle reflète une table dans la base de données et contient des propriétés qui correspondent aux colonnes dans la table.</span><span class="sxs-lookup"><span data-stu-id="25c5a-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="25c5a-130">Avec le bouton droit le **modèles** dossier, puis sélectionnez **ajouter** et **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="25c5a-131">Dans la fenêtre Ajouter un nouvel élément, sélectionnez **données** dans le volet gauche et **ADO.NET Entity Data Model** parmi les options dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="25c5a-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="25c5a-132">Nommez le nouveau fichier de modèle **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="25c5a-133">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-133">Click **Add**.</span></span>

<span data-ttu-id="25c5a-134">Dans l’Assistant Entity Data Model, sélectionnez **Entity Framework Designer à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="25c5a-135">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-135">Click **Next**.</span></span>

<span data-ttu-id="25c5a-136">Si vous avez des connexions de base de données définies dans votre environnement de développement, vous pouvez voir une de ces connexions présélectionnées.</span><span class="sxs-lookup"><span data-stu-id="25c5a-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="25c5a-137">Toutefois, vous souhaitez créer une nouvelle connexion à la base de données que vous avez créé dans la première partie de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="25c5a-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="25c5a-138">Cliquez sur le **nouvelle connexion** bouton.</span><span class="sxs-lookup"><span data-stu-id="25c5a-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="25c5a-139">Dans la fenêtre Propriétés de connexion, indiquez le nom du serveur local où votre base de données a été créé (dans ce cas **(localdb) \ProjectsV13**).</span><span class="sxs-lookup"><span data-stu-id="25c5a-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="25c5a-140">Après avoir entré le nom du serveur, sélectionnez le ContosoUniversityData dans les bases de données disponibles.</span><span class="sxs-lookup"><span data-stu-id="25c5a-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![définir les propriétés de connexion](creating-the-web-application/_static/image8.png)

<span data-ttu-id="25c5a-142">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-142">Click **OK**.</span></span>

<span data-ttu-id="25c5a-143">Les propriétés de connexion correctes sont maintenant affichées.</span><span class="sxs-lookup"><span data-stu-id="25c5a-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="25c5a-144">Vous pouvez utiliser le nom par défaut pour la connexion dans le fichier Web.Config.</span><span class="sxs-lookup"><span data-stu-id="25c5a-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="25c5a-145">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-145">Click **Next**.</span></span>

<span data-ttu-id="25c5a-146">Sélectionnez la dernière version d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25c5a-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="25c5a-147">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-147">Click **Next**.</span></span>

<span data-ttu-id="25c5a-148">Sélectionnez **Tables** pour générer des modèles pour les trois tables.</span><span class="sxs-lookup"><span data-stu-id="25c5a-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="25c5a-149">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="25c5a-149">Click **Finish**.</span></span>

<span data-ttu-id="25c5a-150">Si vous recevez un avertissement de sécurité, sélectionnez **OK** pour continuer l’exécution du modèle.</span><span class="sxs-lookup"><span data-stu-id="25c5a-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="25c5a-151">Les modèles sont générés à partir des tables de base de données, et un diagramme s’affiche et montre les propriétés et les relations entre les tables.</span><span class="sxs-lookup"><span data-stu-id="25c5a-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagramme du modèle](creating-the-web-application/_static/image11.png)

<span data-ttu-id="25c5a-153">Le dossier de modèles inclut désormais les nombreux nouveaux fichiers associés pour les modèles qui ont été générés à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="25c5a-154">Le **ContosoModel.Context.cs** fichier contient une classe qui dérive de la **DbContext** classe et fournit une propriété pour chaque classe de modèle qui correspond à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="25c5a-155">Le **Course.cs**, **Enrollment.cs**, et **Student.cs** fichiers contiennent les classes de modèle qui représentent les tables de bases de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="25c5a-156">Lorsque vous travaillez avec la structure, vous allez utiliser la classe de contexte et les classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="25c5a-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="25c5a-157">Avant de poursuivre ce didacticiel, générez le projet.</span><span class="sxs-lookup"><span data-stu-id="25c5a-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="25c5a-158">Dans la section suivante, vous allez générer du code basé sur les modèles de données, mais cette section ne fonctionnera pas si le projet n’a pas été créé.</span><span class="sxs-lookup"><span data-stu-id="25c5a-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25c5a-159">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="25c5a-159">Next steps</span></span>

<span data-ttu-id="25c5a-160">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="25c5a-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="25c5a-161">Créé une application web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25c5a-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="25c5a-162">Généré les modèles</span><span class="sxs-lookup"><span data-stu-id="25c5a-162">Generated the models</span></span>

<span data-ttu-id="25c5a-163">Passez au didacticiel suivant pour apprendre à créer génère du code basé sur les modèles de données.</span><span class="sxs-lookup"><span data-stu-id="25c5a-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="25c5a-164">Génération de vues</span><span class="sxs-lookup"><span data-stu-id="25c5a-164">Generating views</span></span>](generating-views.md)
