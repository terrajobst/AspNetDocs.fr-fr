---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtres d’action personnalisée ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fournit des filtres d’action pour l’exécution de la logique de filtrage avant ou après l’appel d’une méthode d’action. Les filtres d’action sont des attributs personnalisés...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579694"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="86cb6-104">Filtres d’actions personnalisés d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="86cb6-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="86cb6-105">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="86cb6-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="86cb6-106">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="86cb6-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="86cb6-107">ASP.NET MVC fournit des filtres d’action pour l’exécution de la logique de filtrage avant ou après l’appel d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="86cb6-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="86cb6-108">Les filtres d’action sont des attributs personnalisés qui fournissent des moyens déclaratifs pour ajouter un comportement de pré-action et après action aux méthodes d’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="86cb6-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="86cb6-109">Dans ce laboratoire pratique, vous allez créer un attribut de filtre d’action personnalisé dans la solution MvcMusicStore pour intercepter les demandes du contrôleur et enregistrer l’activité d’un site dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="86cb6-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="86cb6-110">Vous pourrez ajouter votre filtre de journalisation par injection à n’importe quel contrôleur ou action.</span><span class="sxs-lookup"><span data-stu-id="86cb6-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="86cb6-111">Enfin, vous verrez la vue de journal qui affiche la liste des visiteurs.</span><span class="sxs-lookup"><span data-stu-id="86cb6-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="86cb6-112">Ce laboratoire pratique suppose que vous avez une connaissance de base de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="86cb6-113">Si vous n’avez pas encore utilisé **ASP.NET MVC** , nous vous recommandons de passer au-dessus d’un laboratoire pratique de **notions de base sur ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-114">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="86cb6-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="86cb6-115">Le projet propre à ce laboratoire est disponible dans les [filtres d’action personnalisés ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="86cb6-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="86cb6-116">Objectifs</span><span class="sxs-lookup"><span data-stu-id="86cb6-116">Objectives</span></span>

<span data-ttu-id="86cb6-117">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="86cb6-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="86cb6-118">Créer un attribut de filtre d’action personnalisé pour étendre les fonctionnalités de filtrage</span><span class="sxs-lookup"><span data-stu-id="86cb6-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="86cb6-119">Appliquer un attribut de filtre personnalisé par injection à un niveau spécifique</span><span class="sxs-lookup"><span data-stu-id="86cb6-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="86cb6-120">Inscrire globalement des filtres d’action personnalisés</span><span class="sxs-lookup"><span data-stu-id="86cb6-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="86cb6-121">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="86cb6-121">Prerequisites</span></span>

<span data-ttu-id="86cb6-122">Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="86cb6-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="86cb6-123">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).</span><span class="sxs-lookup"><span data-stu-id="86cb6-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="86cb6-124">Installation</span><span class="sxs-lookup"><span data-stu-id="86cb6-124">Setup</span></span>

<span data-ttu-id="86cb6-125">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="86cb6-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="86cb6-126">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86cb6-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="86cb6-127">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="86cb6-128">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="86cb6-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="86cb6-129">Exercices</span><span class="sxs-lookup"><span data-stu-id="86cb6-129">Exercises</span></span>

