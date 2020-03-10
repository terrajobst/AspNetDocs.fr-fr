---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Pages Web ASP.NET de programmation (Razor) à l’aide de Visual Studio | Microsoft Docs
author: Rick-Anderson
description: Cette annexe explique comment vous pouvez utiliser Visual Studio 2010 ou Visual Web Developer 2010 Express pour programmer pages Web ASP.NET avec le syntaxe Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633510"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="c6f22-103">Pages Web ASP.NET de programmation (Razor) à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6f22-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="c6f22-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c6f22-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c6f22-105">Cet article explique comment vous pouvez utiliser Visual Studio ou Visual Web Developer Express pour programmer des sites pages Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="c6f22-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="c6f22-106">Ce que vous allez apprendre</span><span class="sxs-lookup"><span data-stu-id="c6f22-106">What you'll learn</span></span>
>
> - <span data-ttu-id="c6f22-107">Ce que vous devez installer (le cas échéant) pour travailler avec pages Web ASP.NET dans votre version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="c6f22-108">Comment ajouter la prise en charge de pages Web ASP.NET à Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="c6f22-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="c6f22-109">Comment utiliser les fonctionnalités de Visual Studio pour travailler avec des pages Razor ASP.NET, notamment IntelliSense et le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c6f22-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c6f22-110">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="c6f22-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="c6f22-111">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="c6f22-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="c6f22-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c6f22-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="c6f22-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="c6f22-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="c6f22-114">Ce didacticiel fonctionne également avec pages Web ASP.NET 2, Visual Studio 2012, Visual Studio 2010 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="c6f22-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="c6f22-115">Vous pouvez programmer des pages Web ASP.NET avec syntaxe Razor à l’aide de WebMatrix ou de nombreux autres éditeurs de code.</span><span class="sxs-lookup"><span data-stu-id="c6f22-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="c6f22-116">Vous pouvez également utiliser Microsoft Visual Studio qui est un environnement de développement intégré (IDE) complet qui fournit un ensemble d’outils puissants pour créer de nombreux types d’applications (pas seulement des sites Web).</span><span class="sxs-lookup"><span data-stu-id="c6f22-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="c6f22-117">Pour travailler avec les pages Razor ASP.NET, vous pouvez utiliser [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="c6f22-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="c6f22-118">Voici deux fonctionnalités particulièrement utiles que Visual Studio fournit pour la programmation avec les pages Web Razor ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="c6f22-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="c6f22-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="c6f22-119">*IntelliSense*.</span></span> <span data-ttu-id="c6f22-120">La fonctionnalité IntelliSense intégrée à Visual Studio est plus complète qu’IntelliSense dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c6f22-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="c6f22-121">*Débogueur*.</span><span class="sxs-lookup"><span data-stu-id="c6f22-121">*Debugger*.</span></span> <span data-ttu-id="c6f22-122">Le débogueur vous permet de résoudre les problèmes liés à votre code en arrêtant un programme pendant son exécution, en examinant des variables et en parcourant le code ligne par ligne.</span><span class="sxs-lookup"><span data-stu-id="c6f22-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="c6f22-123">Utilisation de Visual Studio avec différentes versions de pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c6f22-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="c6f22-124">Pour développer des applications Web ASP.NET dans Visual Studio 2017, installez la charge de travail **développement ASP.net et Web** .</span><span class="sxs-lookup"><span data-stu-id="c6f22-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="c6f22-125">Visual Studio 2012 et Visual Studio 2013 incluent la prise en charge de pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6f22-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="c6f22-126">(Les packages requis pour prendre en charge les pages Web ASP.NET sont installés lorsque vous installez Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="c6f22-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="c6f22-127">Visual Studio 2010 n’inclut pas la prise en charge par défaut pour pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6f22-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="c6f22-128">Pour utiliser pages Web ASP.NET avec Visual Studio 2010, vous devez installer le package MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6f22-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="c6f22-129">Pour accéder à pages Web ASP.NET 2, vous installez ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c6f22-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="c6f22-130">Le tableau suivant résume la prise en charge de pages Web ASP.NET dans différentes versions de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="c6f22-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="c6f22-131">Visual Studio 2010</span></span> | <span data-ttu-id="c6f22-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c6f22-132">Visual Studio 2012</span></span> | <span data-ttu-id="c6f22-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c6f22-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c6f22-134">**Pages Web ASP.NET 2**</span><span class="sxs-lookup"><span data-stu-id="c6f22-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="c6f22-135">Installer ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c6f22-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="c6f22-136">Fournies</span><span class="sxs-lookup"><span data-stu-id="c6f22-136">(Included)</span></span> | <span data-ttu-id="c6f22-137">Fournies</span><span class="sxs-lookup"><span data-stu-id="c6f22-137">(Included)</span></span> |
| <span data-ttu-id="c6f22-138">**Pages Web ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="c6f22-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="c6f22-139">Mise à jour de pages Web ASP.NET 3 à NuGet</span><span class="sxs-lookup"><span data-stu-id="c6f22-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="c6f22-140">Fournies</span><span class="sxs-lookup"><span data-stu-id="c6f22-140">(Included)</span></span> |

<span data-ttu-id="c6f22-141">Pour utiliser Visual Studio 2010, consultez [installation de la prise en charge de pages Web ASP.net dans Visual studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="c6f22-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="c6f22-142">Lancement de Visual Studio à partir de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="c6f22-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="c6f22-143">Si vous avez démarré un projet dans WebMatrix et que vous souhaitez basculer vers Visual Studio, WebMatrix fournit un bouton permettant d’ouvrir facilement le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="c6f22-144">Pour que ce bouton soit activé, Visual Studio doit être installé sur votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="c6f22-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="c6f22-145">L’image suivante montre le bouton dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c6f22-145">The following image shows the button in WebMatrix.</span></span>

![lancer Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="c6f22-147">Lorsque vous cliquez sur le bouton, le projet est ouvert dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="c6f22-148">Vous pouvez basculer entre WebMatrix et Visual Studio sans aucun problème.</span><span class="sxs-lookup"><span data-stu-id="c6f22-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="c6f22-149">Vous serez averti si des fichiers ont été modifiés dans l’autre environnement et doivent être rechargés pour obtenir les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="c6f22-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="c6f22-150">Création d’un site ASP.NET Razor dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6f22-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="c6f22-151">Pour créer un site Web Razor ASP.NET dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="c6f22-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="c6f22-152">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="c6f22-153">Dans le menu **fichier** , cliquez sur **nouveau site Web**.</span><span class="sxs-lookup"><span data-stu-id="c6f22-153">In the **File** menu, click **New Web Site**.</span></span>

    ![créer un nouveau site Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="c6f22-155">Dans la boîte de dialogue **nouveau site Web** , sélectionnez la langue à utiliser ( C# Visual ou Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="c6f22-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="c6f22-156">Sélectionnez le modèle de **site Web ASP.net (Razor)** .</span><span class="sxs-lookup"><span data-stu-id="c6f22-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![site Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="c6f22-158">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6f22-158">Click **OK**.</span></span>

<span data-ttu-id="c6f22-159">Votre nouveau projet existe et contient des pages Web par défaut pour vous aider à commencer.</span><span class="sxs-lookup"><span data-stu-id="c6f22-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="c6f22-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="c6f22-160">Using IntelliSense</span></span>

<span data-ttu-id="c6f22-161">Maintenant que vous avez créé un site, vous pouvez voir comment IntelliSense fonctionne dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="c6f22-162">Dans le site Web que vous venez de créer, ouvrez la page *default. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="c6f22-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="c6f22-163">Après les balises `<h3>` dans la page, tapez `@ServerInfo.` (y compris le point).</span><span class="sxs-lookup"><span data-stu-id="c6f22-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="c6f22-164">Notez la manière dont IntelliSense affiche les méthodes disponibles pour le programme d’assistance `ServerInfo` dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="c6f22-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="c6f22-166">Sélectionnez la méthode `GetHtml` dans la liste, puis appuyez sur entrée.</span><span class="sxs-lookup"><span data-stu-id="c6f22-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="c6f22-167">IntelliSense remplit automatiquement la méthode.</span><span class="sxs-lookup"><span data-stu-id="c6f22-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="c6f22-168">(Comme avec n’importe quelle C#méthode dans, vous devez ajouter des caractères `()` après la méthode.) Le code complet de la méthode `GetHtml` se présente comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="c6f22-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="c6f22-169">Appuyez sur CTRL + F5 pour exécuter la page.</span><span class="sxs-lookup"><span data-stu-id="c6f22-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="c6f22-170">Voici à quoi ressemble la page lorsqu’elle s’affiche dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="c6f22-170">This is what the page looks like when displayed in a browser:</span></span>

    ![page par défaut dans le navigateur](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="c6f22-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="c6f22-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="c6f22-173">Utilisation du débogueur</span><span class="sxs-lookup"><span data-stu-id="c6f22-173">Using the Debugger</span></span>

1. <span data-ttu-id="c6f22-174">En haut de la page *default. cshtml* , après la ligne qui commence par `Page.Title`, ajoutez la ligne de code suivante :</span><span class="sxs-lookup"><span data-stu-id="c6f22-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="c6f22-175">Dans la marge grise de l’éditeur à gauche du code, cliquez sur en regard de cette nouvelle ligne afin d’ajouter un *point d’arrêt*.</span><span class="sxs-lookup"><span data-stu-id="c6f22-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="c6f22-176">Un point d’arrêt est un marqueur qui indique au débogueur d’arrêter l’exécution du programme à ce stade, ce qui vous permet de voir ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="c6f22-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![définir un point d’arrêt](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="c6f22-178">Supprimez l’appel à la méthode `ServerInfo.GetHtml` et ajoutez un appel à la variable `@myTime` à la place.</span><span class="sxs-lookup"><span data-stu-id="c6f22-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="c6f22-179">Cet appel affiche la valeur d’heure actuelle qui est retournée par la nouvelle ligne de code.</span><span class="sxs-lookup"><span data-stu-id="c6f22-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="c6f22-180">Appuyez sur F5 pour exécuter la page dans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c6f22-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="c6f22-181">La page s’arrête sur le point d’arrêt que vous définissez.</span><span class="sxs-lookup"><span data-stu-id="c6f22-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="c6f22-182">L’illustration suivante montre à quoi ressemble la page dans l’éditeur avec le point d’arrêt (en jaune).</span><span class="sxs-lookup"><span data-stu-id="c6f22-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![point d’arrêt de débogage](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="c6f22-184">Dans la barre d’outils déboguer, cliquez sur le bouton **pas à pas détaillé** (ou appuyez sur F11) pour exécuter la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="c6f22-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="c6f22-185">Chaque fois que vous cliquez sur ce bouton, vous avancez l’exécution jusqu’à la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="c6f22-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Bouton pas à pas détaillé](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="c6f22-187">Examinez la valeur de la variable `myTime` en maintenant le pointeur de la souris sur celle-ci ou en inspectant les valeurs affichées dans les fenêtres **variables locales** et **pile des appels** .</span><span class="sxs-lookup"><span data-stu-id="c6f22-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="c6f22-188">Visual Studio affiche la valeur de la variable.</span><span class="sxs-lookup"><span data-stu-id="c6f22-188">Visual Studio display the value of the variable.</span></span>

    ![afficher la valeur de l’heure](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="c6f22-190">Lorsque vous avez terminé d’examiner la variable et d’exécuter le code pas à pas, appuyez sur F5 pour continuer à exécuter la page sans l’arrêter à chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="c6f22-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="c6f22-191">Lorsque vous avez terminé l’exécution pas à pas de l’ensemble du code, le navigateur affiche la page.</span><span class="sxs-lookup"><span data-stu-id="c6f22-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="c6f22-192">Pour en savoir plus sur le débogueur et sur la façon de déboguer le code dans Visual Studio, consultez [procédure pas à pas : débogage de pages Web dans Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6f22-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="c6f22-193">Utilisation de Razor dans les projets MVC ASP.NET avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6f22-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="c6f22-194">Le syntaxe Razor est également largement utilisé dans les projets MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6f22-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="c6f22-195">MVC est un moyen puissant et basé sur des modèles pour créer des sites Web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="c6f22-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="c6f22-196">Si votre site pages Web ASP.NET devient difficile à gérer, vous pouvez envisager de le convertir en une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6f22-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="c6f22-197">Pour obtenir un exemple de création d’une application MVC, consultez [prise en main avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c6f22-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="c6f22-198">Installation de la prise en charge de pages Web ASP.NET dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="c6f22-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="c6f22-199">Cette section montre comment installer Visual Web Developer Express 2010 et les outils pages Web ASP.NET pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6f22-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="c6f22-200">Si vous n’avez pas encore le Web Platform Installer, téléchargez-le à partir de l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="c6f22-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="c6f22-201">Exécutez le Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="c6f22-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="c6f22-202">Cliquez sur l’onglet **produits** .</span><span class="sxs-lookup"><span data-stu-id="c6f22-202">Click the **Products** tab.</span></span>

    ![Onglet produits de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="c6f22-204">Recherchez **ASP.NET MVC 4** (pour pages Web ASP.NET 2), puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c6f22-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="c6f22-205">Ces produits incluent les outils Visual Studio pour la création de sites Web Razor ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6f22-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Options d’installation de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="c6f22-207">Cliquez sur **installer** pour terminer l’installation.</span><span class="sxs-lookup"><span data-stu-id="c6f22-207">Click **Install** to complete the installation.</span></span>
