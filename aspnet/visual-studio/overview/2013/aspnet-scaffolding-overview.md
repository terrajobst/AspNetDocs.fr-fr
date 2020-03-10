---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Génération de modèles automatique ASP.NET dans Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: La génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557980"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="b4363-103">Génération de modèles automatique ASP.NET dans Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b4363-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>

<span data-ttu-id="b4363-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b4363-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b4363-105">La génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b4363-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>

## <a name="overview"></a><span data-ttu-id="b4363-106">Présentation</span><span class="sxs-lookup"><span data-stu-id="b4363-106">Overview</span></span>

<span data-ttu-id="b4363-107">La génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4363-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="b4363-108">Visual Studio 2013 comprend des générateurs de code préinstallés pour les projets MVC et API Web.</span><span class="sxs-lookup"><span data-stu-id="b4363-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="b4363-109">Vous ajoutez la génération de modèles automatique à votre projet lorsque vous souhaitez ajouter rapidement du code qui interagit avec les modèles de données.</span><span class="sxs-lookup"><span data-stu-id="b4363-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="b4363-110">L’utilisation de la génération de modèles automatique peut réduire le temps nécessaire au développement d’opérations de données standard dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="b4363-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="b4363-111">Par défaut, Visual Studio 2013 ne prend pas en charge la génération de code pour un projet Web Forms, mais vous pouvez utiliser la génération de modèles automatique avec Web Forms en ajoutant des dépendances MVC au projet ou en installant une extension.</span><span class="sxs-lookup"><span data-stu-id="b4363-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="b4363-112">Les deux approches sont présentées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b4363-112">Both approaches are shown below.</span></span>

<span data-ttu-id="b4363-113">Visual Studio 2013 Update 2 (actuellement RC) offre la possibilité d’étendre la génération de modèles automatique ASP.NET pour répondre aux exigences de votre scénario.</span><span class="sxs-lookup"><span data-stu-id="b4363-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="b4363-114">Avec cette fonctionnalité, vous pouvez créer un modèle de génération de modèles automatique personnalisé et l’ajouter à la boîte de dialogue Ajouter une nouvelle structure.</span><span class="sxs-lookup"><span data-stu-id="b4363-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="b4363-115">Dans le modèle personnalisé, vous spécifiez le code généré lors de l’ajout d’un élément de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="b4363-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="b4363-116">Pour plus d’informations, consultez [création d’un générateur de modèles personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="b4363-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4363-117">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="b4363-117">Prerequisites</span></span>

