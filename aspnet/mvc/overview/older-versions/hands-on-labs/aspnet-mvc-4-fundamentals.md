---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Notions de base de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce laboratoire pratique est basé sur le magasin de musique MVC (Model View Controller), une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598818"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="55ebe-103">Concepts de base d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="55ebe-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="55ebe-104">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="55ebe-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="55ebe-105">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="55ebe-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="55ebe-106">Ce laboratoire pratique est basé sur le magasin de musique MVC (Model View Controller), une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="55ebe-107">Tout au long du laboratoire, vous apprendrez la simplicité, mais la puissance de l’utilisation conjointe de ces technologies.</span><span class="sxs-lookup"><span data-stu-id="55ebe-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="55ebe-108">Vous allez commencer par une application simple et la générer jusqu’à ce que vous disposiez d’une application Web ASP.NET MVC 4 entièrement fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="55ebe-109">Ce laboratoire fonctionne avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="55ebe-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="55ebe-110">Si vous souhaitez explorer la version ASP.NET MVC 3 de l’application du didacticiel, vous pouvez la trouver dans [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="55ebe-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="55ebe-111">Ce laboratoire pratique part du principe que le développeur a une expérience dans les technologies de développement Web, telles que HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="55ebe-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="55ebe-112">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="55ebe-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="55ebe-113">Le projet propre à ce laboratoire est disponible à l’adresse [notions de base de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="55ebe-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="55ebe-114">Application du Store Music</span><span class="sxs-lookup"><span data-stu-id="55ebe-114">The Music Store application</span></span>

<span data-ttu-id="55ebe-115">L’application Web du Store musical qui sera créée tout au long de ce laboratoire comprend trois parties principales : achat, retrait et administration.</span><span class="sxs-lookup"><span data-stu-id="55ebe-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="55ebe-116">Les visiteurs pourront parcourir les albums par genre, ajouter des albums à leur panier, passer en revue leur sélection, puis passer à l’extraction pour se connecter et terminer la commande.</span><span class="sxs-lookup"><span data-stu-id="55ebe-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="55ebe-117">En outre, les administrateurs de magasins pourront gérer les albums disponibles ainsi que leurs principales propriétés.</span><span class="sxs-lookup"><span data-stu-id="55ebe-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="55ebe-118">![Écrans du Store musique](aspnet-mvc-4-fundamentals/_static/image1.png "Écrans du Store musique")</span><span class="sxs-lookup"><span data-stu-id="55ebe-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="55ebe-119">*Écrans du Store musique*</span><span class="sxs-lookup"><span data-stu-id="55ebe-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="55ebe-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="55ebe-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="55ebe-121">L’application du Store musical est créée à l’aide du **contrôleur MVC (Model View Controller)** , un modèle architectural qui sépare une application en trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="55ebe-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="55ebe-122">**Modèles**: les objets de modèle sont les parties de l’application qui implémentent la logique de domaine.</span><span class="sxs-lookup"><span data-stu-id="55ebe-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="55ebe-123">Souvent, les objets de modèle récupèrent et stockent également l’état du modèle dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="55ebe-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="55ebe-124">**Vues :** Les vues sont les composants qui affichent l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="55ebe-125">En général, cette interface utilisateur est créée à partir des données du modèle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="55ebe-126">Par exemple, la vue Edit des albums affiche des zones de texte et une liste déroulante en fonction de l’état actuel d’un objet album.</span><span class="sxs-lookup"><span data-stu-id="55ebe-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="55ebe-127">**Contrôleurs :** Les contrôleurs sont les composants qui gèrent l’interaction avec l’utilisateur, manipulent le modèle et finalement sélectionnent une vue pour afficher l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="55ebe-128">Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.</span><span class="sxs-lookup"><span data-stu-id="55ebe-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="55ebe-129">Le modèle MVC vous aide à créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), tout en fournissant un couplage faible entre ces éléments.</span><span class="sxs-lookup"><span data-stu-id="55ebe-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="55ebe-130">Cette séparation vous aide à gérer la complexité lorsque vous générez une application, car elle vous permet de vous concentrer sur un aspect de l’implémentation à la fois.</span><span class="sxs-lookup"><span data-stu-id="55ebe-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="55ebe-131">En outre, le modèle MVC facilite le test des applications, en encourageant également l’utilisation du développement piloté par les tests (TDD) pour la création d’applications.</span><span class="sxs-lookup"><span data-stu-id="55ebe-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="55ebe-132">L’infrastructure **MVC ASP.net** fournit une alternative au modèle de Web Forms ASP.net pour la création d’applications Web basées sur ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="55ebe-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="55ebe-133">L’infrastructure **MVC ASP.net** est un Framework de présentation léger et hautement testable qui (comme avec les applications basées sur les formulaires Web) est intégré aux fonctionnalités ASP.NET existantes, telles que les pages maîtres et l’authentification basée sur l’appartenance. vous bénéficiez ainsi de toute la puissance du .NET Framework principal.</span><span class="sxs-lookup"><span data-stu-id="55ebe-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="55ebe-134">Cela est utile si vous êtes déjà familiarisé avec ASP.NET Web Forms, car toutes les bibliothèques que vous utilisez déjà sont également disponibles dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="55ebe-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="55ebe-135">En outre, le couplage faible entre les trois principaux composants d’une application MVC favorise également le développement parallèle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="55ebe-136">Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur, et un troisième développeur peut se concentrer sur la logique métier dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="55ebe-137">Objectifs</span><span class="sxs-lookup"><span data-stu-id="55ebe-137">Objectives</span></span>

<span data-ttu-id="55ebe-138">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="55ebe-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="55ebe-139">Créer une application ASP.NET MVC à partir de zéro en suivant le didacticiel sur l’application du Store Music</span><span class="sxs-lookup"><span data-stu-id="55ebe-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="55ebe-140">Ajoutez des contrôleurs pour gérer les URL vers la page d’accueil du site et pour parcourir ses principales fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="55ebe-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="55ebe-141">Ajouter une vue pour personnaliser le contenu affiché avec son style</span><span class="sxs-lookup"><span data-stu-id="55ebe-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="55ebe-142">Ajouter des classes de modèle pour contenir et gérer la logique de données et de domaine</span><span class="sxs-lookup"><span data-stu-id="55ebe-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="55ebe-143">Utiliser le modèle afficher le modèle pour passer des informations des actions du contrôleur aux modèles de vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="55ebe-144">Explorez le nouveau modèle ASP.NET MVC 4 pour les applications Internet</span><span class="sxs-lookup"><span data-stu-id="55ebe-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="55ebe-145">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="55ebe-145">Prerequisites</span></span>

<span data-ttu-id="55ebe-146">Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="55ebe-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="55ebe-147">[Visual Studio 2012 Express pour le Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation)</span><span class="sxs-lookup"><span data-stu-id="55ebe-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="55ebe-148">Installation</span><span class="sxs-lookup"><span data-stu-id="55ebe-148">Setup</span></span>

<span data-ttu-id="55ebe-149">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="55ebe-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="55ebe-150">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="55ebe-151">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="55ebe-152">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="55ebe-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="55ebe-153">Exercices</span><span class="sxs-lookup"><span data-stu-id="55ebe-153">Exercises</span></span>

