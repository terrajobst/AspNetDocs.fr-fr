---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Prise en main avec ASP.NET 4,7 Web Forms et Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,7 et Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568221"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="51b41-103">Prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="51b41-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>

<span data-ttu-id="51b41-104">[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="51b41-104">[Download Wingtip Toys Sample Project (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="51b41-105">Cette série de didacticiels vous montre comment créer une application ASP.NET Web Forms avec ASP.NET 4,5 et Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="51b41-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="51b41-106">Introduction</span><span class="sxs-lookup"><span data-stu-id="51b41-106">Introduction</span></span>

<span data-ttu-id="51b41-107">Cette série de didacticiels vous guide tout au long de la création d’une application ASP.NET Web Forms à l’aide de Visual Studio 2017 et ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="51b41-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="51b41-108">Vous allez créer une application nommée **Wingtip Toys** : site Web vitrine simplifié vendre des éléments en ligne.</span><span class="sxs-lookup"><span data-stu-id="51b41-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="51b41-109">Au cours de la série, les nouvelles fonctionnalités ASP.NET 4,5 sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="51b41-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="51b41-110">Public cible</span><span class="sxs-lookup"><span data-stu-id="51b41-110">Target audience</span></span>

<span data-ttu-id="51b41-111">Les développeurs qui débutent avec ASP.NET Web Forms sont le public cible de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="51b41-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="51b41-112">Vous devez avoir des connaissances dans les domaines suivants :</span><span class="sxs-lookup"><span data-stu-id="51b41-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="51b41-113">Programmation orientée objet (OOP) et langues</span><span class="sxs-lookup"><span data-stu-id="51b41-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="51b41-114">Développement Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="51b41-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="51b41-115">Bases de données relationnelles</span><span class="sxs-lookup"><span data-stu-id="51b41-115">Relational databases</span></span>
- <span data-ttu-id="51b41-116">Architecture multiniveau</span><span class="sxs-lookup"><span data-stu-id="51b41-116">N-tier architecture</span></span>

<span data-ttu-id="51b41-117">Pour passer en revue ces domaines, pensez à étudier le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="51b41-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="51b41-118">Prise en main avec VisualC#</span><span class="sxs-lookup"><span data-stu-id="51b41-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="51b41-119">[Développement Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, php, jQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="51b41-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="51b41-120">Base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="51b41-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="51b41-121">Architecture à plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="51b41-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="51b41-122">Fonctionnalités de l’application</span><span class="sxs-lookup"><span data-stu-id="51b41-122">Application features</span></span>

<span data-ttu-id="51b41-123">Les fonctionnalités de Web Form ASP.NET présentées dans cette série sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="51b41-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="51b41-124">Projet d’application Web (pas un projet de site Web)</span><span class="sxs-lookup"><span data-stu-id="51b41-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="51b41-125">Web Forms</span><span class="sxs-lookup"><span data-stu-id="51b41-125">Web Forms</span></span>
- <span data-ttu-id="51b41-126">Pages maîtres, configuration</span><span class="sxs-lookup"><span data-stu-id="51b41-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="51b41-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="51b41-127">Bootstrap</span></span>
- <span data-ttu-id="51b41-128">Entity Framework Code First, base de données locale</span><span class="sxs-lookup"><span data-stu-id="51b41-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="51b41-129">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="51b41-129">Request Validation</span></span>
- <span data-ttu-id="51b41-130">Contrôles de données fortement typés</span><span class="sxs-lookup"><span data-stu-id="51b41-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="51b41-131">Liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="51b41-131">Model Binding</span></span>
- <span data-ttu-id="51b41-132">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="51b41-132">Data Annotations</span></span>
- <span data-ttu-id="51b41-133">Fournisseurs de valeurs</span><span class="sxs-lookup"><span data-stu-id="51b41-133">Value Providers</span></span>
- <span data-ttu-id="51b41-134">SSL et OAuth</span><span class="sxs-lookup"><span data-stu-id="51b41-134">SSL and OAuth</span></span>
- <span data-ttu-id="51b41-135">ASP.NET Identity, configuration et autorisation</span><span class="sxs-lookup"><span data-stu-id="51b41-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="51b41-136">Validation discrète</span><span class="sxs-lookup"><span data-stu-id="51b41-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="51b41-137">Routage</span><span class="sxs-lookup"><span data-stu-id="51b41-137">Routing</span></span>
- <span data-ttu-id="51b41-138">Gestion des erreurs ASP.NET</span><span class="sxs-lookup"><span data-stu-id="51b41-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="51b41-139">Scénarios et tâches d’application</span><span class="sxs-lookup"><span data-stu-id="51b41-139">Application scenarios and tasks</span></span>

<span data-ttu-id="51b41-140">Les tâches de la série de didacticiels sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="51b41-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="51b41-141">Création, examen et exécution d’un nouveau projet</span><span class="sxs-lookup"><span data-stu-id="51b41-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="51b41-142">Création d’une structure de base de données</span><span class="sxs-lookup"><span data-stu-id="51b41-142">Creating a database structure</span></span>
- <span data-ttu-id="51b41-143">Initialisation et amorçage d’une base de données</span><span class="sxs-lookup"><span data-stu-id="51b41-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="51b41-144">Personnalisation de l’interface utilisateur avec des styles, des graphiques et une page maître</span><span class="sxs-lookup"><span data-stu-id="51b41-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="51b41-145">Ajout de pages et de navigation</span><span class="sxs-lookup"><span data-stu-id="51b41-145">Adding pages and navigation</span></span>
- <span data-ttu-id="51b41-146">Affichage des détails des menus et des données sur les produits</span><span class="sxs-lookup"><span data-stu-id="51b41-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="51b41-147">Création d’un panier d’achat</span><span class="sxs-lookup"><span data-stu-id="51b41-147">Creating a shopping cart</span></span>
- <span data-ttu-id="51b41-148">Ajout de la prise en charge de SSL et OAuth</span><span class="sxs-lookup"><span data-stu-id="51b41-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="51b41-149">Ajout d’un mode de paiement</span><span class="sxs-lookup"><span data-stu-id="51b41-149">Adding a payment method</span></span>
- <span data-ttu-id="51b41-150">Inclusion d’un rôle d’administrateur et d’un utilisateur à l’application</span><span class="sxs-lookup"><span data-stu-id="51b41-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="51b41-151">Restriction de l’accès à des pages et des dossiers spécifiques</span><span class="sxs-lookup"><span data-stu-id="51b41-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="51b41-152">Chargement d’un fichier dans l’application Web</span><span class="sxs-lookup"><span data-stu-id="51b41-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="51b41-153">Implémentation de la validation des entrées</span><span class="sxs-lookup"><span data-stu-id="51b41-153">Implementing input validation</span></span>
- <span data-ttu-id="51b41-154">Inscription des itinéraires pour l’application Web</span><span class="sxs-lookup"><span data-stu-id="51b41-154">Registering routes for the web application</span></span>
- <span data-ttu-id="51b41-155">Implémentation de la gestion des erreurs et de la journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="51b41-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="51b41-156">Présentation</span><span class="sxs-lookup"><span data-stu-id="51b41-156">Overview</span></span>

<span data-ttu-id="51b41-157">Cette série de didacticiels s’adresse aux personnes connaissant les concepts de programmation, mais qui sont nouvelles dans ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="51b41-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="51b41-158">Si vous êtes déjà familiarisé avec ASP.NET Web Forms, cette série peut encore vous aider à en savoir plus sur les nouvelles fonctionnalités de ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="51b41-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="51b41-159">Pour les lecteurs qui ne sont pas familiarisés avec les concepts de programmation et les Web Forms ASP.NET, consultez les didacticiels de Web Forms supplémentaires fournis dans la section [prise en main](../../../index.md) sur le site Web ASP.net.</span><span class="sxs-lookup"><span data-stu-id="51b41-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="51b41-160">Le ASP.NET 4,5 fourni dans cette série de didacticiels comprend les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="51b41-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="51b41-161">Interface utilisateur simple pour la création de projets qui offre la [prise en charge de nombreuses infrastructures ASP.net](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).</span><span class="sxs-lookup"><span data-stu-id="51b41-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="51b41-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), mise en page, thèmes et infrastructure de conception réactive.</span><span class="sxs-lookup"><span data-stu-id="51b41-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="51b41-163">[ASP.net Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.net qui fonctionne de la même façon dans toutes les infrastructures ASP.net et fonctionne avec les logiciels d’hébergement Web autres qu’IIS.</span><span class="sxs-lookup"><span data-stu-id="51b41-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="51b41-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="51b41-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="51b41-165">Une mise à jour de la Entity Framework vous permettant d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="51b41-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="51b41-166">Récupérer et manipuler des données en tant qu’objets fortement typés</span><span class="sxs-lookup"><span data-stu-id="51b41-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="51b41-167">Accéder aux données de manière asynchrone</span><span class="sxs-lookup"><span data-stu-id="51b41-167">Access data asynchronously</span></span>
  - <span data-ttu-id="51b41-168">Gérer les erreurs temporaires de connexion</span><span class="sxs-lookup"><span data-stu-id="51b41-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="51b41-169">Journaux des instructions SQL</span><span class="sxs-lookup"><span data-stu-id="51b41-169">Log SQL statements</span></span>

<span data-ttu-id="51b41-170">Pour obtenir une liste complète des fonctionnalités ASP.NET 4,5, consultez [ASP.net et Web Tools pour Visual Studio 2013 notes de publication](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="51b41-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="51b41-171">Exemple d’application Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="51b41-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="51b41-172">Les captures d’écran suivantes proviennent de l’application ASP.NET Web Forms que vous créez dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="51b41-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="51b41-173">Quand vous exécutez l’application dans Visual Studio, la page d’hébergement Web suivante s’affiche.</span><span class="sxs-lookup"><span data-stu-id="51b41-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys-page par défaut](introduction-and-overview/_static/image1.png)

<span data-ttu-id="51b41-175">Vous pouvez vous inscrire en tant que nouvel utilisateur ou vous connecter en tant qu’utilisateur existant.</span><span class="sxs-lookup"><span data-stu-id="51b41-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="51b41-176">Le volet de navigation supérieur contient des liens vers les catégories de produits et leurs produits à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="51b41-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="51b41-177">Si vous sélectionnez **produits**, tous les produits disponibles s’affichent.</span><span class="sxs-lookup"><span data-stu-id="51b41-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys-produits](introduction-and-overview/_static/image2.png)

<span data-ttu-id="51b41-179">Si vous sélectionnez un produit spécifique, les détails du produit s’affichent.</span><span class="sxs-lookup"><span data-stu-id="51b41-179">If you select a specific product, product details are displayed.</span></span>

![Wingtip Toys-détails sur le produit](introduction-and-overview/_static/image3.png)

<span data-ttu-id="51b41-181">En tant qu’utilisateur, vous pouvez vous inscrire et vous connecter avec Web Forms fonctionnalité par défaut du modèle.</span><span class="sxs-lookup"><span data-stu-id="51b41-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="51b41-182">Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant.</span><span class="sxs-lookup"><span data-stu-id="51b41-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="51b41-183">En outre, vous pouvez vous connecter en tant qu’administrateur pour ajouter et supprimer des produits dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="51b41-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys-connexion](introduction-and-overview/_static/image4.png)

<span data-ttu-id="51b41-185">Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et le valider avec PayPal.</span><span class="sxs-lookup"><span data-stu-id="51b41-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="51b41-186">L’exemple d’application est conçu pour fonctionner dans le bac à sable (sandbox) du développeur PayPal.</span><span class="sxs-lookup"><span data-stu-id="51b41-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="51b41-187">Aucune transaction financière réelle n’a lieu.</span><span class="sxs-lookup"><span data-stu-id="51b41-187">No actual money transaction takes place.</span></span>

![Wingtip Toys-panier d’achat](introduction-and-overview/_static/image5.png)

<span data-ttu-id="51b41-189">PayPal confirme vos informations de compte, de commande et de paiement.</span><span class="sxs-lookup"><span data-stu-id="51b41-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="51b41-191">Après avoir retourné PayPal, vous pouvez passer en revue et terminer votre commande.</span><span class="sxs-lookup"><span data-stu-id="51b41-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys-revue des commandes](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="51b41-193">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="51b41-193">Prerequisites</span></span>

<span data-ttu-id="51b41-194">Avant de commencer, assurez-vous que les logiciels suivants sont installés sur votre ordinateur :</span><span class="sxs-lookup"><span data-stu-id="51b41-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="51b41-195">[Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="51b41-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="51b41-196">Le .NET Framework est installé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="51b41-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="51b41-197">Cette série de didacticiels utilise Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="51b41-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="51b41-198">Vous pouvez utiliser ce ou Microsoft Visual Studio 2017 pour suivre cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="51b41-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="51b41-199">Notez les points suivants à propos de Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="51b41-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="51b41-200">Microsoft Visual Studio 2017 et Microsoft Visual Studio Community 2017 sont connus sous le terme de *Visual Studio* dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="51b41-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="51b41-201">Visual Studio 2017 est installé en regard des anciennes versions déjà installées.</span><span class="sxs-lookup"><span data-stu-id="51b41-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="51b41-202">Les sites créés dans des versions antérieures peuvent être ouverts dans Visual Studio 2017 et continuer à s’ouvrir dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="51b41-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="51b41-203">La première fois que vous démarrez Visual Studio, il est supposé que vous avez sélectionné les paramètres de *développement Web* .</span><span class="sxs-lookup"><span data-stu-id="51b41-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="51b41-204">Pour plus d’informations, consultez [Comment : sélectionner les paramètres de l’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="51b41-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="51b41-205">Après avoir installé les composants requis, vous êtes prêt à commencer la création du projet Web présenté dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="51b41-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="51b41-206">Téléchargement de l'exemple d'application</span><span class="sxs-lookup"><span data-stu-id="51b41-206">Download the sample application</span></span>

 <span data-ttu-id="51b41-207">Vous pouvez télécharger l’exemple d’application terminé à tout moment à partir du site d’exemples MSDN :</span><span class="sxs-lookup"><span data-stu-id="51b41-207">You can download the completed sample application at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="51b41-208">[Prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="51b41-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="51b41-209">Ce téléchargement contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="51b41-209">This download has the following items:</span></span>

- <span data-ttu-id="51b41-210">L’exemple d’application dans le dossier *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="51b41-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="51b41-211">Les ressources utilisées pour créer l’exemple d’application dans le dossier *wingtiptoys-biens* du dossier *wingtiptoys* .</span><span class="sxs-lookup"><span data-stu-id="51b41-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="51b41-212">Le téléchargement est un fichier *. zip* .</span><span class="sxs-lookup"><span data-stu-id="51b41-212">The download is a *.zip* file.</span></span> <span data-ttu-id="51b41-213">Pour voir le projet terminé créé par cette série de didacticiels, recherchez et *C#* sélectionnez le dossier dans le fichier. zip.</span><span class="sxs-lookup"><span data-stu-id="51b41-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="51b41-214">Enregistrez le C# dossier dans le dossier que vous utilisez pour travailler avec les projets Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51b41-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="51b41-215">Par défaut, le dossier de projets Visual Studio 2017 est :</span><span class="sxs-lookup"><span data-stu-id="51b41-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="51b41-216"><strong>C:\Users&#92;</strong>  <strong><em>&lt;nom d’utilisateur&gt;</em></strong> <strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="51b41-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="51b41-217">Renommez ***C#*** le dossier en ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="51b41-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="51b41-218">Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de *C#* renommer le dossier *wingtiptoys*.</span><span class="sxs-lookup"><span data-stu-id="51b41-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="51b41-219">Pour exécuter le projet terminé, ouvrez le dossier *wingtiptoys* et double-cliquez sur le fichier *wingtiptoys. sln* .</span><span class="sxs-lookup"><span data-stu-id="51b41-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="51b41-220">Visual Studio 2017 ouvre le projet.</span><span class="sxs-lookup"><span data-stu-id="51b41-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="51b41-221">Ensuite, cliquez avec le bouton droit sur le fichier *default. aspx* dans **Explorateur de solutions** , puis sélectionnez **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="51b41-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="51b41-222">Participez à un ASP.NET Web Forms pour consulter le contenu</span><span class="sxs-lookup"><span data-stu-id="51b41-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="51b41-223">À l’issue de la série de didacticiels, participez à un quiz pour tester vos connaissances et renforcer les concepts clés.</span><span class="sxs-lookup"><span data-stu-id="51b41-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="51b41-224">Chaque question fournit une explication et des liens vers des conseils supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="51b41-224">Each question provides an explanation and links to additional guidance.</span></span>

* [<span data-ttu-id="51b41-225">ASP.NET Web Forms quiz</span><span class="sxs-lookup"><span data-stu-id="51b41-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="51b41-226">Prise en charge des didacticiels et commentaires</span><span class="sxs-lookup"><span data-stu-id="51b41-226">Tutorial support and comments</span></span>

<span data-ttu-id="51b41-227">Pour les questions et les commentaires, utilisez la section Q et une section incluse dans le [prise en main avec la page d’exemple ASP.net 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="51b41-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="51b41-228">Les commentaires de cette série de didacticiels sont les bienvenus.</span><span class="sxs-lookup"><span data-stu-id="51b41-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="51b41-229">Lors de la mise à jour de cette série de didacticiels, il convient de prendre en compte les corrections ou suggestions pour les améliorations.</span><span class="sxs-lookup"><span data-stu-id="51b41-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="51b41-230">Si une erreur se produit, les messages d’erreur correspondants peuvent prêter à confusion, sans bonne explication sur la façon de résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="51b41-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="51b41-231">Pour obtenir de l’aide, vous pouvez consulter les [forums ASP.net](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="51b41-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="51b41-232">Une autre bonne source est la section Q et une section du [prise en main avec la page d’exemple ASP.net 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="51b41-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="51b41-233">Next</span><span class="sxs-lookup"><span data-stu-id="51b41-233">Next</span></span>](create-the-project.md)
