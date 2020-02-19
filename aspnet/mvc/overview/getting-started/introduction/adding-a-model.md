---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: c5525cfe940cadff5113c63eb0508d15697b5606
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456541"
---
# <a name="adding-a-model"></a><span data-ttu-id="7a317-102">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="7a317-102">Adding a Model</span></span>

<span data-ttu-id="7a317-103">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a317-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="7a317-104">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="7a317-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="7a317-105">Ces classes seront le modèle de &quot;&quot; partie de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a317-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="7a317-106">Vous utiliserez une technologie d’accès aux données .NET Framework appelée [Entity Framework](https://docs.microsoft.com/ef/) pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="7a317-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="7a317-107">Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="7a317-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="7a317-108">Code First vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="7a317-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="7a317-109">(Elles sont également appelées classes POCO à partir de &quot;objets CLR ordinaires.&quot;) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide.</span><span class="sxs-lookup"><span data-stu-id="7a317-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="7a317-110">Si vous devez d’abord créer la base de données, vous pouvez tout de même suivre ce didacticiel pour en savoir plus sur le développement d’applications MVC et EF.</span><span class="sxs-lookup"><span data-stu-id="7a317-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="7a317-111">Vous pouvez ensuite suivre le didacticiel de [génération de modèles](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) automatique Tom Fizmakens ASP.net, qui couvre la première approche de la base de données.</span><span class="sxs-lookup"><span data-stu-id="7a317-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="7a317-112">Ajout de classes de modèle</span><span class="sxs-lookup"><span data-stu-id="7a317-112">Adding Model Classes</span></span>

<span data-ttu-id="7a317-113">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="7a317-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="7a317-114">Entrez le nom de la *classe* &quot;&quot;Movie.</span><span class="sxs-lookup"><span data-stu-id="7a317-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="7a317-115">Ajoutez les cinq propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="7a317-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="7a317-116">Nous allons utiliser la classe `Movie` pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="7a317-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="7a317-117">Chaque instance d’un objet `Movie` correspond à une ligne dans une table de base de données, et chaque propriété de la classe `Movie` sera mappée à une colonne de la table.</span><span class="sxs-lookup"><span data-stu-id="7a317-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="7a317-118">Remarque : pour utiliser System. Data. Entity et la classe connexe, vous devez installer le [package NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="7a317-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="7a317-119">Suivez le lien pour obtenir des instructions supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="7a317-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="7a317-120">Dans le même fichier, ajoutez la classe `MovieDBContext` suivante :</span><span class="sxs-lookup"><span data-stu-id="7a317-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="7a317-121">La classe `MovieDBContext` représente le contexte de la base de données de films Entity Framework qui gère l’extraction, le stockage et la mise à jour des instances de classe `Movie` dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="7a317-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="7a317-122">Le `MovieDBContext` dérive de la classe de base `DbContext` fournie par le Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7a317-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="7a317-123">Pour pouvoir référencer `DbContext` et `DbSet`, vous devez ajouter l’instruction `using` suivante en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="7a317-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="7a317-124">Pour ce faire, vous pouvez ajouter manuellement l’instruction using ou vous pouvez pointer sur les lignes ondulées rouges, cliquer sur `Show potential fixes`, puis sur `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="7a317-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="7a317-125">Remarque : plusieurs instructions `using` inutilisées ont été supprimées.</span><span class="sxs-lookup"><span data-stu-id="7a317-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="7a317-126">Visual Studio affiche les dépendances inutilisées en gris.</span><span class="sxs-lookup"><span data-stu-id="7a317-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="7a317-127">Vous pouvez supprimer les dépendances inutilisées en plaçant le curseur sur les dépendances grises, en cliquant sur `Show potential fixes`, puis sur **Supprimer les using inutilisées.**</span><span class="sxs-lookup"><span data-stu-id="7a317-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="7a317-128">Nous avons enfin ajouté un modèle (le M dans MVC).</span><span class="sxs-lookup"><span data-stu-id="7a317-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="7a317-129">Dans la section suivante, vous allez utiliser la chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="7a317-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7a317-130">[Précédent](adding-a-view.md)
> [Suivant](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="7a317-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
