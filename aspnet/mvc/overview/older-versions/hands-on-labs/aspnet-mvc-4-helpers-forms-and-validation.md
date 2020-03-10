---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET, formulaires et validation de MVC 4 | Microsoft Docs
author: rick-anderson
description: Dans les modèles ASP.NET MVC 4 et l’atelier pratique d’accès aux données, vous avez chargé et affiché les données de la base de données. Dans ce laboratoire pratique, vous allez ajouter à...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539577"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="84265-104">Helpers, formulaires et validation d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="84265-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="84265-105">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="84265-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="84265-106">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="84265-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="84265-107">Dans les **modèles ASP.NET MVC 4 et** l’atelier pratique d’accès aux données, vous avez chargé et affiché les données de la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="84265-108">Dans ce laboratoire pratique, vous allez ajouter à l’application du **Store musical** la possibilité de modifier ces données.</span><span class="sxs-lookup"><span data-stu-id="84265-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="84265-109">Avec cet objectif à l’esprit, vous allez d’abord créer le contrôleur qui prendra en charge les actions de création, lecture, mise à jour et suppression (CRUD) des albums.</span><span class="sxs-lookup"><span data-stu-id="84265-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="84265-110">Vous allez générer un modèle d’affichage d’index en tirant parti de la fonctionnalité de génération de modèles automatique de ASP.NET MVC pour afficher les propriétés des albums dans un tableau HTML.</span><span class="sxs-lookup"><span data-stu-id="84265-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="84265-111">Pour améliorer cette vue, vous allez ajouter un programme d’assistance HTML personnalisé qui va tronquer les descriptions longues.</span><span class="sxs-lookup"><span data-stu-id="84265-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="84265-112">Ensuite, vous allez ajouter les affichages modifier et créer qui vous permettront de modifier les albums dans la base de données, à l’aide d’éléments de formulaire tels que les listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="84265-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="84265-113">Enfin, vous permettez aux utilisateurs de supprimer un album. vous les empêchera également d’entrer des données erronées en validant leur entrée.</span><span class="sxs-lookup"><span data-stu-id="84265-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="84265-114">Ce laboratoire pratique suppose que vous avez une connaissance de base de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="84265-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="84265-115">Si vous n’avez pas encore utilisé **ASP.NET MVC** , nous vous recommandons de passer au-dessus d’un laboratoire pratique de **notions de base sur ASP.NET MVC** .</span><span class="sxs-lookup"><span data-stu-id="84265-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="84265-116">Ce laboratoire vous guide tout au long des améliorations et des nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web fournie dans le dossier source.</span><span class="sxs-lookup"><span data-stu-id="84265-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="84265-117">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="84265-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="84265-118">Le projet propre à ce laboratoire est disponible à l’adresse [ASP.net, formulaires et validation de MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="84265-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="84265-119">Objectifs</span><span class="sxs-lookup"><span data-stu-id="84265-119">Objectives</span></span>

<span data-ttu-id="84265-120">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="84265-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="84265-121">Créer un contrôleur pour prendre en charge les opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="84265-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="84265-122">Générer une vue d’index pour afficher les propriétés d’entité dans une table HTML</span><span class="sxs-lookup"><span data-stu-id="84265-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="84265-123">Ajouter un programme d’assistance HTML personnalisé</span><span class="sxs-lookup"><span data-stu-id="84265-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="84265-124">Créer et personnaliser un affichage de modification</span><span class="sxs-lookup"><span data-stu-id="84265-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="84265-125">Faire la différence entre les méthodes d’action qui réagissent à des appels HTTP-is ou HTTP-POSTCONNEXION</span><span class="sxs-lookup"><span data-stu-id="84265-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="84265-126">Ajouter et personnaliser une vue Create</span><span class="sxs-lookup"><span data-stu-id="84265-126">Add and customize a Create View</span></span>
- <span data-ttu-id="84265-127">Gérer la suppression d’une entité</span><span class="sxs-lookup"><span data-stu-id="84265-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="84265-128">Valider les entrées utilisateur</span><span class="sxs-lookup"><span data-stu-id="84265-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="84265-129">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="84265-129">Prerequisites</span></span>

<span data-ttu-id="84265-130">Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="84265-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="84265-131">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).</span><span class="sxs-lookup"><span data-stu-id="84265-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="84265-132">Installation</span><span class="sxs-lookup"><span data-stu-id="84265-132">Setup</span></span>

<span data-ttu-id="84265-133">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="84265-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="84265-134">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84265-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="84265-135">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="84265-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="84265-136">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe B : utilisation d’extraits de Code](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="84265-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="84265-137">Exercices</span><span class="sxs-lookup"><span data-stu-id="84265-137">Exercises</span></span>

<span data-ttu-id="84265-138">Les exercices suivants composent ce laboratoire pratique :</span><span class="sxs-lookup"><span data-stu-id="84265-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="84265-139">Création du contrôleur Store Manager et de sa vue index</span><span class="sxs-lookup"><span data-stu-id="84265-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="84265-140">Ajout d’un programme d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="84265-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="84265-141">Création de la vue Edit</span><span class="sxs-lookup"><span data-stu-id="84265-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="84265-142">Ajout d’une vue Create</span><span class="sxs-lookup"><span data-stu-id="84265-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="84265-143">Gestion de la suppression</span><span class="sxs-lookup"><span data-stu-id="84265-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="84265-144">Ajout de la validation</span><span class="sxs-lookup"><span data-stu-id="84265-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="84265-145">Utilisation de jQuery discrète côté client</span><span class="sxs-lookup"><span data-stu-id="84265-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="84265-146">Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="84265-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="84265-147">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.</span><span class="sxs-lookup"><span data-stu-id="84265-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="84265-148">Durée estimée pour effectuer ce laboratoire : **60 minutes**</span><span class="sxs-lookup"><span data-stu-id="84265-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="84265-149">Exercice 1 : création du contrôleur Store Manager et de sa vue index</span><span class="sxs-lookup"><span data-stu-id="84265-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="84265-150">Dans cet exercice, vous allez apprendre à créer un nouveau contrôleur pour prendre en charge les opérations CRUD, à personnaliser sa méthode d’action d’index pour retourner une liste d’albums de la base de données et enfin à générer un modèle de vue d’index tirant parti de la génération de modèles automatique de ASP.NET MVC. pour afficher les propriétés des albums dans un tableau HTML.</span><span class="sxs-lookup"><span data-stu-id="84265-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="84265-151">Tâche 1 : création de la StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="84265-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="84265-152">Dans cette tâche, vous allez créer un nouveau contrôleur appelé **StoreManagerController** pour prendre en charge les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="84265-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="84265-153">Ouvrez le **début** de la solution situé à l’emplacement **source/EX1-CreatingTheStoreManagerController/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="84265-154">Vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-155">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-156">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-157">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-158">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-159">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-160">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-161">Ajoutez un nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="84265-161">Add a new controller.</span></span> <span data-ttu-id="84265-162">Pour ce faire, cliquez avec le bouton droit sur le dossier **Controllers** dans le Explorateur de solutions, sélectionnez **Ajouter** , puis la commande **contrôleur** .</span><span class="sxs-lookup"><span data-stu-id="84265-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="84265-163">Remplacez le **nom** du contrôleur par **StoreManagerController** et assurez-vous que l’option **contrôleur MVC avec actions de lecture/écriture vides** est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="84265-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="84265-164">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="84265-164">Click **Add**.</span></span>

    <span data-ttu-id="84265-165">![Boîte de dialogue Ajouter un contrôleur](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Boîte de dialogue Ajouter un contrôleur")</span><span class="sxs-lookup"><span data-stu-id="84265-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="84265-166">*Boîte de dialogue Ajouter un contrôleur*</span><span class="sxs-lookup"><span data-stu-id="84265-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="84265-167">Une nouvelle classe de contrôleur est générée.</span><span class="sxs-lookup"><span data-stu-id="84265-167">A new Controller class is generated.</span></span> <span data-ttu-id="84265-168">Étant donné que vous avez indiqué d’ajouter des actions en lecture/écriture, des méthodes stub pour celles-ci, les actions CRUD courantes sont créées avec des commentaires TODO remplis, invitant à inclure la logique spécifique de l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="84265-169">Tâche 2 : personnalisation de l’index StoreManager</span><span class="sxs-lookup"><span data-stu-id="84265-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="84265-170">Dans cette tâche, vous allez personnaliser la méthode d’action d’index StoreManager pour retourner une vue avec la liste des albums de la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="84265-171">Dans la classe StoreManagerController, ajoutez les directives *using* suivantes.</span><span class="sxs-lookup"><span data-stu-id="84265-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="84265-172">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX1 using MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="84265-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="84265-173">Ajoutez un champ à **StoreManagerController** pour contenir une instance de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="84265-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="84265-174">(Extrait de code- *ASP.net et Forms de l’aide de MVC 4-EX1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="84265-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="84265-175">Implémentez l’action d’index StoreManagerController pour retourner une vue avec la liste des albums.</span><span class="sxs-lookup"><span data-stu-id="84265-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="84265-176">La logique d’action du contrôleur sera très similaire à l’action d’index de StoreController écrite précédemment.</span><span class="sxs-lookup"><span data-stu-id="84265-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="84265-177">Utilisez LINQ pour récupérer tous les albums, y compris les informations relatives au genre et à l’artiste à afficher.</span><span class="sxs-lookup"><span data-stu-id="84265-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="84265-178">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX1 StoreManagerController index*)</span><span class="sxs-lookup"><span data-stu-id="84265-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="84265-179">Tâche 3 : création de la vue index</span><span class="sxs-lookup"><span data-stu-id="84265-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="84265-180">Dans cette tâche, vous allez créer le modèle de vue d’index pour afficher la liste des albums retournés par le contrôleur **StoreManager** .</span><span class="sxs-lookup"><span data-stu-id="84265-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="84265-181">Avant de créer le modèle de vue, vous devez générer le projet afin que la **boîte de dialogue Ajouter une vue** sache la classe d' **album** à utiliser.</span><span class="sxs-lookup"><span data-stu-id="84265-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="84265-182">Sélectionner une **Build | Générez MvcMusicStore** pour générer le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="84265-183">Cliquez avec le bouton droit à l’intérieur de la méthode d’action **index** et sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="84265-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="84265-184">La boîte de dialogue **Ajouter une vue s’affiche** .</span><span class="sxs-lookup"><span data-stu-id="84265-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="84265-185">![Ajouter une vue](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ajoute une vue")</span><span class="sxs-lookup"><span data-stu-id="84265-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="84265-186">*Ajout d’une vue à partir de la méthode index*</span><span class="sxs-lookup"><span data-stu-id="84265-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="84265-187">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **index**.</span><span class="sxs-lookup"><span data-stu-id="84265-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="84265-188">Sélectionnez l’option **créer un affichage fortement typé** , puis sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **classe de modèle** .</span><span class="sxs-lookup"><span data-stu-id="84265-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="84265-189">Sélectionnez **liste** dans la liste déroulante **modèle de structure** .</span><span class="sxs-lookup"><span data-stu-id="84265-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="84265-190">Laissez le **moteur** d’affichage **Razor** et les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="84265-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="84265-191">![Ajout d’une vue d’index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Ajout d’une vue d’index")</span><span class="sxs-lookup"><span data-stu-id="84265-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="84265-192">*Ajout d’une vue d’index*</span><span class="sxs-lookup"><span data-stu-id="84265-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="84265-193">Tâche 4 : personnalisation de la structure de la vue index</span><span class="sxs-lookup"><span data-stu-id="84265-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="84265-194">Dans cette tâche, vous allez ajuster le modèle d’affichage simple créé avec la fonctionnalité de génération de modèles automatique ASP.NET MVC pour qu’il affiche les champs souhaités.</span><span class="sxs-lookup"><span data-stu-id="84265-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="84265-195">La prise en charge de la **génération de modèles** automatique dans ASP.NET MVC génère un modèle de vue simple qui répertorie tous les champs du modèle d’album.</span><span class="sxs-lookup"><span data-stu-id="84265-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="84265-196">La **génération de modèles** automatique offre un moyen rapide de commencer à utiliser une vue fortement typée : au lieu d’avoir à écrire le modèle de vue manuellement, la génération de modèles automatique génère rapidement un modèle par défaut, puis vous pouvez modifier le code généré.</span><span class="sxs-lookup"><span data-stu-id="84265-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>

1. <span data-ttu-id="84265-197">Passez en revue le code créé.</span><span class="sxs-lookup"><span data-stu-id="84265-197">Review the code created.</span></span> <span data-ttu-id="84265-198">La liste de champs générée fait partie du tableau HTML suivant que la **génération de modèles** automatique utilise pour afficher des données tabulaires.</span><span class="sxs-lookup"><span data-stu-id="84265-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="84265-199">Remplacez la **table&lt;&gt;** code par le code suivant pour afficher uniquement les champs **genre**, **artiste**, titre de l' **album**et **prix** .</span><span class="sxs-lookup"><span data-stu-id="84265-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="84265-200">Cela supprime les colonnes **AlbumId** et de l’URL de la pochette de l' **album** .</span><span class="sxs-lookup"><span data-stu-id="84265-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="84265-201">En outre, il modifie les colonnes GenreId et ArtistId pour afficher les propriétés de classe liées de **Artist.Name** et **genre.Name**, et supprime le lien **Détails** .</span><span class="sxs-lookup"><span data-stu-id="84265-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="84265-202">Modifiez les descriptions suivantes.</span><span class="sxs-lookup"><span data-stu-id="84265-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="84265-203">Tâche 5 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="84265-204">Dans cette tâche, vous allez tester que le modèle de vue d' **index** **StoreManager** affiche une liste d’albums en fonction de la conception des étapes précédentes.</span><span class="sxs-lookup"><span data-stu-id="84265-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="84265-205">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-206">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-206">The project starts in the Home page.</span></span> <span data-ttu-id="84265-207">Remplacez l’URL par **/StoreManager** pour vérifier qu’une liste d’albums s’affiche, en affichant son **titre**, son **artiste** et son **genre**.</span><span class="sxs-lookup"><span data-stu-id="84265-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="84265-208">![Exploration de la liste des albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Exploration de la liste des albums")</span><span class="sxs-lookup"><span data-stu-id="84265-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="84265-209">*Exploration de la liste des albums*</span><span class="sxs-lookup"><span data-stu-id="84265-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="84265-210">Exercice 2 : ajout d’un programme d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="84265-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="84265-211">La page d’index StoreManager présente un problème potentiel : les propriétés Title et Artist Name peuvent être suffisamment longues pour lever la mise en forme de la table.</span><span class="sxs-lookup"><span data-stu-id="84265-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="84265-212">Dans cet exercice, vous allez apprendre à ajouter un programme d’assistance HTML personnalisé pour tronquer ce texte.</span><span class="sxs-lookup"><span data-stu-id="84265-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="84265-213">Dans l’illustration suivante, vous pouvez voir comment le format est modifié en raison de la longueur du texte lorsque vous utilisez une petite taille de navigateur.</span><span class="sxs-lookup"><span data-stu-id="84265-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="84265-214">![Exploration de la liste des albums avec du texte non tronqué](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Exploration de la liste des albums avec du texte non tronqué")</span><span class="sxs-lookup"><span data-stu-id="84265-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="84265-215">*Exploration de la liste des albums avec du texte non tronqué*</span><span class="sxs-lookup"><span data-stu-id="84265-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="84265-216">Tâche 1 : extension du programme d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="84265-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="84265-217">Dans cette tâche, vous allez ajouter une nouvelle méthode **tronquer** à l’objet **HTML** exposé dans les vues ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="84265-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="84265-218">Pour ce faire, vous allez implémenter une **méthode d’extension** à la classe **System. Web. Mvc. HtmlHelper** intégrée fournie par ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="84265-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="84265-219">Pour en savoir plus sur les **méthodes d’extension**, consultez cet article MSDN.</span><span class="sxs-lookup"><span data-stu-id="84265-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="84265-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="84265-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>

1. <span data-ttu-id="84265-221">Ouvrez le **début** de la solution situé dans **source/EX2-AddingAnHTMLHelper/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="84265-222">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="84265-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="84265-223">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-224">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-225">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-226">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-227">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-228">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-229">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-230">Ouvrez la vue d’index de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="84265-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="84265-231">Pour ce faire, dans le Explorateur de solutions développez le dossier **views** , puis le **StoreManager** et ouvrez le fichier **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="84265-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="84265-232">Ajoutez le code suivant sous la directive <strong>@model</strong> pour définir la méthode d’assistance <strong>TRUNCATE</strong> .</span><span class="sxs-lookup"><span data-stu-id="84265-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="84265-233">Tâche 2 : troncation du texte dans la page</span><span class="sxs-lookup"><span data-stu-id="84265-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="84265-234">Dans cette tâche, vous allez utiliser la méthode **TRUNCATE** pour tronquer le texte dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="84265-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="84265-235">Ouvrez la vue d’index de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="84265-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="84265-236">Pour ce faire, dans le Explorateur de solutions développez le dossier **views** , puis le **StoreManager** et ouvrez le fichier **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="84265-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="84265-237">Remplacez les lignes qui affichent le nom de l' **artiste** et le **titre**de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="84265-238">Pour ce faire, remplacez les lignes suivantes.</span><span class="sxs-lookup"><span data-stu-id="84265-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="84265-239">Tâche 3 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="84265-240">Dans cette tâche, vous allez vérifier que le modèle de vue d' **index** **StoreManager** tronque le titre et le nom de l’artiste de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="84265-241">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-242">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-242">The project starts in the Home page.</span></span> <span data-ttu-id="84265-243">Remplacez l’URL par **/StoreManager** pour vérifier que les longs textes de la colonne **title** et **Artist** sont tronqués.</span><span class="sxs-lookup"><span data-stu-id="84265-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="84265-244">![Noms des titres et artistes tronqués](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Noms des titres et artistes tronqués")</span><span class="sxs-lookup"><span data-stu-id="84265-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="84265-245">*Titres et noms d’artistes tronqués*</span><span class="sxs-lookup"><span data-stu-id="84265-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="84265-246">Exercice 3 : création de la vue Edit</span><span class="sxs-lookup"><span data-stu-id="84265-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="84265-247">Dans cet exercice, vous allez apprendre à créer un formulaire pour permettre aux responsables du magasin de modifier un album.</span><span class="sxs-lookup"><span data-stu-id="84265-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="84265-248">Ils parcourent l’URL **/StoreManager/Edit/ID** (**ID** étant l’ID unique de l’album à modifier), en effectuant un appel http-to sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="84265-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="84265-249">La méthode d’action de modification du contrôleur récupère l’album approprié à partir de la base de données, crée un objet **StoreManagerViewModel** pour l’encapsuler (avec une liste d’artistes et de genres), puis le transmet à un modèle de vue pour restituer la page HTML à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="84265-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="84265-250">Cette page contient une **&lt;formulaire&gt;** élément avec des zones de texte et des listes déroulantes pour modifier les propriétés de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="84265-251">Une fois que l’utilisateur a mis à jour les valeurs de formulaire de l’album et clique sur le bouton **Enregistrer** , les modifications sont soumises via un rappel http-postconnexion à **/StoreManager/Edit/ID**. Bien que l’URL reste la même que dans le dernier appel, ASP.NET MVC identifie cette fois qu’il s’agit d’un HTTP-HTTP et exécute donc une méthode d’action de modification différente (une décorée avec **[HttpPost]** ).</span><span class="sxs-lookup"><span data-stu-id="84265-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="84265-252">Tâche 1 : implémentation de la méthode d’action de modification HTTP-obtenue</span><span class="sxs-lookup"><span data-stu-id="84265-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="84265-253">Dans cette tâche, vous allez implémenter la version HTTP-obtenir de la méthode Edit action pour récupérer l’album approprié de la base de données, ainsi qu’une liste de tous les genres et artistes.</span><span class="sxs-lookup"><span data-stu-id="84265-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="84265-254">Il empaquette ces données dans l’objet **StoreManagerViewModel** défini à la dernière étape, qui est ensuite transmis à un modèle de vue pour afficher la réponse.</span><span class="sxs-lookup"><span data-stu-id="84265-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="84265-255">Ouvrez le **début** de la solution situé dans **source/EX3-CreatingTheEditView/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="84265-256">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="84265-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="84265-257">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-258">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-259">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-260">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-261">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-262">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-263">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-264">Ouvrez la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="84265-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="84265-265">Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="84265-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="84265-266">Remplacez la méthode d’action **http-obtenir Edit** par le code suivant pour récupérer l' **album** approprié ainsi que les listes **genres** et **artistes** .</span><span class="sxs-lookup"><span data-stu-id="84265-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="84265-267">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX3 STOREMANAGERCONTROLLER http-obtenir Edit action*)</span><span class="sxs-lookup"><span data-stu-id="84265-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="84265-268">Vous utilisez **System. Web. Mvc** **SelectList** pour les artistes et les genres à la place de la liste **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="84265-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="84265-269">**SelectList** est un moyen plus clair de remplir les listes déroulantes html et de gérer des éléments tels que la sélection actuelle.</span><span class="sxs-lookup"><span data-stu-id="84265-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="84265-270">L’instanciation et la configuration ultérieure de ces objets ViewModel dans l’action du contrôleur feront du scénario de modification de formulaire.</span><span class="sxs-lookup"><span data-stu-id="84265-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="84265-271">Tâche 2 : création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="84265-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="84265-272">Dans cette tâche, vous allez créer un modèle de vue de modification qui affichera ultérieurement les propriétés de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="84265-273">Créez la vue Edit (édition).</span><span class="sxs-lookup"><span data-stu-id="84265-273">Create the Edit View.</span></span> <span data-ttu-id="84265-274">Pour ce faire, cliquez avec le bouton droit dans la méthode d’action **modifier** , puis sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="84265-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="84265-275">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **modifier**.</span><span class="sxs-lookup"><span data-stu-id="84265-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="84265-276">Cochez la case **créer une vue fortement typée** et sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **afficher la classe de données** .</span><span class="sxs-lookup"><span data-stu-id="84265-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="84265-277">Sélectionnez **modifier** dans la liste déroulante **modèle de structure** .</span><span class="sxs-lookup"><span data-stu-id="84265-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="84265-278">Laissez les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="84265-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="84265-279">![Ajout d’une vue Edit](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Ajout d’une vue Edit")</span><span class="sxs-lookup"><span data-stu-id="84265-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="84265-280">*Ajout d’une vue Edit*</span><span class="sxs-lookup"><span data-stu-id="84265-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="84265-281">Tâche 3 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="84265-282">Dans cette tâche, vous allez tester que la page de la vue **Edit** **StoreManager** affiche les valeurs des propriétés pour l’album passé comme paramètre.</span><span class="sxs-lookup"><span data-stu-id="84265-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="84265-283">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-284">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-284">The project starts in the Home page.</span></span> <span data-ttu-id="84265-285">Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier que les valeurs des propriétés de l’album sont affichées.</span><span class="sxs-lookup"><span data-stu-id="84265-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="84265-286">![Exploration de l’affichage des modifications de l’album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Exploration de l’affichage des modifications de l’album")</span><span class="sxs-lookup"><span data-stu-id="84265-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="84265-287">*Exploration de l’affichage des modifications de l’album*</span><span class="sxs-lookup"><span data-stu-id="84265-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="84265-288">Tâche 4 : implémentation de listes déroulantes sur le modèle éditeur d’albums</span><span class="sxs-lookup"><span data-stu-id="84265-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="84265-289">Dans cette tâche, vous allez ajouter des listes déroulantes au modèle de vue créé dans la dernière tâche, afin que l’utilisateur puisse sélectionner dans une liste d’artistes et de genres.</span><span class="sxs-lookup"><span data-stu-id="84265-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="84265-290">Remplacez tout le code du jeu de champs de l' **album** par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="84265-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="84265-291">Un programme d’assistance **html. DropDownList** a été ajouté pour afficher les listes déroulantes permettant de choisir des artistes et des genres.</span><span class="sxs-lookup"><span data-stu-id="84265-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="84265-292">Les paramètres passés à **html. DropDownList** sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="84265-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="84265-293">Nom du champ de formulaire ( **&quot;ArtistId&quot;** ).</span><span class="sxs-lookup"><span data-stu-id="84265-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="84265-294">**SelectList** de valeurs de la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="84265-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="84265-295">Tâche 5 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="84265-296">Dans cette tâche, vous allez tester que la page de la vue **Edit** **StoreManager** affiche des listes déroulantes à la place des champs de texte artiste et genre ID.</span><span class="sxs-lookup"><span data-stu-id="84265-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="84265-297">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-298">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-298">The project starts in the Home page.</span></span> <span data-ttu-id="84265-299">Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier qu’elle affiche les listes déroulantes à la place des champs de texte artiste et genre ID.</span><span class="sxs-lookup"><span data-stu-id="84265-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="84265-300">![Exploration de l’affichage des modifications de l’album avec des listes déroulantes](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Exploration de l’affichage des modifications de l’album avec des listes déroulantes")</span><span class="sxs-lookup"><span data-stu-id="84265-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="84265-301">*Exploration de l’affichage des modifications de l’album, cette fois avec des listes déroulantes*</span><span class="sxs-lookup"><span data-stu-id="84265-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="84265-302">Tâche 6 : implémentation de la méthode d’action HTTP-après modification</span><span class="sxs-lookup"><span data-stu-id="84265-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="84265-303">Maintenant que la vue Edit s’affiche comme prévu, vous devez implémenter la méthode d’action HTTP-après modification pour enregistrer les modifications apportées à l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="84265-304">Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84265-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="84265-305">Ouvrez **StoreManagerController** dans le dossier **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="84265-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="84265-306">Remplacez le code de méthode d’action **http-postérieur** à la modification par le code suivant (Notez que la méthode qui doit être remplacée est une version surchargée qui reçoit deux paramètres) :</span><span class="sxs-lookup"><span data-stu-id="84265-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="84265-307">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX3 STOREMANAGERCONTROLLER http-après modification*)</span><span class="sxs-lookup"><span data-stu-id="84265-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="84265-308">Cette méthode est exécutée lorsque l’utilisateur clique sur le bouton **Enregistrer** de la vue et effectue une opération http-poster des valeurs de formulaire sur le serveur pour les rendre persistantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="84265-309">L’élément Decorator **[HttpPost]** indique que la méthode doit être utilisée pour ces scénarios http-poster.</span><span class="sxs-lookup"><span data-stu-id="84265-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="84265-310">La méthode prend un objet **album** .</span><span class="sxs-lookup"><span data-stu-id="84265-310">The method takes an **Album** object.</span></span> <span data-ttu-id="84265-311">ASP.NET MVC crée automatiquement l’objet album à partir des valeurs de&gt; de formulaire &lt;publiées.</span><span class="sxs-lookup"><span data-stu-id="84265-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="84265-312">La méthode effectue les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="84265-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="84265-313">Si le modèle est valide :</span><span class="sxs-lookup"><span data-stu-id="84265-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="84265-314">Mettez à jour l’entrée de l’album dans le contexte pour la marquer comme un objet modifié.</span><span class="sxs-lookup"><span data-stu-id="84265-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="84265-315">Enregistrez les modifications et redirigez vers la vue index.</span><span class="sxs-lookup"><span data-stu-id="84265-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="84265-316">Si le modèle n’est pas valide, il remplit le ViewBag avec **GenreId** et **ArtistId**, puis il retourne la vue avec l’objet album reçu pour permettre à l’utilisateur d’effectuer toutes les mises à jour requises.</span><span class="sxs-lookup"><span data-stu-id="84265-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="84265-317">Tâche 7 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="84265-318">Au cours de cette tâche, vous allez tester que la page de la vue de **modification StoreManager** enregistre en fait les données d’album mises à jour dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="84265-319">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-320">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-320">The project starts in the Home page.</span></span> <span data-ttu-id="84265-321">Remplacez l’URL par **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="84265-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="84265-322">Remplacez le titre de l’album par **charger** , puis cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="84265-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="84265-323">Vérifiez que le titre de l’album a effectivement été modifié dans la liste des albums.</span><span class="sxs-lookup"><span data-stu-id="84265-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="84265-324">![Mise à jour d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Mise à jour d’un album")</span><span class="sxs-lookup"><span data-stu-id="84265-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="84265-325">*Mise à jour d’un album*</span><span class="sxs-lookup"><span data-stu-id="84265-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="84265-326">Exercice 4 : ajout d’une vue Create</span><span class="sxs-lookup"><span data-stu-id="84265-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="84265-327">Maintenant que **StoreManagerController** prend en charge la fonctionnalité de **modification** , dans cet exercice, vous allez apprendre à ajouter un modèle de vue Create pour permettre aux responsables du magasin d’ajouter de nouveaux albums à l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="84265-328">Comme vous l’avez fait avec la fonctionnalité d’édition, vous allez implémenter le scénario de création à l’aide de deux méthodes distinctes au sein de la classe **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="84265-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="84265-329">Une méthode d’action affichera un formulaire vide lorsque les responsables des boutiques accèdent d’abord à l’URL **/StoreManager/Create** .</span><span class="sxs-lookup"><span data-stu-id="84265-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="84265-330">Une deuxième méthode d’action gère le scénario dans lequel le responsable du magasin clique sur le bouton **Enregistrer** dans le formulaire et renvoie les valeurs à l’URL **/StoreManager/Create** sous la forme d’une requête http-http.</span><span class="sxs-lookup"><span data-stu-id="84265-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="84265-331">Tâche 1 : implémentation de la méthode d’action de création HTTP-obtien</span><span class="sxs-lookup"><span data-stu-id="84265-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="84265-332">Dans cette tâche, vous allez implémenter la version HTTP-obtenir de la méthode Create action pour récupérer une liste de tous les genres et artistes, empaquetez ces données dans un objet **StoreManagerViewModel** , qui sera ensuite passé à un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="84265-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="84265-333">Ouvrez le **début** de la solution situé dans **source/EX4-AddingACreateView/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="84265-334">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="84265-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="84265-335">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-336">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-337">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-338">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-339">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-340">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-341">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-342">Ouvrez la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="84265-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="84265-343">Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="84265-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="84265-344">Remplacez le code de la méthode de **création** d’action par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="84265-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="84265-345">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX4 STOREMANAGERCONTROLLER http-obtenir Create action*)</span><span class="sxs-lookup"><span data-stu-id="84265-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="84265-346">Tâche 2 : ajout de la vue Create</span><span class="sxs-lookup"><span data-stu-id="84265-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="84265-347">Au cours de cette tâche, vous allez ajouter le modèle de vue créer qui affichera un nouveau formulaire d’album (vide).</span><span class="sxs-lookup"><span data-stu-id="84265-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="84265-348">Cliquez avec le bouton droit à l’intérieur de la méthode **Create** action et sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="84265-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="84265-349">La boîte de dialogue Ajouter une vue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="84265-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="84265-350">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **créer**.</span><span class="sxs-lookup"><span data-stu-id="84265-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="84265-351">Sélectionnez l’option **créer un affichage fortement typé** et sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **classe de modèle** , puis **créez** à partir de la liste déroulante **modèle de structure** .</span><span class="sxs-lookup"><span data-stu-id="84265-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="84265-352">Laissez les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="84265-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="84265-353">![Ajout d’une vue Create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-Create-View. png")</span><span class="sxs-lookup"><span data-stu-id="84265-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="84265-354">*Ajout de la vue Create*</span><span class="sxs-lookup"><span data-stu-id="84265-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="84265-355">Mettez à jour les champs **GenreId** et **ArtistId** pour utiliser une liste déroulante comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="84265-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="84265-356">Tâche 3 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="84265-357">Dans cette tâche, vous allez tester que la page **créer** un affichage de **StoreManager** affiche un formulaire d’album vide.</span><span class="sxs-lookup"><span data-stu-id="84265-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="84265-358">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-359">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-359">The project starts in the Home page.</span></span> <span data-ttu-id="84265-360">Remplacez l’URL par **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="84265-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="84265-361">Vérifiez qu’un formulaire vide s’affiche pour remplir les nouvelles propriétés de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="84265-362">![Créer une vue avec un formulaire vide](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Créer une vue avec un formulaire vide")</span><span class="sxs-lookup"><span data-stu-id="84265-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="84265-363">*Créer une vue avec un formulaire vide*</span><span class="sxs-lookup"><span data-stu-id="84265-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="84265-364">Tâche 4 : implémentation de la méthode d’action HTTP-Après création</span><span class="sxs-lookup"><span data-stu-id="84265-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="84265-365">Dans cette tâche, vous allez implémenter la version HTTP-POSTÉRIEURe de la méthode Create action qui sera appelée lorsqu’un utilisateur clique sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="84265-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="84265-366">La méthode doit enregistrer le nouvel album dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="84265-367">Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84265-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="84265-368">Ouvrez la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="84265-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="84265-369">Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="84265-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="84265-370">Remplacez le code de la méthode d’action **http-après création** par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="84265-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="84265-371">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX4 STOREMANAGERCONTROLLER http-après Create action*)</span><span class="sxs-lookup"><span data-stu-id="84265-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="84265-372">L’action Create est assez similaire à la méthode d’action Edit précédente, mais au lieu de définir l’objet comme modifié, elle est ajoutée au contexte.</span><span class="sxs-lookup"><span data-stu-id="84265-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="84265-373">Tâche 5 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="84265-374">Dans cette tâche, vous allez tester que la page créer un affichage de **StoreManager** vous permet de créer un nouvel album, puis de rediriger vers la vue index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="84265-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="84265-375">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-376">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-376">The project starts in the Home page.</span></span> <span data-ttu-id="84265-377">Remplacez l’URL par **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="84265-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="84265-378">Remplissez tous les champs de formulaire avec les données d’un nouvel album, comme dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="84265-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="84265-379">![Création d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Création d’un album")</span><span class="sxs-lookup"><span data-stu-id="84265-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="84265-380">*Création d’un album*</span><span class="sxs-lookup"><span data-stu-id="84265-380">*Creating an Album*</span></span>
3. <span data-ttu-id="84265-381">Vérifiez que vous êtes redirigé vers la vue d’index StoreManager qui comprend le nouvel album que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="84265-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="84265-382">![Nouvel album créé](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nouvel album créé")</span><span class="sxs-lookup"><span data-stu-id="84265-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="84265-383">*Nouvel album créé*</span><span class="sxs-lookup"><span data-stu-id="84265-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="84265-384">Exercice 5 : gestion de la suppression</span><span class="sxs-lookup"><span data-stu-id="84265-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="84265-385">La possibilité de supprimer des albums n’est pas encore implémentée.</span><span class="sxs-lookup"><span data-stu-id="84265-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="84265-386">C’est ce que cet exercice va vous présenter.</span><span class="sxs-lookup"><span data-stu-id="84265-386">This is what this exercise will be about.</span></span> <span data-ttu-id="84265-387">Comme précédemment, vous allez implémenter le scénario de suppression à l’aide de deux méthodes distinctes au sein de la classe **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="84265-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="84265-388">Une méthode d’action affichera un formulaire de confirmation</span><span class="sxs-lookup"><span data-stu-id="84265-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="84265-389">Une deuxième méthode d’action prend en charge l’envoi de formulaire.</span><span class="sxs-lookup"><span data-stu-id="84265-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="84265-390">Tâche 1 : implémentation de la méthode d’action HTTP-récupérer la suppression</span><span class="sxs-lookup"><span data-stu-id="84265-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="84265-391">Dans cette tâche, vous allez implémenter la version HTTP-obtenir de la méthode d’action de suppression pour récupérer les informations de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="84265-392">Ouvrez le **début** de la solution situé dans **source/EX5-HandlingDeletion/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="84265-393">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="84265-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="84265-394">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-395">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-396">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-397">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-398">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-399">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-400">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-401">Ouvrez la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="84265-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="84265-402">Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="84265-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="84265-403">L’action supprimer le contrôleur est exactement la même que l’action précédente du contrôleur des détails du magasin : elle interroge l’objet **album** à partir de la base de données à l’aide de l' **ID** fourni dans l’URL et retourne la **vue**appropriée.</span><span class="sxs-lookup"><span data-stu-id="84265-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="84265-404">Pour ce faire, remplacez le code de la méthode d’action de **suppression** http-do par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="84265-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="84265-405">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX5 Handling Delete http-obtenir Delete action*)</span><span class="sxs-lookup"><span data-stu-id="84265-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="84265-406">Cliquez avec le bouton droit dans la méthode d’action de **suppression** , puis sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="84265-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="84265-407">La boîte de dialogue Ajouter une vue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="84265-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="84265-408">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="84265-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="84265-409">Sélectionnez l’option **créer un affichage fortement typé** et sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **classe de modèle** .</span><span class="sxs-lookup"><span data-stu-id="84265-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="84265-410">Sélectionnez **supprimer** dans la liste déroulante **modèle de structure** .</span><span class="sxs-lookup"><span data-stu-id="84265-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="84265-411">Laissez les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="84265-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="84265-412">![Ajout d’une vue Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Ajout d’une vue Delete")</span><span class="sxs-lookup"><span data-stu-id="84265-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="84265-413">*Ajout d’une vue Delete*</span><span class="sxs-lookup"><span data-stu-id="84265-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="84265-414">Le modèle supprimer affiche tous les champs du modèle.</span><span class="sxs-lookup"><span data-stu-id="84265-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="84265-415">Vous n’afficherez que le titre de l’album.</span><span class="sxs-lookup"><span data-stu-id="84265-415">You will show only the album's title.</span></span> <span data-ttu-id="84265-416">Pour ce faire, remplacez le contenu de la vue par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="84265-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="84265-417">Tâche 2 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="84265-418">Au cours de cette tâche, vous allez vérifier que la page **supprimer** la vue **StoreManager** affiche un formulaire de suppression de confirmation.</span><span class="sxs-lookup"><span data-stu-id="84265-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="84265-419">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-420">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-420">The project starts in the Home page.</span></span> <span data-ttu-id="84265-421">Remplacez l’URL par **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="84265-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="84265-422">Sélectionnez un album à supprimer en cliquant sur **supprimer** et vérifiez que la nouvelle vue est chargée.</span><span class="sxs-lookup"><span data-stu-id="84265-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="84265-423">![Suppression d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Suppression d’un album")</span><span class="sxs-lookup"><span data-stu-id="84265-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="84265-424">*Suppression d’un album*</span><span class="sxs-lookup"><span data-stu-id="84265-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="84265-425">Tâche 3 : implémentation de la méthode d’action HTTP-après suppression</span><span class="sxs-lookup"><span data-stu-id="84265-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="84265-426">Dans cette tâche, vous allez implémenter la version HTTP-POSTÉRIEURe de la méthode d’action de suppression qui sera appelée lorsqu’un utilisateur cliquera sur le bouton **supprimer** .</span><span class="sxs-lookup"><span data-stu-id="84265-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="84265-427">La méthode doit supprimer l’album dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="84265-428">Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84265-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="84265-429">Ouvrez la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="84265-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="84265-430">Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="84265-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="84265-431">Remplacez le code de la méthode d’action **http-après suppression** par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="84265-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="84265-432">(Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX5 Handling Delete http-après suppression*)</span><span class="sxs-lookup"><span data-stu-id="84265-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="84265-433">Tâche 4 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="84265-434">Dans cette tâche, vous allez tester que la page **StoreManager supprimer** la vue vous permet de supprimer un album, puis de le rediriger vers la vue index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="84265-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="84265-435">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-436">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-436">The project starts in the Home page.</span></span> <span data-ttu-id="84265-437">Remplacez l’URL par **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="84265-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="84265-438">Sélectionnez un album à supprimer en cliquant sur **Supprimer.**</span><span class="sxs-lookup"><span data-stu-id="84265-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="84265-439">Confirmez la suppression en cliquant sur le bouton **supprimer** :</span><span class="sxs-lookup"><span data-stu-id="84265-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="84265-440">![Suppression d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Suppression d’un album")</span><span class="sxs-lookup"><span data-stu-id="84265-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="84265-441">*Suppression d’un album*</span><span class="sxs-lookup"><span data-stu-id="84265-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="84265-442">Vérifiez que l’album a été supprimé, car il n’apparaît pas dans la page **index** .</span><span class="sxs-lookup"><span data-stu-id="84265-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="84265-443">Exercice 6 : ajout de la validation</span><span class="sxs-lookup"><span data-stu-id="84265-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="84265-444">Actuellement, les formulaires de création et de modification que vous avez en place n’effectuent aucun type de validation.</span><span class="sxs-lookup"><span data-stu-id="84265-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="84265-445">Si l’utilisateur laisse un champ obligatoire vide ou que vous tapez des lettres dans le champ Price, la première erreur que vous obtiendrez provient de la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="84265-446">Vous pouvez ajouter la validation à l’application en ajoutant des annotations de données à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="84265-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="84265-447">Les annotations de données permettent de décrire les règles que vous souhaitez appliquer à vos propriétés de modèle, et ASP.NET MVC s’occupera de l’application et de l’affichage des messages appropriés aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="84265-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="84265-448">Tâche 1 : ajout d’annotations de données</span><span class="sxs-lookup"><span data-stu-id="84265-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="84265-449">Dans cette tâche, vous allez ajouter des annotations de données au modèle d’album qui fera en sorte que la page créer et modifier affiche des messages de validation si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="84265-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="84265-450">Pour une classe de modèle simple, l’ajout d’une annotation de données est simplement gérée en ajoutant une instruction **using** pour **System. ComponentModel. DataAnnotation**, puis en plaçant un attribut **[required]** sur les propriétés appropriées.</span><span class="sxs-lookup"><span data-stu-id="84265-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="84265-451">L’exemple suivant fait de la propriété **Name** un champ obligatoire dans la vue.</span><span class="sxs-lookup"><span data-stu-id="84265-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="84265-452">C’est un peu plus complexe dans les cas tels que l’application où la Entity Data Model est générée.</span><span class="sxs-lookup"><span data-stu-id="84265-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="84265-453">Si vous avez ajouté des annotations de données directement aux classes de modèle, elles sont remplacées si vous mettez à jour le modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="84265-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="84265-454">Au lieu de cela, vous pouvez utiliser des classes partielles de métadonnées qui existent pour contenir les annotations et qui sont associées aux classes de modèle à l’aide de l’attribut **[méta DataType]** .</span><span class="sxs-lookup"><span data-stu-id="84265-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="84265-455">Ouvrez le **début** de la solution situé dans **source/Ex6-AddingValidation/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="84265-456">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="84265-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="84265-457">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-458">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-459">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-460">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-461">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-462">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-463">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-464">Ouvrez le **album.cs** à partir du dossier **Models** .</span><span class="sxs-lookup"><span data-stu-id="84265-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="84265-465">Remplacez le contenu **album.cs** par le code en surbrillance, afin qu’il ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="84265-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="84265-466">La ligne **[DisplayFormat (ConvertEmptyStringToNull = false)]** indique que les chaînes vides du modèle ne sont pas converties en valeurs NULL lorsque le champ de données est mis à jour dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="84265-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="84265-467">Ce paramètre permet d’éviter une exception lorsque l’Entity Framework assigne des valeurs NULL au modèle avant que l’annotation de données valide les champs.</span><span class="sxs-lookup"><span data-stu-id="84265-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="84265-468">(Extrait de code- *application auxiliaires ASP.NET MVC 4 et formulaires et validation-classe partielle des métadonnées de l’album Ex6*)</span><span class="sxs-lookup"><span data-stu-id="84265-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="84265-469">Cette classe partielle de l' **album** a un attribut de **type** de données qui pointe vers la classe **AlbumMetaData** pour les annotations de données.</span><span class="sxs-lookup"><span data-stu-id="84265-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="84265-470">Voici quelques-uns des attributs d’annotation de données que vous utilisez pour annoter le modèle d’album :</span><span class="sxs-lookup"><span data-stu-id="84265-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="84265-471">Obligatoire-indique que la propriété est un champ obligatoire</span><span class="sxs-lookup"><span data-stu-id="84265-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="84265-472">DisplayName : définit le texte à utiliser dans les champs de formulaire et les messages de validation</span><span class="sxs-lookup"><span data-stu-id="84265-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="84265-473">DisplayFormat-spécifie le mode d’affichage et de mise en forme des champs de données.</span><span class="sxs-lookup"><span data-stu-id="84265-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="84265-474">StringLength : définit une longueur maximale pour un champ de chaîne.</span><span class="sxs-lookup"><span data-stu-id="84265-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="84265-475">Range : donne une valeur maximale et minimale pour un champ numérique</span><span class="sxs-lookup"><span data-stu-id="84265-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="84265-476">ScaffoldColumn : permet de masquer les champs des formulaires de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="84265-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="84265-477">Tâche 2 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="84265-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="84265-478">Dans cette tâche, vous allez vérifier que les pages créer et modifier valident les champs, en utilisant les noms complets choisis dans la dernière tâche.</span><span class="sxs-lookup"><span data-stu-id="84265-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="84265-479">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="84265-480">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-480">The project starts in the Home page.</span></span> <span data-ttu-id="84265-481">Remplacez l’URL par **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="84265-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="84265-482">Vérifier que les noms d’affichage correspondent à ceux de la classe partielle (comme l’URL de la pochette de l' **album** au lieu de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="84265-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="84265-483">Cliquez sur **créer**, sans remplir le formulaire.</span><span class="sxs-lookup"><span data-stu-id="84265-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="84265-484">Vérifiez que vous recevez les messages de validation correspondants.</span><span class="sxs-lookup"><span data-stu-id="84265-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="84265-485">![Champs validés dans la page Create](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Champs validés dans la page Create")</span><span class="sxs-lookup"><span data-stu-id="84265-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="84265-486">*Champs validés dans la page Create*</span><span class="sxs-lookup"><span data-stu-id="84265-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="84265-487">Vous pouvez vérifier que le même problème se produit avec la page de **modification** .</span><span class="sxs-lookup"><span data-stu-id="84265-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="84265-488">Remplacez l’URL par **/StoreManager/Edit/1** et vérifiez que les noms d’affichage correspondent à ceux de la classe partielle (comme l’URL de la pochette de l' **album** au lieu de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="84265-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="84265-489">Renseignez les champs **titre** et **prix** , puis cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="84265-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="84265-490">Vérifiez que vous recevez les messages de validation correspondants.</span><span class="sxs-lookup"><span data-stu-id="84265-490">Verify that you get the corresponding validation messages.</span></span>

    ![Champs validés dans la page Modifier](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="84265-492">*Champs validés dans la page Modifier*</span><span class="sxs-lookup"><span data-stu-id="84265-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="84265-493">Exercice 7 : utilisation de jQuery discrète côté client</span><span class="sxs-lookup"><span data-stu-id="84265-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="84265-494">Dans cet exercice, vous allez apprendre à activer la validation jQuery discrète de MVC 4 côté client.</span><span class="sxs-lookup"><span data-stu-id="84265-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="84265-495">Le jQuery discrète utilise le préfixe de données Ajax pour appeler des méthodes d’action sur le serveur plutôt que d’émettre indiscrètement des scripts client Inline.</span><span class="sxs-lookup"><span data-stu-id="84265-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="84265-496">Tâche 1 : exécution de l’application avant l’activation de jQuery discrète</span><span class="sxs-lookup"><span data-stu-id="84265-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="84265-497">Dans cette tâche, vous allez exécuter l’application avant d’inclure jQuery pour pouvoir comparer les deux modèles de validation.</span><span class="sxs-lookup"><span data-stu-id="84265-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="84265-498">Ouvrez le **début** de la solution situé dans **source/EX7-UnobtrusivejQueryValidation/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="84265-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="84265-499">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="84265-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="84265-500">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="84265-501">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="84265-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="84265-502">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="84265-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="84265-503">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="84265-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="84265-504">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="84265-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="84265-505">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="84265-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="84265-506">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="84265-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="84265-507">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="84265-508">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-508">The project starts in the Home page.</span></span> <span data-ttu-id="84265-509">Parcourez **/StoreManager/Create** , puis cliquez sur **créer** sans remplir le formulaire pour vérifier que vous recevez des messages de validation :</span><span class="sxs-lookup"><span data-stu-id="84265-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="84265-510">![Validation du client désactivée](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Validation du client désactivée")</span><span class="sxs-lookup"><span data-stu-id="84265-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="84265-511">*Validation du client désactivée*</span><span class="sxs-lookup"><span data-stu-id="84265-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="84265-512">Dans le navigateur, ouvrez le code source HTML :</span><span class="sxs-lookup"><span data-stu-id="84265-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="84265-513">Tâche 2 : activation de la validation de client discrète</span><span class="sxs-lookup"><span data-stu-id="84265-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="84265-514">Dans cette tâche, vous allez activer la **validation client jQuery discrète** à partir du fichier **Web. config** , qui est défini par défaut sur false dans tous les nouveaux projets ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="84265-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="84265-515">En outre, vous allez ajouter les références de scripts nécessaires pour effectuer un travail de validation client jQuery discrète.</span><span class="sxs-lookup"><span data-stu-id="84265-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="84265-516">Ouvrez le fichier **Web. config** à la racine du projet et assurez-vous que les valeurs des clés **ClientValidationEnabled** et **UnobtrusiveJavaScriptEnabled** sont définies sur **true**.</span><span class="sxs-lookup"><span data-stu-id="84265-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="84265-517">Vous pouvez également activer la validation client par code sur Global.asax.cs pour obtenir les mêmes résultats :</span><span class="sxs-lookup"><span data-stu-id="84265-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="84265-518">**HtmlHelper. ClientValidationEnabled = true ;**</span><span class="sxs-lookup"><span data-stu-id="84265-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="84265-519">En outre, vous pouvez affecter un attribut ClientValidationEnabled à n’importe quel contrôleur pour avoir un comportement personnalisé.</span><span class="sxs-lookup"><span data-stu-id="84265-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="84265-520">Ouvrez **Create. cshtml** sur **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="84265-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="84265-521">Assurez-vous que les fichiers de script suivants, **jQuery. Validate** et **jQuery. Validate. discrètes**, sont référencés dans la vue via le bundle &quot; **~/bundles/jqueryval**&quot;.</span><span class="sxs-lookup"><span data-stu-id="84265-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="84265-522">Toutes ces bibliothèques jQuery sont incluses dans les nouveaux projets MVC 4.</span><span class="sxs-lookup"><span data-stu-id="84265-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="84265-523">Vous trouverez d’autres bibliothèques dans le dossier **/scripts** de votre projet.</span><span class="sxs-lookup"><span data-stu-id="84265-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="84265-524">Pour que ces bibliothèques de validation fonctionnent, vous devez ajouter une référence à la bibliothèque jQuery Framework.</span><span class="sxs-lookup"><span data-stu-id="84265-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="84265-525">Étant donné que cette référence est déjà ajoutée au fichier **\_Layout. cshtml** , vous n’avez pas besoin de l’ajouter dans cette vue particulière.</span><span class="sxs-lookup"><span data-stu-id="84265-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="84265-526">Tâche 3 : exécution de l’application à l’aide de la validation jQuery discrète</span><span class="sxs-lookup"><span data-stu-id="84265-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="84265-527">Dans cette tâche, vous allez tester que le modèle de vue Create View **StoreManager** effectue la validation côté client à l’aide de bibliothèques jQuery lorsque l’utilisateur crée un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="84265-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="84265-528">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="84265-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="84265-529">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84265-529">The project starts in the Home page.</span></span> <span data-ttu-id="84265-530">Parcourez **/StoreManager/Create** , puis cliquez sur **créer** sans remplir le formulaire pour vérifier que vous recevez des messages de validation :</span><span class="sxs-lookup"><span data-stu-id="84265-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="84265-531">![Validation du client avec jQuery activée](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validation du client avec jQuery activée")</span><span class="sxs-lookup"><span data-stu-id="84265-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="84265-532">*Validation du client avec jQuery activée*</span><span class="sxs-lookup"><span data-stu-id="84265-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="84265-533">Dans le navigateur, ouvrez le code source de CREATE VIEW :</span><span class="sxs-lookup"><span data-stu-id="84265-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="84265-534">Pour chaque règle de validation client, jQuery discrète ajoute un attribut avec les données-Val-*rulename*=&quot;*message*&quot;.</span><span class="sxs-lookup"><span data-stu-id="84265-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="84265-535">Voici une liste de balises que jQuery insère discrètement dans le champ d’entrée HTML pour effectuer la validation du client :</span><span class="sxs-lookup"><span data-stu-id="84265-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="84265-536">Données-Val</span><span class="sxs-lookup"><span data-stu-id="84265-536">Data-val</span></span>
   > - <span data-ttu-id="84265-537">Données-Val-Number</span><span class="sxs-lookup"><span data-stu-id="84265-537">Data-val-number</span></span>
   > - <span data-ttu-id="84265-538">Données-Val-Range</span><span class="sxs-lookup"><span data-stu-id="84265-538">Data-val-range</span></span>
   > - <span data-ttu-id="84265-539">Données-Val-Range-min/données-Val-Range-Max</span><span class="sxs-lookup"><span data-stu-id="84265-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="84265-540">Données-Val-requis</span><span class="sxs-lookup"><span data-stu-id="84265-540">Data-val-required</span></span>
   > - <span data-ttu-id="84265-541">Données-Val-length</span><span class="sxs-lookup"><span data-stu-id="84265-541">Data-val-length</span></span>
   > - <span data-ttu-id="84265-542">Données-Val-length-Max/données-Val-length-min</span><span class="sxs-lookup"><span data-stu-id="84265-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="84265-543">Toutes les valeurs de données sont remplies avec l' **annotation de données**de modèle.</span><span class="sxs-lookup"><span data-stu-id="84265-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="84265-544">Ensuite, toute la logique qui fonctionne côté serveur peut être exécutée côté client.</span><span class="sxs-lookup"><span data-stu-id="84265-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="84265-545">Par exemple, l’attribut Price a l’annotation de données suivante dans le modèle :</span><span class="sxs-lookup"><span data-stu-id="84265-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="84265-546">Après l’utilisation de jQuery discrète, le code généré est le suivant :</span><span class="sxs-lookup"><span data-stu-id="84265-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="84265-547">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="84265-547">Summary</span></span>

<span data-ttu-id="84265-548">En effectuant ce laboratoire pratique, vous avez appris à permettre aux utilisateurs de modifier les données stockées dans la base de données à l’aide des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="84265-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="84265-549">Actions de contrôleur comme index, créer, modifier, supprimer</span><span class="sxs-lookup"><span data-stu-id="84265-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="84265-550">Fonctionnalité de génération de modèles automatique de ASP.NET MVC pour l’affichage des propriétés dans un tableau HTML</span><span class="sxs-lookup"><span data-stu-id="84265-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="84265-551">Applications auxiliaires HTML personnalisées pour améliorer l’expérience utilisateur</span><span class="sxs-lookup"><span data-stu-id="84265-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="84265-552">Méthodes d’action qui réagissent à des appels HTTP-is ou HTTP-POSTCONNEXION</span><span class="sxs-lookup"><span data-stu-id="84265-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="84265-553">Modèle d’éditeur partagé pour des modèles de vue similaires comme Create et Edit</span><span class="sxs-lookup"><span data-stu-id="84265-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="84265-554">Éléments de formulaire tels que les listes déroulantes</span><span class="sxs-lookup"><span data-stu-id="84265-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="84265-555">Annotations de données pour la validation de modèle</span><span class="sxs-lookup"><span data-stu-id="84265-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="84265-556">Validation côté client à l’aide de la bibliothèque jQuery discrète</span><span class="sxs-lookup"><span data-stu-id="84265-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="84265-557">Annexe A : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="84265-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="84265-558">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="84265-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="84265-559">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="84265-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="84265-560">Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="84265-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="84265-561">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="84265-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="84265-562">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="84265-562">Click on **Install Now**.</span></span> <span data-ttu-id="84265-563">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="84265-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="84265-564">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="84265-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="84265-565">![Installer Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="84265-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="84265-566">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="84265-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="84265-567">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="84265-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="84265-569">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="84265-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="84265-570">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="84265-570">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="84265-572">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="84265-572">*Installation progress*</span></span>
6. <span data-ttu-id="84265-573">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="84265-573">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="84265-575">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="84265-575">*Installation completed*</span></span>
7. <span data-ttu-id="84265-576">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="84265-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="84265-577">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="84265-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="84265-579">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="84265-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="84265-580">Annexe B : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="84265-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="84265-581">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="84265-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="84265-582">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="84265-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="84265-583">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="84265-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="84265-584">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="84265-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="84265-585">***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***</span><span class="sxs-lookup"><span data-stu-id="84265-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="84265-586">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="84265-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="84265-587">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="84265-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="84265-588">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="84265-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="84265-589">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="84265-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="84265-590">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="84265-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="84265-591">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="84265-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="84265-592">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="84265-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="84265-593">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="84265-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="84265-594">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="84265-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="84265-595">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="84265-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="84265-596">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="84265-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="84265-597">***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0.</span><span class="sxs-lookup"><span data-stu-id="84265-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="84265-598">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="84265-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="84265-599">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="84265-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="84265-600">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="84265-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="84265-601">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="84265-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="84265-602">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="84265-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="84265-603">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="84265-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="84265-604">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="84265-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
