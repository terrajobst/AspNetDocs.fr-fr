---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Ajout d’un modèle (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457243"
---
# <a name="adding-a-model-vb"></a><span data-ttu-id="ef8ea-103">Ajout d’un modèle (VB)</span><span class="sxs-lookup"><span data-stu-id="ef8ea-103">Adding a Model (VB)</span></span>

<span data-ttu-id="ef8ea-104">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ef8ea-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="ef8ea-105">Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="ef8ea-106">Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="ef8ea-107">Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="ef8ea-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="ef8ea-108">Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :</span><span class="sxs-lookup"><span data-stu-id="ef8ea-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="ef8ea-109">Conditions préalables pour Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="ef8ea-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="ef8ea-110">Mise à jour des outils ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="ef8ea-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="ef8ea-111">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)</span><span class="sxs-lookup"><span data-stu-id="ef8ea-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="ef8ea-112">Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="ef8ea-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="ef8ea-113">Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="ef8ea-114">[Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="ef8ea-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="ef8ea-115">Si vous préférez C#, passez à la [ C# version](../cs/adding-a-model.md) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="ef8ea-116">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="ef8ea-116">Adding a Model</span></span>

<span data-ttu-id="ef8ea-117">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="ef8ea-118">Ces classes sont la partie « modèle » de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="ef8ea-119">Vous utiliserez une technologie d’accès aux données .NET Framework appelée Entity Framework pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="ef8ea-120">Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="ef8ea-121">Code First vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="ef8ea-122">(Elles sont également appelées « classes POCO » à partir d’objets CLR « Plain-Old ».) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="ef8ea-123">Ajout de classes de modèle</span><span class="sxs-lookup"><span data-stu-id="ef8ea-123">Adding Model Classes</span></span>

<span data-ttu-id="ef8ea-124">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="ef8ea-125">Nommez la classe « Movie ».</span><span class="sxs-lookup"><span data-stu-id="ef8ea-125">Name the class "Movie".</span></span>

<span data-ttu-id="ef8ea-126">Ajoutez les cinq propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="ef8ea-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="ef8ea-127">Nous allons utiliser la classe `Movie` pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="ef8ea-128">Chaque instance d’un objet `Movie` correspond à une ligne dans une table de base de données, et chaque propriété de la classe `Movie` sera mappée à une colonne de la table.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="ef8ea-129">Dans le même fichier, ajoutez la classe `MovieDBContext` suivante :</span><span class="sxs-lookup"><span data-stu-id="ef8ea-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="ef8ea-130">La classe `MovieDBContext` représente le contexte de la base de données de films Entity Framework qui gère l’extraction, le stockage et la mise à jour des instances de classe `Movie` dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="ef8ea-131">Le `MovieDBContext` dérive de la classe de base `DbContext` fournie par le Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="ef8ea-132">Pour plus d’informations sur les `DbContext` et les `DbSet`, consultez [améliorations de la productivité pour le Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="ef8ea-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="ef8ea-133">Pour pouvoir référencer `DbContext` et `DbSet`, vous devez ajouter l’instruction `imports` suivante en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="ef8ea-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="ef8ea-134">Le fichier *Movie. vb* complet est illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="ef8ea-135">Création d’une chaîne de connexion et utilisation de SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="ef8ea-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="ef8ea-136">La classe `MovieDBContext` que vous avez créée gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ef8ea-137">L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="ef8ea-138">Pour ce faire, vous devez ajouter des informations de connexion dans le fichier *Web. config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="ef8ea-139">Ouvrez le fichier *Web. config* racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="ef8ea-140">(Pas le fichier *Web. config* dans le dossier *views* .) L’image ci-dessous affiche les deux fichiers *Web. config* ; Ouvrez le fichier *Web. config* entouré d’un cercle rouge.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="ef8ea-141">Ajoutez la chaîne de connexion suivante à l’élément `<connectionStrings>` dans le fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ef8ea-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="ef8ea-142">L’exemple suivant montre une partie du fichier *Web. config* avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="ef8ea-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="ef8ea-143">Cette petite quantité de code et XML est tout ce dont vous avez besoin pour écrire, afin de représenter et de stocker les données de film dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="ef8ea-144">Ensuite, vous allez générer une nouvelle classe de `MoviesController` que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.</span><span class="sxs-lookup"><span data-stu-id="ef8ea-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef8ea-145">[Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="ef8ea-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
