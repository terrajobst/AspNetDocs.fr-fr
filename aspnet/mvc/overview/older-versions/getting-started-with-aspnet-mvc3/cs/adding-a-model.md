---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Ajout d’un modèleC#() | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a5f494eaa05bcfcd9d49873db728d71c1fd332c8
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457781"
---
# <a name="adding-a-model-c"></a><span data-ttu-id="0090d-104">Ajout d’un modèle (C#)</span><span class="sxs-lookup"><span data-stu-id="0090d-104">Adding a Model (C#)</span></span>

<span data-ttu-id="0090d-105">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0090d-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="0090d-106">Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0090d-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="0090d-107">Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0090d-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="0090d-108">Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="0090d-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="0090d-109">Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :</span><span class="sxs-lookup"><span data-stu-id="0090d-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="0090d-110">Conditions préalables pour Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="0090d-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="0090d-111">Mise à jour des outils ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="0090d-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="0090d-112">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)</span><span class="sxs-lookup"><span data-stu-id="0090d-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="0090d-113">Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="0090d-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="0090d-114">Un projet Visual Web Developer C# avec le code source est disponible pour accompagner cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="0090d-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="0090d-115">[Téléchargez la C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="0090d-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="0090d-116">Si vous préférez Visual Basic, passez à la [version Visual Basic](../vb/adding-a-model.md) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="0090d-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="0090d-117">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="0090d-117">Adding a Model</span></span>

<span data-ttu-id="0090d-118">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0090d-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="0090d-119">Ces classes sont la partie « modèle » de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0090d-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="0090d-120">Vous utiliserez une technologie d’accès aux données .NET Framework appelée Entity Framework pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="0090d-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="0090d-121">Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="0090d-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="0090d-122">Code First vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="0090d-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="0090d-123">(Elles sont également appelées « classes POCO » à partir d’objets CLR « Plain-Old ».) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide.</span><span class="sxs-lookup"><span data-stu-id="0090d-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="0090d-124">Ajout de classes de modèle</span><span class="sxs-lookup"><span data-stu-id="0090d-124">Adding Model Classes</span></span>

<span data-ttu-id="0090d-125">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="0090d-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="0090d-126">Nommez la *classe* « Movie ».</span><span class="sxs-lookup"><span data-stu-id="0090d-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="0090d-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="0090d-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="0090d-128">Ajoutez les cinq propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="0090d-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="0090d-129">Nous allons utiliser la classe `Movie` pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0090d-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="0090d-130">Chaque instance d’un objet `Movie` correspond à une ligne dans une table de base de données, et chaque propriété de la classe `Movie` sera mappée à une colonne de la table.</span><span class="sxs-lookup"><span data-stu-id="0090d-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="0090d-131">Dans le même fichier, ajoutez la classe `MovieDBContext` suivante :</span><span class="sxs-lookup"><span data-stu-id="0090d-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="0090d-132">La classe `MovieDBContext` représente le contexte de la base de données de films Entity Framework qui gère l’extraction, le stockage et la mise à jour des instances de classe `Movie` dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0090d-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="0090d-133">Le `MovieDBContext` dérive de la classe de base `DbContext` fournie par le Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0090d-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="0090d-134">Pour plus d’informations sur les `DbContext` et les `DbSet`, consultez [améliorations de la productivité pour le Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="0090d-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="0090d-135">Pour pouvoir référencer `DbContext` et `DbSet`, vous devez ajouter l’instruction `using` suivante en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="0090d-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="0090d-136">Le fichier *Movie.cs* complet est illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0090d-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="0090d-137">Création d’une chaîne de connexion et utilisation de SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="0090d-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="0090d-138">La classe `MovieDBContext` que vous avez créée gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="0090d-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="0090d-139">L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter.</span><span class="sxs-lookup"><span data-stu-id="0090d-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="0090d-140">Pour ce faire, vous devez ajouter des informations de connexion dans le fichier *Web. config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="0090d-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="0090d-141">Ouvrez le fichier *Web. config* racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="0090d-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="0090d-142">(Pas le fichier *Web. config* dans le dossier *views* .) L’image ci-dessous affiche les deux fichiers *Web. config* ; Ouvrez le fichier *Web. config* entouré d’un cercle rouge.</span><span class="sxs-lookup"><span data-stu-id="0090d-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

<span data-ttu-id="0090d-143">Ajoutez la chaîne de connexion suivante à l’élément `<connectionStrings>` dans le fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="0090d-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="0090d-144">L’exemple suivant montre une partie du fichier *Web. config* avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="0090d-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="0090d-145">Cette petite quantité de code et XML est tout ce dont vous avez besoin pour écrire, afin de représenter et de stocker les données de film dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0090d-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="0090d-146">Ensuite, vous allez générer une nouvelle classe de `MoviesController` que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.</span><span class="sxs-lookup"><span data-stu-id="0090d-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0090d-147">[Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="0090d-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
