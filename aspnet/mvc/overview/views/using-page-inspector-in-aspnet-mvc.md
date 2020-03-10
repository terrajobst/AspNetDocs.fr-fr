---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Utilisation de Inspecteur de page dans ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Inspecteur de page dans Visual Studio 2012 est un outil de développement Web avec un navigateur intégré. Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538016"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="12da5-104">Utilisation de l'Inspecteur de page dans ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="12da5-104">Using Page Inspector in ASP.NET MVC</span></span>

<span data-ttu-id="12da5-105">par Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="12da5-105">by Tim Ammann</span></span>

> <span data-ttu-id="12da5-106">Inspecteur de page dans Visual Studio 2012 est un outil de développement Web avec un navigateur intégré.</span><span class="sxs-lookup"><span data-stu-id="12da5-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="12da5-107">Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page met instantanément en surbrillance la source et le CSS de l’élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="12da5-108">Vous pouvez parcourir n’importe quelle vue MVC, rechercher rapidement les sources du balisage rendu et utiliser les outils de navigation directement dans l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12da5-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="12da5-109">Regarder la vidéo</span><span class="sxs-lookup"><span data-stu-id="12da5-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="12da5-110">Ce didacticiel montre comment activer Mode d’inspection, puis Rechercher et modifier rapidement le balisage et CSS dans votre projet Web.</span><span class="sxs-lookup"><span data-stu-id="12da5-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="12da5-111">Le didacticiel utilise un projet MVC, mais vous pouvez également utiliser Inspecteur de page pour [Web Forms](https://go.microsoft.com/?linkid=9802001) et d’autres applications ASP.net.</span><span class="sxs-lookup"><span data-stu-id="12da5-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="12da5-112">Ce didacticiel contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="12da5-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="12da5-113">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="12da5-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="12da5-114">Créer une application Web</span><span class="sxs-lookup"><span data-stu-id="12da5-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="12da5-115">Utiliser Inspecteur de page pour accéder à une vue</span><span class="sxs-lookup"><span data-stu-id="12da5-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="12da5-116">Activer Mode d’inspection</span><span class="sxs-lookup"><span data-stu-id="12da5-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="12da5-117">Utiliser Inspecteur de page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="12da5-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="12da5-118">Mode d’inspection et la fenêtre HTML</span><span class="sxs-lookup"><span data-stu-id="12da5-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="12da5-119">Aperçu des modifications CSS dans la fenêtre styles</span><span class="sxs-lookup"><span data-stu-id="12da5-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="12da5-120">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="12da5-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="12da5-121">Utilisation du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="12da5-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="12da5-122">Mappage d’éléments de page dynamiques à JavaScript</span><span class="sxs-lookup"><span data-stu-id="12da5-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="12da5-123">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="12da5-123">Prerequisites</span></span>

- <span data-ttu-id="12da5-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="12da5-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="12da5-125">Pour obtenir la dernière version de Inspecteur de page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le kit de développement logiciel (SDK) Windows Azure pour .NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="12da5-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="12da5-126">Inspecteur de page est fourni avec Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="12da5-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="12da5-127">La version la plus récente est 1,3.</span><span class="sxs-lookup"><span data-stu-id="12da5-127">The latest version is 1.3.</span></span> <span data-ttu-id="12da5-128">Pour vérifier la version que vous avez, exécutez Visual Studio et sélectionnez à **propos de Microsoft Visual Studio** dans le menu **aide** .</span><span class="sxs-lookup"><span data-stu-id="12da5-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="12da5-129">Créer une application Web</span><span class="sxs-lookup"><span data-stu-id="12da5-129">Create a Web Application</span></span>

<span data-ttu-id="12da5-130">Tout d’abord, créez une application Web que vous utiliserez Inspecteur de page avec.</span><span class="sxs-lookup"><span data-stu-id="12da5-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="12da5-131">Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="12da5-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="12da5-132">À gauche, développez **visuel C#** , sélectionnez **Web**, puis sélectionnez **application Web ASP.net MVC4**.</span><span class="sxs-lookup"><span data-stu-id="12da5-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nouvelle application ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="12da5-134">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="12da5-134">Click **OK**.</span></span>

<span data-ttu-id="12da5-135">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **application Internet**.</span><span class="sxs-lookup"><span data-stu-id="12da5-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="12da5-136">Laissez **Razor** comme moteur d’affichage par défaut.</span><span class="sxs-lookup"><span data-stu-id="12da5-136">Leave **Razor** as the default view engine.</span></span>

