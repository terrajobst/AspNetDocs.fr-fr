---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Partie 1 : vue d’ensemble et > de fichier nouveau projet | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La première partie couvre la vue d’ensemble et le > de fichiers de nouveau projet.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559982"
---
# <a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="0b498-104">Partie 1 : vue d’ensemble et > un nouveau projet de fichier</span><span class="sxs-lookup"><span data-stu-id="0b498-104">Part 1: Overview and File->New Project</span></span>

<span data-ttu-id="0b498-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0b498-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0b498-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="0b498-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0b498-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="0b498-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="0b498-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="0b498-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0b498-109">La première partie couvre la vue d’ensemble et le&gt;de fichiers de nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="0b498-109">Part 1 covers Overview and File-&gt;New Project.</span></span>

## <a name="overview"></a><span data-ttu-id="0b498-110">Présentation</span><span class="sxs-lookup"><span data-stu-id="0b498-110">Overview</span></span>

<span data-ttu-id="0b498-111">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Web Developer pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="0b498-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="0b498-112">Nous allons démarrer lentement, de sorte que l’expérience de développement Web au niveau débutant est correcte.</span><span class="sxs-lookup"><span data-stu-id="0b498-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="0b498-113">L’application que nous allons créer est un magasin de musique simple.</span><span class="sxs-lookup"><span data-stu-id="0b498-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="0b498-114">L’application comprend trois parties principales : achat, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="0b498-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="0b498-115">Les visiteurs peuvent parcourir les albums par genre :</span><span class="sxs-lookup"><span data-stu-id="0b498-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="0b498-116">Ils peuvent afficher un seul album et l’ajouter à leur panier :</span><span class="sxs-lookup"><span data-stu-id="0b498-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="0b498-117">Ils peuvent examiner leur panier, en supprimant tous les éléments qu’ils ne souhaitent plus :</span><span class="sxs-lookup"><span data-stu-id="0b498-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="0b498-118">En poursuivant l’extraction, vous êtes invité à vous connecter ou à s’inscrire pour obtenir un compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0b498-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="0b498-119">Après avoir créé un compte, il peut terminer la commande en remplissant les informations d’expédition et de paiement.</span><span class="sxs-lookup"><span data-stu-id="0b498-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="0b498-120">Pour simplifier les choses, nous faisons une promotion exceptionnelle : tout est gratuit s’ils entrent dans le code de promotion « gratuit » !</span><span class="sxs-lookup"><span data-stu-id="0b498-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="0b498-121">Après l’ordonnancement, un écran de confirmation simple s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0b498-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="0b498-122">En plus des pages orientées client, nous allons également créer une section administrateur qui affiche la liste des albums à partir desquels les administrateurs peuvent créer, modifier et supprimer des albums :</span><span class="sxs-lookup"><span data-stu-id="0b498-122">In addition to customer-facing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="0b498-123">1. fichier&gt; nouveau projet</span><span class="sxs-lookup"><span data-stu-id="0b498-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="0b498-124">Installation du logiciel</span><span class="sxs-lookup"><span data-stu-id="0b498-124">Installing the software</span></span>

