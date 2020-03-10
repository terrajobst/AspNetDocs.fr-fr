---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduction à ASP.NET MVC | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581584"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="f761f-104">Introduction à ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f761f-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="f761f-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f761f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f761f-106">Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="f761f-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="f761f-107">Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="f761f-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="f761f-108">Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f761f-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f761f-109">Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="f761f-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f761f-110">Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f761f-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="f761f-111">Nous allons créer notre première application Web MVC ASP.NET à l’aide de [Visual Web developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="f761f-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="f761f-112">Nous allons créer une petite application de liste de films qui nous permettra de créer et de répertorier des films.</span><span class="sxs-lookup"><span data-stu-id="f761f-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="f761f-113">Contenu</span><span class="sxs-lookup"><span data-stu-id="f761f-113">What You'll Build</span></span>

<span data-ttu-id="f761f-114">Voici deux captures d’écran de l’application que vous allez générer.</span><span class="sxs-lookup"><span data-stu-id="f761f-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="f761f-115">Vous disposerez d’un simple tableau de films avec différentes colonnes.</span><span class="sxs-lookup"><span data-stu-id="f761f-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="f761f-116">[Liste des films ![-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f761f-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="f761f-117">Et vous disposerez d’un formulaire créer pour pouvoir ajouter des films à la liste.</span><span class="sxs-lookup"><span data-stu-id="f761f-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="f761f-118">[![créer un film-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f761f-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="f761f-119">Compétences</span><span class="sxs-lookup"><span data-stu-id="f761f-119">Skills You'll Learn</span></span>

<span data-ttu-id="f761f-120">Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f761f-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="f761f-121">Vous apprendrez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f761f-121">You'll learn:</span></span>

- <span data-ttu-id="f761f-122">Comment créer un nouveau projet MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f761f-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="f761f-123">Comment créer une base de données avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="f761f-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="f761f-124">Comment créer des contrôleurs et des vues MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f761f-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="f761f-125">Comment récupérer et afficher des données</span><span class="sxs-lookup"><span data-stu-id="f761f-125">How to retrieve and display data</span></span>
- <span data-ttu-id="f761f-126">Comment modifier des données et activer la validation des données</span><span class="sxs-lookup"><span data-stu-id="f761f-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="f761f-127">Comment mettre à jour le schéma de base de données</span><span class="sxs-lookup"><span data-stu-id="f761f-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="f761f-128">Pour commencer</span><span class="sxs-lookup"><span data-stu-id="f761f-128">Get Started</span></span>

<span data-ttu-id="f761f-129">Commencez par exécuter Visual Web Developer 2010 Express (je l’appellerai « VWD existant » à partir de maintenant) et sélectionnez Nouveau projet dans l’écran d’accueil.</span><span class="sxs-lookup"><span data-stu-id="f761f-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="f761f-130">Visual Web Developer est un environnement de développement intégré (IDE).</span><span class="sxs-lookup"><span data-stu-id="f761f-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="f761f-131">Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="f761f-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="f761f-132">Une barre d’outils située en haut montre les différentes options disponibles, ainsi que le menu que vous pouvez également utiliser pour sélectionner fichier | Nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="f761f-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="f761f-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f761f-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="f761f-134">Création de votre première application</span><span class="sxs-lookup"><span data-stu-id="f761f-134">Creating Your First Application</span></span>

<span data-ttu-id="f761f-135">Vous pouvez créer des applications à l’aide C#de Visual Basic ou Visual.</span><span class="sxs-lookup"><span data-stu-id="f761f-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="f761f-136">Pour le moment, sélectionnez C# visuel à gauche, puis choisissez « Application Web ASP.NET MVC 2 ».</span><span class="sxs-lookup"><span data-stu-id="f761f-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="f761f-137">Nommez le projet « films », puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="f761f-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="f761f-138">[![un nouveau projet](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f761f-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="f761f-139">Dans la partie droite se trouve le Explorateur de solutions qui montre tous les fichiers et dossiers de votre application.</span><span class="sxs-lookup"><span data-stu-id="f761f-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="f761f-140">La grande fenêtre au milieu est l’endroit où vous modifiez votre code et passez la majeure partie de votre temps.</span><span class="sxs-lookup"><span data-stu-id="f761f-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="f761f-141">Visual Studio utilisait un modèle par défaut pour le projet MVC ASP.NET que vous venez de créer, ce qui vous permet de disposer d’une application fonctionnelle sans rien faire !</span><span class="sxs-lookup"><span data-stu-id="f761f-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="f761f-142">Il s’agit d’un simple «Hello World !</span><span class="sxs-lookup"><span data-stu-id="f761f-142">This is a simple "Hello World!</span></span> <span data-ttu-id="f761f-143">et c’est un bon point de départ pour notre application.</span><span class="sxs-lookup"><span data-stu-id="f761f-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="f761f-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f761f-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="f761f-145">Sélectionnez le bouton « lecture » dans la barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="f761f-145">Select the "play" button to the toolbar.</span></span>

![Démarrer le débogage](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="f761f-147">Il s’agit d’une flèche verte pointant vers la droite qui compilera votre programme et démarrera votre application dans un navigateur Web.</span><span class="sxs-lookup"><span data-stu-id="f761f-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="f761f-148">*Remarque : vous pouvez appuyer plutôt sur la touche F5 de votre clavier, ou sélectionner déboguer-&gt;démarrer le débogage à partir du menu Déboguer.*</span><span class="sxs-lookup"><span data-stu-id="f761f-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="f761f-149">Visual Web Developer démarrera donc un serveur Web de développement et exécutera l’application Web (aucune étape de configuration ou manuelle n’est requise pour activer cette opération).</span><span class="sxs-lookup"><span data-stu-id="f761f-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="f761f-150">Il lance ensuite un navigateur et le configure pour parcourir la page d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="f761f-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="f761f-151">Notez ci-dessous que la barre d’adresses du navigateur indique « localhost », et non pas une telle example.com.</span><span class="sxs-lookup"><span data-stu-id="f761f-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="f761f-152">C’est parce que localhost pointe toujours vers votre propre ordinateur local, ce qui, dans ce cas, exécute l’application que nous venons de créer.</span><span class="sxs-lookup"><span data-stu-id="f761f-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="f761f-153">[Page d’espace ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="f761f-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="f761f-154">Ce modèle par défaut vous donne deux pages à visiter et une page de connexion de base.</span><span class="sxs-lookup"><span data-stu-id="f761f-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="f761f-155">Nous allons modifier le fonctionnement de cette application et apprendre un peu sur ASP.NET MVC dans le processus.</span><span class="sxs-lookup"><span data-stu-id="f761f-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="f761f-156">Fermez votre navigateur et modifiez du code.</span><span class="sxs-lookup"><span data-stu-id="f761f-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f761f-157">Next</span><span class="sxs-lookup"><span data-stu-id="f761f-157">Next</span></span>](getting-started-with-mvc-part2.md)