<span data-ttu-id="55ebe-154">Ce laboratoire pratique est constitué des exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="55ebe-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="55ebe-155">Exercice 1 : création d’un projet d’application Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="55ebe-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="55ebe-156">Exercice 2 : création d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="55ebe-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="55ebe-157">Exercice 3 : passage de paramètres à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="55ebe-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="55ebe-158">Exercice 4 : création d’une vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="55ebe-159">Exercice 5 : création d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="55ebe-160">Exercice 6 : utilisation des paramètres dans la vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="55ebe-161">Exercice 7 : un nouveau modèle ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="55ebe-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="55ebe-162">Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="55ebe-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="55ebe-163">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.</span><span class="sxs-lookup"><span data-stu-id="55ebe-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="55ebe-164">Durée estimée pour effectuer ce laboratoire : **60 minutes**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="55ebe-165">Exercice 1 : création d’un projet d’application Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="55ebe-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="55ebe-166">Dans cet exercice, vous allez apprendre à créer une application ASP.NET MVC dans Visual Studio 2012 Express pour le Web, ainsi que son organisation principale de dossiers.</span><span class="sxs-lookup"><span data-stu-id="55ebe-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="55ebe-167">En outre, vous apprendrez à ajouter un nouveau contrôleur et à le faire afficher une simple chaîne dans la page d’affichage de l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="55ebe-168">Tâche 1 : création du projet d’application Web MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55ebe-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="55ebe-169">Dans cette tâche, vous allez créer un projet d’application ASP.NET MVC vide à l’aide du modèle MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="55ebe-170">Démarrez **vs Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="55ebe-171">Dans le menu **Fichier**, cliquez sur **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="55ebe-172">Dans la boîte de dialogue **nouveau projet** , sélectionnez le type de projet d' **application Web ASP.NET MVC 4** , situé sous **Visual C#, liste de** modèles **Web** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="55ebe-173">Remplacez le **nom** par *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="55ebe-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="55ebe-174">Définissez l’emplacement de la solution à l’intérieur d’un nouveau dossier **Begin** dans le dossier source de cet exercice, par exemple **[Your-Hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="55ebe-175">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-175">Click **OK**.</span></span>

    <span data-ttu-id="55ebe-176">![Boîte de dialogue créer un nouveau projet](aspnet-mvc-4-fundamentals/_static/image2.png "Boîte de dialogue créer un nouveau projet")</span><span class="sxs-lookup"><span data-stu-id="55ebe-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="55ebe-177">*Boîte de dialogue créer un nouveau projet*</span><span class="sxs-lookup"><span data-stu-id="55ebe-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="55ebe-178">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de **base** et assurez-vous que le **moteur d’affichage** sélectionné est **Razor**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="55ebe-179">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-179">Click **OK**.</span></span>

    <span data-ttu-id="55ebe-180">![Boîte de dialogue Nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Boîte de dialogue Nouveau projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="55ebe-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="55ebe-181">*Boîte de dialogue Nouveau projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="55ebe-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="55ebe-182">Tâche 2 : exploration de la structure de la solution</span><span class="sxs-lookup"><span data-stu-id="55ebe-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="55ebe-183">L’infrastructure MVC ASP.NET comprend un modèle de projet Visual Studio qui vous aide à créer des applications Web prenant en charge le modèle MVC.</span><span class="sxs-lookup"><span data-stu-id="55ebe-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="55ebe-184">Ce modèle crée une application Web ASP.NET MVC avec les dossiers requis, les modèles d’élément et les entrées du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="55ebe-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="55ebe-185">Dans cette tâche, vous allez examiner la structure de la solution pour comprendre les éléments impliqués et leurs relations.</span><span class="sxs-lookup"><span data-stu-id="55ebe-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="55ebe-186">Les dossiers suivants sont inclus dans toutes les applications ASP.NET MVC, car l’infrastructure ASP.NET MVC utilise par défaut une convention &quot;sur la configuration&quot; l’approche et effectue certaines hypothèses par défaut en fonction des conventions d’affectation des noms de dossiers.</span><span class="sxs-lookup"><span data-stu-id="55ebe-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="55ebe-187">Une fois le projet créé, passez en revue la structure de dossiers qui a été créée dans le Explorateur de solutions sur le côté droit :</span><span class="sxs-lookup"><span data-stu-id="55ebe-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="55ebe-188">![Structure de dossiers ASP.NET MVC dans Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image4.png "Structure de dossiers ASP.NET MVC dans Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="55ebe-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="55ebe-189">*Structure de dossiers ASP.NET MVC dans Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="55ebe-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="55ebe-190">Les **contrôleurs**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-190">**Controllers**.</span></span> <span data-ttu-id="55ebe-191">Ce dossier contiendra les classes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="55ebe-192">Dans une application basée sur MVC, les contrôleurs sont responsables du traitement de l’interaction de l’utilisateur final, de la manipulation du modèle et de la sélection d’une vue pour le rendu de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="55ebe-193">L’infrastructure MVC requiert que les noms de tous les contrôleurs se terminent par &quot;contrôleur&quot;, par exemple HomeController, LoginController ou ProductController.</span><span class="sxs-lookup"><span data-stu-id="55ebe-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="55ebe-194">**Modèles**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-194">**Models**.</span></span> <span data-ttu-id="55ebe-195">Ce dossier est fourni pour les classes qui représentent le modèle d’application pour l’application Web MVC.</span><span class="sxs-lookup"><span data-stu-id="55ebe-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="55ebe-196">Cela comprend généralement le code qui définit les objets et la logique d’interaction avec le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="55ebe-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="55ebe-197">En général, les objets de modèle réels sont placés dans des bibliothèques de classes séparées.</span><span class="sxs-lookup"><span data-stu-id="55ebe-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="55ebe-198">Toutefois, lorsque vous créez une application, vous pouvez inclure des classes, puis les déplacer dans des bibliothèques de classes distinctes à un stade ultérieur du cycle de développement.</span><span class="sxs-lookup"><span data-stu-id="55ebe-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="55ebe-199">**Vues**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-199">**Views**.</span></span> <span data-ttu-id="55ebe-200">Ce dossier est l’emplacement recommandé pour les vues, les composants responsables de l’affichage de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="55ebe-201">Les vues utilisent des fichiers. aspx,. ascx,. cshtml et. Master, en plus de tous les autres fichiers liés aux affichages de rendu.</span><span class="sxs-lookup"><span data-stu-id="55ebe-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="55ebe-202">Le dossier views contient un dossier pour chaque contrôleur ; le dossier est nommé avec le préfixe de nom de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="55ebe-203">Par exemple, si vous avez un contrôleur nommé **HomeController**, le dossier views contient un dossier nommé page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="55ebe-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="55ebe-204">Par défaut, lorsque l’infrastructure MVC ASP.NET charge une vue, il recherche un fichier. aspx avec le nom de la vue demandée dans le dossier Views\controllerName (**views [ControllerName] [action]. aspx**) ou (**views [ControllerName] [action]. cshtml**) pour les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="55ebe-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="55ebe-205">En plus des dossiers indiqués précédemment, une application Web MVC utilise le fichier **global. asax** pour définir les valeurs par défaut de routage d’URL globales et utilise le fichier **Web. config** pour configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="55ebe-206">Tâche 3 : ajout d’un HomeController</span><span class="sxs-lookup"><span data-stu-id="55ebe-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="55ebe-207">Dans les applications ASP.NET qui n’utilisent pas l’infrastructure MVC, l’interaction avec l’utilisateur est organisée autour des pages, et autour du déclenchement et de la gestion d’événements à partir de ces pages.</span><span class="sxs-lookup"><span data-stu-id="55ebe-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="55ebe-208">En revanche, l’interaction de l’utilisateur avec les applications ASP.NET MVC est organisée autour des contrôleurs et de leurs méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="55ebe-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="55ebe-209">En revanche, l’infrastructure MVC ASP.NET mappe les URL aux classes appelées contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="55ebe-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="55ebe-210">Les contrôleurs traitent les demandes entrantes, gèrent les entrées et les interactions de l’utilisateur, exécutent la logique d’application appropriée et déterminent la réponse à renvoyer au client (affichage HTML, téléchargement d’un fichier, redirection vers une autre URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="55ebe-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="55ebe-211">Dans le cas de l’affichage HTML, une classe de contrôleur appelle généralement un composant de vue séparé pour générer le balisage HTML de la requête.</span><span class="sxs-lookup"><span data-stu-id="55ebe-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="55ebe-212">Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.</span><span class="sxs-lookup"><span data-stu-id="55ebe-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="55ebe-213">Au cours de cette tâche, vous allez ajouter une classe de contrôleur qui traitera les URL vers la page d’hébergement du site du Store musical.</span><span class="sxs-lookup"><span data-stu-id="55ebe-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="55ebe-214">Cliquez avec le bouton droit sur le dossier **Controllers** dans le Explorateur de solutions, sélectionnez **Ajouter** , puis commande du **contrôleur** :</span><span class="sxs-lookup"><span data-stu-id="55ebe-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="55ebe-215">![Ajouter une commande de contrôleur](aspnet-mvc-4-fundamentals/_static/image5.png "Ajouter une commande de contrôleur")</span><span class="sxs-lookup"><span data-stu-id="55ebe-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="55ebe-216">*Commande Add Controller*</span><span class="sxs-lookup"><span data-stu-id="55ebe-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="55ebe-217">La boîte de dialogue **Ajouter un contrôleur** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="55ebe-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="55ebe-218">Nommez le contrôle *HomeController* et appuyez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="55ebe-219">![Boîte de dialogue Ajouter un contrôleur](aspnet-mvc-4-fundamentals/_static/image6.png "Boîte de dialogue Ajouter un contrôleur")</span><span class="sxs-lookup"><span data-stu-id="55ebe-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="55ebe-220">*Boîte de dialogue Ajouter un contrôleur*</span><span class="sxs-lookup"><span data-stu-id="55ebe-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="55ebe-221">Le fichier **HomeController.cs** est créé dans le dossier **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="55ebe-222">Pour que le **HomeController** retourne une chaîne sur son action d’index, remplacez la méthode d' **index** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="55ebe-223">(Extrait de code- *notions de base de ASP.NET MVC 4-index du HomeController de EX1*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="55ebe-224">Tâche 4 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="55ebe-225">Dans cette tâche, vous allez essayer l’application dans un navigateur Web.</span><span class="sxs-lookup"><span data-stu-id="55ebe-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="55ebe-226">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="55ebe-227">Le projet est compilé et le serveur Web IIS local démarre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="55ebe-228">Le serveur Web IIS local ouvre automatiquement un navigateur Web qui pointe vers l’URL du serveur Web.</span><span class="sxs-lookup"><span data-stu-id="55ebe-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="55ebe-229">![Application s’exécutant dans un navigateur Web](aspnet-mvc-4-fundamentals/_static/image7.png "Application s’exécutant dans un navigateur Web")</span><span class="sxs-lookup"><span data-stu-id="55ebe-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="55ebe-230">*Application s’exécutant dans un navigateur Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-231">Le serveur Web IIS local exécute le site Web sur un numéro de port libre aléatoire.</span><span class="sxs-lookup"><span data-stu-id="55ebe-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="55ebe-232">Dans la figure ci-dessus, le site s’exécute sur `http://localhost:50103/`, donc il utilise le port 50103.</span><span class="sxs-lookup"><span data-stu-id="55ebe-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="55ebe-233">Votre numéro de port peut varier.</span><span class="sxs-lookup"><span data-stu-id="55ebe-233">Your port number may vary.</span></span>
2. <span data-ttu-id="55ebe-234">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="55ebe-235">Exercice 2 : création d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="55ebe-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="55ebe-236">Dans cet exercice, vous allez apprendre à mettre à jour le contrôleur pour implémenter une fonctionnalité simple de l’application du Store Music.</span><span class="sxs-lookup"><span data-stu-id="55ebe-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="55ebe-237">Ce contrôleur définit des méthodes d’action pour gérer chacune des requêtes spécifiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="55ebe-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="55ebe-238">Page de liste des genres musicaux dans le magasin musical</span><span class="sxs-lookup"><span data-stu-id="55ebe-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="55ebe-239">Page parcourir qui répertorie tous les albums musicaux pour un genre particulier</span><span class="sxs-lookup"><span data-stu-id="55ebe-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="55ebe-240">Page de détails qui affiche des informations sur un album de musique spécifique</span><span class="sxs-lookup"><span data-stu-id="55ebe-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="55ebe-241">Dans le cadre de cet exercice, ces actions retournent simplement une chaîne par la suite.</span><span class="sxs-lookup"><span data-stu-id="55ebe-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="55ebe-242">Tâche 1 : ajout d’une nouvelle classe StoreController</span><span class="sxs-lookup"><span data-stu-id="55ebe-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="55ebe-243">Dans cette tâche, vous allez ajouter un nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="55ebe-244">S’il n’est pas déjà ouvert, démarrez **VS Express pour le Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="55ebe-245">Dans le menu **fichier** , choisissez **ouvrir un projet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="55ebe-246">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex02-CreatingAController\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="55ebe-247">Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="55ebe-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="55ebe-248">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="55ebe-249">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="55ebe-250">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="55ebe-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="55ebe-251">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="55ebe-252">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="55ebe-253">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="55ebe-254">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="55ebe-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="55ebe-255">Ajoutez le nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-255">Add the new controller.</span></span> <span data-ttu-id="55ebe-256">Pour ce faire, cliquez avec le bouton droit sur le dossier **Controllers** dans le Explorateur de solutions, sélectionnez **Ajouter** , puis la commande **contrôleur** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="55ebe-257">Remplacez le **nom du contrôleur** par *StoreController*, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="55ebe-258">![Boîte de dialogue Ajouter un contrôleur](aspnet-mvc-4-fundamentals/_static/image8.png "Boîte de dialogue Ajouter un contrôleur")</span><span class="sxs-lookup"><span data-stu-id="55ebe-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="55ebe-259">*Boîte de dialogue Ajouter un contrôleur*</span><span class="sxs-lookup"><span data-stu-id="55ebe-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="55ebe-260">Tâche 2 : modification des actions de StoreController</span><span class="sxs-lookup"><span data-stu-id="55ebe-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="55ebe-261">Dans cette tâche, vous allez modifier les méthodes de contrôleur appelées **actions**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="55ebe-262">Les actions sont responsables de la gestion des demandes d’URL et de la détermination du contenu qui doit être renvoyé au navigateur ou à l’utilisateur qui a appelé l’URL.</span><span class="sxs-lookup"><span data-stu-id="55ebe-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="55ebe-263">La classe **StoreController** a déjà une méthode d' **index** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="55ebe-264">Vous l’utiliserez ultérieurement dans ce laboratoire pour implémenter la page qui répertorie tous les genres du magasin musical.</span><span class="sxs-lookup"><span data-stu-id="55ebe-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="55ebe-265">Pour le moment, il vous suffit de remplacer la méthode **index** par le code suivant, qui retourne une chaîne &quot;Hello de Store. index ()&quot;:</span><span class="sxs-lookup"><span data-stu-id="55ebe-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="55ebe-266">(Extrait de code- *notions de base de ASP.NET MVC 4-EX2 StoreController index*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="55ebe-267">Ajoutez les méthodes **Browse** et **Details** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="55ebe-268">Pour ce faire, ajoutez le code suivant au **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="55ebe-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="55ebe-269">(Extrait de code- *notions de base de ASP.NET MVC 4-EX2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="55ebe-270">Tâche 3 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="55ebe-271">Dans cette tâche, vous allez essayer l’application dans un navigateur Web.</span><span class="sxs-lookup"><span data-stu-id="55ebe-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="55ebe-272">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-273">Le projet démarre sur la page d' **hébergement** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="55ebe-274">Modifiez l’URL pour vérifier l’implémentation de chaque action.</span><span class="sxs-lookup"><span data-stu-id="55ebe-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="55ebe-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-275">**/Store**.</span></span> <span data-ttu-id="55ebe-276">Vous verrez **&quot;Hello de Store. index ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="55ebe-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-277">**/Store/Browse**.</span></span> <span data-ttu-id="55ebe-278">Vous verrez **&quot;Hello de Store. Browse ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="55ebe-279">**/Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-279">**/Store/Details**.</span></span> <span data-ttu-id="55ebe-280">Vous verrez **&quot;Hello de Store. Details ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="55ebe-281">![Navigation dans StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Navigation dans StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="55ebe-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="55ebe-282">*Navigation dans/Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="55ebe-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="55ebe-283">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="55ebe-284">Exercice 3 : passage de paramètres à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="55ebe-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="55ebe-285">Jusqu’à présent, vous renvoyiez des chaînes constantes à partir des contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="55ebe-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="55ebe-286">Dans cet exercice, vous allez apprendre à transmettre des paramètres à un contrôleur à l’aide de l’URL et de QueryString, puis à faire en sorte que les actions de méthode répondent avec du texte au navigateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="55ebe-287">Tâche 1 : ajout du paramètre genre à StoreController</span><span class="sxs-lookup"><span data-stu-id="55ebe-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="55ebe-288">Dans cette tâche, vous allez utiliser **QueryString** pour envoyer des paramètres à la méthode d’action **Browse** dans le **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="55ebe-289">S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="55ebe-290">Dans le menu **fichier** , choisissez **ouvrir un projet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="55ebe-291">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex03-PassingParametersToAController\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="55ebe-292">Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="55ebe-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="55ebe-293">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="55ebe-294">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="55ebe-295">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="55ebe-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="55ebe-296">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="55ebe-297">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="55ebe-298">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="55ebe-299">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="55ebe-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="55ebe-300">Ouvrez la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-300">Open **StoreController** class.</span></span> <span data-ttu-id="55ebe-301">Pour ce faire, dans le **Explorateur de solutions**, développez le dossier **Controllers** , puis double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="55ebe-302">Modifiez la méthode **Browse** , en ajoutant un paramètre de chaîne pour demander un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="55ebe-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="55ebe-303">ASP.NET MVC passe automatiquement tout paramètre de publication QueryString ou de formulaire nommé **genre** à cette méthode d’action lorsqu’il est appelé.</span><span class="sxs-lookup"><span data-stu-id="55ebe-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="55ebe-304">Pour ce faire, remplacez la méthode **Browse** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="55ebe-305">(Extrait de code- *notions de base de ASP.NET MVC 4-EX3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="55ebe-306">Vous utilisez la méthode de l’utilitaire **HttpUtility. HtmlEncode** pour empêcher les utilisateurs d’injecter du code JavaScript dans la vue à l’aide d’un lien tel que **/Store/Browse ? Genre =&lt;script&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="55ebe-307">Pour plus d’informations, consultez [cet article MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="55ebe-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="55ebe-308">Tâche 2 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="55ebe-309">Dans cette tâche, vous allez essayer l’application dans un navigateur Web et utiliser le paramètre **genre** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="55ebe-310">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-311">Le projet démarre sur la page d' **hébergement** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="55ebe-312">Remplacer l’URL par */Store/Browse ? Genre = Disco* pour vérifier que l’action reçoit le paramètre genre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="55ebe-313">![Navigation StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Navigation StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="55ebe-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="55ebe-314">*Vous parcourez/Store/Browse ? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="55ebe-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="55ebe-315">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="55ebe-316">Tâche 3 : ajout d’un paramètre d’ID incorporé dans l’URL</span><span class="sxs-lookup"><span data-stu-id="55ebe-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="55ebe-317">Dans cette tâche, vous allez utiliser l' **URL** pour passer un paramètre **ID** à la méthode d’action **Details** de **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="55ebe-318">La Convention de routage par défaut d’ASP.NET MVC consiste à traiter le segment d’une URL après le nom de la méthode d’action en tant que paramètre nommé **ID**. Si votre méthode d’action a un paramètre nommé ID, ASP.NET MVC passe automatiquement le segment d’URL à vous en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="55ebe-319">Dans le magasin d’URL **/Détails/5**, l' **ID** sera interprété comme **5**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="55ebe-320">Modifiez la méthode **Details** de **StoreController**, en ajoutant un paramètre **int** appelé **ID**. Pour ce faire, remplacez la méthode **Details** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="55ebe-321">(Extrait de code- *notions de base de ASP.NET MVC 4-EX3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="55ebe-322">Tâche 4 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="55ebe-323">Dans cette tâche, vous allez essayer l’application dans un navigateur Web et utiliser le paramètre **ID** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="55ebe-324">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-325">Le projet démarre sur la page d' **hébergement** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="55ebe-326">Remplacez l’URL par */Store/Details/5* pour vérifier que l’action reçoit le paramètre ID.</span><span class="sxs-lookup"><span data-stu-id="55ebe-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="55ebe-327">![Navigation dans StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Navigation dans StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="55ebe-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="55ebe-328">*Navigation dans/Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="55ebe-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="55ebe-329">Exercice 4 : création d’une vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="55ebe-330">Jusqu’à présent, vous retournez des chaînes à partir d’actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="55ebe-331">Bien qu’il s’agisse d’un moyen utile pour comprendre le fonctionnement des contrôleurs, il ne s’agit pas de la façon dont vos applications Web réelles sont générées.</span><span class="sxs-lookup"><span data-stu-id="55ebe-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="55ebe-332">Les vues sont des composants qui offrent une meilleure approche pour la génération de code HTML dans le navigateur avec l’utilisation de fichiers de modèle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="55ebe-333">Dans cet exercice, vous allez apprendre à ajouter une page maître de disposition pour configurer un modèle pour le contenu HTML courant, une feuille de style pour améliorer l’apparence du site et enfin un modèle de vue pour permettre au HomeController de retourner du code HTML.</span><span class="sxs-lookup"><span data-stu-id="55ebe-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a><span data-ttu-id="55ebe-334">Tâche 1 : modification du fichier \_Layout. cshtml</span><span class="sxs-lookup"><span data-stu-id="55ebe-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="55ebe-335">Le fichier **~/Views/Shared/\_Layout. cshtml** vous permet de configurer un modèle de code HTML commun à utiliser sur l’ensemble du site Web.</span><span class="sxs-lookup"><span data-stu-id="55ebe-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="55ebe-336">Au cours de cette tâche, vous allez ajouter une page maître de disposition avec un en-tête commun avec des liens vers la page d’hébergement et la zone de stockage.</span><span class="sxs-lookup"><span data-stu-id="55ebe-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="55ebe-337">S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="55ebe-338">Dans le menu **fichier** , choisissez **ouvrir un projet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="55ebe-339">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex04-CreatingAView\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="55ebe-340">Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="55ebe-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="55ebe-341">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="55ebe-342">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="55ebe-343">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="55ebe-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="55ebe-344">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="55ebe-345">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="55ebe-346">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="55ebe-347">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="55ebe-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="55ebe-348">Le fichier <strong>\_Layout. cshtml</strong> contient la disposition de conteneur html pour toutes les pages du site.</span><span class="sxs-lookup"><span data-stu-id="55ebe-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="55ebe-349">Il comprend l’élément <strong>&lt;html&gt;</strong> pour la réponse HTML, ainsi que les éléments de la <strong>&lt;head&gt;</strong> et <strong>&lt;Body&gt;</strong> .</span><span class="sxs-lookup"><span data-stu-id="55ebe-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="55ebe-350"><strong>@RenderBody()</strong> dans le corps HTML Identifiez les régions dans lesquelles les modèles de vue peuvent être renseignés avec du contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="55ebe-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="55ebe-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="55ebe-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="55ebe-352">Ajoutez un en-tête commun avec des liens vers la page d’hébergement et la zone de stockage sur toutes les pages du site.</span><span class="sxs-lookup"><span data-stu-id="55ebe-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="55ebe-353">Pour ce faire, ajoutez le code suivant sous &lt;instruction&gt; corps.</span><span class="sxs-lookup"><span data-stu-id="55ebe-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="55ebe-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="55ebe-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="55ebe-355">Incluez un DIV pour restituer la section de corps de chaque page.</span><span class="sxs-lookup"><span data-stu-id="55ebe-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="55ebe-356">Remplacez <strong>@RenderBody()</strong> par le code en surbrillanceC#suivant : ()</span><span class="sxs-lookup"><span data-stu-id="55ebe-356">Replace <strong>@RenderBody()</strong> with the following highlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="55ebe-357">Saviez-vous ?</span><span class="sxs-lookup"><span data-stu-id="55ebe-357">Did you know?</span></span> <span data-ttu-id="55ebe-358">Visual Studio 2012 contient des extraits de code qui facilitent l’ajout de code couramment utilisé en HTML, les fichiers de code et bien plus encore !</span><span class="sxs-lookup"><span data-stu-id="55ebe-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="55ebe-359">Essayez-le en tapant **&lt;div&gt;** et en appuyant deux fois sur **Tab** pour insérer une balise **div** complète.</span><span class="sxs-lookup"><span data-stu-id="55ebe-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="55ebe-360">Tâche 2 : ajout d’une feuille de style CSS</span><span class="sxs-lookup"><span data-stu-id="55ebe-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="55ebe-361">Le modèle de projet vide comprend un fichier CSS très rationalisé qui comprend simplement les styles utilisés pour afficher les formulaires de base et les messages de validation.</span><span class="sxs-lookup"><span data-stu-id="55ebe-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="55ebe-362">Vous allez utiliser des CSS et des images supplémentaires (éventuellement fournies par un concepteur) afin d’améliorer l’apparence du site.</span><span class="sxs-lookup"><span data-stu-id="55ebe-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="55ebe-363">Dans cette tâche, vous allez ajouter une feuille de style CSS pour définir les styles du site.</span><span class="sxs-lookup"><span data-stu-id="55ebe-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="55ebe-364">Le fichier CSS et les images sont inclus dans le dossier **Source\Assets\Content** de cet atelier.</span><span class="sxs-lookup"><span data-stu-id="55ebe-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="55ebe-365">Pour les ajouter à l’application, faites glisser leur contenu à partir d’une fenêtre de l' **Explorateur Windows** vers le **Explorateur de solutions** dans Visual Studio Express pour le Web, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="55ebe-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="55ebe-366">![Glissement du contenu de style](aspnet-mvc-4-fundamentals/_static/image12.png "Glissement du contenu de style")</span><span class="sxs-lookup"><span data-stu-id="55ebe-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="55ebe-367">*Glissement du contenu de style*</span><span class="sxs-lookup"><span data-stu-id="55ebe-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="55ebe-368">Une boîte de dialogue d’avertissement s’affiche, vous demandant de confirmer le remplacement du fichier **site. CSS** et de certaines images existantes.</span><span class="sxs-lookup"><span data-stu-id="55ebe-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="55ebe-369">Cochez la case **appliquer à tous les éléments** , puis cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="55ebe-370">Tâche 3 : ajout d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="55ebe-371">Dans cette tâche, vous allez ajouter un modèle de vue pour générer la réponse HTML qui utilisera la page maître de disposition et le CSS ajouté dans cet exercice.</span><span class="sxs-lookup"><span data-stu-id="55ebe-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="55ebe-372">Pour utiliser un modèle de vue lorsque vous parcourez la page d’hébergement du site, vous devez d’abord indiquer qu’au lieu de retourner une chaîne, la méthode d' **index HomeController** renverra une **vue**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="55ebe-373">Ouvrez la classe **HomeController** et modifiez sa méthode d' **index** pour retourner un **ActionResult**et Return **View ()** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="55ebe-374">(Extrait de code- *notions de base de ASP.NET MVC 4-index HomeController EX4*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="55ebe-375">À présent, vous devez ajouter un modèle de vue approprié.</span><span class="sxs-lookup"><span data-stu-id="55ebe-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="55ebe-376">Pour ce faire, **cliquez avec le bouton droit** dans la méthode d’action **index** , puis sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="55ebe-377">La boîte de dialogue **Ajouter une vue s’affiche** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="55ebe-378">![Ajout d’une vue à partir de la méthode index](aspnet-mvc-4-fundamentals/_static/image13.png "Ajout d’une vue à partir de la méthode index")</span><span class="sxs-lookup"><span data-stu-id="55ebe-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="55ebe-379">*Ajout d’une vue à partir de la méthode index*</span><span class="sxs-lookup"><span data-stu-id="55ebe-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="55ebe-380">La boîte de dialogue **Ajouter une vue** s’affiche pour générer un fichier de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="55ebe-381">Par défaut, cette boîte de dialogue prérenseigne le nom du modèle de vue afin qu’il corresponde à la méthode d’action qui va l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="55ebe-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="55ebe-382">Étant donné que vous avez utilisé le menu contextuel **Ajouter une vue** dans la méthode d’action **index** au sein du HomeController, la boîte de dialogue **Ajouter une vue** contient index comme nom de vue par défaut.</span><span class="sxs-lookup"><span data-stu-id="55ebe-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="55ebe-383">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-383">Click **Add**.</span></span>

    <span data-ttu-id="55ebe-384">![Boîte de dialogue Ajouter une vue](aspnet-mvc-4-fundamentals/_static/image14.png "Boîte de dialogue Ajouter une vue")</span><span class="sxs-lookup"><span data-stu-id="55ebe-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="55ebe-385">*Boîte de dialogue Ajouter une vue*</span><span class="sxs-lookup"><span data-stu-id="55ebe-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="55ebe-386">Visual Studio génère un modèle d’affichage **index. cshtml** dans le dossier **Views\Home** , puis l’ouvre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="55ebe-387">![Vue d’index de démarrage créée](aspnet-mvc-4-fundamentals/_static/image15.png "Vue d’index de démarrage créée")</span><span class="sxs-lookup"><span data-stu-id="55ebe-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="55ebe-388">*Vue d’index de démarrage créée*</span><span class="sxs-lookup"><span data-stu-id="55ebe-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-389">le nom et l’emplacement du fichier **index. cshtml** sont pertinents et respectent les conventions d’affectation de noms ASP.NET MVC par défaut.</span><span class="sxs-lookup"><span data-stu-id="55ebe-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="55ebe-390">Le dossier \Views\*la *page d’hébergement*\* correspond au nom du contrôleur (contrôleur d'**hébergement** ).</span><span class="sxs-lookup"><span data-stu-id="55ebe-390">The folder \Views\**Home*\* matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="55ebe-391">Le nom du modèle de vue (**index**) correspond à la méthode d’action du contrôleur qui affiche la vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="55ebe-392">De cette façon, ASP.NET MVC évite d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lors de l’utilisation de cette Convention d’affectation de noms pour retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="55ebe-393">Le modèle de vue généré est basé sur le modèle **\_Layout. cshtml** précédemment défini.</span><span class="sxs-lookup"><span data-stu-id="55ebe-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="55ebe-394">Mettez **à jour**la propriété ViewBag. title et remplacez le contenu principal par celui de **la page d’accueil**, comme indiqué dans le code ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="55ebe-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="55ebe-395">Sélectionnez projet **MvcMusicStore** dans le Explorateur de solutions, puis appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="55ebe-396">Tâche 4 : vérification</span><span class="sxs-lookup"><span data-stu-id="55ebe-396">Task 4: Verification</span></span>

<span data-ttu-id="55ebe-397">Pour vérifier que vous avez correctement effectué toutes les étapes de l’exercice précédent, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="55ebe-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="55ebe-398">Une fois l’application ouverte dans un navigateur, vous devez noter que :</span><span class="sxs-lookup"><span data-stu-id="55ebe-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="55ebe-399">La méthode d’action d’index du HomeController a trouvé et affiché le modèle de vue **\Views\Home\Index.cshtml** , bien que le code appelé **Return View ()** , parce que le modèle de vue a suivi la Convention d’affectation de noms standard.</span><span class="sxs-lookup"><span data-stu-id="55ebe-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="55ebe-400">La page d’accueil affiche le message de bienvenue défini dans le modèle de vue **\Views\Home\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="55ebe-401">La page d’accueil utilise le modèle **\_Layout. cshtml** et le message d’accueil est donc contenu dans la disposition HTML du site standard.</span><span class="sxs-lookup"><span data-stu-id="55ebe-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="55ebe-402">![Affichage de l’index de démarrage à l’aide du LayoutPage et du style définis](aspnet-mvc-4-fundamentals/_static/image16.png "Affichage de l’index de démarrage à l’aide du LayoutPage et du style définis")</span><span class="sxs-lookup"><span data-stu-id="55ebe-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="55ebe-403">*Affichage de l’index de démarrage à l’aide du LayoutPage et du style définis*</span><span class="sxs-lookup"><span data-stu-id="55ebe-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="55ebe-404">Exercice 5 : création d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="55ebe-405">Jusqu’à présent, vous avez fait en sorte que vos vues affichent du code HTML codé en dur, mais, afin de créer des applications Web dynamiques, le modèle de vue doit recevoir des informations du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="55ebe-406">Une technique courante à utiliser à cet effet est le modèle **ViewModel** , qui permet à un contrôleur de regrouper toutes les informations nécessaires à la génération de la réponse HTML appropriée.</span><span class="sxs-lookup"><span data-stu-id="55ebe-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="55ebe-407">Dans cet exercice, vous allez apprendre à créer une classe ViewModel et à ajouter les propriétés requises : le nombre de genres dans le magasin et une liste de ces genres.</span><span class="sxs-lookup"><span data-stu-id="55ebe-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="55ebe-408">Vous allez également mettre à jour le StoreController pour utiliser le ViewModel créé et, enfin, vous allez créer un nouveau modèle de vue qui affichera les propriétés mentionnées dans la page.</span><span class="sxs-lookup"><span data-stu-id="55ebe-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="55ebe-409">Tâche 1 : création d’une classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="55ebe-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="55ebe-410">Dans cette tâche, vous allez créer une classe ViewModel qui implémentera le scénario de liste de genre de magasin.</span><span class="sxs-lookup"><span data-stu-id="55ebe-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="55ebe-411">S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="55ebe-412">Dans le menu **fichier** , choisissez **ouvrir un projet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="55ebe-413">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex05-CreatingAViewModel\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="55ebe-414">Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="55ebe-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="55ebe-415">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="55ebe-416">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="55ebe-417">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="55ebe-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="55ebe-418">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="55ebe-419">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="55ebe-420">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="55ebe-421">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="55ebe-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="55ebe-422">Créez un dossier **ViewModels** pour contenir le ViewModel.</span><span class="sxs-lookup"><span data-stu-id="55ebe-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="55ebe-423">Pour ce faire, cliquez avec le bouton droit sur le projet **MvcMusicStore** de niveau supérieur, sélectionnez **Ajouter** , puis **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="55ebe-424">![Ajout d’un nouveau dossier](aspnet-mvc-4-fundamentals/_static/image17.png "Ajout d’un nouveau dossier")</span><span class="sxs-lookup"><span data-stu-id="55ebe-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="55ebe-425">*Ajout d’un nouveau dossier*</span><span class="sxs-lookup"><span data-stu-id="55ebe-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="55ebe-426">Nommez le dossier *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="55ebe-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="55ebe-427">![Dossier ViewModels dans Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image18.png "Dossier ViewModels dans Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="55ebe-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="55ebe-428">*Dossier ViewModels dans Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="55ebe-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="55ebe-429">Créez une classe **ViewModel** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="55ebe-430">Pour ce faire, cliquez avec le bouton droit sur le dossier **ViewModels** créé récemment, sélectionnez **Ajouter** , puis **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="55ebe-431">Sous **code**, choisissez l’élément de **classe** et nommez le fichier *StoreIndexViewModel.cs*, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="55ebe-432">![Ajout d’une nouvelle classe](aspnet-mvc-4-fundamentals/_static/image19.png "Ajout d’une nouvelle classe")</span><span class="sxs-lookup"><span data-stu-id="55ebe-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="55ebe-433">*Ajout d’une nouvelle classe*</span><span class="sxs-lookup"><span data-stu-id="55ebe-433">*Adding a new Class*</span></span>

    <span data-ttu-id="55ebe-434">![Création de la classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Création de la classe StoreIndexViewModel")</span><span class="sxs-lookup"><span data-stu-id="55ebe-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="55ebe-435">*Création de la classe StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="55ebe-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="55ebe-436">Tâche 2 : ajout de propriétés à la classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="55ebe-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="55ebe-437">Deux paramètres doivent être passés du StoreController au modèle de vue pour générer la réponse HTML attendue : le nombre de genres dans le magasin et une liste de ces genres.</span><span class="sxs-lookup"><span data-stu-id="55ebe-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="55ebe-438">Dans cette tâche, vous allez ajouter ces 2 propriétés à la classe **StoreIndexViewModel** : **NumberOfGenres** (un entier) et **genres** (une liste de chaînes).</span><span class="sxs-lookup"><span data-stu-id="55ebe-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="55ebe-439">Ajoutez les propriétés **NumberOfGenres** et **genres** à la classe **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="55ebe-440">Pour ce faire, ajoutez les 2 lignes suivantes à la définition de classe :</span><span class="sxs-lookup"><span data-stu-id="55ebe-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="55ebe-441">(Extrait de code- *notions de base de ASP.NET MVC 4-propriétés de EX5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="55ebe-442">La notation **{obtient ; Set ;}** utilise la fonctionnalité C#de propriétés implémentées automatiquement de.</span><span class="sxs-lookup"><span data-stu-id="55ebe-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="55ebe-443">Elle offre les avantages d’une propriété sans que nous puissions déclarer un champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="55ebe-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="55ebe-444">Tâche 3 : mise à jour de StoreController pour utiliser StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="55ebe-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="55ebe-445">La classe **StoreIndexViewModel** encapsule les informations nécessaires pour passer de la méthode d' **index** de **StoreController**à un modèle de vue afin de générer une réponse.</span><span class="sxs-lookup"><span data-stu-id="55ebe-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="55ebe-446">Dans cette tâche, vous allez mettre à jour **StoreController** pour utiliser **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="55ebe-447">Ouvrez la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="55ebe-448">![Ouverture de la classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Ouverture de la classe StoreController")</span><span class="sxs-lookup"><span data-stu-id="55ebe-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="55ebe-449">*Ouverture de la classe StoreController*</span><span class="sxs-lookup"><span data-stu-id="55ebe-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="55ebe-450">Pour pouvoir utiliser la classe **StoreIndexViewModel** à partir de **StoreController**, ajoutez l’espace de noms suivant en haut du code **StoreController** :</span><span class="sxs-lookup"><span data-stu-id="55ebe-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="55ebe-451">(Extrait de code- *notions de base de ASP.NET MVC 4-EX5 StoreIndexViewModel à l’aide de ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="55ebe-452">Modifiez la méthode d’action d' **index** de **StoreController**afin qu’elle crée et remplisse un objet **StoreIndexViewModel** , puis la passe à un modèle de vue pour générer une réponse html.</span><span class="sxs-lookup"><span data-stu-id="55ebe-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-453">Dans Lab &quot;les modèles MVC ASP.NET et l’accès aux données&quot; vous allez écrire du code qui récupère la liste des genres du magasin à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="55ebe-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="55ebe-454">Dans le code suivant, vous allez créer une **liste** de genres de données factices qui rempliront le **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="55ebe-455">Une fois l’objet **StoreIndexViewModel** créé et configuré, il est passé comme argument à la méthode **View** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="55ebe-456">Cela indique que le modèle de vue utilisera cet objet pour générer une réponse HTML.</span><span class="sxs-lookup"><span data-stu-id="55ebe-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="55ebe-457">Remplacez la méthode d' **index** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="55ebe-458">(Extrait de code- *notions de base de ASP.NET MVC 4-méthode d’index EX5 StoreController*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="55ebe-459">Si vous n’êtes pas familiarisé C#avec, vous pouvez supposer que l’utilisation de **var** signifie que la variable **ViewModel** est à liaison tardive.</span><span class="sxs-lookup"><span data-stu-id="55ebe-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="55ebe-460">Ce n’est pas correct : C# le compilateur utilise l’inférence de type en fonction de ce que vous attribuez à la variable pour déterminer que le **ViewModel** est de type **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="55ebe-461">En outre, en compilant la variable **ViewModel** locale comme type **StoreIndexViewModel** , vous bénéficiez de la vérification au moment de la compilation et de la prise en charge de l’éditeur de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="55ebe-462">Tâche 4 : création d’un modèle de vue qui utilise StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="55ebe-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="55ebe-463">Dans cette tâche, vous allez créer un modèle de vue qui utilisera un objet StoreIndexViewModel transmis à partir du contrôleur pour afficher une liste de genres.</span><span class="sxs-lookup"><span data-stu-id="55ebe-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="55ebe-464">Avant de créer le modèle de vue, nous allons générer le projet afin que la **boîte de dialogue Ajouter une vue** sache la classe **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="55ebe-465">Générez le projet en sélectionnant l’élément de menu **générer** , puis **créez MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="55ebe-466">![Génération du projet](aspnet-mvc-4-fundamentals/_static/image22.png "Génération du projet")</span><span class="sxs-lookup"><span data-stu-id="55ebe-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="55ebe-467">*Génération du projet*</span><span class="sxs-lookup"><span data-stu-id="55ebe-467">*Building the project*</span></span>
2. <span data-ttu-id="55ebe-468">Créez un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-468">Create a new View template.</span></span> <span data-ttu-id="55ebe-469">Pour ce faire, cliquez avec le bouton droit à l’intérieur de la méthode **index** et sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="55ebe-470">![Ajout d’une vue](aspnet-mvc-4-fundamentals/_static/image23.png "Ajout d’une vue")</span><span class="sxs-lookup"><span data-stu-id="55ebe-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="55ebe-471">*Ajout d’une vue*</span><span class="sxs-lookup"><span data-stu-id="55ebe-471">*Adding a View*</span></span>
3. <span data-ttu-id="55ebe-472">Étant donné que la **boîte de dialogue Ajouter une vue** a été appelée à partir du **StoreController**, le modèle d’affichage est ajouté par défaut dans un fichier **\Views\Store\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="55ebe-473">Cochez la case **créer une vue fortement typée** , puis sélectionnez **StoreIndexViewModel** comme classe de **modèle**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="55ebe-474">En outre, assurez-vous que le moteur d’affichage sélectionné est **Razor**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="55ebe-475">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-475">Click **Add**.</span></span>

    <span data-ttu-id="55ebe-476">![Boîte de dialogue Ajouter une vue](aspnet-mvc-4-fundamentals/_static/image24.png "Boîte de dialogue Ajouter une vue")</span><span class="sxs-lookup"><span data-stu-id="55ebe-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="55ebe-477">*Boîte de dialogue Ajouter une vue*</span><span class="sxs-lookup"><span data-stu-id="55ebe-477">*Add View Dialog*</span></span>

    <span data-ttu-id="55ebe-478">Le fichier de modèle de vue **\Views\Store\Index.cshtml** est créé et ouvert.</span><span class="sxs-lookup"><span data-stu-id="55ebe-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="55ebe-479">En fonction des informations fournies dans la boîte de dialogue **Ajouter une vue** à la dernière étape, le modèle de vue attend une instance **StoreIndexViewModel** comme données à utiliser pour générer une réponse html.</span><span class="sxs-lookup"><span data-stu-id="55ebe-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="55ebe-480">Vous remarquerez que le modèle hérite d’un C#`ViewPage<musicstore.viewmodels.storeindexviewmodel>` dans.</span><span class="sxs-lookup"><span data-stu-id="55ebe-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="55ebe-481">Tâche 5 : mise à jour du modèle de vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="55ebe-482">Dans cette tâche, vous allez mettre à jour le modèle de vue créé dans la dernière tâche pour récupérer le nombre de genres et leurs noms dans la page.</span><span class="sxs-lookup"><span data-stu-id="55ebe-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="55ebe-483">Vous allez utiliser la syntaxe @ (souvent appelée &quot;code pépites&quot;) pour exécuter du code dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="55ebe-484">Dans le fichier **index. cshtml** , dans le dossier **Store** , remplacez son code par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="55ebe-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="55ebe-485">Parcourez la liste de genres dans **StoreIndexViewModel** et créez une liste HTML **&lt;UL&gt;** à l’aide d’une boucle **foreach** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="55ebe-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="55ebe-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="55ebe-487">Appuyez sur **F5** pour exécuter l’application et parcourir **/Store**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="55ebe-488">Vous verrez la liste des genres transmis à l’objet **StoreIndexViewModel** à partir du **StoreController** vers le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="55ebe-489">![Affichage affichant une liste de genres](aspnet-mvc-4-fundamentals/_static/image26.png "Affichage affichant une liste de genres")</span><span class="sxs-lookup"><span data-stu-id="55ebe-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="55ebe-490">*Affichage affichant une liste de genres*</span><span class="sxs-lookup"><span data-stu-id="55ebe-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="55ebe-491">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="55ebe-492">Exercice 6 : utilisation des paramètres dans la vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="55ebe-493">Dans l’exercice 3, vous avez appris à passer des paramètres au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="55ebe-494">Dans cet exercice, vous allez apprendre à utiliser ces paramètres dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="55ebe-495">À cet effet, vous allez tout d’abord créer des classes de modèle qui vous aideront à gérer vos données et la logique de domaine.</span><span class="sxs-lookup"><span data-stu-id="55ebe-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="55ebe-496">En outre, vous apprendrez à créer des liens vers des pages à l’intérieur de l’application ASP.NET MVC sans vous soucier de l’encodage des chemins d’URL.</span><span class="sxs-lookup"><span data-stu-id="55ebe-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="55ebe-497">Tâche 1 : ajout de classes de modèle</span><span class="sxs-lookup"><span data-stu-id="55ebe-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="55ebe-498">Contrairement à ViewModels, qui sont créés juste pour transmettre des informations du contrôleur à la vue, les classes de modèle sont conçues pour contenir et gérer la logique des données et du domaine.</span><span class="sxs-lookup"><span data-stu-id="55ebe-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="55ebe-499">Dans cette tâche, vous allez ajouter deux classes de modèle pour représenter ces concepts : **genre** et **album**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="55ebe-500">S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**</span><span class="sxs-lookup"><span data-stu-id="55ebe-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="55ebe-501">Dans le menu **fichier** , choisissez **ouvrir un projet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="55ebe-502">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex06-UsingParametersInView\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="55ebe-503">Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="55ebe-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="55ebe-504">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="55ebe-505">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="55ebe-506">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="55ebe-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="55ebe-507">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="55ebe-508">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="55ebe-509">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="55ebe-510">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="55ebe-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="55ebe-511">Ajoutez une classe de modèle de **genre** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="55ebe-512">Pour ce faire, cliquez avec le bouton droit sur le dossier **Models** dans le **Explorateur de solutions**, sélectionnez **Ajouter** , puis l’option **nouvel élément** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="55ebe-513">Sous **code**, choisissez l’élément de **classe** et nommez le fichier *genre.cs*, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="55ebe-514">![Ajout d’une classe](aspnet-mvc-4-fundamentals/_static/image27.png "Ajout d’une classe")</span><span class="sxs-lookup"><span data-stu-id="55ebe-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="55ebe-515">*Ajout d’un nouvel élément*</span><span class="sxs-lookup"><span data-stu-id="55ebe-515">*Adding a new item*</span></span>

    <span data-ttu-id="55ebe-516">![Ajouter une classe de modèle de genre](aspnet-mvc-4-fundamentals/_static/image28.png "Ajouter une classe de modèle de genre")</span><span class="sxs-lookup"><span data-stu-id="55ebe-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="55ebe-517">*Ajouter une classe de modèle de genre*</span><span class="sxs-lookup"><span data-stu-id="55ebe-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="55ebe-518">Ajoutez une propriété **Name** à la classe genre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="55ebe-519">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-519">To do this, add the following code:</span></span>

    <span data-ttu-id="55ebe-520">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 genre*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="55ebe-521">En suivant la même procédure qu’avant, ajoutez une classe **album** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="55ebe-522">Pour ce faire, cliquez avec le bouton droit sur le dossier **Models** dans le **Explorateur de solutions**, sélectionnez **Ajouter** , puis l’option **nouvel élément** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="55ebe-523">Sous **code**, choisissez l’élément de **classe** et nommez le fichier *album.cs*, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="55ebe-524">Ajoutez deux propriétés à la classe album : **genre** et **titre**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="55ebe-525">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-525">To do this, add the following code:</span></span>

    <span data-ttu-id="55ebe-526">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 album*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="55ebe-527">Tâche 2 : ajout d’un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="55ebe-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="55ebe-528">Un **StoreBrowseViewModel** sera utilisé dans cette tâche pour afficher les albums qui correspondent à un genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="55ebe-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="55ebe-529">Dans cette tâche, vous allez créer cette classe, puis ajouter deux propriétés pour gérer le **genre** et la liste de son **album**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="55ebe-530">Ajoutez une classe **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="55ebe-531">Pour ce faire, cliquez avec le bouton droit sur le dossier **ViewModels** dans le **Explorateur de solutions**, sélectionnez **Ajouter** , puis l’option **nouvel élément** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="55ebe-532">Sous **code**, choisissez l’élément de **classe** et nommez le fichier *StoreBrowseViewModel.cs*, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="55ebe-533">Ajoutez une référence aux modèles dans la classe **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="55ebe-534">Pour ce faire, ajoutez les éléments suivants à l’aide de l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="55ebe-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="55ebe-535">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="55ebe-536">Ajoutez deux propriétés à la classe **StoreBrowseViewModel** : **genre** et **albums**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="55ebe-537">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-537">To do this, add the following code:</span></span>

    <span data-ttu-id="55ebe-538">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="55ebe-539">Qu’est-ce que la **liste&lt;Album&gt;** ?: cette définition utilise le type de **liste&lt;t&gt;** , où **t** limite le type auquel les éléments de cette **liste** appartiennent, dans ce cas **album** (ou l’un de ses descendants).</span><span class="sxs-lookup"><span data-stu-id="55ebe-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="55ebe-540">Cette capacité à concevoir des classes et des méthodes qui diffèrent la spécification d’un ou plusieurs types jusqu’à ce que la classe ou la méthode soit déclarée et instanciée C# par le code client est une fonctionnalité du langage appelé **génériques**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="55ebe-541">**List&lt;t&gt;** est l’équivalent générique du type **ArrayList** et est disponible dans l’espace de noms **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="55ebe-542">L’un des avantages de l’utilisation des **génériques** est que puisque le type est spécifié, vous n’avez pas besoin de prendre en charge les opérations de vérification de type telles que le casting des éléments dans l' **album** comme vous le feriez avec une **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="55ebe-543">Tâche 3 : utilisation du nouveau ViewModel dans StoreController</span><span class="sxs-lookup"><span data-stu-id="55ebe-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="55ebe-544">Dans cette tâche, vous allez modifier les méthodes d’action **Browse** et **Details** de **StoreController**pour utiliser le nouveau **StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="55ebe-545">Ajoutez une référence au dossier Models dans la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="55ebe-546">Pour ce faire, développez le dossier **Controllers** dans le **Explorateur de solutions** et ouvrez la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="55ebe-547">Ajoutez ensuite le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-547">Then add the following code:</span></span>

    <span data-ttu-id="55ebe-548">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="55ebe-549">Remplacez la méthode d’action **Browse** pour utiliser la classe **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="55ebe-550">Vous allez créer un genre et deux nouveaux objets d’albums avec des données factices (dans le prochain atelier pratique, vous utiliserez les données réelles d’une base de données).</span><span class="sxs-lookup"><span data-stu-id="55ebe-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="55ebe-551">Pour ce faire, remplacez la méthode **Browse** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="55ebe-552">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="55ebe-553">Remplacez la méthode d’action **Details** pour utiliser la classe **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="55ebe-554">Vous allez créer un nouvel objet **album** à retourner à la **vue**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="55ebe-555">Pour ce faire, remplacez la méthode **Details** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="55ebe-556">(Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="55ebe-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="55ebe-557">Tâche 4 : ajout d’un modèle de vue Parcourir</span><span class="sxs-lookup"><span data-stu-id="55ebe-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="55ebe-558">Dans cette tâche, vous allez ajouter une vue **Parcourir** pour afficher les albums trouvés pour un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="55ebe-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="55ebe-559">Avant de créer le modèle de vue, vous devez générer le projet afin que la boîte de dialogue **Ajouter une vue** sache la classe **ViewModel** à utiliser.</span><span class="sxs-lookup"><span data-stu-id="55ebe-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="55ebe-560">Générez le projet en sélectionnant l’élément de menu **générer** , puis **créez MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="55ebe-561">Ajoutez une vue **Parcourir** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-561">Add a **Browse** View.</span></span> <span data-ttu-id="55ebe-562">Pour ce faire, cliquez avec le bouton droit dans la méthode d’action **Browse** du **StoreController** , puis cliquez sur **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="55ebe-563">Dans la boîte de dialogue **Ajouter une vue** , vérifiez que le nom de la vue est **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="55ebe-564">Cochez la case **créer une vue fortement typée** et sélectionnez **StoreBrowseViewModel** dans la liste déroulante **classe de modèle** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="55ebe-565">Laissez les autres champs avec leur valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="55ebe-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="55ebe-566">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-566">Then click **Add**.</span></span>

    <span data-ttu-id="55ebe-567">![Ajout d’une vue Parcourir](aspnet-mvc-4-fundamentals/_static/image29.png "Ajout d’une vue Parcourir")</span><span class="sxs-lookup"><span data-stu-id="55ebe-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="55ebe-568">*Ajout d’une vue Parcourir*</span><span class="sxs-lookup"><span data-stu-id="55ebe-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="55ebe-569">Modifiez **Browse. cshtml** pour afficher les informations du genre, en accédant à l’objet **StoreBrowseViewModel** qui est passé au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="55ebe-570">Pour ce faire, remplacez le contenu par le code suivant :C#()</span><span class="sxs-lookup"><span data-stu-id="55ebe-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="55ebe-571">Tâche 5 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="55ebe-572">Dans cette tâche, vous allez vérifier que la méthode **Browse** récupère des albums à partir de l’action de méthode **Browse** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="55ebe-573">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-574">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="55ebe-574">The project starts in the Home page.</span></span> <span data-ttu-id="55ebe-575">Remplacer l’URL par **/Store/Browse ? Genre = Disco** pour vérifier que l’action retourne deux albums.</span><span class="sxs-lookup"><span data-stu-id="55ebe-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="55ebe-576">![Exploration des albums de magasin Disco](aspnet-mvc-4-fundamentals/_static/image30.png "Exploration des albums de magasin Disco")</span><span class="sxs-lookup"><span data-stu-id="55ebe-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="55ebe-577">*Exploration des albums de magasin Disco*</span><span class="sxs-lookup"><span data-stu-id="55ebe-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="55ebe-578">Tâche 6 : affichage d’informations sur un album spécifique</span><span class="sxs-lookup"><span data-stu-id="55ebe-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="55ebe-579">Dans cette tâche, vous allez implémenter l’affichage **magasin/détails** pour afficher des informations sur un album spécifique.</span><span class="sxs-lookup"><span data-stu-id="55ebe-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="55ebe-580">Dans ce laboratoire pratique, tout ce que vous allez voir sur l’album est déjà contenu dans le modèle de **vue** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="55ebe-581">Ainsi, au lieu de créer une classe **StoreDetailsViewModel** , vous allez utiliser le modèle **StoreBrowseViewModel** actuel en lui transmettant l’album.</span><span class="sxs-lookup"><span data-stu-id="55ebe-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="55ebe-582">Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="55ebe-583">Ajoutez un nouvel affichage **Détails** pour la méthode d’action **Details** de **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="55ebe-584">Pour ce faire, cliquez avec le bouton droit sur la méthode **Details** dans la classe **StoreController** , puis cliquez sur **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="55ebe-585">Dans la boîte de dialogue **Ajouter une vue** , vérifiez que le nom de la **vue** est **Détails**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="55ebe-586">Cochez la case **créer une vue fortement typée** et sélectionnez **album** dans la liste déroulante **classe de modèle** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="55ebe-587">Laissez les autres champs avec leur valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="55ebe-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="55ebe-588">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-588">Then click **Add**.</span></span> <span data-ttu-id="55ebe-589">Cela permet de créer et d’ouvrir un fichier **\Views\Store\Details.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="55ebe-590">![Ajout d’un mode Détails](aspnet-mvc-4-fundamentals/_static/image31.png "Ajout d’un mode Détails")</span><span class="sxs-lookup"><span data-stu-id="55ebe-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="55ebe-591">*Ajout d’un mode Détails*</span><span class="sxs-lookup"><span data-stu-id="55ebe-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="55ebe-592">Modifiez le fichier **Details. cshtml** pour afficher les informations de l’album, en accédant à l’objet **album** qui est transmis au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="55ebe-593">Pour ce faire, remplacez le contenu par le code suivant :C#()</span><span class="sxs-lookup"><span data-stu-id="55ebe-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="55ebe-594">Tâche 7 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="55ebe-595">Dans cette tâche, vous allez vérifier que la vue **Détails** récupère les informations de l’album à partir de la méthode d' **action Details** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="55ebe-596">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-597">Le projet démarre sur la page d' **hébergement** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="55ebe-598">Remplacez l’URL par **/Store/Details/5** pour vérifier les informations de l’album.</span><span class="sxs-lookup"><span data-stu-id="55ebe-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="55ebe-599">![Exploration des détails des albums](aspnet-mvc-4-fundamentals/_static/image32.png "Exploration des détails des albums")</span><span class="sxs-lookup"><span data-stu-id="55ebe-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="55ebe-600">*Exploration des détails de l’album*</span><span class="sxs-lookup"><span data-stu-id="55ebe-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="55ebe-601">Tâche 8 : ajout de liens entre les pages</span><span class="sxs-lookup"><span data-stu-id="55ebe-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="55ebe-602">Au cours de cette tâche, vous allez ajouter un lien dans la vue Store pour avoir un lien dans chaque nom de genre vers l’URL **/Store/Browse** appropriée.</span><span class="sxs-lookup"><span data-stu-id="55ebe-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="55ebe-603">Ainsi, lorsque vous cliquez sur un genre, par exemple **Disco**, vous accédez à **/Store/Browse ? genre = URL Disco** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="55ebe-604">Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="55ebe-605">Mettez à jour la page d' **index** pour ajouter un lien vers la page de **navigation** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="55ebe-606">Pour ce faire, dans le **Explorateur de solutions** développez le dossier **views** , puis le dossier **Store** et double-cliquez sur la page **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="55ebe-607">Ajoutez un lien vers l’affichage de navigation indiquant le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="55ebe-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="55ebe-608">Pour ce faire, remplacez le code en surbrillance suivant dans les balises **&lt;li&gt;** : (C#)</span><span class="sxs-lookup"><span data-stu-id="55ebe-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="55ebe-609">une autre approche consisterait à établir une liaison directe avec la page, avec un code semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="55ebe-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="55ebe-610">&lt;a href =&quot;/Store/Browse ? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="55ebe-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="55ebe-611">Bien que cette approche fonctionne, elle dépend d’une chaîne codée en dur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="55ebe-612">Si, par la suite, vous renommez le contrôleur, vous devrez modifier cette instruction manuellement.</span><span class="sxs-lookup"><span data-stu-id="55ebe-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="55ebe-613">Une meilleure solution consiste à utiliser une méthode **d’assistance HTML** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="55ebe-614">ASP.NET MVC comprend une méthode d’assistance HTML qui est disponible pour de telles tâches.</span><span class="sxs-lookup"><span data-stu-id="55ebe-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="55ebe-615">La méthode d’assistance **html. ActionLink ()** facilite la génération de code HTML **&lt;un&gt;** des liens, en veillant à ce que les chemins d’URL soient correctement encodés dans une URL.</span><span class="sxs-lookup"><span data-stu-id="55ebe-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="55ebe-616">Html. ActionLink a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="55ebe-616">Html.ActionLink has several overloads.</span></span> <span data-ttu-id="55ebe-617">Dans cet exercice, vous allez utiliser un qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="55ebe-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="55ebe-618">Texte du lien qui affichera le nom du genre</span><span class="sxs-lookup"><span data-stu-id="55ebe-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="55ebe-619">Nom de l’action du contrôleur (**Parcourir**)</span><span class="sxs-lookup"><span data-stu-id="55ebe-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="55ebe-620">Router les valeurs des paramètres, en spécifiant à la fois le nom (**genre**) et la valeur (**genre Name**)</span><span class="sxs-lookup"><span data-stu-id="55ebe-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="55ebe-621">Tâche 9 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="55ebe-622">Dans cette tâche, vous allez vérifier que chaque genre s’affiche avec un lien vers l’URL **/Store/Browse** appropriée.</span><span class="sxs-lookup"><span data-stu-id="55ebe-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="55ebe-623">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-624">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="55ebe-624">The project starts in the Home page.</span></span> <span data-ttu-id="55ebe-625">Remplacez l’URL par **/Store** pour vérifier que chaque genre est lié à l’URL **/Store/Browse** appropriée.</span><span class="sxs-lookup"><span data-stu-id="55ebe-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="55ebe-626">![Exploration des genres avec des liens vers la page parcourir](aspnet-mvc-4-fundamentals/_static/image33.png "Exploration des genres avec des liens vers la page parcourir")</span><span class="sxs-lookup"><span data-stu-id="55ebe-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="55ebe-627">*Exploration des genres avec des liens vers la page parcourir*</span><span class="sxs-lookup"><span data-stu-id="55ebe-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="55ebe-628">Tâche 10-utilisation de la collection ViewModel dynamique pour passer des valeurs</span><span class="sxs-lookup"><span data-stu-id="55ebe-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="55ebe-629">Dans cette tâche, vous allez apprendre une méthode simple et puissante pour passer des valeurs entre le contrôleur et la vue sans apporter de modifications au modèle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="55ebe-630">ASP.NET MVC 4 fournit la collection &quot;&quot;ViewModel, qui peut être assignée à toute valeur dynamique et accessible dans les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="55ebe-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="55ebe-631">Vous allez maintenant utiliser la collection dynamique ViewBag pour transmettre une liste de &quot;**genres une étoile**&quot; du contrôleur à la vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="55ebe-632">La vue store index peut accéder à **ViewModel** et afficher les informations.</span><span class="sxs-lookup"><span data-stu-id="55ebe-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="55ebe-633">Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="55ebe-634">Ouvrez **StoreController.cs** et modifiez la méthode **index** pour créer une liste de genres une étoile dans la collection ViewModel :</span><span class="sxs-lookup"><span data-stu-id="55ebe-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="55ebe-635">Vous pouvez également utiliser la syntaxe **ViewBag [&quot;une étoile&quot;]** pour accéder aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="55ebe-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="55ebe-636">L’icône d’étoile **&quot;une étoile. png&quot;** est incluse dans le dossier **Source\Assets\Images** de ce Lab.</span><span class="sxs-lookup"><span data-stu-id="55ebe-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="55ebe-637">Pour l’ajouter à l’application, faites glisser son contenu à partir d’une fenêtre de l' **Explorateur Windows** vers le **Explorateur de solutions** dans Visual Web Developer Express, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="55ebe-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="55ebe-638">![Ajout d’une image étoile à la solution](aspnet-mvc-4-fundamentals/_static/image34.png "Ajout d’une image étoile à la solution")</span><span class="sxs-lookup"><span data-stu-id="55ebe-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="55ebe-639">*Ajout d’une image étoile à la solution*</span><span class="sxs-lookup"><span data-stu-id="55ebe-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="55ebe-640">Ouvrez la vue **Store/index. cshtml** et modifiez le contenu.</span><span class="sxs-lookup"><span data-stu-id="55ebe-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="55ebe-641">Vous allez lire la propriété &quot;une étoile&quot; dans la collection **ViewBag** et demander si le nom actuel du genre figure dans la liste.</span><span class="sxs-lookup"><span data-stu-id="55ebe-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="55ebe-642">Dans ce cas, vous affichez une icône d’étoile directement sur le lien genre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="55ebe-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="55ebe-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="55ebe-644">Tâche 11 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="55ebe-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="55ebe-645">Dans cette tâche, vous allez vérifier que les genres une étoile affichent une icône en étoile.</span><span class="sxs-lookup"><span data-stu-id="55ebe-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="55ebe-646">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="55ebe-647">Le projet démarre sur la page d' **hébergement** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="55ebe-648">Remplacez l’URL par **/Store** pour vérifier que chaque genre proposé a l’étiquette de respect :</span><span class="sxs-lookup"><span data-stu-id="55ebe-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="55ebe-649">![Exploration de genres avec des éléments une étoile](aspnet-mvc-4-fundamentals/_static/image35.png "Exploration de genres avec des éléments une étoile")</span><span class="sxs-lookup"><span data-stu-id="55ebe-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="55ebe-650">*Exploration de genres avec des éléments une étoile*</span><span class="sxs-lookup"><span data-stu-id="55ebe-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="55ebe-651">Exercice 7 : un nouveau modèle ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="55ebe-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="55ebe-652">Dans cet exercice, vous allez découvrir les améliorations apportées aux modèles de projet ASP.NET MVC 4, en examinant les fonctionnalités les plus pertinentes du nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="55ebe-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="55ebe-653">Tâche 1 : exploration du modèle d’application Internet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="55ebe-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="55ebe-654">S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**</span><span class="sxs-lookup"><span data-stu-id="55ebe-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="55ebe-655">Sélectionnez le **fichier | Nouveau |** Commande de menu projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="55ebe-656">Dans la boîte de dialogue **nouveau projet** , sélectionnez le **visuel C#| Modèle Web** dans l’arborescence du volet gauche, puis choisissez l' **Application Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="55ebe-657">**Nommez** le projet *MusicStore* et mettez à jour le nom de la **solution** pour *commencer*, puis sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="55ebe-658">![Création d’un nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Création d’un nouveau projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="55ebe-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="55ebe-659">*Création d’un nouveau projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="55ebe-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="55ebe-660">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de projet **application Internet** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="55ebe-661">Notez que vous pouvez sélectionner Razor ou ASPX comme moteur de vue.</span><span class="sxs-lookup"><span data-stu-id="55ebe-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="55ebe-662">![Création d’une nouvelle application Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Création d’une nouvelle application Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="55ebe-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="55ebe-663">*Création d’une nouvelle application Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="55ebe-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-664">Syntaxe Razor a été introduite dans ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="55ebe-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="55ebe-665">Son objectif est de réduire le nombre de caractères et les séquences de touches nécessaires dans un fichier, ce qui permet un flux de travail de codage rapide et fluide.</span><span class="sxs-lookup"><span data-stu-id="55ebe-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="55ebe-666">Razor exploite les C#compétences du langage/vb (ou d’autres) existantes et fournit une syntaxe de balisage de modèle qui permet un flux de travail de construction html impressionnant.</span><span class="sxs-lookup"><span data-stu-id="55ebe-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="55ebe-667">Appuyez sur **F5** pour exécuter la solution et voir le modèle renouvelé.</span><span class="sxs-lookup"><span data-stu-id="55ebe-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="55ebe-668">Vous pouvez consulter les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="55ebe-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="55ebe-669">**Modèles modernes**</span><span class="sxs-lookup"><span data-stu-id="55ebe-669">**Modern-style templates**</span></span>

        <span data-ttu-id="55ebe-670">Les modèles ont été renouvelés, ce qui fournit des styles plus modernes.</span><span class="sxs-lookup"><span data-stu-id="55ebe-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="55ebe-671">![Modèles ASP.NET MVC 4 restylisés](aspnet-mvc-4-fundamentals/_static/image38.png "Modèles ASP.NET MVC 4 restylisés")</span><span class="sxs-lookup"><span data-stu-id="55ebe-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="55ebe-672">*Modèles ASP.NET MVC 4 restylisés*</span><span class="sxs-lookup"><span data-stu-id="55ebe-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="55ebe-673">**Rendu adaptatif**</span><span class="sxs-lookup"><span data-stu-id="55ebe-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="55ebe-674">Examinez le redimensionnement de la fenêtre du navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre.</span><span class="sxs-lookup"><span data-stu-id="55ebe-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="55ebe-675">Ces modèles utilisent la technique de rendu adaptatif pour un rendu correct sur les plateformes de bureau et mobiles sans aucune personnalisation.</span><span class="sxs-lookup"><span data-stu-id="55ebe-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="55ebe-676">![Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur](aspnet-mvc-4-fundamentals/_static/image39.png "Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur")</span><span class="sxs-lookup"><span data-stu-id="55ebe-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="55ebe-677">*Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur*</span><span class="sxs-lookup"><span data-stu-id="55ebe-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="55ebe-678">Fermez le navigateur pour arrêter le débogueur et revenir à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="55ebe-679">Vous pouvez à présent explorer la solution et découvrir les nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="55ebe-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="55ebe-680">![ASP.NET MVC4-Internet-application-modèle de projet](aspnet-mvc-4-fundamentals/_static/image40.png "Modèle de projet d’application Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="55ebe-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="55ebe-681">*Modèle de projet d’application Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="55ebe-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="55ebe-682">**Balisage HTML5**</span><span class="sxs-lookup"><span data-stu-id="55ebe-682">**HTML5 markup**</span></span>

       <span data-ttu-id="55ebe-683">Parcourez les vues de modèle pour connaître le nouveau balisage du thème, par exemple ouvrir la vue **about. cshtml** dans le dossier de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="55ebe-684">![Nouveau modèle, à l’aide du balisage Razor et HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nouveau modèle, à l’aide du balisage Razor et HTML5")</span><span class="sxs-lookup"><span data-stu-id="55ebe-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="55ebe-685">*Nouveau modèle, à l’aide du balisage Razor et HTML5*</span><span class="sxs-lookup"><span data-stu-id="55ebe-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="55ebe-686">**Bibliothèques JavaScript incluses**</span><span class="sxs-lookup"><span data-stu-id="55ebe-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="55ebe-687">**jQuery**: jQuery simplifie le parcours des documents html, la gestion des événements, l’animation et les interactions Ajax.</span><span class="sxs-lookup"><span data-stu-id="55ebe-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="55ebe-688">**interface utilisateur jQuery**: cette bibliothèque fournit des abstractions pour l’interaction et l’animation de bas niveau, les effets avancés et les widgets thématiques, basés sur la bibliothèque JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="55ebe-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="55ebe-689">Vous pouvez en savoir plus sur jQuery et l’interface utilisateur jQuery dans [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="55ebe-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="55ebe-690">**KnockoutJS**: le modèle par défaut ASP.NET MVC 4 comprend désormais **KnockoutJS**, une infrastructure MVVM JavaScript qui vous permet de créer des applications Web riches et très réactives à l’aide de JavaScript et html.</span><span class="sxs-lookup"><span data-stu-id="55ebe-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="55ebe-691">Comme dans ASP.NET MVC 3, les bibliothèques d’interface utilisateur jQuery et jQuery sont également incluses dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="55ebe-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="55ebe-692">Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="55ebe-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="55ebe-693">**Modernizr**: cette bibliothèque s’exécute automatiquement, ce qui rend votre site compatible avec les anciens navigateurs lors de l’utilisation des technologies HTML5 et CSS3.</span><span class="sxs-lookup"><span data-stu-id="55ebe-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="55ebe-694">Vous pouvez obtenir plus d’informations sur la bibliothèque Modernizr dans ce lien : [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="55ebe-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="55ebe-695">**SimpleMembership inclus dans la solution**</span><span class="sxs-lookup"><span data-stu-id="55ebe-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="55ebe-696">SimpleMembership a été conçu pour remplacer le rôle ASP.NET et le système de fournisseur d’appartenances précédents.</span><span class="sxs-lookup"><span data-stu-id="55ebe-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="55ebe-697">Il offre de nombreuses nouvelles fonctionnalités qui facilitent l’utilisation du développeur pour sécuriser les pages Web de manière plus flexible.</span><span class="sxs-lookup"><span data-stu-id="55ebe-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="55ebe-698">Le modèle Internet a déjà configuré quelques éléments pour intégrer SimpleMembership, par exemple, AccountController est prêt à utiliser OAuthWebSecurity (pour l’inscription, la connexion, la gestion, etc. du compte OAuth) et la sécurité Web.</span><span class="sxs-lookup"><span data-stu-id="55ebe-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="55ebe-699">![SimpleMembership inclus dans la solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclus dans la solution")</span><span class="sxs-lookup"><span data-stu-id="55ebe-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="55ebe-700">*SimpleMembership inclus dans la solution*</span><span class="sxs-lookup"><span data-stu-id="55ebe-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="55ebe-701">Pour plus d’informations sur [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) , consultez MSDN.</span><span class="sxs-lookup"><span data-stu-id="55ebe-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="55ebe-702">En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="55ebe-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="55ebe-703">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="55ebe-703">Summary</span></span>

<span data-ttu-id="55ebe-704">En suivant ce laboratoire pratique, vous avez appris les notions de base de ASP.NET MVC :</span><span class="sxs-lookup"><span data-stu-id="55ebe-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="55ebe-705">Les éléments de base d’une application MVC et la façon dont ils interagissent</span><span class="sxs-lookup"><span data-stu-id="55ebe-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="55ebe-706">Comment créer une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="55ebe-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="55ebe-707">Comment ajouter et configurer des contrôleurs pour gérer les paramètres transmis par le biais de l’URL et de QueryString</span><span class="sxs-lookup"><span data-stu-id="55ebe-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="55ebe-708">Comment ajouter une page maître de disposition pour configurer un modèle pour du contenu HTML commun, une feuille de style pour améliorer l’apparence et un modèle de vue pour afficher du contenu HTML</span><span class="sxs-lookup"><span data-stu-id="55ebe-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="55ebe-709">Comment utiliser le modèle ViewModel pour passer des propriétés au modèle de vue pour afficher des informations dynamiques</span><span class="sxs-lookup"><span data-stu-id="55ebe-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="55ebe-710">Comment utiliser les paramètres passés aux contrôleurs dans le modèle de vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="55ebe-711">Comment ajouter des liens vers des pages à l’intérieur de l’application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="55ebe-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="55ebe-712">Comment ajouter et utiliser des propriétés dynamiques dans une vue</span><span class="sxs-lookup"><span data-stu-id="55ebe-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="55ebe-713">Les améliorations apportées aux modèles de projet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="55ebe-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="55ebe-714">Annexe A : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="55ebe-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="55ebe-715">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="55ebe-716">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="55ebe-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="55ebe-717">Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="55ebe-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="55ebe-718">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="55ebe-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="55ebe-719">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-719">Click on **Install Now**.</span></span> <span data-ttu-id="55ebe-720">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="55ebe-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="55ebe-721">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="55ebe-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="55ebe-722">![Installer Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="55ebe-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="55ebe-723">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="55ebe-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="55ebe-724">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="55ebe-726">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="55ebe-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="55ebe-727">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="55ebe-727">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="55ebe-729">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="55ebe-729">*Installation progress*</span></span>
6. <span data-ttu-id="55ebe-730">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-730">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="55ebe-732">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="55ebe-732">*Installation completed*</span></span>
7. <span data-ttu-id="55ebe-733">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="55ebe-734">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="55ebe-736">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="55ebe-737">Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="55ebe-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="55ebe-738">Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="55ebe-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="55ebe-739">Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure</span><span class="sxs-lookup"><span data-stu-id="55ebe-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="55ebe-740">Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="55ebe-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-741">Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="55ebe-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="55ebe-742">Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="55ebe-742">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="55ebe-743">![Ouvrir une session sur Windows Portail Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Ouvrir une session sur Windows Portail Azure")</span><span class="sxs-lookup"><span data-stu-id="55ebe-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="55ebe-744">*Connectez-vous à Windows Azure Portail de gestion*</span><span class="sxs-lookup"><span data-stu-id="55ebe-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="55ebe-745">Cliquez sur **nouveau** dans la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="55ebe-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="55ebe-746">![Création d’un nouveau site Web](aspnet-mvc-4-fundamentals/_static/image49.png "Création d’un nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="55ebe-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="55ebe-747">*Création d’un nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="55ebe-748">Cliquez sur **compute** | **site Web**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="55ebe-749">Sélectionnez ensuite l’option **création rapide** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="55ebe-750">Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-751">Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="55ebe-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="55ebe-752">L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="55ebe-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="55ebe-753">Elle n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="55ebe-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="55ebe-754">![Création d’un nouveau site Web à l’aide de la création rapide](aspnet-mvc-4-fundamentals/_static/image50.png "Création d’un nouveau site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="55ebe-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="55ebe-755">*Création d’un nouveau site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="55ebe-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="55ebe-756">Patientez jusqu’à la création du nouveau **site Web** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="55ebe-757">Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="55ebe-758">Vérifiez que le nouveau site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="55ebe-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="55ebe-759">![Navigation vers le nouveau site Web](aspnet-mvc-4-fundamentals/_static/image51.png "Navigation vers le nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="55ebe-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="55ebe-760">*Navigation vers le nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="55ebe-761">![Site Web en cours d’exécution](aspnet-mvc-4-fundamentals/_static/image52.png "Site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="55ebe-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="55ebe-762">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="55ebe-762">*Web site running*</span></span>
6. <span data-ttu-id="55ebe-763">Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="55ebe-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="55ebe-764">![Ouverture des pages de gestion de site Web](aspnet-mvc-4-fundamentals/_static/image53.png "Ouverture des pages de gestion de site Web")</span><span class="sxs-lookup"><span data-stu-id="55ebe-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="55ebe-765">*Ouverture des pages de gestion de site Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="55ebe-766">Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .</span><span class="sxs-lookup"><span data-stu-id="55ebe-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-767">Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="55ebe-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="55ebe-768">Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="55ebe-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="55ebe-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="55ebe-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="55ebe-770">![Téléchargement du profil de publication du site Web](aspnet-mvc-4-fundamentals/_static/image54.png "Téléchargement du profil de publication du site Web")</span><span class="sxs-lookup"><span data-stu-id="55ebe-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="55ebe-771">*Téléchargement du profil de publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="55ebe-772">Téléchargez le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="55ebe-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="55ebe-773">Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ebe-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="55ebe-774">![Enregistrement du fichier de profil de publication](aspnet-mvc-4-fundamentals/_static/image55.png "Enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="55ebe-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="55ebe-775">*Enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="55ebe-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="55ebe-776">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="55ebe-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="55ebe-777">Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database.</span><span class="sxs-lookup"><span data-stu-id="55ebe-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="55ebe-778">Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="55ebe-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="55ebe-779">Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="55ebe-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="55ebe-780">Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="55ebe-781">Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="55ebe-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="55ebe-782">Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="55ebe-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="55ebe-783">Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="55ebe-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="55ebe-784">![Tableau de bord du serveur SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "Tableau de bord du serveur SQL Database")</span><span class="sxs-lookup"><span data-stu-id="55ebe-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="55ebe-785">*Tableau de bord du serveur SQL Database*</span><span class="sxs-lookup"><span data-stu-id="55ebe-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="55ebe-786">Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="55ebe-787">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](aspnet-mvc-4-fundamentals/_static/image57.png).</span><span class="sxs-lookup"><span data-stu-id="55ebe-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Ajout de l’adresse IP du client](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="55ebe-789">*Ajout de l’adresse IP du client*</span><span class="sxs-lookup"><span data-stu-id="55ebe-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="55ebe-790">Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="55ebe-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="55ebe-792">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="55ebe-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="55ebe-793">Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="55ebe-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="55ebe-794">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="55ebe-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="55ebe-795">Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="55ebe-796">![Publication de l’application](aspnet-mvc-4-fundamentals/_static/image60.png "Publication de l'application")</span><span class="sxs-lookup"><span data-stu-id="55ebe-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="55ebe-797">*Publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="55ebe-798">Importez le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="55ebe-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="55ebe-799">![Importation du profil de publication](aspnet-mvc-4-fundamentals/_static/image61.png "Importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="55ebe-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="55ebe-800">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="55ebe-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="55ebe-801">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-801">Click **Validate Connection**.</span></span> <span data-ttu-id="55ebe-802">Une fois la validation terminée, cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55ebe-803">La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="55ebe-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="55ebe-804">![Validation de la connexion](aspnet-mvc-4-fundamentals/_static/image62.png "Validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="55ebe-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="55ebe-805">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="55ebe-805">*Validating connection*</span></span>
4. <span data-ttu-id="55ebe-806">Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="55ebe-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="55ebe-807">![Configuration de Web Deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Configuration de Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="55ebe-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="55ebe-808">*Configuration de Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="55ebe-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="55ebe-809">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="55ebe-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="55ebe-810">Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .</span><span class="sxs-lookup"><span data-stu-id="55ebe-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="55ebe-811">Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="55ebe-812">Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="55ebe-813">Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="55ebe-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="55ebe-814">![Configuration de la chaîne de connexion de destination](aspnet-mvc-4-fundamentals/_static/image64.png "Configuration de la chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="55ebe-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="55ebe-815">*Configuration de la chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="55ebe-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="55ebe-816">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-816">Then click **OK**.</span></span> <span data-ttu-id="55ebe-817">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="55ebe-818">![Création de la base de données](aspnet-mvc-4-fundamentals/_static/image65.png "Création de la chaîne de base de données")</span><span class="sxs-lookup"><span data-stu-id="55ebe-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="55ebe-819">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="55ebe-819">*Creating the database*</span></span>
7. <span data-ttu-id="55ebe-820">La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="55ebe-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="55ebe-821">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-821">Then click **Next**.</span></span>

    <span data-ttu-id="55ebe-822">![Chaîne de connexion pointant vers SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Chaîne de connexion pointant vers SQL Database")</span><span class="sxs-lookup"><span data-stu-id="55ebe-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="55ebe-823">*Chaîne de connexion pointant vers SQL Database*</span><span class="sxs-lookup"><span data-stu-id="55ebe-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="55ebe-824">Dans la page **Aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="55ebe-825">![Publication de l’application Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publication de l’application Web")</span><span class="sxs-lookup"><span data-stu-id="55ebe-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="55ebe-826">*Publication de l’application Web*</span><span class="sxs-lookup"><span data-stu-id="55ebe-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="55ebe-827">Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.</span><span class="sxs-lookup"><span data-stu-id="55ebe-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="55ebe-828">![Application publiée sur Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application publiée sur Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="55ebe-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="55ebe-829">*Application publiée sur Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="55ebe-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="55ebe-830">Annexe C : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="55ebe-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="55ebe-831">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="55ebe-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="55ebe-832">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="55ebe-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="55ebe-833">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-fundamentals/_static/image69.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="55ebe-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="55ebe-834">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="55ebe-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="55ebe-835">***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***</span><span class="sxs-lookup"><span data-stu-id="55ebe-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="55ebe-836">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="55ebe-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="55ebe-837">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="55ebe-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="55ebe-838">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="55ebe-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="55ebe-839">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="55ebe-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="55ebe-840">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="55ebe-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="55ebe-841">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-fundamentals/_static/image70.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="55ebe-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="55ebe-842">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="55ebe-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="55ebe-843">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-fundamentals/_static/image71.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="55ebe-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="55ebe-844">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="55ebe-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="55ebe-845">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-fundamentals/_static/image72.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="55ebe-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="55ebe-846">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="55ebe-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="55ebe-847">***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0.</span><span class="sxs-lookup"><span data-stu-id="55ebe-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="55ebe-848">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="55ebe-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="55ebe-849">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="55ebe-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="55ebe-850">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="55ebe-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="55ebe-851">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-fundamentals/_static/image73.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="55ebe-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="55ebe-852">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="55ebe-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="55ebe-853">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-fundamentals/_static/image74.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="55ebe-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="55ebe-854">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="55ebe-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
