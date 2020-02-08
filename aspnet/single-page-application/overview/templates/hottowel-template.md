---
uid: single-page-application/overview/templates/hottowel-template
title: Modèle d’essuie à chaud | Microsoft Docs
author: madskristensen
description: Modèle HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075058"
---
# <a name="hot-towel-template"></a><span data-ttu-id="f3565-103">Modèle Hot Towel</span><span class="sxs-lookup"><span data-stu-id="f3565-103">Hot Towel template</span></span>

<span data-ttu-id="f3565-104">par [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="f3565-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="f3565-105">Le modèle MVC de la serviette chaude est écrit par John Papa</span><span class="sxs-lookup"><span data-stu-id="f3565-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="f3565-106">Choisissez la version à télécharger :</span><span class="sxs-lookup"><span data-stu-id="f3565-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="f3565-107">Modèle MVC pour les serviettes à chaud pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f3565-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="f3565-108">Modèle MVC pour les serviettes à chaud pour Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f3565-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="f3565-109">Serviette en chaud : parce que vous ne souhaitez pas accéder au SPA sans l’utiliser !</span><span class="sxs-lookup"><span data-stu-id="f3565-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>

<span data-ttu-id="f3565-110">Vous souhaitez créer un SPA, mais vous ne pouvez pas décider où commencer ?</span><span class="sxs-lookup"><span data-stu-id="f3565-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="f3565-111">Utilisez une serviette et, en quelques secondes, vous disposerez d’un SPA et de tous les outils dont vous avez besoin pour créer vos images !</span><span class="sxs-lookup"><span data-stu-id="f3565-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="f3565-112">La serviette constitue un excellent point de départ pour la création d’une application à page unique (SPA) avec ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f3565-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="f3565-113">Vous pouvez dès à présent disposer d’une structure modulaire pour votre code, de la navigation dans l’affichage, de la liaison de données, de la gestion de données enrichie et du style simple mais élégant.</span><span class="sxs-lookup"><span data-stu-id="f3565-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="f3565-114">La serviette à chaud fournit tout ce dont vous avez besoin pour créer un SPA, ce qui vous permet de vous concentrer sur votre application, et non sur la plomberie.</span><span class="sxs-lookup"><span data-stu-id="f3565-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="f3565-115">En savoir plus sur la création d’un SPA à partir des [vidéos, didacticiels et cours Pluralsight de John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="f3565-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>

## <a name="application-structure"></a><span data-ttu-id="f3565-116">Structure d'application</span><span class="sxs-lookup"><span data-stu-id="f3565-116">Application Structure</span></span>

<span data-ttu-id="f3565-117">Le SPA à chaud fournit un dossier d’application qui contient les fichiers JavaScript et HTML qui définissent votre application.</span><span class="sxs-lookup"><span data-stu-id="f3565-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="f3565-118">Dans le dossier de l’application :</span><span class="sxs-lookup"><span data-stu-id="f3565-118">Inside the App folder:</span></span>

- <span data-ttu-id="f3565-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="f3565-119">durandal</span></span>
- <span data-ttu-id="f3565-120">services</span><span class="sxs-lookup"><span data-stu-id="f3565-120">services</span></span>
- <span data-ttu-id="f3565-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="f3565-121">viewmodels</span></span>
- <span data-ttu-id="f3565-122">views</span><span class="sxs-lookup"><span data-stu-id="f3565-122">views</span></span>

<span data-ttu-id="f3565-123">Le dossier d’application contient une collection de modules.</span><span class="sxs-lookup"><span data-stu-id="f3565-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="f3565-124">Ces modules encapsulent les fonctionnalités et déclarent des dépendances sur d’autres modules.</span><span class="sxs-lookup"><span data-stu-id="f3565-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="f3565-125">Le dossier views contient le code HTML de votre application et le dossier ViewModels contient la logique de présentation des vues (modèle MVVM courant).</span><span class="sxs-lookup"><span data-stu-id="f3565-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="f3565-126">Le dossier services est idéal pour héberger tous les services courants dont votre application peut avoir besoin, tels que l’extraction de données HTTP ou l’interaction de stockage local.</span><span class="sxs-lookup"><span data-stu-id="f3565-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="f3565-127">Il est courant que plusieurs ViewModels réutilisent le code des modules de service.</span><span class="sxs-lookup"><span data-stu-id="f3565-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="f3565-128">Structure d’application côté serveur ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f3565-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="f3565-129">La serviette chaude repose sur la structure ASP.NET MVC familière et puissante.</span><span class="sxs-lookup"><span data-stu-id="f3565-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="f3565-130">Démarrage de l’application\_</span><span class="sxs-lookup"><span data-stu-id="f3565-130">App\_Start</span></span>
- <span data-ttu-id="f3565-131">Contenu</span><span class="sxs-lookup"><span data-stu-id="f3565-131">Content</span></span>
- <span data-ttu-id="f3565-132">Controllers</span><span class="sxs-lookup"><span data-stu-id="f3565-132">Controllers</span></span>
- <span data-ttu-id="f3565-133">Modèles</span><span class="sxs-lookup"><span data-stu-id="f3565-133">Models</span></span>
- <span data-ttu-id="f3565-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="f3565-134">Scripts</span></span>
- <span data-ttu-id="f3565-135">Les vues</span><span class="sxs-lookup"><span data-stu-id="f3565-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="f3565-136">Bibliothèques proposées</span><span class="sxs-lookup"><span data-stu-id="f3565-136">Featured Libraries</span></span>

- <span data-ttu-id="f3565-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f3565-137">ASP.NET MVC</span></span>
- <span data-ttu-id="f3565-138">API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f3565-138">ASP.NET Web API</span></span>
- <span data-ttu-id="f3565-139">Optimisation Web ASP.NET-regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="f3565-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="f3565-140">[Breeze. js](http://Breezejs.com) -gestion de données enrichie</span><span class="sxs-lookup"><span data-stu-id="f3565-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="f3565-141">[Durandal. js](http://Durandaljs.com) -navigation et vue de la composition</span><span class="sxs-lookup"><span data-stu-id="f3565-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="f3565-142">[Knockout. js](http://Knockoutjs.com) -liaisons de données</span><span class="sxs-lookup"><span data-stu-id="f3565-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="f3565-143">[Exiger. js](http://requirejs.org) -modularité avec AMD et optimisation</span><span class="sxs-lookup"><span data-stu-id="f3565-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="f3565-144">[Toastr. js](http://jpapa.me/c7toastr) -messages contextuels</span><span class="sxs-lookup"><span data-stu-id="f3565-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="f3565-145">[Démarrage Twitter](https://twitter.github.com/bootstrap/) -style CSS robuste</span><span class="sxs-lookup"><span data-stu-id="f3565-145">[Twitter Bootstrap](https://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="f3565-146">Installation via le modèle SPA de l’essuie à chaud Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f3565-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="f3565-147">La serviette peut être installée en tant que modèle Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f3565-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="f3565-148">Cliquez simplement sur `File` | `New Project`, puis sélectionnez `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="f3565-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="f3565-149">Sélectionnez ensuite le modèle « application à une seule page » et exécutez !</span><span class="sxs-lookup"><span data-stu-id="f3565-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="f3565-150">Installation via le package NuGet</span><span class="sxs-lookup"><span data-stu-id="f3565-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="f3565-151">La serviette à chaud est également un package NuGet qui augmente un projet MVC ASP.NET vide existant.</span><span class="sxs-lookup"><span data-stu-id="f3565-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="f3565-152">Installez simplement à l’aide de NuGet, puis exécutez !</span><span class="sxs-lookup"><span data-stu-id="f3565-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="f3565-153">Comment créer une serviette à chaud ?</span><span class="sxs-lookup"><span data-stu-id="f3565-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="f3565-154">Commencez simplement à ajouter du code !</span><span class="sxs-lookup"><span data-stu-id="f3565-154">Simply start adding code!</span></span>

1. <span data-ttu-id="f3565-155">Ajoutez votre propre code côté serveur, de préférence Entity Framework et WebAPI (ce qui est vraiment brillant avec Breeze. js).</span><span class="sxs-lookup"><span data-stu-id="f3565-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="f3565-156">Ajouter des vues au dossier `App/views`</span><span class="sxs-lookup"><span data-stu-id="f3565-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="f3565-157">Ajouter ViewModels au dossier `App/viewmodels`</span><span class="sxs-lookup"><span data-stu-id="f3565-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="f3565-158">Ajouter des liaisons de données HTML et Knockout à vos nouvelles vues</span><span class="sxs-lookup"><span data-stu-id="f3565-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="f3565-159">Mettre à jour les itinéraires de navigation dans `shell.js`</span><span class="sxs-lookup"><span data-stu-id="f3565-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="f3565-160">Procédure pas à pas du HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="f3565-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="f3565-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f3565-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="f3565-162">index. cshtml est l’itinéraire et la vue de départ pour l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="f3565-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="f3565-163">Il contient toutes les balises méta standard, les liens CSS et les références JavaScript que vous attendez.</span><span class="sxs-lookup"><span data-stu-id="f3565-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="f3565-164">Le corps contient un `<div>` unique, où tout le contenu (vos vues) sera placé lorsqu’il sera demandé.</span><span class="sxs-lookup"><span data-stu-id="f3565-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="f3565-165">Le `@Scripts.Render` utilise Require. js pour exécuter le point d’entrée pour le code de l’application, contenu dans le fichier `main.js`.</span><span class="sxs-lookup"><span data-stu-id="f3565-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="f3565-166">Un écran de démarrage est fourni pour montrer comment créer un écran de démarrage pendant le chargement de votre application.</span><span class="sxs-lookup"><span data-stu-id="f3565-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="f3565-167">App/main. js</span><span class="sxs-lookup"><span data-stu-id="f3565-167">App/main.js</span></span>

<span data-ttu-id="f3565-168">Le fichier `main.js` contient le code qui s’exécutera dès que votre application sera chargée.</span><span class="sxs-lookup"><span data-stu-id="f3565-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="f3565-169">Il s’agit de l’emplacement où vous souhaitez définir vos itinéraires de navigation, définir vos vues de démarrage et effectuer les paramétrages et les amorçages, tels que l’amorçage des données de votre application.</span><span class="sxs-lookup"><span data-stu-id="f3565-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="f3565-170">Le fichier `main.js` définit plusieurs modules de Durandal pour aider l’application à démarrer.</span><span class="sxs-lookup"><span data-stu-id="f3565-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="f3565-171">L’instruction define aide à résoudre les dépendances de modules afin qu’elles soient disponibles pour la fonction.</span><span class="sxs-lookup"><span data-stu-id="f3565-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="f3565-172">Tout d’abord, les messages de débogage sont activés, ce qui permet d’envoyer des messages sur les événements que l’application exécute dans la fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="f3565-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="f3565-173">Le code App. Start indique à Durandal Framework de démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="f3565-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="f3565-174">Les conventions sont définies de sorte que Durandal sache que tous les affichages et ViewModels sont contenus dans les mêmes dossiers nommés, respectivement.</span><span class="sxs-lookup"><span data-stu-id="f3565-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="f3565-175">Enfin, le `app.setRoot` lance le chargement du `shell` à l’aide d’une animation `entrance` prédéfinie.</span><span class="sxs-lookup"><span data-stu-id="f3565-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="f3565-176">Les vues</span><span class="sxs-lookup"><span data-stu-id="f3565-176">Views</span></span>

<span data-ttu-id="f3565-177">Les vues se trouvent dans le dossier `App/views`.</span><span class="sxs-lookup"><span data-stu-id="f3565-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="f3565-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="f3565-178">shell.html</span></span>

<span data-ttu-id="f3565-179">Le `shell.html` contient la disposition maître de votre code HTML.</span><span class="sxs-lookup"><span data-stu-id="f3565-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="f3565-180">Tous vos autres affichages sont composés à l’emplacement de votre vue de `shell`.</span><span class="sxs-lookup"><span data-stu-id="f3565-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="f3565-181">La serviette à chaud fournit une `shell` avec trois régions de ce type : un en-tête, une zone de contenu et un pied de page.</span><span class="sxs-lookup"><span data-stu-id="f3565-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="f3565-182">Chacune de ces régions est chargée avec des contenus à partir d’autres vues quand elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="f3565-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="f3565-183">Les liaisons `compose` pour l’en-tête et le pied de page sont codées en dur dans une serviette chaude pour pointer vers les vues `nav` et `footer`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="f3565-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="f3565-184">La liaison compose pour la section `#content` est liée à l’élément actif du module `router`.</span><span class="sxs-lookup"><span data-stu-id="f3565-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="f3565-185">En d’autres termes, lorsque vous cliquez sur un lien de navigation, la vue correspondante est chargée dans cette zone.</span><span class="sxs-lookup"><span data-stu-id="f3565-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="f3565-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="f3565-186">nav.html</span></span>

<span data-ttu-id="f3565-187">Le `nav.html` contient les liens de navigation pour le SPA.</span><span class="sxs-lookup"><span data-stu-id="f3565-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="f3565-188">C’est ici que la structure de menu peut être placée, par exemple.</span><span class="sxs-lookup"><span data-stu-id="f3565-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="f3565-189">Il s’agit souvent d’une liaison de données (à l’aide de Knockout) au module `router` pour afficher la navigation que vous avez définie dans la `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="f3565-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="f3565-190">Knockout recherche les attributs de liaison de données et les lie au ViewModel `shell` pour afficher les itinéraires de navigation et afficher un ProgressBar (à l’aide de l’amorçage Twitter) si le module `router` est en train de naviguer d’une vue à l’autre (voir `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="f3565-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="f3565-191">fichier. html et details. html</span><span class="sxs-lookup"><span data-stu-id="f3565-191">home.html and details.html</span></span>

<span data-ttu-id="f3565-192">Ces vues contiennent du code HTML pour les vues personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f3565-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="f3565-193">Lorsque l’utilisateur clique sur le lien `home` dans le menu de l’affichage de `nav`, la vue `home` est placée dans la zone de contenu de la vue `shell`.</span><span class="sxs-lookup"><span data-stu-id="f3565-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="f3565-194">Ces vues peuvent être augmentées ou remplacées par vos propres vues personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f3565-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="f3565-195">pied de page. html</span><span class="sxs-lookup"><span data-stu-id="f3565-195">footer.html</span></span>

<span data-ttu-id="f3565-196">Le `footer.html` contient du code HTML qui apparaît dans le pied de page, en bas de la vue de `shell`.</span><span class="sxs-lookup"><span data-stu-id="f3565-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="f3565-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="f3565-197">ViewModels</span></span>

<span data-ttu-id="f3565-198">ViewModels se trouvent dans le dossier `App/viewmodels`.</span><span class="sxs-lookup"><span data-stu-id="f3565-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="f3565-199">Shell. js</span><span class="sxs-lookup"><span data-stu-id="f3565-199">shell.js</span></span>

<span data-ttu-id="f3565-200">Le ViewModel `shell` contient des propriétés et des fonctions liées à la vue de `shell`.</span><span class="sxs-lookup"><span data-stu-id="f3565-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="f3565-201">C’est souvent là que les liaisons de navigation de menu sont trouvées (voir la logique `router.mapNav`).</span><span class="sxs-lookup"><span data-stu-id="f3565-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="f3565-202">Page d’informations. js et details. js</span><span class="sxs-lookup"><span data-stu-id="f3565-202">home.js and details.js</span></span>

<span data-ttu-id="f3565-203">Ces ViewModels contiennent les propriétés et les fonctions liées à la vue `home`.</span><span class="sxs-lookup"><span data-stu-id="f3565-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="f3565-204">Il contient également la logique de présentation de la vue, et est le collage entre les données et la vue.</span><span class="sxs-lookup"><span data-stu-id="f3565-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="f3565-205">Services</span><span class="sxs-lookup"><span data-stu-id="f3565-205">Services</span></span>

<span data-ttu-id="f3565-206">Les services se trouvent dans le dossier app/services.</span><span class="sxs-lookup"><span data-stu-id="f3565-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="f3565-207">Dans l’idéal, vos futurs services tels qu’un module DataService, qui est responsable de l’obtention et de la publication de données distantes, peuvent être placés.</span><span class="sxs-lookup"><span data-stu-id="f3565-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="f3565-208">Logger. js</span><span class="sxs-lookup"><span data-stu-id="f3565-208">logger.js</span></span>

<span data-ttu-id="f3565-209">La serviette à chaud fournit un module `logger` dans le dossier services.</span><span class="sxs-lookup"><span data-stu-id="f3565-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="f3565-210">Le module `logger` est idéal pour la journalisation des messages dans la console et pour l’utilisateur dans les toasts publicitaires.</span><span class="sxs-lookup"><span data-stu-id="f3565-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