<span data-ttu-id="86cb6-130">Ce laboratoire pratique est constitué des exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="86cb6-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="86cb6-131">Exercice 1 : journalisation des actions</span><span class="sxs-lookup"><span data-stu-id="86cb6-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="86cb6-132">Exercice 2 : gestion de plusieurs filtres d’action</span><span class="sxs-lookup"><span data-stu-id="86cb6-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="86cb6-133">Durée estimée pour effectuer ce laboratoire : **30 minutes**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-134">Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="86cb6-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="86cb6-135">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.</span><span class="sxs-lookup"><span data-stu-id="86cb6-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="86cb6-136">Exercice 1 : journalisation des actions</span><span class="sxs-lookup"><span data-stu-id="86cb6-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="86cb6-137">Dans cet exercice, vous allez apprendre à créer un filtre de journal des actions personnalisées à l’aide de fournisseurs de filtres ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="86cb6-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="86cb6-138">À cet effet, vous allez appliquer un filtre de journalisation au site MusicStore qui enregistre toutes les activités dans les contrôleurs sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="86cb6-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="86cb6-139">Le filtre étend **ActionFilterAttributeClass** et remplace la méthode **OnActionExecuting** pour intercepter chaque demande, puis effectuer les actions de journalisation.</span><span class="sxs-lookup"><span data-stu-id="86cb6-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="86cb6-140">Les informations de contexte relatives aux requêtes HTTP, à l’exécution des méthodes, aux résultats et aux paramètres seront fournies par la classe **ACTIONEXECUTINGCONTEXT** MVC ASP.NET **.**</span><span class="sxs-lookup"><span data-stu-id="86cb6-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-141">ASP.NET MVC 4 possède également des fournisseurs de filtres par défaut que vous pouvez utiliser sans créer de filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="86cb6-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="86cb6-142">ASP.NET MVC 4 fournit les types de filtres suivants :</span><span class="sxs-lookup"><span data-stu-id="86cb6-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="86cb6-143">Filtre **d’autorisation** , qui prend des décisions en matière de sécurité quant à l’exécution d’une méthode d’action, telle que l’exécution de l’authentification ou la validation des propriétés de la demande.</span><span class="sxs-lookup"><span data-stu-id="86cb6-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="86cb6-144">Filtre d' **action** , qui encapsule l’exécution de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="86cb6-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="86cb6-145">Ce filtre peut effectuer un traitement supplémentaire, tel que la fourniture de données supplémentaires à la méthode d’action, l’inspection de la valeur de retour ou l’annulation de l’exécution de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="86cb6-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="86cb6-146">Filtre de **résultat** , qui encapsule l’exécution de l’objet ActionResult.</span><span class="sxs-lookup"><span data-stu-id="86cb6-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="86cb6-147">Ce filtre peut effectuer un traitement supplémentaire du résultat, tel que la modification de la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="86cb6-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="86cb6-148">Filtre d' **exception** , qui s’exécute si une exception non gérée est levée quelque part dans la méthode d’action, en commençant par les filtres d’autorisation et en terminant par l’exécution du résultat.</span><span class="sxs-lookup"><span data-stu-id="86cb6-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="86cb6-149">Les filtres d'exception peuvent être utilisés pour des tâches telles que la journalisation ou l'affichage d'une page d'erreurs.</span><span class="sxs-lookup"><span data-stu-id="86cb6-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="86cb6-150">Pour plus d’informations sur les fournisseurs de filtres, visitez ce lien MSDN : ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="86cb6-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="86cb6-151">À propos de la fonctionnalité de journalisation des applications du Store musical MVC</span><span class="sxs-lookup"><span data-stu-id="86cb6-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="86cb6-152">Cette solution du store de musique contient une nouvelle table de modèle de données pour la journalisation du site, **ActionLog**, avec les champs suivants : nom du contrôleur qui a reçu une demande, appelée action, adresse IP du client et horodatage.</span><span class="sxs-lookup"><span data-stu-id="86cb6-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="86cb6-153">![Modèle de données. Table ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modèle de données. Table ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="86cb6-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="86cb6-154">*Modèle de données-table ActionLog*</span><span class="sxs-lookup"><span data-stu-id="86cb6-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="86cb6-155">La solution fournit une vue MVC ASP.NET pour le journal des actions, accessible à **MvcMusicStores/views/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="86cb6-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="86cb6-156">![Vue du journal des actions](aspnet-mvc-4-custom-action-filters/_static/image2.png "Vue du journal des actions")</span><span class="sxs-lookup"><span data-stu-id="86cb6-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="86cb6-157">*Vue du journal des actions*</span><span class="sxs-lookup"><span data-stu-id="86cb6-157">*Action Log view*</span></span>

<span data-ttu-id="86cb6-158">Avec cette structure donnée, tout le travail est axé sur l’interruption de la demande du contrôleur et sur l’exécution de la journalisation à l’aide d’un filtrage personnalisé.</span><span class="sxs-lookup"><span data-stu-id="86cb6-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="86cb6-159">Tâche 1 : création d’un filtre personnalisé pour intercepter la demande d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="86cb6-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="86cb6-160">Au cours de cette tâche, vous allez créer une classe d’attributs de filtre personnalisée qui contiendra la logique de journalisation.</span><span class="sxs-lookup"><span data-stu-id="86cb6-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="86cb6-161">Dans ce but, vous étendez la classe **ACTIONFILTERATTRIBUTE** MVC ASP.net et implémenterez l’interface **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-162">**ActionFilterAttribute** est la classe de base pour tous les filtres d’attribut.</span><span class="sxs-lookup"><span data-stu-id="86cb6-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="86cb6-163">Il fournit les méthodes suivantes pour exécuter une logique spécifique après et avant l’exécution de l’action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="86cb6-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="86cb6-164">**OnActionExecuting**(ActionExecutingContext filterContext) : juste avant l’appel de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="86cb6-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="86cb6-165">**OnActionExecuted**(ActionExecutedContext filterContext) : après l’appel de la méthode d’action et avant l’exécution du résultat (avant le rendu de la vue).</span><span class="sxs-lookup"><span data-stu-id="86cb6-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="86cb6-166">**OnResultExecuting**(ResultExecutingContext filterContext) : juste avant l’exécution du résultat (avant le rendu de la vue).</span><span class="sxs-lookup"><span data-stu-id="86cb6-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="86cb6-167">**OnResultExecuted**(ResultExecutedContext filterContext) : après l’exécution du résultat (après le rendu de la vue).</span><span class="sxs-lookup"><span data-stu-id="86cb6-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="86cb6-168">En remplaçant chacune de ces méthodes dans une classe dérivée, vous pouvez exécuter votre propre code de filtrage.</span><span class="sxs-lookup"><span data-stu-id="86cb6-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>

1. <span data-ttu-id="86cb6-169">Ouvrez le dossier **Begin** solution situé dans le dossier **\Source\Ex01-LoggingActions\Begin** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="86cb6-170">Vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="86cb6-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="86cb6-171">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="86cb6-172">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="86cb6-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="86cb6-173">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="86cb6-174">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="86cb6-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="86cb6-175">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="86cb6-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="86cb6-176">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="86cb6-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="86cb6-177">Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="86cb6-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="86cb6-178">Ajoutez une nouvelle C# classe dans le dossier **Filters** et nommez-la *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="86cb6-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="86cb6-179">Ce dossier stocke tous les filtres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="86cb6-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="86cb6-180">Ouvrez **CustomActionFilter.cs** et ajoutez une référence aux espaces de noms **System. Web. Mvc** et **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="86cb6-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="86cb6-181">(Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX1-CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="86cb6-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="86cb6-182">Héritez de la classe **CustomActionFilter** de **ActionFilterAttribute** , puis faites en sorte que la classe **CustomActionFilter** implémente l’interface **IActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="86cb6-183">Faites en sorte que la classe **CustomActionFilter** remplace la méthode **OnActionExecuting** et ajoute la logique nécessaire pour enregistrer l’exécution du filtre.</span><span class="sxs-lookup"><span data-stu-id="86cb6-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="86cb6-184">Pour ce faire, ajoutez le code en surbrillance suivant dans la classe **CustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="86cb6-185">(Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX1-LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="86cb6-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="86cb6-186">La méthode **OnActionExecuting** utilise **Entity Framework** pour ajouter un nouveau registre ActionLog.</span><span class="sxs-lookup"><span data-stu-id="86cb6-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="86cb6-187">Il crée et remplit une nouvelle instance d’entité avec les informations de contexte de **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="86cb6-188">Vous pouvez en savoir plus sur la classe **ControllerContext** sur [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="86cb6-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="86cb6-189">Tâche 2 : injection d’un intercepteur de code dans la classe de contrôleur du magasin</span><span class="sxs-lookup"><span data-stu-id="86cb6-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="86cb6-190">Au cours de cette tâche, vous allez ajouter le filtre personnalisé en l’injectant à toutes les classes de contrôleur et à toutes les actions du contrôleur qui seront journalisées.</span><span class="sxs-lookup"><span data-stu-id="86cb6-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="86cb6-191">Dans le cadre de cet exercice, la classe de contrôleur de magasin aura un journal.</span><span class="sxs-lookup"><span data-stu-id="86cb6-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="86cb6-192">La méthode **OnActionExecuting** à partir du filtre personnalisé **ActionLogFilterAttribute** s’exécute lorsqu’un élément injecté est appelé.</span><span class="sxs-lookup"><span data-stu-id="86cb6-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="86cb6-193">Il est également possible d’intercepter une méthode de contrôleur spécifique.</span><span class="sxs-lookup"><span data-stu-id="86cb6-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="86cb6-194">Ouvrez **StoreController** sur **MvcMusicStore\Controllers** et ajoutez une référence à l’espace de noms **Filters** :</span><span class="sxs-lookup"><span data-stu-id="86cb6-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="86cb6-195">Injectez le filtre personnalisé **CustomActionFilter** dans la classe **StoreController** en ajoutant l’attribut **[CustomActionFilter]** avant la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="86cb6-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="86cb6-196">Lorsqu’un filtre est injecté dans une classe de contrôleur, toutes ses actions sont également injectées.</span><span class="sxs-lookup"><span data-stu-id="86cb6-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="86cb6-197">Si vous souhaitez appliquer le filtre uniquement à un ensemble d’actions, vous devez injecter **[CustomActionFilter]** à chacun d’eux :</span><span class="sxs-lookup"><span data-stu-id="86cb6-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="86cb6-198">Tâche 3 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="86cb6-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="86cb6-199">Dans cette tâche, vous allez tester le fonctionnement du filtre de journalisation.</span><span class="sxs-lookup"><span data-stu-id="86cb6-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="86cb6-200">Vous allez démarrer l’application et visiter le Windows Store, puis vérifier les activités journalisées.</span><span class="sxs-lookup"><span data-stu-id="86cb6-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="86cb6-201">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="86cb6-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="86cb6-202">Accédez à **/ActionLog** pour afficher l’état initial de l’affichage des journaux :</span><span class="sxs-lookup"><span data-stu-id="86cb6-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="86cb6-203">![État du suivi des journaux avant l’activité de la page](aspnet-mvc-4-custom-action-filters/_static/image3.png "État du suivi des journaux avant l’activité de la page")</span><span class="sxs-lookup"><span data-stu-id="86cb6-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="86cb6-204">*État du suivi des journaux avant l’activité de la page*</span><span class="sxs-lookup"><span data-stu-id="86cb6-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="86cb6-205">Par défaut, il affiche toujours un élément qui est généré lors de la récupération des genres existants du menu.</span><span class="sxs-lookup"><span data-stu-id="86cb6-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="86cb6-206">Pour des raisons de simplicité, nous supprimons la table **ActionLog** chaque fois que l’application s’exécute, de sorte qu’elle affiche uniquement les journaux de la vérification de chaque tâche particulière.</span><span class="sxs-lookup"><span data-stu-id="86cb6-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="86cb6-207">Vous devrez peut-être supprimer le code suivant de la **Session\_méthode Start** (dans la classe **global. asax** ), afin d’enregistrer un journal historique pour toutes les actions exécutées dans le contrôleur du magasin.</span><span class="sxs-lookup"><span data-stu-id="86cb6-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="86cb6-208">Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.</span><span class="sxs-lookup"><span data-stu-id="86cb6-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="86cb6-209">Accédez à **/ActionLog** et, si le journal est vide, appuyez sur **F5** pour actualiser la page.</span><span class="sxs-lookup"><span data-stu-id="86cb6-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="86cb6-210">Vérifiez que vos visites ont été suivies :</span><span class="sxs-lookup"><span data-stu-id="86cb6-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="86cb6-211">![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image4.png "Journal des actions avec activité journalisée")</span><span class="sxs-lookup"><span data-stu-id="86cb6-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="86cb6-212">*Journal des actions avec activité journalisée*</span><span class="sxs-lookup"><span data-stu-id="86cb6-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="86cb6-213">Exercice 2 : gestion de plusieurs filtres d’action</span><span class="sxs-lookup"><span data-stu-id="86cb6-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="86cb6-214">Dans cet exercice, vous allez ajouter un deuxième filtre d’action personnalisé à la classe StoreController et définir l’ordre spécifique dans lequel les deux filtres seront exécutés.</span><span class="sxs-lookup"><span data-stu-id="86cb6-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="86cb6-215">Ensuite, vous allez mettre à jour le code pour inscrire le filtre globalement.</span><span class="sxs-lookup"><span data-stu-id="86cb6-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="86cb6-216">Il existe différentes options à prendre en compte lors de la définition de l’ordre d’exécution des filtres.</span><span class="sxs-lookup"><span data-stu-id="86cb6-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="86cb6-217">Par exemple, la propriété Order et l’étendue des filtres :</span><span class="sxs-lookup"><span data-stu-id="86cb6-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="86cb6-218">Vous pouvez définir une **étendue** pour chaque filtre. par exemple, vous pouvez définir l’étendue de tous les filtres d’action à exécuter dans l' **étendue du contrôleur**et tous les filtres d’autorisation à exécuter dans l' **étendue globale**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="86cb6-219">Les étendues ont un ordre d’exécution défini.</span><span class="sxs-lookup"><span data-stu-id="86cb6-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="86cb6-220">En outre, chaque filtre d’action a une propriété Order qui est utilisée pour déterminer l’ordre d’exécution dans l’étendue du filtre.</span><span class="sxs-lookup"><span data-stu-id="86cb6-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="86cb6-221">Pour plus d’informations sur l’ordre d’exécution des filtres d’action personnalisés, consultez cet article MSDN : ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="86cb6-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="86cb6-222">Tâche 1 : création d’un filtre d’action personnalisé</span><span class="sxs-lookup"><span data-stu-id="86cb6-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="86cb6-223">Dans cette tâche, vous allez créer un filtre d’action personnalisé à injecter dans la classe StoreController, en apprenant à gérer l’ordre d’exécution des filtres.</span><span class="sxs-lookup"><span data-stu-id="86cb6-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="86cb6-224">Ouvrez le dossier **Begin** solution situé dans le dossier **\Source\Ex02-ManagingMultipleActionFilters\Begin** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="86cb6-225">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="86cb6-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="86cb6-226">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="86cb6-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="86cb6-227">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="86cb6-228">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="86cb6-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="86cb6-229">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="86cb6-230">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="86cb6-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="86cb6-231">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="86cb6-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="86cb6-232">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="86cb6-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="86cb6-233">Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="86cb6-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="86cb6-234">Ajoutez une nouvelle C# classe dans le dossier **Filters** et nommez-la *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="86cb6-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="86cb6-235">Ouvrez **MyNewCustomActionFilter.cs** et ajoutez une référence à **System. Web. Mvc** et à l’espace de noms **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="86cb6-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="86cb6-236">(Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX2-MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="86cb6-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="86cb6-237">Remplacez la déclaration de classe par défaut par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="86cb6-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="86cb6-238">(Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX2-MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="86cb6-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="86cb6-239">Ce filtre d’action personnalisé est quasiment identique à celui que vous avez créé au cours de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="86cb6-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="86cb6-240">La principale différence réside dans le fait qu’elle a le *&quot;enregistré par&quot;* attribut mis à jour avec ce nouveau nom de classe pour identifier le filtre qui a inscrit le journal.</span><span class="sxs-lookup"><span data-stu-id="86cb6-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify which filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="86cb6-241">Tâche 2 : injection d’un nouvel intercepteur de code dans la classe StoreController</span><span class="sxs-lookup"><span data-stu-id="86cb6-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="86cb6-242">Dans cette tâche, vous allez ajouter un nouveau filtre personnalisé à la classe StoreController et exécuter la solution pour vérifier la façon dont les deux filtres fonctionnent ensemble.</span><span class="sxs-lookup"><span data-stu-id="86cb6-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="86cb6-243">Ouvrez la classe **StoreController** située dans **MvcMusicStore\Controllers** et injectez le nouveau filtre personnalisé **MyNewCustomActionFilter** dans la classe **StoreController** comme indiqué dans le code suivant.</span><span class="sxs-lookup"><span data-stu-id="86cb6-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="86cb6-244">À présent, exécutez l’application afin de voir comment ces deux filtres d’action personnalisés fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="86cb6-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="86cb6-245">Pour ce faire, appuyez sur **F5** et attendez que l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="86cb6-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="86cb6-246">Accédez à **/ActionLog** pour afficher l’état initial de l’affichage des journaux.</span><span class="sxs-lookup"><span data-stu-id="86cb6-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="86cb6-247">![État du suivi des journaux avant l’activité de la page](aspnet-mvc-4-custom-action-filters/_static/image5.png "État du suivi des journaux avant l’activité de la page")</span><span class="sxs-lookup"><span data-stu-id="86cb6-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="86cb6-248">*État du suivi des journaux avant l’activité de la page*</span><span class="sxs-lookup"><span data-stu-id="86cb6-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="86cb6-249">Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.</span><span class="sxs-lookup"><span data-stu-id="86cb6-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="86cb6-250">Vérifiez que cette fois-ci ; vos visites ont été suivies deux fois : une fois pour chacun des filtres d’action personnalisés que vous avez ajoutés à la classe **StorageController** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="86cb6-251">![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image6.png "Journal des actions avec activité journalisée")</span><span class="sxs-lookup"><span data-stu-id="86cb6-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="86cb6-252">*Journal des actions avec activité journalisée*</span><span class="sxs-lookup"><span data-stu-id="86cb6-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="86cb6-253">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="86cb6-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="86cb6-254">Tâche 3 : gestion de l’ordre des filtres</span><span class="sxs-lookup"><span data-stu-id="86cb6-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="86cb6-255">Dans cette tâche, vous allez apprendre à gérer l’ordre d’exécution des filtres à l’aide de la propriété Order.</span><span class="sxs-lookup"><span data-stu-id="86cb6-255">In this task, you will learn how to manage the filters' execution order by using the Order property.</span></span>

1. <span data-ttu-id="86cb6-256">Ouvrez la classe **StoreController** située dans **MvcMusicStore\Controllers** et spécifiez la propriété **Order** dans les deux filtres, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="86cb6-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="86cb6-257">À présent, vérifiez la façon dont les filtres sont exécutés en fonction de la valeur de la propriété Order.</span><span class="sxs-lookup"><span data-stu-id="86cb6-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="86cb6-258">Vous constaterez que le filtre avec la plus petite valeur d’ordre (**CustomActionFilter**) est le premier qui est exécuté.</span><span class="sxs-lookup"><span data-stu-id="86cb6-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="86cb6-259">Appuyez sur **F5** et attendez que l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="86cb6-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="86cb6-260">Accédez à **/ActionLog** pour afficher l’état initial de l’affichage des journaux.</span><span class="sxs-lookup"><span data-stu-id="86cb6-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="86cb6-261">![État du suivi des journaux avant l’activité de la page](aspnet-mvc-4-custom-action-filters/_static/image7.png "État du suivi des journaux avant l’activité de la page")</span><span class="sxs-lookup"><span data-stu-id="86cb6-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="86cb6-262">*État du suivi des journaux avant l’activité de la page*</span><span class="sxs-lookup"><span data-stu-id="86cb6-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="86cb6-263">Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.</span><span class="sxs-lookup"><span data-stu-id="86cb6-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="86cb6-264">Vérifiez que cette fois, vos visites ont été suivies triées par la valeur de commande des filtres : **CustomActionFilter** logs.</span><span class="sxs-lookup"><span data-stu-id="86cb6-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="86cb6-265">![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image8.png "Journal des actions avec activité journalisée")</span><span class="sxs-lookup"><span data-stu-id="86cb6-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="86cb6-266">*Journal des actions avec activité journalisée*</span><span class="sxs-lookup"><span data-stu-id="86cb6-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="86cb6-267">À présent, vous allez mettre à jour la valeur de commande des filtres et vérifier comment l’ordre de journalisation change.</span><span class="sxs-lookup"><span data-stu-id="86cb6-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="86cb6-268">Dans la classe **StoreController** , mettez à jour la valeur d’ordre des filtres comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="86cb6-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="86cb6-269">Réexécutez l’application en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="86cb6-270">Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.</span><span class="sxs-lookup"><span data-stu-id="86cb6-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="86cb6-271">Vérifiez que cette fois, le filtre journaux créés par **MyNewCustomActionFilter** s’affiche en premier.</span><span class="sxs-lookup"><span data-stu-id="86cb6-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="86cb6-272">![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image9.png "Journal des actions avec activité journalisée")</span><span class="sxs-lookup"><span data-stu-id="86cb6-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="86cb6-273">*Journal des actions avec activité journalisée*</span><span class="sxs-lookup"><span data-stu-id="86cb6-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="86cb6-274">Tâche 4 : enregistrement global des filtres</span><span class="sxs-lookup"><span data-stu-id="86cb6-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="86cb6-275">Dans cette tâche, vous allez mettre à jour la solution pour inscrire le nouveau filtre (**MyNewCustomActionFilter**) en tant que filtre global.</span><span class="sxs-lookup"><span data-stu-id="86cb6-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="86cb6-276">En procédant ainsi, elle est déclenchée par toutes les actions effectuées dans l’application et non seulement dans les StoreController, comme dans la tâche précédente.</span><span class="sxs-lookup"><span data-stu-id="86cb6-276">By doing this, it will be triggered by all the actions performed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="86cb6-277">Dans la classe **StoreController** , supprimez l’attribut **[MyNewCustomActionFilter]** et la propriété Order de **[CustomActionFilter]** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="86cb6-278">Il doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="86cb6-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="86cb6-279">Ouvrez le fichier **global. asax** et recherchez l' **application\_méthode Start** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="86cb6-280">Notez que chaque fois que l’application démarre, il inscrit les filtres globaux en appelant la méthode **RegisterGlobalFilters** dans la classe **FilterConfig** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="86cb6-281">![Inscription de filtres globaux dans global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Inscription de filtres globaux dans global. asax")</span><span class="sxs-lookup"><span data-stu-id="86cb6-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="86cb6-282">*Inscription de filtres globaux dans global. asax*</span><span class="sxs-lookup"><span data-stu-id="86cb6-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="86cb6-283">Ouvrez le fichier **FilterConfig.cs** dans le dossier de démarrage de l' **application\_** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="86cb6-284">Ajoutez une référence à à l’aide de System. Web. Mvc ; utilisation de MvcMusicStore. filters ; joint.</span><span class="sxs-lookup"><span data-stu-id="86cb6-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="86cb6-285">Mettez à jour la méthode **RegisterGlobalFilters** en ajoutant votre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="86cb6-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="86cb6-286">Pour ce faire, ajoutez le code en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="86cb6-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="86cb6-287">Exécutez l’application en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="86cb6-288">Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.</span><span class="sxs-lookup"><span data-stu-id="86cb6-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="86cb6-289">Vérifiez que **[MyNewCustomActionFilter]** est également injecté dans HomeController et ActionLogController.</span><span class="sxs-lookup"><span data-stu-id="86cb6-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="86cb6-290">![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image11.png "Journal des actions avec activité journalisée")</span><span class="sxs-lookup"><span data-stu-id="86cb6-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="86cb6-291">*Journal des actions avec activité globale journalisée*</span><span class="sxs-lookup"><span data-stu-id="86cb6-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-292">En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="86cb6-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="86cb6-293">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="86cb6-293">Summary</span></span>

<span data-ttu-id="86cb6-294">En effectuant ce laboratoire pratique, vous avez appris à étendre un filtre d’action pour exécuter des actions personnalisées.</span><span class="sxs-lookup"><span data-stu-id="86cb6-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="86cb6-295">Vous avez également appris à injecter un filtre sur vos contrôleurs de page.</span><span class="sxs-lookup"><span data-stu-id="86cb6-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="86cb6-296">Les concepts suivants ont été utilisés :</span><span class="sxs-lookup"><span data-stu-id="86cb6-296">The following concepts were used:</span></span>

- <span data-ttu-id="86cb6-297">Comment créer des filtres d’action personnalisés avec la classe ActionFilterAttribute MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86cb6-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="86cb6-298">Comment injecter des filtres dans les contrôleurs MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86cb6-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="86cb6-299">Comment gérer le classement des filtres à l’aide de la propriété Order</span><span class="sxs-lookup"><span data-stu-id="86cb6-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="86cb6-300">Comment inscrire des filtres globalement</span><span class="sxs-lookup"><span data-stu-id="86cb6-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="86cb6-301">Annexe A : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="86cb6-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="86cb6-302">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="86cb6-303">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="86cb6-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="86cb6-304">Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="86cb6-304">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="86cb6-305">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="86cb6-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="86cb6-306">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-306">Click on **Install Now**.</span></span> <span data-ttu-id="86cb6-307">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="86cb6-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="86cb6-308">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="86cb6-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="86cb6-309">![Installer Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="86cb6-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="86cb6-310">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="86cb6-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="86cb6-311">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="86cb6-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="86cb6-313">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="86cb6-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="86cb6-314">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="86cb6-314">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="86cb6-316">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="86cb6-316">*Installation progress*</span></span>
6. <span data-ttu-id="86cb6-317">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-317">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="86cb6-319">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="86cb6-319">*Installation completed*</span></span>
7. <span data-ttu-id="86cb6-320">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="86cb6-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="86cb6-321">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="86cb6-323">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="86cb6-324">Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="86cb6-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="86cb6-325">Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="86cb6-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="86cb6-326">Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure</span><span class="sxs-lookup"><span data-stu-id="86cb6-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="86cb6-327">Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="86cb6-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="86cb6-328">Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="86cb6-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="86cb6-329">Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="86cb6-329">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="86cb6-330">![Ouvrir une session sur Windows Portail Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Ouvrir une session sur Windows Portail Azure")</span><span class="sxs-lookup"><span data-stu-id="86cb6-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="86cb6-331">*Connectez-vous à Windows Azure Portail de gestion*</span><span class="sxs-lookup"><span data-stu-id="86cb6-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="86cb6-332">Cliquez sur **nouveau** dans la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="86cb6-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="86cb6-333">![Création d’un nouveau site Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Création d’un nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="86cb6-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="86cb6-334">*Création d’un nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="86cb6-335">Cliquez sur **compute** | **site Web**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="86cb6-336">Sélectionnez ensuite l’option **création rapide** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="86cb6-337">Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="86cb6-338">Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="86cb6-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="86cb6-339">L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="86cb6-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="86cb6-340">Elle n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="86cb6-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="86cb6-341">![Création d’un nouveau site Web à l’aide de la création rapide](aspnet-mvc-4-custom-action-filters/_static/image19.png "Création d’un nouveau site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="86cb6-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="86cb6-342">*Création d’un nouveau site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="86cb6-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="86cb6-343">Patientez jusqu’à la création du nouveau **site Web** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="86cb6-344">Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="86cb6-345">Vérifiez que le nouveau site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="86cb6-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="86cb6-346">![Navigation vers le nouveau site Web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Navigation vers le nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="86cb6-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="86cb6-347">*Navigation vers le nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="86cb6-348">![Site Web en cours d’exécution](aspnet-mvc-4-custom-action-filters/_static/image21.png "Site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="86cb6-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="86cb6-349">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="86cb6-349">*Web site running*</span></span>
6. <span data-ttu-id="86cb6-350">Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="86cb6-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="86cb6-351">![Ouverture des pages de gestion de site Web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Ouverture des pages de gestion de site Web")</span><span class="sxs-lookup"><span data-stu-id="86cb6-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="86cb6-352">*Ouverture des pages de gestion de site Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="86cb6-353">Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .</span><span class="sxs-lookup"><span data-stu-id="86cb6-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="86cb6-354">Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="86cb6-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="86cb6-355">Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="86cb6-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="86cb6-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="86cb6-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="86cb6-357">![Téléchargement du profil de publication du site Web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Téléchargement du profil de publication du site Web")</span><span class="sxs-lookup"><span data-stu-id="86cb6-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="86cb6-358">*Téléchargement du profil de publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="86cb6-359">Téléchargez le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="86cb6-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="86cb6-360">Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86cb6-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="86cb6-361">![Enregistrement du fichier de profil de publication](aspnet-mvc-4-custom-action-filters/_static/image24.png "Enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="86cb6-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="86cb6-362">*Enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="86cb6-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="86cb6-363">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="86cb6-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="86cb6-364">Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database.</span><span class="sxs-lookup"><span data-stu-id="86cb6-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="86cb6-365">Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="86cb6-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="86cb6-366">Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="86cb6-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="86cb6-367">Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="86cb6-368">Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="86cb6-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="86cb6-369">Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="86cb6-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="86cb6-370">Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="86cb6-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="86cb6-371">![Tableau de bord du serveur SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "Tableau de bord du serveur SQL Database")</span><span class="sxs-lookup"><span data-stu-id="86cb6-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="86cb6-372">*Tableau de bord du serveur SQL Database*</span><span class="sxs-lookup"><span data-stu-id="86cb6-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="86cb6-373">Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur.</span><span class="sxs-lookup"><span data-stu-id="86cb6-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="86cb6-374">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](aspnet-mvc-4-custom-action-filters/_static/image26.png).</span><span class="sxs-lookup"><span data-stu-id="86cb6-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Ajout de l’adresse IP du client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="86cb6-376">*Ajout de l’adresse IP du client*</span><span class="sxs-lookup"><span data-stu-id="86cb6-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="86cb6-377">Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="86cb6-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="86cb6-379">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="86cb6-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="86cb6-380">Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="86cb6-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="86cb6-381">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="86cb6-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="86cb6-382">Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="86cb6-383">![Publication de l’application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publication de l'application")</span><span class="sxs-lookup"><span data-stu-id="86cb6-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="86cb6-384">*Publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="86cb6-385">Importez le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="86cb6-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="86cb6-386">![Importation du profil de publication](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="86cb6-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="86cb6-387">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="86cb6-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="86cb6-388">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-388">Click **Validate Connection**.</span></span> <span data-ttu-id="86cb6-389">Une fois la validation terminée, cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="86cb6-390">La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="86cb6-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="86cb6-391">![Validation de la connexion](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="86cb6-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="86cb6-392">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="86cb6-392">*Validating connection*</span></span>
4. <span data-ttu-id="86cb6-393">Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="86cb6-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="86cb6-394">![Configuration de Web Deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configuration de Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="86cb6-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="86cb6-395">*Configuration de Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="86cb6-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="86cb6-396">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="86cb6-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="86cb6-397">Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .</span><span class="sxs-lookup"><span data-stu-id="86cb6-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="86cb6-398">Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="86cb6-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="86cb6-399">Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="86cb6-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="86cb6-400">Tapez un nouveau nom de base de données.</span><span class="sxs-lookup"><span data-stu-id="86cb6-400">Type a new database name.</span></span>

     <span data-ttu-id="86cb6-401">![Configuration de la chaîne de connexion de destination](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuration de la chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="86cb6-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="86cb6-402">*Configuration de la chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="86cb6-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="86cb6-403">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-403">Then click **OK**.</span></span> <span data-ttu-id="86cb6-404">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="86cb6-405">![Création de la base de données](aspnet-mvc-4-custom-action-filters/_static/image34.png "Création de la chaîne de base de données")</span><span class="sxs-lookup"><span data-stu-id="86cb6-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="86cb6-406">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="86cb6-406">*Creating the database*</span></span>
7. <span data-ttu-id="86cb6-407">La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="86cb6-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="86cb6-408">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-408">Then click **Next**.</span></span>

    <span data-ttu-id="86cb6-409">![Chaîne de connexion pointant vers SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Chaîne de connexion pointant vers SQL Database")</span><span class="sxs-lookup"><span data-stu-id="86cb6-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="86cb6-410">*Chaîne de connexion pointant vers SQL Database*</span><span class="sxs-lookup"><span data-stu-id="86cb6-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="86cb6-411">Dans la page **Aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="86cb6-412">![Publication de l’application Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publication de l’application Web")</span><span class="sxs-lookup"><span data-stu-id="86cb6-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="86cb6-413">*Publication de l’application Web*</span><span class="sxs-lookup"><span data-stu-id="86cb6-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="86cb6-414">Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.</span><span class="sxs-lookup"><span data-stu-id="86cb6-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="86cb6-415">Annexe C : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="86cb6-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="86cb6-416">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="86cb6-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="86cb6-417">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="86cb6-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="86cb6-418">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-custom-action-filters/_static/image37.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="86cb6-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="86cb6-419">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="86cb6-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="86cb6-420">***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***</span><span class="sxs-lookup"><span data-stu-id="86cb6-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="86cb6-421">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="86cb6-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="86cb6-422">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="86cb6-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="86cb6-423">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="86cb6-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="86cb6-424">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="86cb6-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="86cb6-425">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="86cb6-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="86cb6-426">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-custom-action-filters/_static/image38.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="86cb6-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="86cb6-427">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="86cb6-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="86cb6-428">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-custom-action-filters/_static/image39.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="86cb6-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="86cb6-429">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="86cb6-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="86cb6-430">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-custom-action-filters/_static/image40.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="86cb6-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="86cb6-431">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="86cb6-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="86cb6-432">***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0.</span><span class="sxs-lookup"><span data-stu-id="86cb6-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="86cb6-433">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="86cb6-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="86cb6-434">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="86cb6-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="86cb6-435">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="86cb6-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="86cb6-436">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-custom-action-filters/_static/image41.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="86cb6-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="86cb6-437">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="86cb6-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="86cb6-438">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-custom-action-filters/_static/image42.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="86cb6-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="86cb6-439">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="86cb6-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