<span data-ttu-id="b4363-118">Pour utiliser la génération de modèles automatique ASP.NET, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b4363-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="b4363-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b4363-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="b4363-120">Outils de développement Web (partie de l’installation Visual Studio 2013 par défaut)</span><span class="sxs-lookup"><span data-stu-id="b4363-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="b4363-121">Frameworks et outils Web ASP.NET 2013 (partie de l’installation Visual Studio 2013 par défaut)</span><span class="sxs-lookup"><span data-stu-id="b4363-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="b4363-122">Ajouter un élément de génération de modèles automatique à MVC ou à l’API Web</span><span class="sxs-lookup"><span data-stu-id="b4363-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="b4363-123">Pour ajouter une structure, cliquez avec le bouton droit sur le projet ou sur un dossier dans le projet, puis sélectionnez **Ajouter** – **nouvel élément**généré automatiquement, comme illustré dans l’image suivante.</span><span class="sxs-lookup"><span data-stu-id="b4363-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Ajouter un élément de structure](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="b4363-125">Dans la fenêtre **Ajouter une structure** , sélectionnez le type de structure à ajouter.</span><span class="sxs-lookup"><span data-stu-id="b4363-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Sélectionner le type de structure](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="b4363-127">La fenêtre **Ajouter un contrôleur** vous donne la possibilité de sélectionner des options pour générer le contrôleur, notamment si vous souhaitez utiliser les nouvelles fonctionnalités async de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b4363-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Ajouter un contrôleur](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="b4363-129">Les classes et les pages pertinentes sont créées pour votre scénario.</span><span class="sxs-lookup"><span data-stu-id="b4363-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="b4363-130">Par exemple, l’illustration suivante montre le contrôleur MVC et les vues qui ont été créées à l’aide de la génération de modèles automatique pour une classe de modèle nommée movies.</span><span class="sxs-lookup"><span data-stu-id="b4363-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Fichiers créés](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="b4363-132">Ajouter un élément de génération de modèles automatique à Web Forms</span><span class="sxs-lookup"><span data-stu-id="b4363-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="b4363-133">Pour ajouter la génération de modèles automatique qui génère Web Forms code, vous devez installer une extension dans Visual Studio ou ajouter des dépendances MVC.</span><span class="sxs-lookup"><span data-stu-id="b4363-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="b4363-134">Les deux approches sont présentées ci-dessous, mais vous n’avez besoin d’effectuer qu’une seule de ces approches.</span><span class="sxs-lookup"><span data-stu-id="b4363-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="b4363-135">Extension de génération de modèles automatique Web Forms</span><span class="sxs-lookup"><span data-stu-id="b4363-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="b4363-136">Vous pouvez installer une extension Visual Studio qui vous permet d’utiliser la génération de modèles automatique avec un projet de Web Forms.</span><span class="sxs-lookup"><span data-stu-id="b4363-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="b4363-137">Dans Visual Studio, sélectionnez **Outils** , puis **extensions et mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="b4363-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="b4363-138">Dans cette boîte de dialogue, recherchez **Web Forms génération de modèles**automatique dans la Galerie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4363-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![installer la génération de modèles automatique Web Forms](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="b4363-140">Pour plus d’informations, consultez [génération de modèles automatique Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="b4363-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="b4363-141">Dépendances MVC</span><span class="sxs-lookup"><span data-stu-id="b4363-141">MVC Dependencies</span></span>

<span data-ttu-id="b4363-142">Pour ajouter des dépendances MVC, sélectionnez **ajouter** - **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="b4363-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="b4363-143">Dans la fenêtre Ajouter une structure, sélectionnez **dépendances MVC**, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b4363-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Ajouter des dépendances MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="b4363-145">Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète.</span><span class="sxs-lookup"><span data-stu-id="b4363-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="b4363-146">Si vous sélectionnez minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet.</span><span class="sxs-lookup"><span data-stu-id="b4363-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="b4363-147">Si vous sélectionnez l’option complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.</span><span class="sxs-lookup"><span data-stu-id="b4363-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="b4363-148">Pour utiliser facilement la génération de modèles automatique, sélectionnez dépendances complètes.</span><span class="sxs-lookup"><span data-stu-id="b4363-148">To easily use scaffolding, select Full dependencies.</span></span>

![sélectionner les dépendances complètes](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="b4363-150">Après avoir ajouté les dépendances, vous verrez un fichier **Readme. txt** .</span><span class="sxs-lookup"><span data-stu-id="b4363-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="b4363-151">Suivez attentivement les instructions de ce fichier pour vous assurer que votre projet fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="b4363-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="b4363-152">Une fois que vous avez effectué les étapes du fichier Readme. txt, vous pouvez ajouter un nouvel élément de génération de modèles automatique comme indiqué dans la section précédente sur MVC et l’API Web.</span><span class="sxs-lookup"><span data-stu-id="b4363-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="b4363-153">Les vues et le contrôleur générés automatiquement fonctionneront correctement dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="b4363-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="b4363-154">Didacticiels</span><span class="sxs-lookup"><span data-stu-id="b4363-154">Tutorials</span></span>

<span data-ttu-id="b4363-155">Pour créer un modèle de modèle personnalisé, consultez [création d’un générateur de modèles personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="b4363-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="b4363-156">Pour personnaliser les fichiers générés, consultez [Comment personnaliser les fichiers générés à partir de la boîte de dialogue Nouvel élément de génération de modèles](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)automatique.</span><span class="sxs-lookup"><span data-stu-id="b4363-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="b4363-157">Pour obtenir un exemple d’utilisation de la génération de modèles automatique avec le **développement Database First**, consultez [EF Database First avec ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="b4363-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="b4363-158">Pour obtenir un exemple d’utilisation de la génération de modèles automatique dans un projet **MVC** , consultez [prise en main avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b4363-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="b4363-159">Pour obtenir un exemple d’utilisation de la génération de modèles automatique dans un projet d' **API Web** , consultez [créer une API REST avec routage d’attributs dans Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="b4363-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
