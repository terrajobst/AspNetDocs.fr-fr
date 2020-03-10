---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540305"
---
# <a name="adding-a-model"></a><span data-ttu-id="0427f-104">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="0427f-104">Adding a Model</span></span>

<span data-ttu-id="0427f-105">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0427f-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0427f-106">Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0427f-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0427f-107">Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="0427f-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="0427f-108">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0427f-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="0427f-109">Ces classes seront le modèle de &quot;&quot; partie de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0427f-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="0427f-110">Vous utiliserez une technologie d’accès aux données .NET Framework appelée [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="0427f-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="0427f-111">Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="0427f-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="0427f-112">Code First vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="0427f-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="0427f-113">(Elles sont également appelées classes POCO à partir de &quot;objets CLR ordinaires.&quot;) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide.</span><span class="sxs-lookup"><span data-stu-id="0427f-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="0427f-114">Ajout de classes de modèle</span><span class="sxs-lookup"><span data-stu-id="0427f-114">Adding Model Classes</span></span>

<span data-ttu-id="0427f-115">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="0427f-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="0427f-116">Entrez le nom de la *classe* &quot;&quot;Movie.</span><span class="sxs-lookup"><span data-stu-id="0427f-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="0427f-117">Ajoutez les cinq propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="0427f-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="0427f-118">Nous allons utiliser la classe `Movie` pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0427f-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="0427f-119">Chaque instance d’un objet `Movie` correspond à une ligne dans une table de base de données, et chaque propriété de la classe `Movie` sera mappée à une colonne de la table.</span><span class="sxs-lookup"><span data-stu-id="0427f-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="0427f-120">Dans le même fichier, ajoutez la classe `MovieDBContext` suivante :</span><span class="sxs-lookup"><span data-stu-id="0427f-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="0427f-121">La classe `MovieDBContext` représente le contexte de la base de données de films Entity Framework qui gère l’extraction, le stockage et la mise à jour des instances de classe `Movie` dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0427f-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="0427f-122">Le `MovieDBContext` dérive de la classe de base `DbContext` fournie par le Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0427f-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="0427f-123">Pour pouvoir référencer `DbContext` et `DbSet`, vous devez ajouter l’instruction `using` suivante en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="0427f-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="0427f-124">Le fichier *Movie.cs* complet est illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0427f-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="0427f-125">(Plusieurs instructions using qui ne sont pas nécessaires ont été supprimées.)</span><span class="sxs-lookup"><span data-stu-id="0427f-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="0427f-126">Création d’une chaîne de connexion et utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="0427f-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="0427f-127">La classe `MovieDBContext` que vous avez créée gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="0427f-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="0427f-128">L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter.</span><span class="sxs-lookup"><span data-stu-id="0427f-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="0427f-129">Pour ce faire, vous devez ajouter des informations de connexion dans le fichier *Web. config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="0427f-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="0427f-130">Ouvrez le fichier *Web. config* racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="0427f-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="0427f-131">(Pas le fichier *Web. config* dans le dossier *views* .) Ouvrez le fichier *Web. config* présenté en rouge.</span><span class="sxs-lookup"><span data-stu-id="0427f-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="0427f-132">Ajoutez la chaîne de connexion suivante à l’élément `<connectionStrings>` dans le fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="0427f-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="0427f-133">L’exemple suivant montre une partie du fichier *Web. config* avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="0427f-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="0427f-134">Cette petite quantité de code et XML est tout ce dont vous avez besoin pour écrire, afin de représenter et de stocker les données de film dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="0427f-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="0427f-135">Ensuite, vous allez générer une nouvelle classe de `MoviesController` que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.</span><span class="sxs-lookup"><span data-stu-id="0427f-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0427f-136">[Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="0427f-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
