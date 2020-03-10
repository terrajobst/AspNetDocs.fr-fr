---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Atelier pratique : outils Web Visual Studio 2013 | Microsoft Docs'
author: rick-anderson
description: Visual Studio est un excellent environnement de développement pour. Projets Windows et Web basés sur .net. Il comprend un éditeur de texte puissant qui peut facilement être utilisé pour...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622674"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="cd76d-104">Atelier pratique : outils Web Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cd76d-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="cd76d-105">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="cd76d-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="cd76d-106">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="cd76d-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="cd76d-107">Visual Studio est un excellent environnement de développement pour. Projets Windows et Web basés sur .net.</span><span class="sxs-lookup"><span data-stu-id="cd76d-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="cd76d-108">Il comprend un éditeur de texte puissant qui peut facilement être utilisé pour modifier des fichiers autonomes sans projet.</span><span class="sxs-lookup"><span data-stu-id="cd76d-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="cd76d-109">Visual Studio gère une arborescence d’analyse complète à mesure que vous modifiez chaque fichier.</span><span class="sxs-lookup"><span data-stu-id="cd76d-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="cd76d-110">Cela permet à Visual Studio de fournir une saisie semi-automatique inégalée et des actions basées sur les documents tout en rendant l’expérience de développement plus rapide et plus agréable.</span><span class="sxs-lookup"><span data-stu-id="cd76d-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="cd76d-111">Ces fonctionnalités sont particulièrement puissantes dans les documents HTML et CSS.</span><span class="sxs-lookup"><span data-stu-id="cd76d-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="cd76d-112">Toute cette puissance est également disponible pour les extensions, ce qui simplifie l’extension des éditeurs avec de nouvelles fonctionnalités puissantes adaptées à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="cd76d-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="cd76d-113">Web Essentials est une collection d' (principalement) améliorations liées au Web dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="cd76d-114">Il comprend un grand nombre de nouvelles saisies semi-automatiques IntelliSense (en particulier pour CSS), de nouvelles fonctionnalités de lien de navigateur, des JSHint automatiques pour les fichiers JavaScript, de nouveaux avertissements pour HTML et CSS, et de nombreuses autres fonctionnalités qui sont essentielles au développement Web moderne.</span><span class="sxs-lookup"><span data-stu-id="cd76d-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="cd76d-115">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="cd76d-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="cd76d-116">Présentation</span><span class="sxs-lookup"><span data-stu-id="cd76d-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="cd76d-117">Objectifs</span><span class="sxs-lookup"><span data-stu-id="cd76d-117">Objectives</span></span>

<span data-ttu-id="cd76d-118">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="cd76d-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="cd76d-119">Utilisez les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, telles que les extraits de code HTML5 enrichis et le codage Zen</span><span class="sxs-lookup"><span data-stu-id="cd76d-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="cd76d-120">Utilisez les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials, telles que le sélecteur de couleurs et l’info-bulle de la matrice du navigateur</span><span class="sxs-lookup"><span data-stu-id="cd76d-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="cd76d-121">Utilisez les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Web Essentials, telles que Extract to file et IntelliSense pour tous les éléments HTML</span><span class="sxs-lookup"><span data-stu-id="cd76d-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="cd76d-122">Échanger des données entre votre navigateur et Visual Studio à l’aide d’un lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="cd76d-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="cd76d-123">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="cd76d-123">Prerequisites</span></span>

<span data-ttu-id="cd76d-124">Les éléments suivants sont requis pour effectuer ce laboratoire pratique :</span><span class="sxs-lookup"><span data-stu-id="cd76d-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="cd76d-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cd76d-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="cd76d-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="cd76d-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="cd76d-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="cd76d-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="cd76d-128">Installation</span><span class="sxs-lookup"><span data-stu-id="cd76d-128">Setup</span></span>

<span data-ttu-id="cd76d-129">Pour exécuter les exercices dans ce laboratoire pratique, vous devez d’abord configurer votre environnement.</span><span class="sxs-lookup"><span data-stu-id="cd76d-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="cd76d-130">Ouvrez une fenêtre de l’Explorateur Windows et accédez au dossier **source** du laboratoire.</span><span class="sxs-lookup"><span data-stu-id="cd76d-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="cd76d-131">Cliquez avec le bouton droit sur **Setup. cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui va configurer votre environnement et installer les extraits de code Visual Studio pour ce Lab.</span><span class="sxs-lookup"><span data-stu-id="cd76d-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="cd76d-132">Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.</span><span class="sxs-lookup"><span data-stu-id="cd76d-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="cd76d-133">Vérifiez que vous avez vérifié toutes les dépendances de ce Lab avant d’exécuter le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="cd76d-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="cd76d-134">Utilisation des extraits de code</span><span class="sxs-lookup"><span data-stu-id="cd76d-134">Using the Code Snippets</span></span>

<span data-ttu-id="cd76d-135">Tout au long du document Lab, vous serez invité à insérer des blocs de code.</span><span class="sxs-lookup"><span data-stu-id="cd76d-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="cd76d-136">Pour plus de commodité, la majeure partie de ce code est fournie sous la forme d’extraits de Visual Studio Code, auxquels vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à l’ajouter manuellement.</span><span class="sxs-lookup"><span data-stu-id="cd76d-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="cd76d-137">Chaque exercice est accompagné d’une solution de démarrage située dans le dossier **Begin** de l’exercice, qui vous permet de suivre chaque exercice indépendamment des autres.</span><span class="sxs-lookup"><span data-stu-id="cd76d-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="cd76d-138">N’oubliez pas que les extraits de code ajoutés au cours d’un exercice sont absents de ces solutions de démarrage et peuvent ne pas fonctionner tant que vous n’avez pas terminé l’exercice.</span><span class="sxs-lookup"><span data-stu-id="cd76d-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="cd76d-139">À l’intérieur du code source d’un exercice, vous trouverez également un dossier de **fin** contenant une solution Visual Studio avec le code qui résulte de l’exécution des étapes de l’exercice correspondant.</span><span class="sxs-lookup"><span data-stu-id="cd76d-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="cd76d-140">Vous pouvez utiliser ces solutions comme conseils si vous avez besoin d’aide supplémentaire au fur et à mesure que vous travaillez dans ce laboratoire pratique.</span><span class="sxs-lookup"><span data-stu-id="cd76d-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="cd76d-141">Exercices</span><span class="sxs-lookup"><span data-stu-id="cd76d-141">Exercises</span></span>