![Nouveau projet MVC ASP.NET-application Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="12da5-138">L’application s’ouvre en mode **source** .</span><span class="sxs-lookup"><span data-stu-id="12da5-138">The application opens in **Source** view.</span></span>

![Nouvelle application ASP.NET MVC en mode Source](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="12da5-140">Maintenant que vous disposez d’une application à utiliser, vous pouvez utiliser Inspecteur de page pour l’examiner et la modifier.</span><span class="sxs-lookup"><span data-stu-id="12da5-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="12da5-141">Utiliser Inspecteur de page pour accéder à une vue</span><span class="sxs-lookup"><span data-stu-id="12da5-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="12da5-142">Dans Visual Studio 2012, vous pouvez cliquer avec le bouton droit sur n’importe quelle vue de votre projet, sélectionner **afficher dans inspecteur de page**, et inspecteur de page déterminer l’itinéraire et afficher la page.</span><span class="sxs-lookup"><span data-stu-id="12da5-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="12da5-143">Dans **Explorateur de solutions**, développez le dossier **views** , puis le dossier de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="12da5-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="12da5-144">Cliquez avec le bouton droit sur le fichier index. cshtml, puis choisissez **afficher dans inspecteur de page**.</span><span class="sxs-lookup"><span data-stu-id="12da5-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Afficher index. cshtml dans Inspecteur de page](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="12da5-146">Par défaut, Inspecteur de page est ancré sous la forme d’une fenêtre sur le côté gauche de l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12da5-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="12da5-147">Si vous préférez, vous pouvez l’ancrer ailleurs ou détacher la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="12da5-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="12da5-148">Consultez [Comment : organiser et ancrer des fenêtres](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="12da5-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="12da5-149">Le volet supérieur de la fenêtre Inspecteur de page affiche la page actuelle dans une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="12da5-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="12da5-150">Le volet inférieur affiche la page dans le balisage HTML, ainsi que certains onglets qui vous permettent d’inspecter différents aspects de la page.</span><span class="sxs-lookup"><span data-stu-id="12da5-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="12da5-151">Le volet inférieur est semblable au [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="12da5-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Application ASP.NET MVC dans Inspecteur de page](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="12da5-153">Dans ce didacticiel, vous allez utiliser les onglets **HTML** et **styles** pour naviguer rapidement et apporter des modifications à l’application.</span><span class="sxs-lookup"><span data-stu-id="12da5-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="12da5-154">Mode EnableInspection</span><span class="sxs-lookup"><span data-stu-id="12da5-154">EnableInspection Mode</span></span>

<span data-ttu-id="12da5-155">Pour placer Inspecteur de page dans Mode d’inspection, cliquez sur le bouton **inspecter** .</span><span class="sxs-lookup"><span data-stu-id="12da5-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="12da5-156">Dans Mode d’inspection, lorsque vous maintenez le pointeur de la souris sur une partie de la page rendue, le balisage ou le code source correspondant est mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="12da5-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Mode d’inspection bascule](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="12da5-158">À présent, déplacez votre souris sur les différentes parties de la page dans Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="12da5-159">Comme c’est le cas, le pointeur de la souris se transforme en un plus grand signe plus, et l’élément sous est mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="12da5-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Pointage sur div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="12da5-161">Lorsque vous déplacez le pointeur de la souris, Visual Studio met en surbrillance les syntaxe Razor correspondants dans le fichier source.</span><span class="sxs-lookup"><span data-stu-id="12da5-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="12da5-162">Si l’élément HTML provient d’un autre fichier source, Visual Studio ouvre automatiquement le fichier.</span><span class="sxs-lookup"><span data-stu-id="12da5-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="12da5-163">Dans Inspecteur de page, l’onglet **HTML** affiche le code HTML généré à partir du syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="12da5-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="12da5-164">Lorsque vous déplacez le pointeur de la souris, les éléments HTML sont mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="12da5-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="12da5-165">L’onglet **styles** affiche les règles CSS pour l’élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="12da5-166">Utiliser Inspecteur de page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="12da5-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="12da5-167">Inspecteur de page vous permet de trouver le balisage dont l’emplacement n’est peut-être pas évident.</span><span class="sxs-lookup"><span data-stu-id="12da5-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="12da5-168">Vous pouvez ensuite modifier le balisage et afficher les modifications qui en résultent.</span><span class="sxs-lookup"><span data-stu-id="12da5-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="12da5-169">Pour le voir, cliquez sur **inspecter** , puis faites défiler la page jusqu’en bas de la page dans la fenêtre de inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="12da5-170">Lorsque vous déplacez le pointeur de la souris dans la zone de pied de page, Inspecteur de page ouvre le fichier \_Layout. cshtml et met en surbrillance la section de la page de disposition que vous avez sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="12da5-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="12da5-171">Comme vous pouvez le voir, le pied de page est défini dans le fichier de disposition, et non dans la vue elle-même.</span><span class="sxs-lookup"><span data-stu-id="12da5-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Pied de page](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="12da5-173">À présent, déplacez le pointeur de la souris sur la <a id="a"> </a>ligne avec la mention de droits d’auteur.</span><span class="sxs-lookup"><span data-stu-id="12da5-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="12da5-174">Dans la page disposition. cshtml \_, la ligne correspondante est mise en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="12da5-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Ligne de copyright du pied de page en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="12da5-176">Ajoutez du texte à la fin de la ligne dans le fichier \_Layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="12da5-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="12da5-177">&lt;p&gt;&amp;copie ; @DateTime.Now.Year-mon application ASP.NET MVC Rocks !&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="12da5-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="12da5-178">Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre du navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Mon application ASP.NET Rocks !](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="12da5-180">Vous avez peut-être pensé que le pied de page est défini dans index. cshtml, mais il s’est avéré dans le \_Layout. cshtml, et Inspecteur de page l’a trouvé pour vous.</span><span class="sxs-lookup"><span data-stu-id="12da5-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="12da5-181">Mode d’inspection et la fenêtre HTML</span><span class="sxs-lookup"><span data-stu-id="12da5-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="12da5-182">Ensuite, vous aurez un aperçu rapide de la fenêtre HTML et de la façon dont elle mappe les éléments pour vous.</span><span class="sxs-lookup"><span data-stu-id="12da5-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="12da5-183">Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="12da5-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="12da5-184">Cliquez sur la partie supérieure de la page qui indique « votre logo ici ».</span><span class="sxs-lookup"><span data-stu-id="12da5-184">Click the top part of the page that says "Your logo here".</span></span> <span data-ttu-id="12da5-185">Vous examinez un élément particulier plus en détail. par conséquent, l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.</span><span class="sxs-lookup"><span data-stu-id="12da5-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="12da5-186">Déplacez maintenant le pointeur de la souris vers la fenêtre **HTML** .</span><span class="sxs-lookup"><span data-stu-id="12da5-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="12da5-187">Lorsque vous déplacez le pointeur de la souris, Inspecteur de page présente l’élément dans la fenêtre **HTML** et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="12da5-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Fenêtre HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="12da5-189">Comme précédemment, Inspecteur de page ouvre le fichier \_Layout. cshtml pour vous dans un onglet temporaire. cliquez sur l’onglet \_Layout. cshtml temporaire, et le balisage correspondant sera mis en surbrillance dans la section&gt; d’en-tête de &lt;pour vous :</span><span class="sxs-lookup"><span data-stu-id="12da5-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Balisage en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="12da5-191">Aperçu des modifications CSS dans la fenêtre styles</span><span class="sxs-lookup"><span data-stu-id="12da5-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="12da5-192">Ensuite, vous allez utiliser la fenêtre Inspecteur de page **styles** pour afficher un aperçu des modifications apportées à CSS.</span><span class="sxs-lookup"><span data-stu-id="12da5-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="12da5-193">Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="12da5-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="12da5-194">Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse.</span><span class="sxs-lookup"><span data-stu-id="12da5-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Pointage sur div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="12da5-196">Cliquez une fois dans la section div. Content-Wrapper, puis placez le pointeur de la souris dans la fenêtre **styles** .</span><span class="sxs-lookup"><span data-stu-id="12da5-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="12da5-197">La fenêtre **styles** affiche toutes les règles CSS pour cet élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="12da5-198">Faites défiler la liste pour rechercher le sélecteur de classe. featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="12da5-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="12da5-199">Maintenant, désactivez la case à cocher de la propriété Background-color.</span><span class="sxs-lookup"><span data-stu-id="12da5-199">Now clear the checkbox for the background-color property.</span></span>

![Effacer la couleur d’arrière-plan](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="12da5-201">Notez la manière dont les préversions changent instantanément dans la fenêtre du navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="12da5-202">Activez à nouveau la case à cocher, puis double-cliquez sur la valeur de la propriété et remplacez-la par la valeur rouge.</span><span class="sxs-lookup"><span data-stu-id="12da5-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="12da5-203">La modification apparaît immédiatement :</span><span class="sxs-lookup"><span data-stu-id="12da5-203">The change shows immediately:</span></span>

![Couleur d’arrière-plan rouge](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="12da5-205">La fenêtre **styles** facilite le test et l’aperçu des modifications CSS avant de valider les modifications apportées à la feuille de style elle-même.</span><span class="sxs-lookup"><span data-stu-id="12da5-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="12da5-206">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="12da5-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="12da5-207">Cette fonctionnalité nécessite la version 1,3 de Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-207">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="12da5-208">La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et de voir immédiatement les modifications dans le navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="12da5-209">Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="12da5-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="12da5-210">Dans le navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse.</span><span class="sxs-lookup"><span data-stu-id="12da5-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="12da5-211">Cliquez une fois pour sélectionner cet élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-211">Click once to select this element.</span></span>

<span data-ttu-id="12da5-212">La fenêtre **styles** affiche toutes les règles CSS pour cet élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="12da5-213">Faites défiler la liste pour rechercher le sélecteur de classe. featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="12da5-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="12da5-214">Cliquez sur « . featured. Content-wrapper ».</span><span class="sxs-lookup"><span data-stu-id="12da5-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="12da5-215">Inspecteur de page ouvre le fichier CSS qui définit ce style (site. CSS) et met en surbrillance le style CSS correspondant.</span><span class="sxs-lookup"><span data-stu-id="12da5-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="12da5-216">À présent, remplacez la valeur de `background-color` par « Red ».</span><span class="sxs-lookup"><span data-stu-id="12da5-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="12da5-217">La modification s’affiche immédiatement dans le navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="12da5-218">Utilisation du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="12da5-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="12da5-219">L’éditeur CSS dans Visual Studio 2012 possède un sélecteur de couleurs qui facilite la sélection et l’insertion de couleurs.</span><span class="sxs-lookup"><span data-stu-id="12da5-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="12da5-220">Le sélecteur de couleurs contient une palette de couleurs standard, prend en charge les couleurs standard, les codes de hachage, les couleurs RVB, RVBA, TSL et HSLA, et conserve une liste des couleurs que vous avez utilisées le plus récemment dans le document.</span><span class="sxs-lookup"><span data-stu-id="12da5-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="12da5-221">Dans la section précédente, vous avez modifié la valeur de la propriété `background-color`.</span><span class="sxs-lookup"><span data-stu-id="12da5-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="12da5-222">Pour appeler le sélecteur de couleurs, placez le point d’insertion après le nom de la propriété et tapez **#** ou **RVB (** .</span><span class="sxs-lookup"><span data-stu-id="12da5-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Barre du sélecteur de couleurs CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="12da5-224">Cliquez sur une couleur pour la sélectionner, ou appuyez sur la touche de direction bas, puis utilisez les touches de direction gauche et droite pour parcourir les couleurs.</span><span class="sxs-lookup"><span data-stu-id="12da5-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="12da5-225">Lorsque vous visitez une couleur, la valeur hexadécimale correspondante est prévisualisée :</span><span class="sxs-lookup"><span data-stu-id="12da5-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![couleur d’arrière-plan-aperçu de la valeur de propriété](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="12da5-227">Si la barre de couleurs n’a pas la couleur exacte que vous souhaitez, vous pouvez utiliser le menu contextuel du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="12da5-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="12da5-228">Pour l’ouvrir, cliquez sur le double Chevron à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche bas une fois ou deux fois sur le clavier.</span><span class="sxs-lookup"><span data-stu-id="12da5-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Menu contextuel du sélecteur de couleurs CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="12da5-230">Cliquez sur une couleur à partir de la barre verticale à droite.</span><span class="sxs-lookup"><span data-stu-id="12da5-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="12da5-231">Cela montre un dégradé pour cette couleur dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="12da5-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="12da5-232">Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée ou en cliquant sur n’importe quel point dans la fenêtre principale pour choisir avec une plus grande précision.</span><span class="sxs-lookup"><span data-stu-id="12da5-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="12da5-233">S’il y a une couleur sur l’écran de l’ordinateur que vous souhaitez utiliser (il n’est pas nécessaire qu’il se trouve à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil pipette situé en bas à droite.</span><span class="sxs-lookup"><span data-stu-id="12da5-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="12da5-234">Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="12da5-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="12da5-235">Cela modifie les valeurs de couleur en valeurs RVBA, car le format RVBA peut représenter l’opacité.</span><span class="sxs-lookup"><span data-stu-id="12da5-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="12da5-236">Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de couleur d’arrière-plan dans le fichier *site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="12da5-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="12da5-237">Barre de mise à jour Inspecteur de page</span><span class="sxs-lookup"><span data-stu-id="12da5-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="12da5-238">Inspecteur de page détecte immédiatement la modification apportée au fichier *site. CSS* et affiche une alerte dans une barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="12da5-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barre de mise à jour](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="12da5-240">Pour enregistrer tous vos fichiers et actualiser le navigateur Inspecteur de page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="12da5-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="12da5-241">La modification de la couleur de surbrillance s’affiche dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="12da5-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="12da5-242">Mappage d’éléments de page dynamiques à JavaScript</span><span class="sxs-lookup"><span data-stu-id="12da5-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="12da5-243">Dans les applications Web modernes, les éléments de la page sont souvent générés de manière dynamique avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="12da5-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="12da5-244">Cela signifie qu’il n’y a pas de balisage statique (HTML ou Razor) qui correspond à ces éléments de page.</span><span class="sxs-lookup"><span data-stu-id="12da5-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="12da5-245">Avec la version 1,3, Inspecteur de page pouvez maintenant mapper des éléments qui ont été ajoutés dynamiquement à la page vers le code JavaScript correspondant.</span><span class="sxs-lookup"><span data-stu-id="12da5-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="12da5-246">Pour illustrer cette fonctionnalité, nous allons utiliser le [modèle d’application à page unique (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="12da5-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="12da5-247">Le modèle SPA requiert la mise à jour [ASP.NET et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .</span><span class="sxs-lookup"><span data-stu-id="12da5-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>

<span data-ttu-id="12da5-248">Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="12da5-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="12da5-249">À gauche, développez **visuel C#** , sélectionnez **Web**, puis sélectionnez **application Web ASP.net MVC4**.</span><span class="sxs-lookup"><span data-stu-id="12da5-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="12da5-250">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="12da5-250">Click **OK**.</span></span>

<span data-ttu-id="12da5-251">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **application à page unique**.</span><span class="sxs-lookup"><span data-stu-id="12da5-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="12da5-252">Dans Explorateur de solutions, développez le dossier **views** , puis le dossier de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="12da5-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="12da5-253">Cliquez avec le bouton droit sur le fichier index. cshtml, puis choisissez **afficher dans inspecteur de page**.</span><span class="sxs-lookup"><span data-stu-id="12da5-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="12da5-254">La première chose affichée dans le navigateur Inspecteur de page est une page de connexion.</span><span class="sxs-lookup"><span data-stu-id="12da5-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="12da5-255">Cliquez sur « s’inscrire » et créez un nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="12da5-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="12da5-256">Une fois inscrit, l’application vous connecte et crée une liste des tâches avec quelques exemples d’éléments.</span><span class="sxs-lookup"><span data-stu-id="12da5-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="12da5-257">Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="12da5-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="12da5-258">Dans le navigateur Inspecteur de page, cliquez sur l’un des éléments de tâche.</span><span class="sxs-lookup"><span data-stu-id="12da5-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="12da5-259">Notez qu’au lieu d’être mis en surbrillance en bleu, l’élément est mis en surbrillance en orange, avec « JS » en regard du nom de l’élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="12da5-260">Cela indique que l’élément a été créé dynamiquement par le biais d’un script.</span><span class="sxs-lookup"><span data-stu-id="12da5-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="12da5-261">En outre, un trait de soulignement orange apparaît sous l’onglet **pile des appels** . Cela indique que le volet **pile des appels** contient plus d’informations sur l’élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="12da5-262">Cliquez sur l’onglet **pile des appels** . Le volet **pile des appels** affiche la pile des appels pour l’appel JavaScript qui a créé l’élément.</span><span class="sxs-lookup"><span data-stu-id="12da5-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="12da5-263">Les appels à des bibliothèques externes telles que jQuery sont réduits, afin que vous puissiez facilement voir les appels à votre script d’application.</span><span class="sxs-lookup"><span data-stu-id="12da5-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="12da5-264">Pour voir la pile complète, y compris les appels aux bibliothèques externes, vous pouvez développer les nœuds étiquetés « bibliothèques externes » :</span><span class="sxs-lookup"><span data-stu-id="12da5-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="12da5-265">Si vous cliquez sur un élément dans la pile des appels, Visual Studio ouvre le fichier de code et met en surbrillance le script correspondant.</span><span class="sxs-lookup"><span data-stu-id="12da5-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="12da5-266">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="12da5-266">See Also</span></span>

<span data-ttu-id="12da5-267">[Introduction à ASP.NET MVC 4 avec Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (site Web ASP.net)</span><span class="sxs-lookup"><span data-stu-id="12da5-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="12da5-268">[Présentation de inspecteur de page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vidéo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="12da5-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="12da5-269">[Inspecteur de page des messages d’erreur](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="12da5-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
