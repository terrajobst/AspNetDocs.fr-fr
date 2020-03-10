---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543749"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="889cd-104">Accès aux données de votre modèle à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="889cd-104">Accessing your Model's Data from a Controller</span></span>

<span data-ttu-id="889cd-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="889cd-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="889cd-106">Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="889cd-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="889cd-107">Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="889cd-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="889cd-108">Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="889cd-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="889cd-109">Dans cette section, nous allons créer une nouvelle classe MoviesController et écrire du code qui récupère nos données de film et les affiche à nouveau dans le navigateur à l’aide d’un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="889cd-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="889cd-110">Cliquez avec le bouton droit sur le dossier Controllers et créez un nouveau MoviesController.</span><span class="sxs-lookup"><span data-stu-id="889cd-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="889cd-111">[![ajouter un contrôleur](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="889cd-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="889cd-112">Cela créera un nouveau fichier « MoviesController.cs » sous notre dossier \Controllers au sein de notre projet.</span><span class="sxs-lookup"><span data-stu-id="889cd-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="889cd-113">Nous allons mettre à jour MovieController pour récupérer la liste des films à partir de notre base de données qui vient d’être remplie.</span><span class="sxs-lookup"><span data-stu-id="889cd-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="889cd-114">Nous effectuons une requête LINQ pour que nous puissions uniquement récupérer les films publiés après l’été de 1984.</span><span class="sxs-lookup"><span data-stu-id="889cd-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="889cd-115">Nous aurons besoin d’un modèle de vue pour restituer cette liste de films. par conséquent, cliquez avec le bouton droit dans la méthode, puis sélectionnez Ajouter une vue pour la créer.</span><span class="sxs-lookup"><span data-stu-id="889cd-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="889cd-116">Dans la boîte de dialogue Ajouter une vue, nous indiquons que nous transmettons une liste&lt;movies. Models. Movie&gt; à notre modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="889cd-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="889cd-117">Contrairement aux heures précédentes, nous avons utilisé la boîte de dialogue Ajouter une vue et choisi de créer un modèle « vide ». cette fois-ci, nous indiquons que Visual Studio doit automatiquement « échafauder » un modèle de vue pour nous avec un contenu par défaut.</span><span class="sxs-lookup"><span data-stu-id="889cd-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="889cd-118">Pour ce faire, nous allons sélectionner l’élément « liste » dans le menu déroulant Afficher le contenu.</span><span class="sxs-lookup"><span data-stu-id="889cd-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="889cd-119">N’oubliez pas qu’une fois que vous avez créé une nouvelle classe, vous devez compiler votre application pour qu’elle s’affiche dans la boîte de dialogue Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="889cd-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Ajouter une vue](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="889cd-121">Cliquez sur Ajouter pour que le système génère automatiquement le code pour une vue pour nous qui affiche notre liste de films.</span><span class="sxs-lookup"><span data-stu-id="889cd-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="889cd-122">C’est le moment idéal pour modifier l’en-tête &lt;H2&gt; par un texte tel que « ma liste de films » comme nous l’avons fait précédemment avec la vue de Hello World.</span><span class="sxs-lookup"><span data-stu-id="889cd-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="889cd-123">[Films ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="889cd-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="889cd-124">Exécutez votre application et visitez/movies dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="889cd-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="889cd-125">À présent, nous avons extrait les données de la base de données à l’aide d’une requête de base dans le contrôleur et j’ai renvoyé les données à une vue qui connaît les films.</span><span class="sxs-lookup"><span data-stu-id="889cd-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="889cd-126">Cette vue tourne ensuite dans la liste des films et crée une table de données pour nous.</span><span class="sxs-lookup"><span data-stu-id="889cd-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="889cd-127">[Liste des films ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="889cd-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="889cd-128">Nous n’implémenterons pas les fonctionnalités de modification, de détails et de suppression avec cette application. nous n’avons donc pas besoin des liens par défaut créés pour nous.</span><span class="sxs-lookup"><span data-stu-id="889cd-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="889cd-129">Ouvrez le fichier/Movies/Index.aspx et supprimez-le.</span><span class="sxs-lookup"><span data-stu-id="889cd-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="889cd-130">Voici le code source de ce à quoi doit ressembler notre modèle de vue mis à jour une fois que nous apportons ces modifications :</span><span class="sxs-lookup"><span data-stu-id="889cd-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="889cd-131">Il crée des liens dont nous n’aurons pas besoin. nous allons donc les supprimer pour cet exemple.</span><span class="sxs-lookup"><span data-stu-id="889cd-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="889cd-132">Nous allons conserver notre lien créer, car c’est le cas.</span><span class="sxs-lookup"><span data-stu-id="889cd-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="889cd-133">Voici à quoi ressemble votre application avec cette colonne supprimée.</span><span class="sxs-lookup"><span data-stu-id="889cd-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="889cd-134">[Liste des films ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="889cd-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="889cd-135">Nous disposons à présent d’une simple liste de nos données de films.</span><span class="sxs-lookup"><span data-stu-id="889cd-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="889cd-136">Toutefois, si nous cliquons sur le lien « créer nouveau », nous obtenons une erreur, car elle n’est pas raccrochée.</span><span class="sxs-lookup"><span data-stu-id="889cd-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="889cd-137">Nous allons implémenter une méthode de création d’action et permettre à un utilisateur d’entrer de nouveaux films dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="889cd-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="889cd-138">[Précédent](getting-started-with-mvc-part4.md)
> [Suivant](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="889cd-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
