---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Créer des API RESTful avec API Web ASP.NET ASP.NET 4. x
author: rick-anderson
description: 'Atelier pratique : utilisez l’API Web dans ASP.NET 4. x pour créer une API REST simple pour une application de gestionnaire de contacts.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621813"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="71a80-103">Créer des API RESTful avec API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71a80-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="71a80-104">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="71a80-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="71a80-105">Atelier pratique : utilisez l’API Web dans ASP.NET 4. x pour créer une API REST simple pour une application de gestionnaire de contacts.</span><span class="sxs-lookup"><span data-stu-id="71a80-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="71a80-106">Vous allez également créer un client pour consommer l’API.</span><span class="sxs-lookup"><span data-stu-id="71a80-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="71a80-107">Au cours des dernières années, il est devenu évident que le protocole HTTP ne sert pas seulement à traiter les pages HTML.</span><span class="sxs-lookup"><span data-stu-id="71a80-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="71a80-108">Il s’agit également d’une plateforme puissante pour créer des API Web, à l’aide de quelques verbes (obtenir, poster, etc.), ainsi que de quelques concepts simples tels que les *URI* et *les en-têtes*.</span><span class="sxs-lookup"><span data-stu-id="71a80-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="71a80-109">API Web ASP.NET est un ensemble de composants qui simplifient la programmation HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a80-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="71a80-110">Comme il repose sur le runtime ASP.NET MVC, l’API Web gère automatiquement les détails de transport de bas niveau de HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a80-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="71a80-111">En même temps, l’API Web expose naturellement le modèle de programmation HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a80-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="71a80-112">En fait, l’un des objectifs de l’API Web est de *ne pas* faire abstraction de la réalité du protocole http.</span><span class="sxs-lookup"><span data-stu-id="71a80-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="71a80-113">Par conséquent, l’API Web est à la fois flexible et facile à étendre.</span><span class="sxs-lookup"><span data-stu-id="71a80-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="71a80-114">Le style architectural REST s’est avéré être un moyen efficace de tirer parti du protocole HTTP, bien qu’il ne s’agisse pas de la seule approche valide pour HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a80-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="71a80-115">Le gestionnaire de contacts exposera la RESTful pour répertorier, ajouter et supprimer des contacts, entre autres.</span><span class="sxs-lookup"><span data-stu-id="71a80-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="71a80-116">Ce laboratoire requiert une compréhension de base du protocole HTTP, REST et suppose que vous avez une connaissance de base de HTML, JavaScript et jQuery.</span><span class="sxs-lookup"><span data-stu-id="71a80-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="71a80-117">Le site Web ASP.NET dispose d’un domaine dédié à l’infrastructure API Web ASP.NET dans [https://asp.net/web-api](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="71a80-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="71a80-118">Ce site continuera à fournir des informations, des exemples et des informations de dernière minute relatifs à l’API Web. il est donc important de le faire si vous souhaitez approfondir l’art de créer des API Web personnalisées accessibles à pratiquement n’importe quel Framework d’appareil ou de développement.</span><span class="sxs-lookup"><span data-stu-id="71a80-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="71a80-119">API Web ASP.NET, similaire à ASP.NET MVC 4, offre une grande flexibilité en termes de séparation de la couche de service des contrôleurs, ce qui vous permet d’utiliser assez facilement plusieurs infrastructures d’injection de dépendances disponibles.</span><span class="sxs-lookup"><span data-stu-id="71a80-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="71a80-120">Il existe un bon exemple dans MSDN qui montre comment utiliser Ninject pour l’injection de dépendances dans un projet API Web ASP.NET que vous pouvez télécharger à partir d' [ici](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="71a80-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="71a80-121">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="71a80-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="71a80-122">Objectifs</span><span class="sxs-lookup"><span data-stu-id="71a80-122">Objectives</span></span>

<span data-ttu-id="71a80-123">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="71a80-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="71a80-124">Implémenter une API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="71a80-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="71a80-125">Appeler l’API à partir d’un client HTML</span><span class="sxs-lookup"><span data-stu-id="71a80-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="71a80-126">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="71a80-126">Prerequisites</span></span>

<span data-ttu-id="71a80-127">Les éléments suivants sont requis pour effectuer ce laboratoire pratique :</span><span class="sxs-lookup"><span data-stu-id="71a80-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="71a80-128">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe B](#AppendixB) pour obtenir des instructions sur la façon de l’installer).</span><span class="sxs-lookup"><span data-stu-id="71a80-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="71a80-129">Installation</span><span class="sxs-lookup"><span data-stu-id="71a80-129">Setup</span></span>

<span data-ttu-id="71a80-130">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="71a80-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="71a80-131">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71a80-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="71a80-132">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="71a80-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="71a80-133">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;[annexe A : utilisation d’extraits de Code](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="71a80-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="71a80-134">Exercices</span><span class="sxs-lookup"><span data-stu-id="71a80-134">Exercises</span></span>

<span data-ttu-id="71a80-135">Ce laboratoire pratique comprend l’exercice suivant :</span><span class="sxs-lookup"><span data-stu-id="71a80-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="71a80-136">Exercice 1 : créer une API Web en lecture seule</span><span class="sxs-lookup"><span data-stu-id="71a80-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="71a80-137">Exercice 2 : créer une API Web en lecture/écriture</span><span class="sxs-lookup"><span data-stu-id="71a80-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="71a80-138">Exercice 3 : utilisation de l’API Web à partir d’un client HTML</span><span class="sxs-lookup"><span data-stu-id="71a80-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="71a80-139">Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="71a80-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="71a80-140">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.</span><span class="sxs-lookup"><span data-stu-id="71a80-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="71a80-141">Durée estimée pour effectuer ce laboratoire : **60 minutes**.</span><span class="sxs-lookup"><span data-stu-id="71a80-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="71a80-142">Exercice 1 : créer une API Web en lecture seule</span><span class="sxs-lookup"><span data-stu-id="71a80-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="71a80-143">Dans cet exercice, vous allez implémenter les méthodes d’extraction en lecture seule pour le gestionnaire de contacts.</span><span class="sxs-lookup"><span data-stu-id="71a80-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="71a80-144">Tâche 1 : création du projet d’API</span><span class="sxs-lookup"><span data-stu-id="71a80-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="71a80-145">Dans cette tâche, vous allez utiliser les nouveaux modèles de projet Web ASP.NET pour créer une application Web API Web.</span><span class="sxs-lookup"><span data-stu-id="71a80-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="71a80-146">Exécutez **Visual Studio 2012 Express pour le Web**. pour ce faire, accédez à **démarrer** , tapez **vs Express pour le Web** puis appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="71a80-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="71a80-147">Dans le menu **fichier** , sélectionnez **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="71a80-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="71a80-148">Sélectionner le **visuel C# |** Type de projet Web à partir de la vue d’arborescence de type de projet, puis sélectionnez le type de projet d' **Application Web ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="71a80-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="71a80-149">Définissez le **nom** du projet sur *ContactManager* et le **nom** de la solution à *commencer*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="71a80-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="71a80-150">![Création d’un nouveau projet d’application Web ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Création d’un nouveau projet d’application Web ASP.NET MVC 4,0")</span><span class="sxs-lookup"><span data-stu-id="71a80-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="71a80-151">*Création d’un nouveau projet d’application Web ASP.NET MVC 4,0*</span><span class="sxs-lookup"><span data-stu-id="71a80-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="71a80-152">Dans la boîte de dialogue type de projet ASP.NET MVC 4, sélectionnez le type de projet **API Web** .</span><span class="sxs-lookup"><span data-stu-id="71a80-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="71a80-153">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="71a80-153">Click **OK**.</span></span>

    <span data-ttu-id="71a80-154">![Spécification du type de projet d’API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Spécification du type de projet d’API Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="71a80-155">*Spécification du type de projet d’API Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="71a80-156">Tâche 2 : création des contrôleurs d’API du gestionnaire de contacts</span><span class="sxs-lookup"><span data-stu-id="71a80-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="71a80-157">Dans cette tâche, vous allez créer les classes de contrôleur dans lesquelles les méthodes d’API résideront.</span><span class="sxs-lookup"><span data-stu-id="71a80-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="71a80-158">Supprimez le fichier nommé **ValuesController.cs** dans le dossier **Controllers** du projet.</span><span class="sxs-lookup"><span data-stu-id="71a80-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="71a80-159">Cliquez avec le bouton droit sur le dossier **Controllers** dans le projet et sélectionnez **Ajouter | Dans le** menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="71a80-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="71a80-160">![Ajout d’un nouveau contrôleur au projet](build-restful-apis-with-aspnet-web-api/_static/image3.png "Ajout d’un nouveau contrôleur au projet")</span><span class="sxs-lookup"><span data-stu-id="71a80-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="71a80-161">*Ajout d’un nouveau contrôleur au projet*</span><span class="sxs-lookup"><span data-stu-id="71a80-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="71a80-162">Dans la boîte de dialogue **Ajouter un contrôleur** qui s’affiche, sélectionnez **contrôleur d’API vide** dans le menu modèle.</span><span class="sxs-lookup"><span data-stu-id="71a80-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="71a80-163">Nommez la classe de contrôleur **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="71a80-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="71a80-164">Cliquez ensuite sur **Ajouter.**</span><span class="sxs-lookup"><span data-stu-id="71a80-164">Then, click **Add.**</span></span>

    <span data-ttu-id="71a80-165">![Utilisation de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur d’API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Utilisation de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur d’API Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="71a80-166">*Utilisation de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur d’API Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="71a80-167">Ajoutez le code suivant à **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="71a80-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="71a80-168">(Extrait de code- *API Web Lab-Ex01-obtient la méthode API*)</span><span class="sxs-lookup"><span data-stu-id="71a80-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="71a80-169">Appuyez sur **F5** pour déboguer l’application.</span><span class="sxs-lookup"><span data-stu-id="71a80-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="71a80-170">La page d’hébergement par défaut d’un projet d’API Web doit s’afficher.</span><span class="sxs-lookup"><span data-stu-id="71a80-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="71a80-171">![Page d’hébergement par défaut d’une application API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Page d’hébergement par défaut d’une application API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="71a80-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="71a80-172">*Page d’hébergement par défaut d’une application API Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="71a80-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="71a80-173">Dans la fenêtre Internet Explorer, appuyez sur la touche **F12** pour ouvrir la fenêtre **outils de développement** .</span><span class="sxs-lookup"><span data-stu-id="71a80-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="71a80-174">Cliquez sur l’onglet **réseau** , puis sur le bouton **Démarrer la capture** pour commencer à capturer le trafic réseau dans la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="71a80-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="71a80-175">![Ouverture de l’onglet réseau et lancement de la capture réseau](build-restful-apis-with-aspnet-web-api/_static/image6.png "Ouverture de l’onglet réseau et lancement de la capture réseau")</span><span class="sxs-lookup"><span data-stu-id="71a80-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="71a80-176">*Ouverture de l’onglet réseau et lancement de la capture réseau*</span><span class="sxs-lookup"><span data-stu-id="71a80-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="71a80-177">Ajoutez l’URL dans la barre d’adresse du navigateur avec **/API/contact** et appuyez sur entrée.</span><span class="sxs-lookup"><span data-stu-id="71a80-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="71a80-178">Les détails de la transmission s’affichent dans la fenêtre capture réseau.</span><span class="sxs-lookup"><span data-stu-id="71a80-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="71a80-179">Notez que le type MIME de la réponse est **application/JSON**.</span><span class="sxs-lookup"><span data-stu-id="71a80-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="71a80-180">Cela montre comment le format de sortie par défaut est JSON.</span><span class="sxs-lookup"><span data-stu-id="71a80-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="71a80-181">![Affichage de la sortie de la requête d’API Web dans la vue réseau](build-restful-apis-with-aspnet-web-api/_static/image7.png "Affichage de la sortie de la requête d’API Web dans la vue réseau")</span><span class="sxs-lookup"><span data-stu-id="71a80-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="71a80-182">*Affichage de la sortie de la requête d’API Web dans la vue réseau*</span><span class="sxs-lookup"><span data-stu-id="71a80-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="71a80-183">À ce stade, le comportement par défaut d’Internet Explorer 10 consiste à demander si l’utilisateur souhaite enregistrer ou ouvrir le flux résultant de l’appel de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="71a80-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="71a80-184">La sortie est un fichier texte contenant le résultat JSON de l’appel de l’URL de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="71a80-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="71a80-185">N’annulez pas la boîte de dialogue afin de pouvoir regarder le contenu de la réponse via la fenêtre de l’outil développeurs.</span><span class="sxs-lookup"><span data-stu-id="71a80-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="71a80-186">Cliquez sur le bouton **accéder à la vue détaillée** pour afficher plus de détails sur la réponse de cet appel d’API.</span><span class="sxs-lookup"><span data-stu-id="71a80-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="71a80-187">![Basculer vers la vue détaillée](build-restful-apis-with-aspnet-web-api/_static/image8.png "Basculer vers le mode Détails")</span><span class="sxs-lookup"><span data-stu-id="71a80-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="71a80-188">*Basculer vers la vue détaillée*</span><span class="sxs-lookup"><span data-stu-id="71a80-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="71a80-189">Cliquez sur l’onglet corps de la **réponse** pour afficher le texte de réponse JSON réel.</span><span class="sxs-lookup"><span data-stu-id="71a80-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="71a80-190">![Affichage du texte de sortie JSON dans le moniteur réseau](build-restful-apis-with-aspnet-web-api/_static/image9.png "Affichage du texte de sortie JSON dans le moniteur réseau")</span><span class="sxs-lookup"><span data-stu-id="71a80-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="71a80-191">*Affichage du texte de sortie JSON dans le moniteur réseau*</span><span class="sxs-lookup"><span data-stu-id="71a80-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="71a80-192">Tâche 3 : création des modèles de contact et ajout du contrôleur de contact</span><span class="sxs-lookup"><span data-stu-id="71a80-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="71a80-193">Dans cette tâche, vous allez créer les classes de contrôleur dans lesquelles les méthodes d’API résideront.</span><span class="sxs-lookup"><span data-stu-id="71a80-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="71a80-194">Cliquez avec le bouton droit sur le dossier **modèles** et sélectionnez **Ajouter | Classe...** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="71a80-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="71a80-195">![Ajout d’un nouveau modèle à l’application Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Ajout d’un nouveau modèle à l’application Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="71a80-196">*Ajout d’un nouveau modèle à l’application Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="71a80-197">Dans la boîte de dialogue **Ajouter un nouvel élément** , nommez le nouveau fichier **contact.cs** , puis cliquez sur **Ajouter.**</span><span class="sxs-lookup"><span data-stu-id="71a80-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="71a80-198">![Création du fichier de classe contact](build-restful-apis-with-aspnet-web-api/_static/image11.png "Création du fichier de classe contact")</span><span class="sxs-lookup"><span data-stu-id="71a80-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="71a80-199">*Création du fichier de classe contact*</span><span class="sxs-lookup"><span data-stu-id="71a80-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="71a80-200">Ajoutez le code en surbrillance suivant à la classe **contact** .</span><span class="sxs-lookup"><span data-stu-id="71a80-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="71a80-201">(Extrait de code- *Lab API Web-Ex01-classe contact*)</span><span class="sxs-lookup"><span data-stu-id="71a80-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="71a80-202">Dans la classe **ContactController** , sélectionnez la **chaîne** de mot dans la définition de méthode de la méthode d' **extraction** , puis tapez le mot *contact*.</span><span class="sxs-lookup"><span data-stu-id="71a80-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="71a80-203">Une fois le mot tapé, un indicateur s’affiche au début du **contact**Word.</span><span class="sxs-lookup"><span data-stu-id="71a80-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="71a80-204">Maintenez la touche **CTRL** enfoncée et appuyez sur la touche point (.) ou cliquez sur l’icône à l’aide de la souris pour ouvrir la boîte de dialogue d’assistance dans l’éditeur de code, afin de renseigner automatiquement la directive **using** pour l’espace de noms Models.</span><span class="sxs-lookup"><span data-stu-id="71a80-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Utilisation de l’assistance IntelliSense pour les déclarations d’espaces de noms](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="71a80-206">*Utilisation de l’assistance IntelliSense pour les déclarations d’espaces de noms*</span><span class="sxs-lookup"><span data-stu-id="71a80-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="71a80-207">Modifiez le code de la méthode d' **extraction** afin qu’il retourne un tableau d’instances de modèle de contact.</span><span class="sxs-lookup"><span data-stu-id="71a80-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="71a80-208">(Extrait de code- *API Web-Lab-Ex01-retournant une liste de contacts*)</span><span class="sxs-lookup"><span data-stu-id="71a80-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="71a80-209">Appuyez sur **F5** pour déboguer l’application Web dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="71a80-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="71a80-210">Pour afficher les modifications apportées à la sortie de réponse de l’API, procédez comme suit.</span><span class="sxs-lookup"><span data-stu-id="71a80-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="71a80-211">Une fois que le navigateur s’ouvre, appuyez sur **F12** si les outils de développement ne sont pas encore ouverts.</span><span class="sxs-lookup"><span data-stu-id="71a80-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="71a80-212">Cliquez sur l’onglet **réseau** .</span><span class="sxs-lookup"><span data-stu-id="71a80-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="71a80-213">Appuyez sur le bouton **Démarrer la capture** .</span><span class="sxs-lookup"><span data-stu-id="71a80-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="71a80-214">Ajoutez le suffixe d’URL **/API/contact** à l’URL dans la barre d’adresses et appuyez sur la touche **entrée** .</span><span class="sxs-lookup"><span data-stu-id="71a80-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="71a80-215">Appuyez sur le bouton **accéder à la vue détaillée** .</span><span class="sxs-lookup"><span data-stu-id="71a80-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="71a80-216">Sélectionnez l’onglet corps de la **réponse** . Vous devez voir une chaîne JSON représentant la forme sérialisée d’un tableau d’instances de contact.</span><span class="sxs-lookup"><span data-stu-id="71a80-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="71a80-217">![Sortie sérialisée JSON d’un appel de méthode d’API Web complexe](build-restful-apis-with-aspnet-web-api/_static/image13.png "Sortie sérialisée JSON d’un appel de méthode d’API Web complexe")</span><span class="sxs-lookup"><span data-stu-id="71a80-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="71a80-218">*Sortie sérialisée JSON d’un appel de méthode d’API Web complexe*</span><span class="sxs-lookup"><span data-stu-id="71a80-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="71a80-219">Tâche 4 : extraction des fonctionnalités dans une couche de service</span><span class="sxs-lookup"><span data-stu-id="71a80-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="71a80-220">Cette tâche montre comment extraire des fonctionnalités dans une couche de service pour permettre aux développeurs de séparer facilement leurs fonctionnalités de service de la couche de contrôleur, autorisant ainsi la réutilisation des services qui effectuent le travail.</span><span class="sxs-lookup"><span data-stu-id="71a80-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="71a80-221">Créez un nouveau dossier dans la racine de la solution et nommez-le **services**informatiques.</span><span class="sxs-lookup"><span data-stu-id="71a80-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="71a80-222">Pour ce faire, cliquez avec le bouton droit sur le projet **ContactManager** , sélectionnez **Ajouter** | **nouveau dossier**, puis nommez-le *services*informatiques.</span><span class="sxs-lookup"><span data-stu-id="71a80-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="71a80-223">![Création du dossier services](build-restful-apis-with-aspnet-web-api/_static/image14.png "Création du dossier services")</span><span class="sxs-lookup"><span data-stu-id="71a80-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="71a80-224">*Création du dossier services*</span><span class="sxs-lookup"><span data-stu-id="71a80-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="71a80-225">Cliquez avec le bouton droit sur le dossier **services** et sélectionnez **Ajouter | Classe...** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="71a80-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="71a80-226">![Ajout d’une nouvelle classe au dossier services](build-restful-apis-with-aspnet-web-api/_static/image15.png "Ajout d’une nouvelle classe au dossier services")</span><span class="sxs-lookup"><span data-stu-id="71a80-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="71a80-227">*Ajout d’une nouvelle classe au dossier services*</span><span class="sxs-lookup"><span data-stu-id="71a80-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="71a80-228">Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, nommez la nouvelle classe **ContactRepository** , puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="71a80-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="71a80-229">![Création d’un fichier de classe pour contenir le code de la couche de service du dépôt de contacts](build-restful-apis-with-aspnet-web-api/_static/image16.png "Création d’un fichier de classe pour contenir le code de la couche de service du dépôt de contacts")</span><span class="sxs-lookup"><span data-stu-id="71a80-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="71a80-230">*Création d’un fichier de classe pour contenir le code de la couche de service du dépôt de contacts*</span><span class="sxs-lookup"><span data-stu-id="71a80-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="71a80-231">Ajoutez une directive using au fichier **ContactRepository.cs** pour inclure l’espace de noms Models.</span><span class="sxs-lookup"><span data-stu-id="71a80-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="71a80-232">Ajoutez le code en surbrillance suivant au fichier **ContactRepository.cs** pour implémenter la méthode GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="71a80-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="71a80-233">(Extrait de code- *Lab API Web-Ex01-dépôt du contact*)</span><span class="sxs-lookup"><span data-stu-id="71a80-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="71a80-234">Ouvrez le fichier **ContactController.cs** s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="71a80-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="71a80-235">Ajoutez l’instruction using suivante à la section de déclaration d’espace de noms du fichier.</span><span class="sxs-lookup"><span data-stu-id="71a80-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="71a80-236">Ajoutez le code en surbrillance suivant à la classe **ContactController.cs** pour ajouter un champ privé afin de représenter l’instance du référentiel, afin que les autres membres de la classe puissent utiliser l’implémentation du service.</span><span class="sxs-lookup"><span data-stu-id="71a80-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="71a80-237">(Extrait de code- *Lab API Web-Ex01-Contact Controller*)</span><span class="sxs-lookup"><span data-stu-id="71a80-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="71a80-238">Modifiez la méthode d' **extraction** afin qu’elle utilise le service de référentiel de contacts.</span><span class="sxs-lookup"><span data-stu-id="71a80-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="71a80-239">(Extrait de code- *Lab API Web-Ex01-renvoi d’une liste de contacts via le référentiel*)</span><span class="sxs-lookup"><span data-stu-id="71a80-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="71a80-240">Placez un point d’arrêt sur la définition de méthode d' **extraction** de **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="71a80-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="71a80-241">![Ajout de points d’arrêt au contrôleur de contact](build-restful-apis-with-aspnet-web-api/_static/image17.png "Ajout de points d’arrêt au contrôleur de contact")</span><span class="sxs-lookup"><span data-stu-id="71a80-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="71a80-242">*Ajout de points d’arrêt au contrôleur de contact*</span><span class="sxs-lookup"><span data-stu-id="71a80-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="71a80-243">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="71a80-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="71a80-244">Lorsque le navigateur s’ouvre, appuyez sur **F12** pour ouvrir les outils de développement.</span><span class="sxs-lookup"><span data-stu-id="71a80-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="71a80-245">Cliquez sur l’onglet **réseau** .</span><span class="sxs-lookup"><span data-stu-id="71a80-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="71a80-246">Cliquez sur le bouton **Démarrer la capture** .</span><span class="sxs-lookup"><span data-stu-id="71a80-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="71a80-247">Ajoutez l’URL dans la barre d’adresses avec le suffixe **/API/contact** et appuyez sur **entrée** pour charger le contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="71a80-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="71a80-248">Visual Studio 2012 doit s’arrêter une fois que l’exécution de la méthode de **récupération** commence.</span><span class="sxs-lookup"><span data-stu-id="71a80-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="71a80-249">![Rupture dans la méthode d’extraction](build-restful-apis-with-aspnet-web-api/_static/image18.png "Rupture dans la méthode d’extraction")</span><span class="sxs-lookup"><span data-stu-id="71a80-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="71a80-250">*Rupture dans la méthode d’extraction*</span><span class="sxs-lookup"><span data-stu-id="71a80-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="71a80-251">Appuyez sur **F5** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="71a80-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="71a80-252">Revenez à Internet Explorer s’il n’est pas déjà actif.</span><span class="sxs-lookup"><span data-stu-id="71a80-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="71a80-253">Notez la fenêtre de capture réseau.</span><span class="sxs-lookup"><span data-stu-id="71a80-253">Note the network capture window.</span></span>

    <span data-ttu-id="71a80-254">![Vue réseau dans Internet Explorer présentant les résultats de l’appel de l’API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Vue réseau dans Internet Explorer présentant les résultats de l’appel de l’API Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="71a80-255">*Vue réseau dans Internet Explorer présentant les résultats de l’appel de l’API Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="71a80-256">Cliquez sur le bouton **accéder à la vue détaillée** .</span><span class="sxs-lookup"><span data-stu-id="71a80-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="71a80-257">Cliquez sur l’onglet corps de la **réponse** . Notez la sortie JSON de l’appel d’API et la façon dont il représente les deux contacts récupérés par la couche de service.</span><span class="sxs-lookup"><span data-stu-id="71a80-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="71a80-258">![Affichage de la sortie JSON de l’API Web dans la fenêtre outils de développement](build-restful-apis-with-aspnet-web-api/_static/image20.png "Affichage de la sortie JSON de l’API Web dans la fenêtre outils de développement")</span><span class="sxs-lookup"><span data-stu-id="71a80-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="71a80-259">*Affichage de la sortie JSON de l’API Web dans la fenêtre outils de développement*</span><span class="sxs-lookup"><span data-stu-id="71a80-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="71a80-260">Exercice 2 : créer une API Web en lecture/écriture</span><span class="sxs-lookup"><span data-stu-id="71a80-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="71a80-261">Dans cet exercice, vous allez implémenter des méthodes de publication et de placement pour que le gestionnaire de contacts les active avec les fonctionnalités d’édition de données.</span><span class="sxs-lookup"><span data-stu-id="71a80-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="71a80-262">Tâche 1 : ouverture du projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="71a80-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="71a80-263">Dans cette tâche, vous allez préparer l’amélioration du projet d’API Web créé dans l’exercice 1 afin qu’il puisse accepter les entrées utilisateur.</span><span class="sxs-lookup"><span data-stu-id="71a80-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="71a80-264">Exécutez **Visual Studio 2012 Express pour le Web**. pour ce faire, accédez à **démarrer** , tapez **vs Express pour le Web** puis appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="71a80-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="71a80-265">Ouvrez le **début** de la solution situé dans **source/Ex02-ReadWriteWebAPI/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="71a80-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="71a80-266">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="71a80-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="71a80-267">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="71a80-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="71a80-268">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="71a80-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="71a80-269">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="71a80-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="71a80-270">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="71a80-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="71a80-271">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="71a80-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="71a80-272">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="71a80-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="71a80-273">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="71a80-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="71a80-274">Ouvrez le fichier **services/ContactRepository. cs** .</span><span class="sxs-lookup"><span data-stu-id="71a80-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="71a80-275">Tâche 2 : ajout de fonctionnalités de persistance des données à l’implémentation du dépôt de contacts</span><span class="sxs-lookup"><span data-stu-id="71a80-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="71a80-276">Dans cette tâche, vous allez augmenter la classe ContactRepository du projet d’API Web créé dans l’exercice 1 afin qu’elle puisse conserver et accepter les entrées d’utilisateur et les nouvelles instances de contact.</span><span class="sxs-lookup"><span data-stu-id="71a80-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="71a80-277">Ajoutez la constante suivante à la classe **ContactRepository** pour représenter le nom du nom de clé de l’élément de cache du serveur Web plus loin dans cet exercice.</span><span class="sxs-lookup"><span data-stu-id="71a80-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="71a80-278">Ajoutez un constructeur au **ContactRepository** contenant le code suivant.</span><span class="sxs-lookup"><span data-stu-id="71a80-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="71a80-279">(Extrait de code- *Lab API Web-Ex02-constructeur du dépôt de contacts*)</span><span class="sxs-lookup"><span data-stu-id="71a80-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="71a80-280">Modifiez le code de la méthode **GetAllContacts** comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="71a80-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="71a80-281">(Extrait de code- *API Web-Lab-Ex02-obtient tous les contacts*)</span><span class="sxs-lookup"><span data-stu-id="71a80-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="71a80-282">Cet exemple est fourni à des fins de démonstration et utilise le cache du serveur Web comme support de stockage, afin que les valeurs soient disponibles simultanément pour plusieurs clients, au lieu d’utiliser un mécanisme de stockage de session ou une durée de vie de stockage des demandes.</span><span class="sxs-lookup"><span data-stu-id="71a80-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="71a80-283">Vous pouvez utiliser Entity Framework, le stockage XML ou toute autre variété à la place du cache du serveur Web.</span><span class="sxs-lookup"><span data-stu-id="71a80-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="71a80-284">Implémentez une nouvelle méthode nommée **SaveContact** à la classe **ContactRepository** pour effectuer le travail d’enregistrement d’un contact.</span><span class="sxs-lookup"><span data-stu-id="71a80-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="71a80-285">La méthode **SaveContact** doit accepter un seul paramètre de **contact** et retourner une valeur booléenne indiquant la réussite ou l’échec.</span><span class="sxs-lookup"><span data-stu-id="71a80-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="71a80-286">(Extrait de code- *Lab API Web-Ex02-implémentation de la méthode SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="71a80-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="71a80-287">Exercice 3 : utilisation de l’API Web à partir d’un client HTML</span><span class="sxs-lookup"><span data-stu-id="71a80-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="71a80-288">Dans cet exercice, vous allez créer un client HTML pour appeler l’API Web.</span><span class="sxs-lookup"><span data-stu-id="71a80-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="71a80-289">Ce client facilite l’échange de données avec l’API Web à l’aide de JavaScript et affiche les résultats dans un navigateur Web à l’aide du balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="71a80-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="71a80-290">Tâche 1 : modification de la vue d’index pour fournir une interface utilisateur graphique pour l’affichage des contacts</span><span class="sxs-lookup"><span data-stu-id="71a80-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="71a80-291">Dans cette tâche, vous allez modifier la vue d’index par défaut de l’application Web pour qu’elle prenne en charge l’affichage de la liste des contacts existants dans un navigateur HTML.</span><span class="sxs-lookup"><span data-stu-id="71a80-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="71a80-292">Ouvrez **Visual Studio 2012 Express pour le Web** s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="71a80-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="71a80-293">Ouvrez le **début** de la solution situé dans **source/Ex03-ConsumingWebAPI/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="71a80-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="71a80-294">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="71a80-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="71a80-295">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="71a80-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="71a80-296">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="71a80-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="71a80-297">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="71a80-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="71a80-298">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="71a80-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="71a80-299">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="71a80-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="71a80-300">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="71a80-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="71a80-301">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="71a80-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="71a80-302">Ouvrez le fichier **index. cshtml** situé dans **views/** dossier de démarrage.</span><span class="sxs-lookup"><span data-stu-id="71a80-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="71a80-303">Remplacez le code HTML dans l’élément div par le **corps** de l’ID afin qu’il ressemble au code suivant.</span><span class="sxs-lookup"><span data-stu-id="71a80-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="71a80-304">Ajoutez le code JavaScript suivant au bas du fichier pour effectuer la requête HTTP à l’API Web.</span><span class="sxs-lookup"><span data-stu-id="71a80-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="71a80-305">Ouvrez le fichier **ContactController.cs** s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="71a80-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="71a80-306">Placez un point d’arrêt sur la méthode d' **extraction** de la classe **ContactController** .</span><span class="sxs-lookup"><span data-stu-id="71a80-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="71a80-307">![Placement d’un point d’arrêt sur la méthode d’extraction du contrôleur d’API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placement d’un point d’arrêt sur la méthode d’extraction du contrôleur d’API")</span><span class="sxs-lookup"><span data-stu-id="71a80-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="71a80-308">*Placement d’un point d’arrêt sur la méthode d’extraction du contrôleur d’API*</span><span class="sxs-lookup"><span data-stu-id="71a80-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="71a80-309">Appuyez sur **F5** pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="71a80-309">Press **F5** to run the project.</span></span> <span data-ttu-id="71a80-310">Le navigateur chargera le document HTML.</span><span class="sxs-lookup"><span data-stu-id="71a80-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="71a80-311">Assurez-vous que vous accédez à l’URL racine de votre application.</span><span class="sxs-lookup"><span data-stu-id="71a80-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="71a80-312">Une fois la page chargée et le JavaScript exécuté, le point d’arrêt est atteint et l’exécution du code s’interrompt dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="71a80-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="71a80-313">![Débogage dans les appels d’API Web à l’aide de VS Express pour le Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Débogage dans les appels d’API Web à l’aide de VS Express pour le Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="71a80-314">*Débogage dans l’appel d’API Web à l’aide de Visual Studio 2012 Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="71a80-315">Supprimez le point d’arrêt et appuyez sur **F5** ou sur le bouton **Continuer** de la barre d’outils de débogage pour continuer à charger la vue dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="71a80-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="71a80-316">Une fois l’appel de l’API Web terminé, vous devez voir les contacts renvoyés à partir de l’appel d’API Web affiché en tant qu’éléments de liste dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="71a80-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="71a80-317">![Résultats de l’appel d’API affiché dans le navigateur en tant qu’éléments de liste](build-restful-apis-with-aspnet-web-api/_static/image23.png "Résultats de l’appel d’API affiché dans le navigateur en tant qu’éléments de liste")</span><span class="sxs-lookup"><span data-stu-id="71a80-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="71a80-318">*Résultats de l’appel d’API affiché dans le navigateur en tant qu’éléments de liste*</span><span class="sxs-lookup"><span data-stu-id="71a80-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="71a80-319">Arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="71a80-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="71a80-320">Tâche 2 : modification de la vue d’index pour fournir une interface utilisateur graphique pour la création de contacts</span><span class="sxs-lookup"><span data-stu-id="71a80-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="71a80-321">Dans cette tâche, vous allez continuer à modifier la vue index de l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="71a80-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="71a80-322">Un formulaire est ajouté à la page HTML qui capture l’entrée utilisateur et l’envoie à l’API Web pour créer un nouveau contact, et une nouvelle méthode de contrôleur d’API Web est créée pour collecter la date à partir de l’interface graphique.</span><span class="sxs-lookup"><span data-stu-id="71a80-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="71a80-323">Ouvrez le fichier **ContactController.cs** .</span><span class="sxs-lookup"><span data-stu-id="71a80-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="71a80-324">Ajoutez une nouvelle méthode à la classe de contrôleur nommée « **publication** », comme indiqué dans le code suivant.</span><span class="sxs-lookup"><span data-stu-id="71a80-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="71a80-325">(Extrait de code- *API Web Lab-Ex03-méthode de publication*)</span><span class="sxs-lookup"><span data-stu-id="71a80-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="71a80-326">Ouvrez le fichier **index. cshtml** dans Visual Studio s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="71a80-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="71a80-327">Ajoutez le code HTML ci-dessous au fichier juste après la liste non triée que vous avez ajoutée au cours de la tâche précédente.</span><span class="sxs-lookup"><span data-stu-id="71a80-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="71a80-328">Dans l’élément script situé en bas du document, ajoutez le code en surbrillance suivant pour gérer les événements de clic de bouton, qui publient les données dans l’API Web à l’aide d’un appel HTTP postérieur.</span><span class="sxs-lookup"><span data-stu-id="71a80-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="71a80-329">Dans **ContactController.cs**, placez un point d’arrêt sur la méthode de **publication** .</span><span class="sxs-lookup"><span data-stu-id="71a80-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="71a80-330">Appuyez sur **F5** pour exécuter l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="71a80-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="71a80-331">Une fois la page chargée dans le navigateur, tapez un nouveau nom et un nouvel ID de contact, puis cliquez sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="71a80-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="71a80-332">![Document HTML client chargé dans le navigateur](build-restful-apis-with-aspnet-web-api/_static/image24.png "Document HTML client chargé dans le navigateur")</span><span class="sxs-lookup"><span data-stu-id="71a80-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="71a80-333">*Document HTML client chargé dans le navigateur*</span><span class="sxs-lookup"><span data-stu-id="71a80-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="71a80-334">Lorsque la fenêtre du débogueur s’arrête dans la méthode de **publication** , examinez les propriétés du paramètre de **contact** .</span><span class="sxs-lookup"><span data-stu-id="71a80-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="71a80-335">Les valeurs doivent correspondre aux données que vous avez entrées dans le formulaire.</span><span class="sxs-lookup"><span data-stu-id="71a80-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="71a80-336">![Objet contact envoyé à l’API Web à partir du client](build-restful-apis-with-aspnet-web-api/_static/image25.png "Objet contact envoyé à l’API Web à partir du client")</span><span class="sxs-lookup"><span data-stu-id="71a80-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="71a80-337">*Objet contact envoyé à l’API Web à partir du client*</span><span class="sxs-lookup"><span data-stu-id="71a80-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="71a80-338">Exécutez pas à pas la méthode dans le débogueur jusqu’à ce que la variable de **réponse** ait été créée.</span><span class="sxs-lookup"><span data-stu-id="71a80-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="71a80-339">Une fois l’inspection effectuée dans la fenêtre **variables locales** du débogueur, vous verrez que toutes les propriétés ont été définies.</span><span class="sxs-lookup"><span data-stu-id="71a80-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="71a80-340">![La réponse qui suit la création dans le débogueur](build-restful-apis-with-aspnet-web-api/_static/image26.png "La réponse qui suit la création dans le débogueur")</span><span class="sxs-lookup"><span data-stu-id="71a80-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="71a80-341">*La réponse qui suit la création dans le débogueur*</span><span class="sxs-lookup"><span data-stu-id="71a80-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="71a80-342">Si vous appuyez sur **F5** ou cliquez sur **Continuer** dans le débogueur, la demande se termine.</span><span class="sxs-lookup"><span data-stu-id="71a80-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="71a80-343">Une fois que vous revenez dans le navigateur, le nouveau contact a été ajouté à la liste des contacts stockés par l’implémentation de **ContactRepository** .</span><span class="sxs-lookup"><span data-stu-id="71a80-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="71a80-344">![Le navigateur reflète la création réussie de la nouvelle instance de contact](build-restful-apis-with-aspnet-web-api/_static/image27.png "Le navigateur reflète la création réussie de la nouvelle instance de contact")</span><span class="sxs-lookup"><span data-stu-id="71a80-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="71a80-345">*Le navigateur reflète la création réussie de la nouvelle instance de contact*</span><span class="sxs-lookup"><span data-stu-id="71a80-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="71a80-346">En outre, vous pouvez déployer cette application sur Azure à l' [aide de l’annexe C : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="71a80-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="71a80-347">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="71a80-347">Summary</span></span>

<span data-ttu-id="71a80-348">Ce laboratoire vous a présenté le nouvel API Web ASP.NET Framework et l’implémentation des API Web RESTful à l’aide de l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="71a80-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="71a80-349">À partir de là, vous pouvez créer un référentiel qui facilite la persistance des données à l’aide de n’importe quel nombre de mécanismes et relier ce service au lieu du simple, fourni comme exemple dans ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="71a80-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="71a80-350">L’API Web prend en charge un certain nombre de fonctionnalités supplémentaires, telles que l’activation de la communication à partir de clients non HTML écrits dans n’importe quel langage qui prend en charge les protocoles HTTP et JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="71a80-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="71a80-351">La possibilité d’héberger une API Web en dehors d’une application Web classique est également possible, ainsi que la possibilité de créer vos propres formats de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="71a80-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="71a80-352">Le site Web ASP.NET dispose d’un domaine dédié à l’infrastructure API Web ASP.NET dans [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="71a80-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="71a80-353">Ce site continuera à fournir des informations, des exemples et des informations de dernière minute relatifs à l’API Web. il est donc important de le faire si vous souhaitez approfondir l’art de créer des API Web personnalisées accessibles à pratiquement n’importe quel Framework d’appareil ou de développement.</span><span class="sxs-lookup"><span data-stu-id="71a80-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="71a80-354">Annexe A : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="71a80-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="71a80-355">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="71a80-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="71a80-356">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="71a80-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="71a80-357">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](build-restful-apis-with-aspnet-web-api/_static/image28.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="71a80-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="71a80-358">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="71a80-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="71a80-359">Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)</span><span class="sxs-lookup"><span data-stu-id="71a80-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="71a80-360">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="71a80-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="71a80-361">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="71a80-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="71a80-362">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="71a80-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="71a80-363">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="71a80-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="71a80-364">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="71a80-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="71a80-365">![Commencez à taper le nom de l’extrait de code](build-restful-apis-with-aspnet-web-api/_static/image29.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="71a80-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="71a80-366">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="71a80-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="71a80-367">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](build-restful-apis-with-aspnet-web-api/_static/image30.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="71a80-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="71a80-368">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="71a80-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="71a80-369">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](build-restful-apis-with-aspnet-web-api/_static/image31.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="71a80-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="71a80-370">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="71a80-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="71a80-371">Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)</span><span class="sxs-lookup"><span data-stu-id="71a80-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="71a80-372">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="71a80-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="71a80-373">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="71a80-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="71a80-374">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="71a80-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="71a80-375">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](build-restful-apis-with-aspnet-web-api/_static/image32.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="71a80-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="71a80-376">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="71a80-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="71a80-377">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](build-restful-apis-with-aspnet-web-api/_static/image33.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="71a80-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="71a80-378">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="71a80-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="71a80-379">Annexe B : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="71a80-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="71a80-380">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="71a80-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="71a80-381">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="71a80-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="71a80-382">Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="71a80-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="71a80-383">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="71a80-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="71a80-384">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="71a80-384">Click on **Install Now**.</span></span> <span data-ttu-id="71a80-385">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="71a80-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="71a80-386">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="71a80-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="71a80-387">![Installer Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="71a80-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="71a80-388">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="71a80-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="71a80-389">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="71a80-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="71a80-391">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="71a80-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="71a80-392">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="71a80-392">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="71a80-394">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="71a80-394">*Installation progress*</span></span>
6. <span data-ttu-id="71a80-395">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="71a80-395">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="71a80-397">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="71a80-397">*Installation completed*</span></span>
7. <span data-ttu-id="71a80-398">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="71a80-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="71a80-399">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="71a80-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="71a80-401">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="71a80-402">Annexe C : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="71a80-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="71a80-403">Cette annexe va vous montrer comment créer un nouveau site Web à partir du portail Azure et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Azure.</span><span class="sxs-lookup"><span data-stu-id="71a80-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="71a80-404">Tâche 1 : création d’un nouveau site Web à partir du portail Azure</span><span class="sxs-lookup"><span data-stu-id="71a80-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="71a80-405">Accédez à la [portail de gestion Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="71a80-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="71a80-406">Avec Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="71a80-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="71a80-407">Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="71a80-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="71a80-408">![Ouvrir une session sur Windows Portail Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Ouvrir une session sur Windows Portail Azure")</span><span class="sxs-lookup"><span data-stu-id="71a80-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="71a80-409">*Se connecter au portail*</span><span class="sxs-lookup"><span data-stu-id="71a80-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="71a80-410">Cliquez sur **nouveau** dans la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="71a80-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="71a80-411">![Création d’un nouveau site Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Création d’un nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="71a80-412">*Création d’un nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="71a80-413">Cliquez sur **compute** | **site Web**.</span><span class="sxs-lookup"><span data-stu-id="71a80-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="71a80-414">Sélectionnez ensuite l’option **création rapide** .</span><span class="sxs-lookup"><span data-stu-id="71a80-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="71a80-415">Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.</span><span class="sxs-lookup"><span data-stu-id="71a80-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="71a80-416">Azure est l’hôte d’une application Web exécutée dans le Cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="71a80-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="71a80-417">L’option création rapide vous permet de déployer une application Web terminée sur Azure à partir de l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="71a80-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="71a80-418">Elle n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="71a80-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="71a80-419">![Création d’un nouveau site Web à l’aide de la création rapide](build-restful-apis-with-aspnet-web-api/_static/image41.png "Création d’un nouveau site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="71a80-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="71a80-420">*Création d’un nouveau site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="71a80-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="71a80-421">Patientez jusqu’à la création du nouveau **site Web** .</span><span class="sxs-lookup"><span data-stu-id="71a80-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="71a80-422">Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** .</span><span class="sxs-lookup"><span data-stu-id="71a80-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="71a80-423">Vérifiez que le nouveau site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="71a80-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="71a80-424">![Navigation vers le nouveau site Web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navigation vers le nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="71a80-425">*Navigation vers le nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="71a80-426">![Site Web en cours d’exécution](build-restful-apis-with-aspnet-web-api/_static/image43.png "Site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="71a80-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="71a80-427">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="71a80-427">*Web site running*</span></span>
6. <span data-ttu-id="71a80-428">Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="71a80-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="71a80-429">![Ouverture des pages de gestion de site Web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Ouverture des pages de gestion de site Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="71a80-430">*Ouverture des pages de gestion de site Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="71a80-431">Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .</span><span class="sxs-lookup"><span data-stu-id="71a80-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="71a80-432">Le *profil de publication* contient toutes les informations requises pour publier une application Web sur Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="71a80-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="71a80-433">Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="71a80-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="71a80-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur Azure.</span><span class="sxs-lookup"><span data-stu-id="71a80-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="71a80-435">![Téléchargement du profil de publication du site Web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Téléchargement du profil de publication du site Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="71a80-436">*Téléchargement du profil de publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="71a80-437">Téléchargez le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="71a80-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="71a80-438">Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71a80-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="71a80-439">![Enregistrement du fichier de profil de publication](build-restful-apis-with-aspnet-web-api/_static/image46.png "Enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="71a80-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="71a80-440">*Enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="71a80-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="71a80-441">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="71a80-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="71a80-442">Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database.</span><span class="sxs-lookup"><span data-stu-id="71a80-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="71a80-443">Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="71a80-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="71a80-444">Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="71a80-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="71a80-445">Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Azure, dans **bases de données SQL** | **serveurs** | **tableau de bord du serveur**.</span><span class="sxs-lookup"><span data-stu-id="71a80-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="71a80-446">Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="71a80-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="71a80-447">Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="71a80-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="71a80-448">Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="71a80-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="71a80-449">![Tableau de bord du serveur SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "Tableau de bord du serveur SQL Database")</span><span class="sxs-lookup"><span data-stu-id="71a80-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="71a80-450">*Tableau de bord du serveur SQL Database*</span><span class="sxs-lookup"><span data-stu-id="71a80-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="71a80-451">Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur.</span><span class="sxs-lookup"><span data-stu-id="71a80-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="71a80-452">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](build-restful-apis-with-aspnet-web-api/_static/image48.png).</span><span class="sxs-lookup"><span data-stu-id="71a80-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Ajout de l’adresse IP du client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="71a80-454">*Ajout de l’adresse IP du client*</span><span class="sxs-lookup"><span data-stu-id="71a80-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="71a80-455">Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="71a80-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="71a80-457">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="71a80-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="71a80-458">Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="71a80-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="71a80-459">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="71a80-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="71a80-460">Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="71a80-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="71a80-461">![Publication de l’application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publication de l'application")</span><span class="sxs-lookup"><span data-stu-id="71a80-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="71a80-462">*Publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="71a80-463">Importez le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="71a80-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="71a80-464">![Importation du profil de publication](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="71a80-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="71a80-465">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="71a80-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="71a80-466">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="71a80-466">Click **Validate Connection**.</span></span> <span data-ttu-id="71a80-467">Une fois la validation terminée, cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="71a80-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="71a80-468">La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="71a80-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="71a80-469">![Validation de la connexion](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="71a80-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="71a80-470">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="71a80-470">*Validating connection*</span></span>
4. <span data-ttu-id="71a80-471">Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="71a80-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="71a80-472">![Configuration de Web Deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuration de Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="71a80-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="71a80-473">*Configuration de Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="71a80-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="71a80-474">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="71a80-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="71a80-475">Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .</span><span class="sxs-lookup"><span data-stu-id="71a80-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="71a80-476">Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="71a80-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="71a80-477">Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="71a80-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="71a80-478">Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="71a80-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="71a80-479">![Configuration de la chaîne de connexion de destination](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuration de la chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="71a80-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="71a80-480">*Configuration de la chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="71a80-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="71a80-481">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="71a80-481">Then click **OK**.</span></span> <span data-ttu-id="71a80-482">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="71a80-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="71a80-483">![Création de la base de données](build-restful-apis-with-aspnet-web-api/_static/image56.png "Création de la chaîne de base de données")</span><span class="sxs-lookup"><span data-stu-id="71a80-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="71a80-484">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="71a80-484">*Creating the database*</span></span>
7. <span data-ttu-id="71a80-485">La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="71a80-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="71a80-486">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="71a80-486">Then click **Next**.</span></span>

    <span data-ttu-id="71a80-487">![Chaîne de connexion pointant vers SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Chaîne de connexion pointant vers SQL Database")</span><span class="sxs-lookup"><span data-stu-id="71a80-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="71a80-488">*Chaîne de connexion pointant vers SQL Database*</span><span class="sxs-lookup"><span data-stu-id="71a80-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="71a80-489">Dans la page **Aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="71a80-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="71a80-490">![Publication de l’application Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publication de l’application Web")</span><span class="sxs-lookup"><span data-stu-id="71a80-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="71a80-491">*Publication de l’application Web*</span><span class="sxs-lookup"><span data-stu-id="71a80-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="71a80-492">Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.</span><span class="sxs-lookup"><span data-stu-id="71a80-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="71a80-493">![Application publiée sur Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application publiée sur Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="71a80-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="71a80-494">*Application publiée sur Azure*</span><span class="sxs-lookup"><span data-stu-id="71a80-494">*Application published to Azure*</span></span>