<span data-ttu-id="cd76d-142">Ce laboratoire pratique comprend les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="cd76d-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="cd76d-143">Utilisation du lien de navigateur et de Web Essentials</span><span class="sxs-lookup"><span data-stu-id="cd76d-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="cd76d-144">Tirer parti des extraits de code et IntelliSense</span><span class="sxs-lookup"><span data-stu-id="cd76d-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="cd76d-145">Lorsque vous démarrez Visual Studio pour la première fois, vous devez sélectionner l’une des collections de paramètres prédéfinies.</span><span class="sxs-lookup"><span data-stu-id="cd76d-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="cd76d-146">Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, les extraits de code IntelliSense et les options de boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="cd76d-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="cd76d-147">Les procédures de ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lors de l’utilisation de la collection de **paramètres de développement généraux** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="cd76d-148">Si vous choisissez une autre collection de paramètres pour votre environnement de développement, il peut y avoir des différences entre les étapes que vous devez prendre en compte.</span><span class="sxs-lookup"><span data-stu-id="cd76d-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="cd76d-149">Exercice 1 : utilisation de la liaison de navigateur et de Web Essentials</span><span class="sxs-lookup"><span data-stu-id="cd76d-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="cd76d-150">**Web Essentials** est une extension Visual Studio qui ajoute une variété de fonctionnalités utiles pour le développement Web moderne, principalement axée sur l’expérience de développement Web beaucoup plus rapide et plus agréable.</span><span class="sxs-lookup"><span data-stu-id="cd76d-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="cd76d-151">Vous pouvez installer Web Essentials à partir de la Galerie d’extensions dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="cd76d-152">Le **lien du navigateur** est une nouvelle fonctionnalité incluse dans Visual Studio 2013 qui fournit un canal entre l’IDE de Visual Studio et tout navigateur ouvert pour échanger des données entre votre application Web et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="cd76d-153">Web Essentials étend le lien du navigateur avec les outils permettant de manipuler le modèle objet DOM et les styles CSS de vos pages Web directement à partir du navigateur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="cd76d-154">Dans cet exercice, vous allez explorer certaines des fonctionnalités prises en charge par **Web Essentials** et le **lien du navigateur** pour améliorer une page de quiz simple.</span><span class="sxs-lookup"><span data-stu-id="cd76d-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="cd76d-155">Tâche 1 : exécution du projet dans plusieurs navigateurs</span><span class="sxs-lookup"><span data-stu-id="cd76d-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="cd76d-156">Dans cette tâche, vous allez configurer votre application Web pour qu’elle s’exécute dans plusieurs navigateurs à la fois, ce qui est utile pour les tests entre navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="cd76d-157">Ouvrez **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="cd76d-158">Dans le menu **fichier** , sélectionnez **ouvrir | Projet/solution...** et accédez à **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** dans le dossier **source** du Lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="cd76d-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="cd76d-159">Sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="cd76d-160">Dans la barre d’outils de Visual Studio, développez le menu du navigateur et sélectionnez **Parcourir avec...** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="cd76d-161">![Browse with (option de menu)](visual-studio-2013-web-tools/_static/image1.png "Naviguer avec... dans le menu du navigateur")</span><span class="sxs-lookup"><span data-stu-id="cd76d-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="cd76d-162">*Browse with (option de menu)*</span><span class="sxs-lookup"><span data-stu-id="cd76d-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="cd76d-163">Dans la boîte de dialogue **naviguer avec** , sélectionnez **Google Chrome** et **Internet Explorer** en maintenant la touche **CTRL** enfoncée et cliquez sur **définir par défaut**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="cd76d-164">![Boîte de dialogue naviguer avec](visual-studio-2013-web-tools/_static/image2.png "Boîte de dialogue naviguer avec")</span><span class="sxs-lookup"><span data-stu-id="cd76d-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="cd76d-165">*Sélection de plusieurs navigateurs par défaut*</span><span class="sxs-lookup"><span data-stu-id="cd76d-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="cd76d-166">Google Chrome et Internet Explorer doivent maintenant apparaître comme navigateurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="cd76d-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="cd76d-167">Cliquez sur **Annuler** pour refermer la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="cd76d-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="cd76d-168">![Google Chrome et Internet Explorer en tant que navigateurs par défaut](visual-studio-2013-web-tools/_static/image3.png "Navigateurs par défaut Google Chrome et Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="cd76d-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="cd76d-169">*Google Chrome et Internet Explorer en tant que navigateurs par défaut*</span><span class="sxs-lookup"><span data-stu-id="cd76d-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-170">Après la configuration des navigateurs par défaut, l’option **plusieurs navigateurs** est sélectionnée dans le menu du navigateur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="cd76d-171">![Plusieurs navigateurs](visual-studio-2013-web-tools/_static/image4.png "Plusieurs navigateurs")</span><span class="sxs-lookup"><span data-stu-id="cd76d-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="cd76d-172">Appuyez sur **CTRL** + **F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="cd76d-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="cd76d-173">Lorsque les deux fenêtres du navigateur sont ouvertes, placez l’une d’elles au-dessus de l’autre pour voir les mises à jour sur les deux navigateurs simultanément.</span><span class="sxs-lookup"><span data-stu-id="cd76d-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="cd76d-174">Les navigateurs doivent afficher une page Web avec un rectangle bleu clair.</span><span class="sxs-lookup"><span data-stu-id="cd76d-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="cd76d-175">![Placer un navigateur au-dessus de l’autre](visual-studio-2013-web-tools/_static/image5.png "Placer un navigateur au-dessus de l’autre")</span><span class="sxs-lookup"><span data-stu-id="cd76d-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="cd76d-176">*Placer un navigateur au-dessus de l’autre*</span><span class="sxs-lookup"><span data-stu-id="cd76d-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="cd76d-177">Ne fermez pas les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-177">Do not close the browsers.</span></span> <span data-ttu-id="cd76d-178">Vous les utiliserez dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="cd76d-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="cd76d-179">Tâche 2 : utilisation du codage zen pour créer des éléments HTML</span><span class="sxs-lookup"><span data-stu-id="cd76d-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="cd76d-180">Le **codage Zen** est un plug-in d’éditeur pour le codage et la modification de code HTML, XML, XSL (ou tout autre format de code structuré) à grande vitesse.</span><span class="sxs-lookup"><span data-stu-id="cd76d-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="cd76d-181">Le cœur de ce plug-in est un moteur d’abréviation puissant qui vous permet de développer des expressions, similaires aux sélecteurs CSS, dans du code HTML.</span><span class="sxs-lookup"><span data-stu-id="cd76d-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="cd76d-182">Le codage Zen est un moyen rapide d’écrire du code HTML à l’aide d’une syntaxe de sélecteur de style CSS.</span><span class="sxs-lookup"><span data-stu-id="cd76d-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="cd76d-183">Dans cet exercice, vous allez utiliser la fonctionnalité de codage Zen fournie par Web Essentials pour générer les boutons HTML qui représentent les options de la question.</span><span class="sxs-lookup"><span data-stu-id="cd76d-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="cd76d-184">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="cd76d-185">Ouvrez le fichier **index. cshtml** situé dans les **vues** | dossier de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="cd76d-186">Remplacez le **&lt;!--TODO : ajoutez des options ici--&gt;** commentaire avec le code suivant, puis appuyez sur la touche **Tab**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="cd76d-187">Le code doit être développé en HTML.</span><span class="sxs-lookup"><span data-stu-id="cd76d-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="cd76d-188">![HTML développé](visual-studio-2013-web-tools/_static/image6.png "HTML développé")</span><span class="sxs-lookup"><span data-stu-id="cd76d-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="cd76d-189">*HTML développé*</span><span class="sxs-lookup"><span data-stu-id="cd76d-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-190">Pour en savoir plus sur la syntaxe de codage Zen, consultez l' [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)suivant.</span><span class="sxs-lookup"><span data-stu-id="cd76d-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="cd76d-191">Cliquez sur le bouton **Actualiser les navigateurs liés** pour mettre à jour les deux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="cd76d-192">![Actualiser les navigateurs liés](visual-studio-2013-web-tools/_static/image7.png "Actualiser les navigateurs liés")</span><span class="sxs-lookup"><span data-stu-id="cd76d-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="cd76d-193">*Actualiser les navigateurs liés*</span><span class="sxs-lookup"><span data-stu-id="cd76d-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="cd76d-194">![Internet Explorer-page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-page mise à jour avec quatre boutons")</span><span class="sxs-lookup"><span data-stu-id="cd76d-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="cd76d-195">*Internet Explorer-page mise à jour avec quatre boutons*</span><span class="sxs-lookup"><span data-stu-id="cd76d-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="cd76d-196">![Google Chrome-page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-page mise à jour avec quatre boutons")</span><span class="sxs-lookup"><span data-stu-id="cd76d-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="cd76d-197">*Google Chrome-page mise à jour avec quatre boutons*</span><span class="sxs-lookup"><span data-stu-id="cd76d-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="cd76d-198">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="cd76d-199">Vous avez ajouté les boutons à la page, mais vous devez toujours ajouter une question de modèle.</span><span class="sxs-lookup"><span data-stu-id="cd76d-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="cd76d-200">Pour ce faire, vous allez utiliser une nouvelle fonctionnalité dans Web Essentials, appelée **Lorem ipsum Generator**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="cd76d-201">Localisez l’élément **div** avec l’attribut de **classe** **front**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="cd76d-202">Ajoutez le code suivant en tant que premier élément enfant de la **balise div**, puis appuyez sur la touche **Tab**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="cd76d-203">Le code doit être développé en HTML.</span><span class="sxs-lookup"><span data-stu-id="cd76d-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="cd76d-204">![Lorem ipsum généré automatiquement](visual-studio-2013-web-tools/_static/image10.png "Lorem ipsum généré automatiquement")</span><span class="sxs-lookup"><span data-stu-id="cd76d-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="cd76d-205">*Lorem ipsum généré automatiquement*</span><span class="sxs-lookup"><span data-stu-id="cd76d-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-206">Dans le cadre du codage Zen, vous pouvez désormais générer du code Lorem ipsum directement dans l’éditeur HTML.</span><span class="sxs-lookup"><span data-stu-id="cd76d-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="cd76d-207">Tapez simplement **Lorem** et appuyez sur Tab pour insérer un texte de 30 mots **(** Lorem).</span><span class="sxs-lookup"><span data-stu-id="cd76d-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="cd76d-208">Par exemple,</span><span class="sxs-lookup"><span data-stu-id="cd76d-208">E.g.</span></span> <span data-ttu-id="cd76d-209">*lorem10* insère 10 mots de Lorem ipsum.</span><span class="sxs-lookup"><span data-stu-id="cd76d-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="cd76d-210">Vous allez ajouter un logo en haut de la question à l’aide d’une autre nouvelle fonctionnalité de Web Essentials appelée « **Générateur de pixels Lorem**».</span><span class="sxs-lookup"><span data-stu-id="cd76d-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="cd76d-211">Ajoutez le code suivant en tant que premier élément enfant de l’élément **div** avec le **conteneur** comme valeur de **classe** , puis appuyez sur la touche **Tab**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="cd76d-212">Le code doit se développer en HTML.</span><span class="sxs-lookup"><span data-stu-id="cd76d-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="cd76d-213">![Pixel de Lorem généré automatiquement](visual-studio-2013-web-tools/_static/image11.png "Pixel de Lorem généré automatiquement")</span><span class="sxs-lookup"><span data-stu-id="cd76d-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="cd76d-214">*Pixel de Lorem généré automatiquement*</span><span class="sxs-lookup"><span data-stu-id="cd76d-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-215">Dans le cadre du codage Zen, vous pouvez également générer du code de pixel Lorem directement dans l’éditeur HTML.</span><span class="sxs-lookup"><span data-stu-id="cd76d-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="cd76d-216">Pour ce faire, tapez **pix-200x200-animaux** et appuyez sur la **touche Tab** . une balise **img** avec une image 200x200 d’un animal est insérée.</span><span class="sxs-lookup"><span data-stu-id="cd76d-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="cd76d-217">Pour plus d’informations, reportez-vous au [Pixel Lorem](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="cd76d-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="cd76d-218">Cliquez sur le bouton **Actualiser les navigateurs liés** pour mettre à jour les deux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="cd76d-219">![Internet Explorer-image et texte générés automatiquement](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-image et texte générés automatiquement")</span><span class="sxs-lookup"><span data-stu-id="cd76d-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="cd76d-220">*Internet Explorer-image et texte générés automatiquement*</span><span class="sxs-lookup"><span data-stu-id="cd76d-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="cd76d-221">![Google Chrome-image et texte générés automatiquement](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-image et texte générés automatiquement")</span><span class="sxs-lookup"><span data-stu-id="cd76d-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="cd76d-222">*Google Chrome-image et texte générés automatiquement*</span><span class="sxs-lookup"><span data-stu-id="cd76d-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-223">Étant donné que l’image est sélectionnée de manière aléatoire lors de l’ajout de l’extrait de code, l’image affichée dans les navigateurs peut être différente.</span><span class="sxs-lookup"><span data-stu-id="cd76d-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="cd76d-224">Ne fermez pas les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-224">Do not close the browsers.</span></span> <span data-ttu-id="cd76d-225">Vous les utiliserez dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="cd76d-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="cd76d-226">Tâche 3 : mise à jour d’une propriété de style</span><span class="sxs-lookup"><span data-stu-id="cd76d-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="cd76d-227">Dans cette tâche, vous allez utiliser la fonctionnalité de **mode d’inspection** du lien de navigateur pour détecter l’emplacement exact où l’élément DOM spécifique est généré, puis mettre à jour la propriété Color de cet élément à l’aide d’un sélecteur de couleurs fourni par Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="cd76d-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="cd76d-228">Dans le navigateur Internet Explorer, appuyez sur **CTRL** + **ALT** + **I** pour activer le mode inspection.</span><span class="sxs-lookup"><span data-stu-id="cd76d-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="cd76d-229">Placez le pointeur sur la bordure bleue claire, puis cliquez sur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="cd76d-230">![Déplacement du pointeur sur la bordure bleue claire](visual-studio-2013-web-tools/_static/image14.png "Déplacement du pointeur sur la bordure bleue claire")</span><span class="sxs-lookup"><span data-stu-id="cd76d-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="cd76d-231">*Déplacement du pointeur sur la bordure bleue claire*</span><span class="sxs-lookup"><span data-stu-id="cd76d-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="cd76d-232">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="cd76d-233">Notez comment l’élément HTML que vous avez sélectionné dans le navigateur est également sélectionné dans l’éditeur HTML de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="cd76d-234">![Élément HTML sélectionné dans l’éditeur HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Élément HTML sélectionné dans l’éditeur HTML de Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="cd76d-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="cd76d-235">*Élément HTML sélectionné dans l’éditeur HTML de Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="cd76d-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="cd76d-236">Vous allez maintenant mettre à jour la classe CSS **front** pour modifier le style de l’élément sélectionné.</span><span class="sxs-lookup"><span data-stu-id="cd76d-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="cd76d-237">Pour ce faire, appuyez sur **CTRL** +  **,** pour ouvrir la zone **de** recherche.</span><span class="sxs-lookup"><span data-stu-id="cd76d-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="cd76d-238">Tapez **site. CSS** et appuyez sur **entrée** pour ouvrir le fichier.</span><span class="sxs-lookup"><span data-stu-id="cd76d-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="cd76d-239">![Ouverture du fichier site. CSS](visual-studio-2013-web-tools/_static/image16.png "Ouverture du fichier site. CSS")</span><span class="sxs-lookup"><span data-stu-id="cd76d-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="cd76d-240">*Ouverture du fichier site. CSS*</span><span class="sxs-lookup"><span data-stu-id="cd76d-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="cd76d-241">Appuyez sur **CTRL** + **F** et tapez **. Flip-Container. front** pour trouver le sélecteur CSS.</span><span class="sxs-lookup"><span data-stu-id="cd76d-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="cd76d-242">Cliquez sur le carré bleu clair dans la propriété border de la classe pour ouvrir le sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="cd76d-243">![Ouverture du sélecteur de couleurs](visual-studio-2013-web-tools/_static/image17.png "Ouverture du sélecteur de couleurs")</span><span class="sxs-lookup"><span data-stu-id="cd76d-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="cd76d-244">*Ouverture du sélecteur de couleurs*</span><span class="sxs-lookup"><span data-stu-id="cd76d-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="cd76d-245">Développez le sélecteur de couleurs en cliquant sur le bouton de Chevron et en sélectionnant une nouvelle couleur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="cd76d-246">![Agrandissement du sélecteur de couleurs](visual-studio-2013-web-tools/_static/image18.png "Agrandissement du sélecteur de couleurs")</span><span class="sxs-lookup"><span data-stu-id="cd76d-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="cd76d-247">*Agrandissement du sélecteur de couleurs*</span><span class="sxs-lookup"><span data-stu-id="cd76d-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="cd76d-248">Appuyez sur **CTRL** + **ALT** + **entrée** pour actualiser les navigateurs liés.</span><span class="sxs-lookup"><span data-stu-id="cd76d-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="cd76d-249">Basculez vers Internet Explorer et notez la modification de la couleur de la bordure.</span><span class="sxs-lookup"><span data-stu-id="cd76d-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="cd76d-250">![Internet Explorer-couleur de bordure mise à jour](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-couleur de bordure mise à jour")</span><span class="sxs-lookup"><span data-stu-id="cd76d-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="cd76d-251">*Internet Explorer-couleur de bordure mise à jour*</span><span class="sxs-lookup"><span data-stu-id="cd76d-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="cd76d-252">Basculez vers Google Chrome et notez la modification de la couleur de la bordure.</span><span class="sxs-lookup"><span data-stu-id="cd76d-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="cd76d-253">![Google Chrome-couleur de bordure mise à jour](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-couleur de bordure mise à jour")</span><span class="sxs-lookup"><span data-stu-id="cd76d-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="cd76d-254">*Google Chrome-couleur de bordure mise à jour*</span><span class="sxs-lookup"><span data-stu-id="cd76d-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="cd76d-255">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="cd76d-256">Accédez à la fin du fichier **site. CSS** et appuyez sur **CTRL** + **F** pour rechercher le sélecteur **. BTN** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="cd76d-257">Notez que la propriété **-WebKit-border-radius** est soulignée en vert.</span><span class="sxs-lookup"><span data-stu-id="cd76d-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="cd76d-258">![-WebKit-propriété border-radius du sélecteur BTN](visual-studio-2013-web-tools/_static/image21.png "-WebKit-propriété border-radius du sélecteur BTN")</span><span class="sxs-lookup"><span data-stu-id="cd76d-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="cd76d-259">*-WebKit-propriété border-radius du sélecteur BTN*</span><span class="sxs-lookup"><span data-stu-id="cd76d-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="cd76d-260">Placez le signe insertion dans la propriété **-WebKit-border-radius** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="cd76d-261">Une ligne bleue doit s’afficher sous la première lettre du premier mot de la propriété.</span><span class="sxs-lookup"><span data-stu-id="cd76d-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="cd76d-262">Il s’agit de la **balise active**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="cd76d-263">Appuyez sur **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="cd76d-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="cd76d-264">pour ouvrir le menu suggestions et cliquez sur **Ajouter la propriété standard manquante (frontière-RADIUS)** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="cd76d-265">![Ajouter une suggestion de propriété standard manquante](visual-studio-2013-web-tools/_static/image22.png "Ajouter une suggestion de propriété standard manquante")</span><span class="sxs-lookup"><span data-stu-id="cd76d-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="cd76d-266">*Ajouter une suggestion de propriété standard manquante*</span><span class="sxs-lookup"><span data-stu-id="cd76d-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="cd76d-267">La propriété **border-radius** est automatiquement ajoutée à la règle CSS.</span><span class="sxs-lookup"><span data-stu-id="cd76d-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="cd76d-268">![Propriété standard ajoutée manquante](visual-studio-2013-web-tools/_static/image23.png "Propriété standard ajoutée manquante")</span><span class="sxs-lookup"><span data-stu-id="cd76d-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="cd76d-269">*Propriété standard ajoutée manquante*</span><span class="sxs-lookup"><span data-stu-id="cd76d-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="cd76d-270">Déplacez le pointeur sur la propriété **border-radius** pour afficher l' **info-bulle**de la matrice du navigateur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="cd76d-271">L' **info-bulle** de la matrice du navigateur affiche la disponibilité de la propriété dans chaque navigateur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="cd76d-272">![Info-bulle de la matrice du navigateur](visual-studio-2013-web-tools/_static/image24.png "Info-bulle de la matrice du navigateur")</span><span class="sxs-lookup"><span data-stu-id="cd76d-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="cd76d-273">*Info-bulle de la matrice du navigateur*</span><span class="sxs-lookup"><span data-stu-id="cd76d-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="cd76d-274">Notez que la valeur de la propriété **border-radius** est toujours soulignée.</span><span class="sxs-lookup"><span data-stu-id="cd76d-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="cd76d-275">Déplacez le pointeur sur la valeur pour afficher le message d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="cd76d-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="cd76d-276">![Border-avertissement de valeur de propriété RADIUS](visual-studio-2013-web-tools/_static/image25.png "Border-avertissement de valeur de propriété RADIUS")</span><span class="sxs-lookup"><span data-stu-id="cd76d-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="cd76d-277">*Border-avertissement de valeur de propriété RADIUS*</span><span class="sxs-lookup"><span data-stu-id="cd76d-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="cd76d-278">Supprimez l’unité de la valeur de propriété **border-radius** comme suggéré par l’info-bulle.</span><span class="sxs-lookup"><span data-stu-id="cd76d-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="cd76d-279">Comme **border-radius** est la propriété standard permettant de définir le mode d’affichage des angles de bordure arrondis, vous pouvez supprimer la propriété et la valeur **-WebKit-border-radius** de la règle CSS.</span><span class="sxs-lookup"><span data-stu-id="cd76d-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="cd76d-280">Placez le signe insertion dans la propriété **retour automatique** à la ligne et notez que la balise active s’affiche également ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="cd76d-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="cd76d-281">Ouvrez le menu et cliquez sur **Ajouter les détails du fournisseur manquant**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="cd76d-282">![Ajouter une suggestion de spécificités de fournisseur manquantes](visual-studio-2013-web-tools/_static/image26.png "Ajouter une suggestion de spécificités de fournisseur manquantes")</span><span class="sxs-lookup"><span data-stu-id="cd76d-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="cd76d-283">*Ajouter une suggestion de spécificités de fournisseur manquantes*</span><span class="sxs-lookup"><span data-stu-id="cd76d-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="cd76d-284">La propriété **-MS-Word-Wrap** est automatiquement ajoutée à la règle CSS.</span><span class="sxs-lookup"><span data-stu-id="cd76d-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="cd76d-285">![Propriété spécifique au fournisseur ajoutée](visual-studio-2013-web-tools/_static/image27.png "Propriété spécifique au fournisseur ajoutée")</span><span class="sxs-lookup"><span data-stu-id="cd76d-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="cd76d-286">*Propriété spécifique au fournisseur ajoutée*</span><span class="sxs-lookup"><span data-stu-id="cd76d-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="cd76d-287">Tâche 4 : mise à jour du code HTML à partir du navigateur</span><span class="sxs-lookup"><span data-stu-id="cd76d-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="cd76d-288">Dans cette tâche, vous allez utiliser la fonctionnalité de **mode création** du lien du navigateur pour modifier l’objet DOM à partir du navigateur et transférer les modifications vers le fichier source HTML dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="cd76d-289">Dans Google Chrome, appuyez sur **CTRL** + **ALT** + **D** pour activer le mode création.</span><span class="sxs-lookup"><span data-stu-id="cd76d-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="cd76d-290">Déplacez le pointeur sur l’étiquette **Lorem ipsum dolor sit amet** , puis cliquez sur.</span><span class="sxs-lookup"><span data-stu-id="cd76d-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="cd76d-291">![Modification de la question](visual-studio-2013-web-tools/_static/image28.png "Modification de la question")</span><span class="sxs-lookup"><span data-stu-id="cd76d-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="cd76d-292">*Modification de la question*</span><span class="sxs-lookup"><span data-stu-id="cd76d-292">*Editing the question*</span></span>
3. <span data-ttu-id="cd76d-293">Un curseur doit s’afficher.</span><span class="sxs-lookup"><span data-stu-id="cd76d-293">A cursor should appear.</span></span> <span data-ttu-id="cd76d-294">Remplacez le texte d’origine par ce qui ressemble à l' *écriture d’une question plus longue*, puis appuyez sur **Échap** pour quitter le mode création.</span><span class="sxs-lookup"><span data-stu-id="cd76d-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="cd76d-295">![Question modifiée](visual-studio-2013-web-tools/_static/image29.png "Question modifiée")</span><span class="sxs-lookup"><span data-stu-id="cd76d-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="cd76d-296">*Question modifiée*</span><span class="sxs-lookup"><span data-stu-id="cd76d-296">*Question edited*</span></span>
4. <span data-ttu-id="cd76d-297">Revenez à Visual Studio et ouvrez **index. cshtml**, s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="cd76d-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="cd76d-298">Notez que le texte interne de l’élément **&lt;p&gt;** a été mis à jour.</span><span class="sxs-lookup"><span data-stu-id="cd76d-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="cd76d-299">![Mise à jour de la question dans la page HTML](visual-studio-2013-web-tools/_static/image30.png "Mise à jour de la question dans la page HTML")</span><span class="sxs-lookup"><span data-stu-id="cd76d-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="cd76d-300">*Mise à jour de la question dans la page HTML*</span><span class="sxs-lookup"><span data-stu-id="cd76d-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="cd76d-301">Tâche 5 : examen des avertissements liés au SEO</span><span class="sxs-lookup"><span data-stu-id="cd76d-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="cd76d-302">L' **optimisation du moteur de recherche** (SEO) est le processus qui consiste à rendre un site Web plus haut dans la liste des résultats d’un moteur de recherche.</span><span class="sxs-lookup"><span data-stu-id="cd76d-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="cd76d-303">Plus le nombre de sites est élevé et plus la liste est cohérente, plus le nombre de visiteurs du moteur de recherche sera important.</span><span class="sxs-lookup"><span data-stu-id="cd76d-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="cd76d-304">Web Essentials intègre un outil analytique qui examine le code HTML, signale les problèmes détectés et fournit de l’aide pour les résoudre.</span><span class="sxs-lookup"><span data-stu-id="cd76d-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="cd76d-305">Accédez au menu **affichage** , puis cliquez sur **liste d’erreurs** pour ouvrir la fenêtre **liste d’erreurs** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="cd76d-306">![Liste d’erreurs dans le menu Affichage](visual-studio-2013-web-tools/_static/image31.png "Liste d’erreurs dans le menu Affichage")</span><span class="sxs-lookup"><span data-stu-id="cd76d-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="cd76d-307">*Liste d’erreurs dans le menu Affichage*</span><span class="sxs-lookup"><span data-stu-id="cd76d-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="cd76d-308">Notez qu’il existe un avertissement SEO indiquant qu’il manque une balise de **méta&gt;&lt;** pour la description de la page.</span><span class="sxs-lookup"><span data-stu-id="cd76d-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="cd76d-309">Double-cliquez sur l’entrée d’avertissement SEO pour la corriger.</span><span class="sxs-lookup"><span data-stu-id="cd76d-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="cd76d-310">![Fenêtre Liste d’erreurs](visual-studio-2013-web-tools/_static/image32.png "Liste d'erreurs (fenêtre)")</span><span class="sxs-lookup"><span data-stu-id="cd76d-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="cd76d-311">*Fenêtre Liste d’erreurs*</span><span class="sxs-lookup"><span data-stu-id="cd76d-311">*Error List window*</span></span>
3. <span data-ttu-id="cd76d-312">Dans la boîte de dialogue **Web Essentials** , cliquez sur **Oui** pour insérer une description &lt;balise Meta&gt;.</span><span class="sxs-lookup"><span data-stu-id="cd76d-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="cd76d-313">![Boîte de dialogue Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Boîte de dialogue Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="cd76d-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="cd76d-314">*Boîte de dialogue Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="cd76d-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="cd76d-315">L’éditeur pour **\_Layout. cshtml** s’ouvre et la balise de **méta-&gt;&lt;** est automatiquement ajoutée à la section **Head** du fichier html.</span><span class="sxs-lookup"><span data-stu-id="cd76d-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="cd76d-316">![Balise méta ajoutée automatiquement dans _Layout page](visual-studio-2013-web-tools/_static/image34.png "Balise méta ajoutée automatiquement dans _Layout page")</span><span class="sxs-lookup"><span data-stu-id="cd76d-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="cd76d-317">*Balise méta ajoutée automatiquement à \_page de disposition*</span><span class="sxs-lookup"><span data-stu-id="cd76d-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="cd76d-318">Remplacez la valeur de l’attribut **content** par *GeekQuiz* et enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="cd76d-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="cd76d-319">Exercice 2 : tirer parti des extraits de code et d’IntelliSense</span><span class="sxs-lookup"><span data-stu-id="cd76d-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="cd76d-320">Avec Web Essentials, l’éditeur HTML a été étendu avec des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="cd76d-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="cd76d-321">Dans cet exercice, vous verrez quelques nouvelles fonctionnalités qui sont utiles lors du développement d’applications Web.</span><span class="sxs-lookup"><span data-stu-id="cd76d-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="cd76d-322">Tâche 1 : utilisation d’IntelliSense dans les documents HTML</span><span class="sxs-lookup"><span data-stu-id="cd76d-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="cd76d-323">La première nouvelle fonctionnalité que vous verrez dans cette tâche est appelée **Dynamic IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="cd76d-324">Dynamic IntelliSense lit d’autres balises et attributs pour déduire les ID possibles que vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="cd76d-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="cd76d-325">Dans cette tâche, vous allez créer un nouvel élément de formulaire HTML qui contient une étiquette et un champ d’entrée.</span><span class="sxs-lookup"><span data-stu-id="cd76d-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="cd76d-326">Ensuite, vous allez ajouter un attribut **for** à l’étiquette pour le lier à l’entrée, et vous verrez des suggestions IntelliSense basées sur les ID des entrées dans la portée.</span><span class="sxs-lookup"><span data-stu-id="cd76d-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="cd76d-327">Ouvrez **Visual Studio Express 2013 pour le Web** et la solution **Begin. sln** située dans le dossier **source/EX2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="cd76d-328">Vous pouvez également continuer avec la solution que vous avez obtenue au cours de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="cd76d-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="cd76d-329">Dans **Explorateur de solutions**, ouvrez le fichier **index. cshtml** situé dans les **vues** | dossier de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="cd76d-330">Ajoutez le formulaire suivant à la **section&lt;&gt;** élément.</span><span class="sxs-lookup"><span data-stu-id="cd76d-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="cd76d-331">(Extrait de code- *VisualStudio2013WebTooling* - *EX2* - *formulaire*)</span><span class="sxs-lookup"><span data-stu-id="cd76d-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="cd76d-332">La balise d’entrée doit être précédée d’une étiquette avec une description du champ.</span><span class="sxs-lookup"><span data-stu-id="cd76d-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="cd76d-333">Ajoutez l’étiquette suivante avant la balise d’entrée.</span><span class="sxs-lookup"><span data-stu-id="cd76d-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="cd76d-334">L’attribut **for** d’un **&lt;étiquette&gt;** spécifie l’élément de formulaire auquel une étiquette est liée.</span><span class="sxs-lookup"><span data-stu-id="cd76d-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="cd76d-335">La valeur de l’attribut doit être égale à l’ID de l’élément associé.</span><span class="sxs-lookup"><span data-stu-id="cd76d-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="cd76d-336">Ajoutez l’attribut **for** à l' **&lt;étiquette&gt;** élément.</span><span class="sxs-lookup"><span data-stu-id="cd76d-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="cd76d-337">Comme indiqué dans l’illustration suivante, la valeur&quot; nom de l' &quot;s’affiche dans la zone IntelliSense, en fonction de l’ID des éléments dans la même portée (le **&lt;de formulaire** englobant&gt;).</span><span class="sxs-lookup"><span data-stu-id="cd76d-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="cd76d-338">![Indication de l’ID dans IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Indication de l’ID dans IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="cd76d-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="cd76d-339">*Indication de l’ID dans IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="cd76d-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="cd76d-340">Supprimez le **formulaire&lt;** ajouté récemment&gt;et son contenu.</span><span class="sxs-lookup"><span data-stu-id="cd76d-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="cd76d-341">Tâche 2 : utilisation d’extraits de code HTML</span><span class="sxs-lookup"><span data-stu-id="cd76d-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="cd76d-342">HTML5 a introduit plus de 25 nouvelles balises sémantiques.</span><span class="sxs-lookup"><span data-stu-id="cd76d-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="cd76d-343">Visual Studio offrait déjà la prise en charge d’IntelliSense pour ces balises, mais Visual Studio 2013 le rend plus rapide et plus facile à écrire en ajoutant de nouveaux extraits de code.</span><span class="sxs-lookup"><span data-stu-id="cd76d-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="cd76d-344">Bien que ces balises ne soient pas compliquées, elles sont accompagnées de quelques subtilités, telles que l’ajout du codec de secours approprié pour la balise *audio* .</span><span class="sxs-lookup"><span data-stu-id="cd76d-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="cd76d-345">Dans cette tâche, vous verrez les extraits de code HTML pour la balise audio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="cd76d-346">Dans le fichier **index. cshtml** , tapez **&lt;AUD** à l’intérieur de la **section&lt;&gt;** élément, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="cd76d-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="cd76d-347">![Insertion d’un élément audio](visual-studio-2013-web-tools/_static/image36.png "Insertion d’un élément audio")</span><span class="sxs-lookup"><span data-stu-id="cd76d-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="cd76d-348">*Insertion d’un élément audio*</span><span class="sxs-lookup"><span data-stu-id="cd76d-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="cd76d-349">Appuyez deux fois sur la **touche Tab** et notez la façon dont le code suivant est ajouté à la page et le curseur est placé sur l’attribut **src** de la première source.</span><span class="sxs-lookup"><span data-stu-id="cd76d-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="cd76d-350">En appuyant deux fois sur la touche **Tab** , l’extrait de code est inséré.</span><span class="sxs-lookup"><span data-stu-id="cd76d-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="cd76d-351">L’extrait de code audio montre l’utilisation standard de la balise *audio* , avec deux fichiers sources pour une meilleure prise en charge.</span><span class="sxs-lookup"><span data-stu-id="cd76d-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="cd76d-352">Supprimez la deuxième ligne et mettez à jour la source de la première ligne à l’aide du lien suivant vers le WebCampsTV Katana Show : [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="cd76d-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="cd76d-353">Le code obtenu est illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="cd76d-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="cd76d-354">Le fichier source *KatanaProject. mp3* est utilisé à titre d’exemple.</span><span class="sxs-lookup"><span data-stu-id="cd76d-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="cd76d-355">Vous pouvez utiliser une autre source si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="cd76d-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="cd76d-356">Appuyez sur **CTRL** + **S** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="cd76d-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="cd76d-357">Appuyez sur **CTRL** + **F5** pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="cd76d-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="cd76d-358">Notez qu’un lecteur audio a été ajouté à l’application.</span><span class="sxs-lookup"><span data-stu-id="cd76d-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="cd76d-359">![Lecteur audio dans Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Lecteur audio dans Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="cd76d-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="cd76d-360">*Lecteur audio dans Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="cd76d-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="cd76d-361">![Lecteur audio dans Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Lecteur audio dans Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="cd76d-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="cd76d-362">*Lecteur audio dans Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="cd76d-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="cd76d-363">Ne fermez pas les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="cd76d-363">Do not close the browsers.</span></span> <span data-ttu-id="cd76d-364">Vous les utiliserez dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="cd76d-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="cd76d-365">Tâche 3 : utilisation d’IntelliSense dans les documents JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd76d-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="cd76d-366">Avec Web Essentials 2013, les feuilles de style et les pages HTML produisent une liste des ID et des noms de classe.</span><span class="sxs-lookup"><span data-stu-id="cd76d-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="cd76d-367">Dans cette tâche, vous allez apprendre comment ces listes améliorent la prise en charge de JavaScript IntelliSense dans Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="cd76d-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="cd76d-368">Dans le fichier **index. cshtml** , ajoutez le code suivant pour définir une balise de **script** pour le code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cd76d-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="cd76d-369">Ajoutez le code suivant à l’intérieur de la balise de **script** pour définir la fonction de rappel Ready.</span><span class="sxs-lookup"><span data-stu-id="cd76d-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="cd76d-370">(Extrait de code- *VisualStudio2013WebTooling* - *EX2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="cd76d-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="cd76d-371">Placez le signe insertion dans la balise **script** et appuyez sur **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="cd76d-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="cd76d-372">pour ouvrir le menu de suggestion.</span><span class="sxs-lookup"><span data-stu-id="cd76d-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="cd76d-373">Cliquez sur **Extraire dans un fichier**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="cd76d-374">![Suggestions JavaScript d’extraction vers un fichier](visual-studio-2013-web-tools/_static/image39.png "Suggestions JavaScript d’extraction vers un fichier")</span><span class="sxs-lookup"><span data-stu-id="cd76d-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="cd76d-375">*Suggestions JavaScript d’extraction vers un fichier*</span><span class="sxs-lookup"><span data-stu-id="cd76d-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="cd76d-376">Dans la fenêtre **Enregistrer sous** , sélectionnez le dossier **scripts** , nommez le fichier **init. js** , puis cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="cd76d-377">![Enregistrer en tant que fenêtre](visual-studio-2013-web-tools/_static/image40.png "Enregistrer en tant que fenêtre")</span><span class="sxs-lookup"><span data-stu-id="cd76d-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="cd76d-378">*Enregistrer en tant que fenêtre*</span><span class="sxs-lookup"><span data-stu-id="cd76d-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-379">Le fichier **init. js** est créé et le contenu du script est déplacé vers le fichier.</span><span class="sxs-lookup"><span data-stu-id="cd76d-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="cd76d-380">![Fichier init. js créé avec le contenu inclus](visual-studio-2013-web-tools/_static/image41.png "Fichier init. js créé avec le contenu inclus")</span><span class="sxs-lookup"><span data-stu-id="cd76d-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="cd76d-381">*Fichier init. js créé avec le contenu inclus*</span><span class="sxs-lookup"><span data-stu-id="cd76d-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="cd76d-382">Ouvrez le fichier **index. cshtml** et vérifiez que la balise de script a été remplacée par une référence au fichier **init. js** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="cd76d-383">![Référence HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Référence HTML init. js")</span><span class="sxs-lookup"><span data-stu-id="cd76d-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="cd76d-384">*Référence HTML init. js*</span><span class="sxs-lookup"><span data-stu-id="cd76d-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="cd76d-385">Accédez à la **Explorateur de solutions** et notez que le fichier **init. js** a été inclus automatiquement dans la solution.</span><span class="sxs-lookup"><span data-stu-id="cd76d-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="cd76d-386">![Fichier init. js inclus dans la solution](visual-studio-2013-web-tools/_static/image43.png "Fichier init. js inclus dans la solution")</span><span class="sxs-lookup"><span data-stu-id="cd76d-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="cd76d-387">*Fichier init. js inclus dans la solution*</span><span class="sxs-lookup"><span data-stu-id="cd76d-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="cd76d-388">Revenez au fichier **init. js** pour mettre à jour le rappel de fonction **Ready** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="cd76d-389">À l’intérieur de la définition de rappel de fonction qui est passée à *Ready*, ajoutez le code suivant pour obtenir tous les éléments par un attribut de classe spécifique.</span><span class="sxs-lookup"><span data-stu-id="cd76d-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="cd76d-390">Appuyez sur **CTRL** + **espace** entre les guillemets à l’intérieur de l’appel de fonction **getElementsByClassName** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="cd76d-391">![Présentation d’IntelliSense pour la fonction getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Présentation d’IntelliSense pour la fonction getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="cd76d-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="cd76d-392">*Présentation d’IntelliSense pour la fonction getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="cd76d-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-393">Notez qu’IntelliSense affiche les classes définies dans les feuilles de style de projet.</span><span class="sxs-lookup"><span data-stu-id="cd76d-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="cd76d-394">Remplacez la ligne que vous avez créée par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cd76d-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="cd76d-395">Placez le **curseur après l'** intérieur des guillemets de la fonction **GetElementsByTagName** et appuyez sur **CTRL** + **espace**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="cd76d-396">![Présentation d’IntelliSense pour la méthode getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Présentation d’IntelliSense pour la méthode getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="cd76d-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="cd76d-397">*Présentation d’IntelliSense pour la méthode getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="cd76d-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="cd76d-398">Sélectionnez **&quot;&quot;audio** dans la liste et appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="cd76d-399">Le résultat est affiché dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="cd76d-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="cd76d-400">![Récupération d’éléments audio](visual-studio-2013-web-tools/_static/image46.png "Récupération d’éléments audio")</span><span class="sxs-lookup"><span data-stu-id="cd76d-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="cd76d-401">*Récupération d’éléments audio*</span><span class="sxs-lookup"><span data-stu-id="cd76d-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="cd76d-402">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier **init. js** dans le dossier **scripts** et sélectionnez **réduire le ou les fichiers JavaScript** dans le menu **Web Essentials** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="cd76d-403">![Réduire le ou les fichiers JavaScript](visual-studio-2013-web-tools/_static/image47.png "Réduire les fichiers JavaScript")</span><span class="sxs-lookup"><span data-stu-id="cd76d-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="cd76d-404">*Réduire le ou les fichiers JavaScript*</span><span class="sxs-lookup"><span data-stu-id="cd76d-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="cd76d-405">Lorsque vous êtes invité à activer la minimisation automatique lorsque le fichier source change, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="cd76d-406">![Activation de l’avertissement de minimisation automatique](visual-studio-2013-web-tools/_static/image48.png "Activation de l’avertissement de minimisation automatique")</span><span class="sxs-lookup"><span data-stu-id="cd76d-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="cd76d-407">*Activation de l’avertissement de minimisation automatique*</span><span class="sxs-lookup"><span data-stu-id="cd76d-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd76d-408">**Init. min. js** est créé et ajouté en tant que dépendance du fichier **init. js** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="cd76d-409">![Fichier init. min. js créé](visual-studio-2013-web-tools/_static/image49.png "Fichier init. min. js créé")</span><span class="sxs-lookup"><span data-stu-id="cd76d-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="cd76d-410">*Fichier init. min. js créé*</span><span class="sxs-lookup"><span data-stu-id="cd76d-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="cd76d-411">Ouvrez le fichier **init. min. js** et notez que le fichier est minimisés.</span><span class="sxs-lookup"><span data-stu-id="cd76d-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="cd76d-412">![Contenu du fichier init. min. js](visual-studio-2013-web-tools/_static/image50.png "Contenu du fichier init. min. js")</span><span class="sxs-lookup"><span data-stu-id="cd76d-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="cd76d-413">*Contenu du fichier init. min. js*</span><span class="sxs-lookup"><span data-stu-id="cd76d-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="cd76d-414">Dans le fichier **init. js** , ajoutez le code suivant sous l’appel de fonction **GetElementsByTagName** pour lire tous les éléments audio.</span><span class="sxs-lookup"><span data-stu-id="cd76d-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="cd76d-415">(Extrait de code- *VisualStudio2013WebTooling* - *EX2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="cd76d-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="cd76d-416">Cliquez sur **CTRL** + **S** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="cd76d-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="cd76d-417">Étant donné que le fichier minimisés est déjà ouvert, une boîte de dialogue s’affiche indiquant que le fichier a été modifié en dehors de l’éditeur de code source.</span><span class="sxs-lookup"><span data-stu-id="cd76d-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="cd76d-418">Cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="cd76d-418">Click **Yes**.</span></span>

    <span data-ttu-id="cd76d-419">![AVERTISSEMENT Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "AVERTISSEMENT Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="cd76d-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="cd76d-420">*AVERTISSEMENT Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="cd76d-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="cd76d-421">Revenez au fichier **init. min. js** pour vérifier que le fichier a été mis à jour avec le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="cd76d-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="cd76d-422">![Fichier init. min. js mis à jour](visual-studio-2013-web-tools/_static/image52.png "Fichier init. min. js mis à jour")</span><span class="sxs-lookup"><span data-stu-id="cd76d-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="cd76d-423">*Fichier init. min. js mis à jour*</span><span class="sxs-lookup"><span data-stu-id="cd76d-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="cd76d-424">Cliquez sur le bouton **Actualiser le lien du navigateur** .</span><span class="sxs-lookup"><span data-stu-id="cd76d-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="cd76d-425">Une fois les deux navigateurs actualisés, les lecteurs audio que vous avez vus au cours de la tâche précédente commencent à être lus automatiquement.</span><span class="sxs-lookup"><span data-stu-id="cd76d-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="cd76d-426">![Lecteur audio inclus dans l’affichage](visual-studio-2013-web-tools/_static/image53.png "Lecteur audio inclus dans l’affichage")</span><span class="sxs-lookup"><span data-stu-id="cd76d-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="cd76d-427">*Lecteur audio inclus dans l’affichage*</span><span class="sxs-lookup"><span data-stu-id="cd76d-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="cd76d-428">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="cd76d-428">Summary</span></span>

<span data-ttu-id="cd76d-429">En effectuant ce laboratoire pratique, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="cd76d-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="cd76d-430">Utilisez les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, telles que les extraits de code HTML5 enrichis et le codage Zen</span><span class="sxs-lookup"><span data-stu-id="cd76d-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="cd76d-431">Utilisez les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials, telles que le sélecteur de couleurs et l’info-bulle de la matrice du navigateur</span><span class="sxs-lookup"><span data-stu-id="cd76d-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="cd76d-432">Utilisez les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Web Essentials, telles que Extract to file et IntelliSense pour tous les éléments HTML</span><span class="sxs-lookup"><span data-stu-id="cd76d-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="cd76d-433">Échanger des données entre votre navigateur et Visual Studio à l’aide d’un lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="cd76d-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
