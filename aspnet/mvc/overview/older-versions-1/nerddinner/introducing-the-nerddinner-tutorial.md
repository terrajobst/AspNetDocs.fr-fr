---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Présentation du didacticiel NerdDinner | Microsoft Docs
author: shanselman
description: La meilleure façon d’apprendre une nouvelle infrastructure consiste à en créer un autre. Ce didacticiel explique comment créer une application de petite taille, mais complète, à l’aide de ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580576"
---
# <a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="e55c1-104">Introduction au didacticiel NerdDinner</span><span class="sxs-lookup"><span data-stu-id="e55c1-104">Introducing the NerdDinner Tutorial</span></span>

<span data-ttu-id="e55c1-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e55c1-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="e55c1-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="e55c1-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="e55c1-107">La meilleure façon d’apprendre une nouvelle infrastructure consiste à en créer un autre.</span><span class="sxs-lookup"><span data-stu-id="e55c1-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="e55c1-108">Ce didacticiel vous guide dans la création d’une application de petite taille, mais complète, à l’aide de ASP.NET MVC 1, et présente quelques-uns des principaux concepts qui l’appuient.</span><span class="sxs-lookup"><span data-stu-id="e55c1-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="e55c1-109">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.</span><span class="sxs-lookup"><span data-stu-id="e55c1-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-tutorial"></a><span data-ttu-id="e55c1-110">Didacticiel NerdDinner</span><span class="sxs-lookup"><span data-stu-id="e55c1-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="e55c1-111">La meilleure façon d’apprendre une nouvelle infrastructure consiste à en créer un autre.</span><span class="sxs-lookup"><span data-stu-id="e55c1-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="e55c1-112">Ce didacticiel explique comment créer une application de petite taille, mais complète, à l’aide de ASP.NET MVC, et présente quelques-uns des principaux concepts qui l’appuient.</span><span class="sxs-lookup"><span data-stu-id="e55c1-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="e55c1-113">L’application que nous allons générer est appelée « NerdDinner ».</span><span class="sxs-lookup"><span data-stu-id="e55c1-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="e55c1-114">NerdDinner permet aux utilisateurs de trouver et d’organiser facilement les dîners en ligne :</span><span class="sxs-lookup"><span data-stu-id="e55c1-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="e55c1-115">NerdDinner permet aux utilisateurs inscrits de créer, de modifier et de supprimer des dîners.</span><span class="sxs-lookup"><span data-stu-id="e55c1-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="e55c1-116">Il applique un ensemble cohérent de règles de validation et d’entreprise à l’ensemble de l’application :</span><span class="sxs-lookup"><span data-stu-id="e55c1-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="e55c1-117">Les visiteurs peuvent utiliser un mappage basé sur AJAX pour rechercher les dîners à venir à proximité :</span><span class="sxs-lookup"><span data-stu-id="e55c1-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="e55c1-118">En cliquant sur un dîner, vous les trouverez dans une page de détails où ils pourront en savoir plus :</span><span class="sxs-lookup"><span data-stu-id="e55c1-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="e55c1-119">S’ils souhaitent assister au dîner, ils peuvent se connecter ou s’inscrire sur le site :</span><span class="sxs-lookup"><span data-stu-id="e55c1-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="e55c1-120">Ils peuvent ensuite cliquer sur un lien RSVP basé sur AJAX pour assister à l’événement :</span><span class="sxs-lookup"><span data-stu-id="e55c1-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="e55c1-121">Implémentation de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="e55c1-121">Implementing NerdDinner</span></span>