<span data-ttu-id="0b498-125">Ce didacticiel commence par créer un nouveau projet ASP.NET MVC 3 à l’aide de la version gratuite de Visual Web Developer 2010 Express (qui est gratuite), puis nous ajouterons de manière incrémentielle des fonctionnalités pour créer une application fonctionnelle complète.</span><span class="sxs-lookup"><span data-stu-id="0b498-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="0b498-126">En cours de route, nous allons aborder l’accès aux bases de données, les scénarios de publication de formulaire, la validation des données, l’utilisation de pages maîtres pour une mise en page cohérente, l’utilisation d’AJAX pour les mises à jour et la validation des pages, la connexion des utilisateurs, etc.</span><span class="sxs-lookup"><span data-stu-id="0b498-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="0b498-127">Vous pouvez suivre la procédure pas à pas, ou vous pouvez télécharger l’application terminée à partir de [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="0b498-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="0b498-128">Vous pouvez utiliser Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (une version gratuite de Visual Studio 2010) pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="0b498-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="0b498-129">Nous utiliserons le SQL Server Compact (également gratuit) pour héberger la base de données.</span><span class="sxs-lookup"><span data-stu-id="0b498-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="0b498-130">Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0b498-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="0b498-131">[Composants requis pour Visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="0b498-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="0b498-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="0b498-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="0b498-133">[SQL Server Compact 4,0]-inclut la prise en charge du runtime et des outils</span><span class="sxs-lookup"><span data-stu-id="0b498-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>

### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="0b498-134">Création d’un nouveau projet ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="0b498-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="0b498-135">Nous allons commencer par sélectionner « nouveau projet » dans le menu fichier de Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="0b498-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="0b498-136">La boîte de dialogue Nouveau projet s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0b498-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="0b498-137">Nous allons sélectionner le groupe C# de modèles Web&gt; Visual sur la gauche, puis choisir le modèle « Application Web ASP.NET MVC 3 » dans la colonne centrale.</span><span class="sxs-lookup"><span data-stu-id="0b498-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="0b498-138">Nommez votre projet MvcMusicStore et appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="0b498-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="0b498-139">Cette opération affiche une boîte de dialogue secondaire qui nous permet de définir des paramètres spécifiques à MVC pour notre projet.</span><span class="sxs-lookup"><span data-stu-id="0b498-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="0b498-140">Sélectionnez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0b498-140">Select the following:</span></span>

<span data-ttu-id="0b498-141">Modèle de projet-sélectionnez vide</span><span class="sxs-lookup"><span data-stu-id="0b498-141">Project Template - select Empty</span></span>

<span data-ttu-id="0b498-142">Moteur d’affichage-sélectionner Razor</span><span class="sxs-lookup"><span data-stu-id="0b498-142">View Engine - select Razor</span></span>

<span data-ttu-id="0b498-143">Utiliser la balise sémantique HTML5-activée</span><span class="sxs-lookup"><span data-stu-id="0b498-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="0b498-144">Vérifiez que vos paramètres sont comme indiqué ci-dessous, puis appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="0b498-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="0b498-145">Cela va créer notre projet.</span><span class="sxs-lookup"><span data-stu-id="0b498-145">This will create our project.</span></span> <span data-ttu-id="0b498-146">Jetons un coup d’œil aux dossiers qui ont été ajoutés à notre application dans le Explorateur de solutions sur le côté droit.</span><span class="sxs-lookup"><span data-stu-id="0b498-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="0b498-147">Le modèle MVC 3 vide n’est pas complètement vide : il ajoute une structure de dossier de base :</span><span class="sxs-lookup"><span data-stu-id="0b498-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="0b498-148">ASP.NET MVC utilise des conventions d’affectation de noms de base pour les noms de dossiers :</span><span class="sxs-lookup"><span data-stu-id="0b498-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="0b498-149">**Dossier**</span><span class="sxs-lookup"><span data-stu-id="0b498-149">**Folder**</span></span> | <span data-ttu-id="0b498-150">**Fonction**</span><span class="sxs-lookup"><span data-stu-id="0b498-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="0b498-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="0b498-151">**/Controllers**</span></span> | <span data-ttu-id="0b498-152">Les contrôleurs répondent aux entrées du navigateur, décident de la marche à suivre et retournent une réponse à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0b498-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="0b498-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="0b498-153">**/Views**</span></span> | <span data-ttu-id="0b498-154">Les vues contiennent nos modèles d’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="0b498-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="0b498-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="0b498-155">**/Models**</span></span> | <span data-ttu-id="0b498-156">Les modèles contiennent et manipulent des données</span><span class="sxs-lookup"><span data-stu-id="0b498-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="0b498-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="0b498-157">**/Content**</span></span> | <span data-ttu-id="0b498-158">Ce dossier contient nos images, CSS et tout autre contenu statique</span><span class="sxs-lookup"><span data-stu-id="0b498-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="0b498-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="0b498-159">**/Scripts**</span></span> | <span data-ttu-id="0b498-160">Ce dossier contient nos fichiers JavaScript</span><span class="sxs-lookup"><span data-stu-id="0b498-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="0b498-161">Ces dossiers sont inclus même dans une application ASP.NET MVC vide, car l’infrastructure ASP.NET MVC utilise par défaut une approche « Convention sur la configuration » et effectue certaines hypothèses par défaut en fonction des conventions d’affectation des noms de dossiers.</span><span class="sxs-lookup"><span data-stu-id="0b498-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="0b498-162">Par exemple, les contrôleurs recherchent des vues dans le dossier vues par défaut sans avoir à les spécifier explicitement dans votre code.</span><span class="sxs-lookup"><span data-stu-id="0b498-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="0b498-163">L’utilisation des conventions par défaut permet de réduire la quantité de code que vous devez écrire et peut également faciliter la compréhension de votre projet par d’autres développeurs.</span><span class="sxs-lookup"><span data-stu-id="0b498-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="0b498-164">Nous expliquerons ces conventions plus en détail lors de la création de notre application.</span><span class="sxs-lookup"><span data-stu-id="0b498-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0b498-165">Next</span><span class="sxs-lookup"><span data-stu-id="0b498-165">Next</span></span>](mvc-music-store-part-2.md)
