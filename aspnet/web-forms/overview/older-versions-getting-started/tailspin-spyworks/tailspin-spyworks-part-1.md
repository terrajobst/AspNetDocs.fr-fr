---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Partie 1 : fichier-> nouveau projet | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 1 couvre la vue d’ensemble et le fichier/nouveau projet.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636128"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="f19a3-104">Partie 1 : fichier > nouveau projet</span><span class="sxs-lookup"><span data-stu-id="f19a3-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="f19a3-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="f19a3-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="f19a3-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="f19a3-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="f19a3-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="f19a3-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="f19a3-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="f19a3-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="f19a3-109">La partie 1 couvre la vue d’ensemble et le fichier/nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="f19a3-109">Part 1 covers Overview and File/New Project.</span></span>

## <a id="_Toc260221666"></a><span data-ttu-id="f19a3-110">Vue</span><span class="sxs-lookup"><span data-stu-id="f19a3-110">Overview</span></span>

<span data-ttu-id="f19a3-111">Ce didacticiel est une introduction à ASP.NET WebForms.</span><span class="sxs-lookup"><span data-stu-id="f19a3-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="f19a3-112">Nous allons démarrer lentement, de sorte que l’expérience de développement Web au niveau débutant est correcte.</span><span class="sxs-lookup"><span data-stu-id="f19a3-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="f19a3-113">L’application que nous allons créer est un magasin en ligne simple.</span><span class="sxs-lookup"><span data-stu-id="f19a3-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)

<span data-ttu-id="f19a3-114">Les visiteurs peuvent parcourir les produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="f19a3-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="f19a3-115">Ils peuvent afficher un seul produit et l’ajouter à leur panier :</span><span class="sxs-lookup"><span data-stu-id="f19a3-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="f19a3-116">Ils peuvent examiner leur panier, en supprimant tous les éléments qu’ils ne souhaitent plus :</span><span class="sxs-lookup"><span data-stu-id="f19a3-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="f19a3-117">En poursuivant l’extraction, vous êtes invité à</span><span class="sxs-lookup"><span data-stu-id="f19a3-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="f19a3-118">Après l’ordonnancement, un écran de confirmation simple s’affiche :</span><span class="sxs-lookup"><span data-stu-id="f19a3-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)

<span data-ttu-id="f19a3-119">Nous allons commencer par créer un nouveau projet ASP.NET WebForms dans Visual Studio 2010, et nous ajouterons de manière incrémentielle des fonctionnalités pour créer une application fonctionnelle complète.</span><span class="sxs-lookup"><span data-stu-id="f19a3-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="f19a3-120">En cours de route, nous allons aborder l’accès aux bases de données, les affichages de listes et de grilles, les pages de mise à jour des données, la validation des données, l’utilisation de pages maîtres pour une mise en page cohérente, AJAX, la validation, l’appartenance des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="f19a3-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="f19a3-121">Vous pouvez suivre la procédure pas à pas, ou vous pouvez télécharger l’application terminée à partir de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="f19a3-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="f19a3-122">Vous pouvez utiliser Visual Studio 2010 ou la version gratuite de Visual Web Developer 2010 de [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="f19a3-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="f19a3-123">Pour générer l’application, vous pouvez utiliser l’une ou l’autre SQL Server SQL Server Express libre pour héberger la base de données.</span><span class="sxs-lookup"><span data-stu-id="f19a3-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="f19a3-124">Fichier/nouveau projet</span><span class="sxs-lookup"><span data-stu-id="f19a3-124">File / New Project</span></span>

<span data-ttu-id="f19a3-125">Nous allons commencer par sélectionner le nouveau projet dans le menu fichier de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f19a3-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="f19a3-126">La boîte de dialogue Nouveau projet s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f19a3-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="f19a3-127">Nous allons sélectionner le groupe C# Visual/Web Templates sur la gauche, puis choisir le modèle « Application Web ASP.net » dans la colonne centrale.</span><span class="sxs-lookup"><span data-stu-id="f19a3-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="f19a3-128">Nommez votre projet TailspinSpyworks et appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="f19a3-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="f19a3-129">Cela va créer notre projet.</span><span class="sxs-lookup"><span data-stu-id="f19a3-129">This will create our project.</span></span> <span data-ttu-id="f19a3-130">Jetons un coup d’œil aux dossiers inclus dans notre application dans le Explorateur de solutions sur le côté droit.</span><span class="sxs-lookup"><span data-stu-id="f19a3-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="f19a3-131">La solution vide n’est pas complètement vide : elle ajoute une structure de dossier de base :</span><span class="sxs-lookup"><span data-stu-id="f19a3-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="f19a3-132">Notez les conventions implémentées par le modèle de projet par défaut ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="f19a3-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="f19a3-133">Le dossier « Account » implémente une interface utilisateur de base pour ASP. Sous-système d’appartenance du réseau.</span><span class="sxs-lookup"><span data-stu-id="f19a3-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="f19a3-134">Le dossier « scripts » sert de référentiel pour les fichiers JavaScript côté client et les fichiers jQuery. js de base sont mis à disposition par défaut.</span><span class="sxs-lookup"><span data-stu-id="f19a3-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="f19a3-135">Le dossier « styles » est utilisé pour organiser les visuels de sites Web (feuilles de style CSS)</span><span class="sxs-lookup"><span data-stu-id="f19a3-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="f19a3-136">Quand nous appuyons sur F5 pour exécuter notre application et afficher la page default. aspx, nous voyons ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="f19a3-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="f19a3-137">Notre première amélioration de l’application consiste à remplacer le fichier style. CSS du modèle WebForms par défaut par les classes CSS et les fichiers image associés qui rendront le asthetics visuel que nous souhaitons pour notre application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="f19a3-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="f19a3-138">Après cela, notre page default. aspx s’affiche comme suit.</span><span class="sxs-lookup"><span data-stu-id="f19a3-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="f19a3-139">Notez les liens d’image situés en haut à droite de la page, ainsi que les éléments de menu qui ont été ajoutés à la page maître.</span><span class="sxs-lookup"><span data-stu-id="f19a3-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="f19a3-140">Seuls les liens « connexion » et « compte » pointent vers les pages qui existent (générées par le modèle par défaut) et les autres pages que nous allons implémenter lors de la création de notre application.</span><span class="sxs-lookup"><span data-stu-id="f19a3-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="f19a3-141">Nous allons également déplacer la page maître vers le répertoire styles.</span><span class="sxs-lookup"><span data-stu-id="f19a3-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="f19a3-142">Bien qu’il ne s’agisse que d’une préférence, il peut faciliter un peu les choses si nous décidons de rendre notre application « peau » à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="f19a3-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="f19a3-143">Après cela, nous devrons modifier les références de page maître dans tous les fichiers. aspx générés par les pages Web Forms ASP.NET par défaut.</span><span class="sxs-lookup"><span data-stu-id="f19a3-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f19a3-144">Next</span><span class="sxs-lookup"><span data-stu-id="f19a3-144">Next</span></span>](tailspin-spyworks-part-2.md)