<span data-ttu-id="e55c1-122">Nous allons commencer notre application NerdDinner à l’aide de la commande de nouveau projet de fichier&gt;dans Visual Studio pour créer un nouveau projet MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e55c1-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="e55c1-123">Nous ajouterons ensuite de manière incrémentielle des fonctionnalités et des fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="e55c1-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="e55c1-124">Nous allons aborder les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e55c1-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="e55c1-125">Comment créer un nouveau projet MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e55c1-125">How to create a new ASP.NET MVC Project</span></span>](create-a-new-aspnet-mvc-project.md)
2. [<span data-ttu-id="e55c1-126">Comment créer une base de données</span><span class="sxs-lookup"><span data-stu-id="e55c1-126">How to create a database</span></span>](create-a-database.md)
3. [<span data-ttu-id="e55c1-127">Comment créer un modèle avec des validations de règles d’entreprise</span><span class="sxs-lookup"><span data-stu-id="e55c1-127">How to build a model with business rule validations</span></span>](build-a-model-with-business-rule-validations.md)
4. [<span data-ttu-id="e55c1-128">Comment utiliser des contrôleurs et des vues pour implémenter une interface utilisateur de liste/détails</span><span class="sxs-lookup"><span data-stu-id="e55c1-128">How to use controllers and views to implement a listing/details UI</span></span>](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [<span data-ttu-id="e55c1-129">Comment fournir une prise en charge de l’entrée de formulaire CRUD (créer, lire, mettre à jour, supprimer)</span><span class="sxs-lookup"><span data-stu-id="e55c1-129">How to provide CRUD (create, read, update, delete) data form entry support</span></span>](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [<span data-ttu-id="e55c1-130">Comment utiliser ViewData et implémenter des classes ViewModel</span><span class="sxs-lookup"><span data-stu-id="e55c1-130">How to use ViewData and implement ViewModel classes</span></span>](use-viewdata-and-implement-viewmodel-classes.md)
7. [<span data-ttu-id="e55c1-131">Réutilisation de l’interface utilisateur à l’aide des pages maîtres et des parties partielles</span><span class="sxs-lookup"><span data-stu-id="e55c1-131">How to re-use UI using master pages and partials</span></span>](re-use-ui-using-master-pages-and-partials.md)
8. [<span data-ttu-id="e55c1-132">Comment implémenter une pagination des données efficace</span><span class="sxs-lookup"><span data-stu-id="e55c1-132">How to implement efficient data paging</span></span>](implement-efficient-data-paging.md)
9. [<span data-ttu-id="e55c1-133">Comment sécuriser des applications à l’aide de l’authentification et de l’autorisation</span><span class="sxs-lookup"><span data-stu-id="e55c1-133">How to secure applications using authentication and authorization</span></span>](secure-applications-using-authentication-and-authorization.md)
10. [<span data-ttu-id="e55c1-134">Comment utiliser AJAX pour fournir des mises à jour dynamiques</span><span class="sxs-lookup"><span data-stu-id="e55c1-134">How to use AJAX to deliver dynamic updates</span></span>](use-ajax-to-deliver-dynamic-updates.md)
11. [<span data-ttu-id="e55c1-135">Comment utiliser AJAX pour implémenter des scénarios de mappage</span><span class="sxs-lookup"><span data-stu-id="e55c1-135">How to use AJAX to implement mapping scenarios</span></span>](use-ajax-to-implement-mapping-scenarios.md)
12. [<span data-ttu-id="e55c1-136">Comment activer les tests unitaires automatisés</span><span class="sxs-lookup"><span data-stu-id="e55c1-136">How to enable automated unit testing</span></span>](enable-automated-unit-testing.md)

<span data-ttu-id="e55c1-137">Vous pouvez créer votre propre copie de NerdDinner à partir de zéro en effectuant chaque étape de la procédure pas à pas dans ce chapitre.</span><span class="sxs-lookup"><span data-stu-id="e55c1-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="e55c1-138">Vous pouvez également télécharger une version complète du code source ici : [NerdDinner sur GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="e55c1-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="e55c1-139">Vous pouvez également [Télécharger une version PDF gratuite de ce didacticiel](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si vous souhaitez lire le didacticiel en mode hors connexion.</span><span class="sxs-lookup"><span data-stu-id="e55c1-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="e55c1-140">Pour générer l’application, vous pouvez utiliser Visual Studio 2008 ou la version gratuite de Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="e55c1-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="e55c1-141">Vous pouvez utiliser SQL Server ou la SQL Server Express libre pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="e55c1-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="e55c1-142">Vous pouvez installer ASP.NET MVC, Visual Web Developer 2008 Express et SQL Server Express (tout gratuitement) à l’aide de V2 du [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="e55c1-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="e55c1-143">Commençons...</span><span class="sxs-lookup"><span data-stu-id="e55c1-143">Now let's get started....</span></span>

<span data-ttu-id="e55c1-144">Maintenant que nous avons abordé le NerdDinner, nous allons nous reporter à nos manches et écrire du code.</span><span class="sxs-lookup"><span data-stu-id="e55c1-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="e55c1-145">Nous allons commencer par utiliser file-&gt;nouveau projet dans Visual Studio pour créer l’application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="e55c1-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e55c1-146">Next</span><span class="sxs-lookup"><span data-stu-id="e55c1-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
