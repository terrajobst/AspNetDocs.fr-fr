---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Utilisation de Inspecteur de page pour Visual Studio 2012 dans ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Inspecteur de page pour Visual Studio 2012 est un outil de développement Web avec un navigateur intégré. Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575921"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="4f26b-104">Utilisation de l’Inspecteur de page pour Visual Studio 2012 dans Web Forms ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f26b-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>

<span data-ttu-id="4f26b-105">par Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="4f26b-105">by Tim Ammann</span></span>

> <span data-ttu-id="4f26b-106">Inspecteur de page pour Visual Studio 2012 est un outil de développement Web avec un navigateur intégré.</span><span class="sxs-lookup"><span data-stu-id="4f26b-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="4f26b-107">Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page met instantanément en surbrillance la source et le CSS de l’élément.</span><span class="sxs-lookup"><span data-stu-id="4f26b-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="4f26b-108">Vous pouvez parcourir n’importe quelle page de votre application, rechercher rapidement les sources du balisage rendu et utiliser les outils de navigation directement dans l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f26b-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="4f26b-109">Ce didacticiel montre comment activer Mode d’inspection, puis Rechercher et modifier rapidement les règles CSS et le texte dans votre projet Web.</span><span class="sxs-lookup"><span data-stu-id="4f26b-109">This tutorial shows how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="4f26b-110">Le didacticiel utilise un projet d’application Web Forms, mais vous pouvez également utiliser des Inspecteur de page pour les projets de site Web et les applications [MVC](https://go.microsoft.com/?linkid=9802002) .</span><span class="sxs-lookup"><span data-stu-id="4f26b-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="4f26b-111">Ce didacticiel contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="4f26b-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="4f26b-112">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="4f26b-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="4f26b-113">Créer une application Web</span><span class="sxs-lookup"><span data-stu-id="4f26b-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="4f26b-114">Utiliser Inspecteur de page pour afficher l’application</span><span class="sxs-lookup"><span data-stu-id="4f26b-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="4f26b-115">Activer Mode d’inspection</span><span class="sxs-lookup"><span data-stu-id="4f26b-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="4f26b-116">Utiliser Inspecteur de page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="4f26b-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="4f26b-117">Mode d’inspection et la fenêtre HTML</span><span class="sxs-lookup"><span data-stu-id="4f26b-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="4f26b-118">Aperçu des modifications CSS dans la fenêtre styles</span><span class="sxs-lookup"><span data-stu-id="4f26b-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="4f26b-119">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="4f26b-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="4f26b-120">Utilisation du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="4f26b-120">Using the CSS Color Picker</span></span>](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="4f26b-121">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="4f26b-121">Prerequisites</span></span>

- <span data-ttu-id="4f26b-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="4f26b-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="4f26b-123">Pour obtenir la dernière version de Inspecteur de page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le kit de développement logiciel (SDK) Azure pour .NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="4f26b-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="4f26b-124">Inspecteur de page est fourni avec Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="4f26b-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="4f26b-125">La version la plus récente est 1,3.</span><span class="sxs-lookup"><span data-stu-id="4f26b-125">The latest version is 1.3.</span></span> <span data-ttu-id="4f26b-126">Pour vérifier la version que vous avez, exécutez Visual Studio et sélectionnez à **propos de Microsoft Visual Studio** dans le menu **aide** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="4f26b-127">Créer une application Web</span><span class="sxs-lookup"><span data-stu-id="4f26b-127">Create a Web Application</span></span>

<span data-ttu-id="4f26b-128">Tout d’abord, vous allez créer une application Web que vous utiliserez Inspecteur de page avec.</span><span class="sxs-lookup"><span data-stu-id="4f26b-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="4f26b-129">Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="4f26b-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="4f26b-130">À gauche, développez **visuel C#** , sélectionnez **Web**, puis sélectionnez **ASP.NET Web Forms application**.</span><span class="sxs-lookup"><span data-stu-id="4f26b-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nouvelle Web Forms application](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="4f26b-132">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f26b-132">Click **OK**.</span></span>

<span data-ttu-id="4f26b-133">L’application s’ouvre en mode **source** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-133">The application opens in **Source** view.</span></span>

![Nouvelle Web Forms application en mode Source](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="4f26b-135">Maintenant que vous disposez d’une application à utiliser, vous pouvez utiliser Inspecteur de page pour l’examiner et la modifier.</span><span class="sxs-lookup"><span data-stu-id="4f26b-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="4f26b-136">Utiliser Inspecteur de page pour afficher l’application</span><span class="sxs-lookup"><span data-stu-id="4f26b-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="4f26b-137">Ensuite, vous allez afficher l’application avec Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="4f26b-138">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis choisissez **afficher dans inspecteur de page**.</span><span class="sxs-lookup"><span data-stu-id="4f26b-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Afficher dans l'inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="4f26b-140">Par défaut, lorsque Inspecteur de page démarre pour la première fois, il est ancré sous la forme d’une fenêtre étroite sur le côté gauche de l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f26b-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="4f26b-141">Laissez-le ancré sur le côté gauche et affectez-lui une largeur qui vous convient, ou ancrez-le dans l’une des zones d’outils en haut, en bas ou à droite :</span><span class="sxs-lookup"><span data-stu-id="4f26b-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Inspecteur de page les positions d’ancrage](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="4f26b-143">Si vous détachez la fenêtre de Inspecteur de page, vous pouvez la placer en dehors de Visual Studio, ou même sur un deuxième écran, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="4f26b-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="4f26b-144">Toutefois, pour pouvoir appuyer sur ALT + TAB entre Inspecteur de page et Visual Studio lorsque la fenêtre de Inspecteur de page n’est pas ancrée, accédez à **outils** &gt; **Options** &gt; **environnement** &gt; **onglets et fenêtres**, et sous **onglets**, désactivez la case à cocher appelée **fenêtres outil flottant toujours rester au-dessus de la fenêtre principale**:</span><span class="sxs-lookup"><span data-stu-id="4f26b-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Désactivez la case à cocher fenêtres outil flottantes pour ALT + TAB entre Visual Studio et la fenêtre de Inspecteur de page désancrée.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="4f26b-146">Le volet supérieur de la fenêtre Inspecteur de page affiche la page actuelle dans une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="4f26b-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="4f26b-147">Le volet inférieur affiche la page dans le balisage HTML à gauche, ainsi que certains onglets à droite qui vous permettent d’inspecter différents aspects de la page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="4f26b-148">Le volet inférieur est semblable au [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="4f26b-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="4f26b-149">(Toutefois, contrairement aux outils de développement, vous pouvez utiliser Inspecteur de page directement dans Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="4f26b-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="4f26b-151">Dans ce didacticiel, vous allez utiliser le volet navigateur Inspecteur de page, ainsi que les onglets **HTML** et **styles** pour vous aider à naviguer rapidement et à apporter des modifications à l’application.</span><span class="sxs-lookup"><span data-stu-id="4f26b-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="4f26b-152">Activer Mode d’inspection</span><span class="sxs-lookup"><span data-stu-id="4f26b-152">Enable Inspection Mode</span></span>

<span data-ttu-id="4f26b-153">Vous verrez ensuite comment le Mode d’inspection de Inspecteur de page fonctionne.</span><span class="sxs-lookup"><span data-stu-id="4f26b-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="4f26b-154">Dans la fenêtre Inspecteur de page, cliquez sur le bouton **inspecter** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="4f26b-156">Pour voir le mode inspection en action, déplacez votre souris sur les différentes parties de la page dans la fenêtre du navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="4f26b-157">Comme c’est le cas, le pointeur de la souris se transforme en un plus grand signe plus, et l’élément sous est mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="4f26b-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Pointage sur div. Content-Wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="4f26b-159">Au fur et à mesure que vous déplacez le pointeur de la souris, Notez que</span><span class="sxs-lookup"><span data-stu-id="4f26b-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="4f26b-160">Le contenu en mode **source** change pour afficher le balisage correspondant à l’élément sélectionné sur la page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="4f26b-161">La balise appropriée est mise en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="4f26b-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="4f26b-162">Si la source se trouve dans un autre fichier, ce fichier est ouvert en mode Source avec le balisage approprié mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="4f26b-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="4f26b-163">Le balisage affiché sous l’onglet **HTML** dans inspecteur de page change également pour correspondre à l’élément sélectionné sur la page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="4f26b-164">Dans l’onglet **HTML** , le balisage approprié est décrit.</span><span class="sxs-lookup"><span data-stu-id="4f26b-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="4f26b-165">L’onglet **styles** affiche les règles CSS qui s’appliquent à la sélection actuelle.</span><span class="sxs-lookup"><span data-stu-id="4f26b-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="4f26b-166">Utiliser Inspecteur de page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="4f26b-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="4f26b-167">À présent, vous verrez comment utiliser Inspecteur de page pour rechercher et apporter des modifications au balisage ou au texte dont l’emplacement peut ne pas être immédiatement évident.</span><span class="sxs-lookup"><span data-stu-id="4f26b-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="4f26b-168">Placez Inspecteur de page dans Mode d’inspection puis faites défiler vers le bas de la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4f26b-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="4f26b-169">Dès que vous entrez dans la zone de pied de page, Inspecteur de page ouvre le fichier de disposition *site. Master* en mode **source** dans un onglet temporaire à droite des autres onglets et met en surbrillance la section de la page maître que vous avez sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="4f26b-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="4f26b-170">Cela vous montre comment Inspecteur de page pouvez rechercher et afficher le contenu d’une page qui peut provenir d’un fichier différent de celui que vous avez ouvert à l’origine.</span><span class="sxs-lookup"><span data-stu-id="4f26b-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Le pied de page met en surbrillance Mode d’inspection](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="4f26b-172">Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur la ligne <a id="a"> </a>avec la mention de droits d’auteur.</span><span class="sxs-lookup"><span data-stu-id="4f26b-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="4f26b-173">Dans la page *site. Master* , la ligne correspondante est mise en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="4f26b-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Ligne de copyright du pied de page en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="4f26b-175">Ajoutez du texte à la fin de la ligne dans le fichier *site. Master* .</span><span class="sxs-lookup"><span data-stu-id="4f26b-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="4f26b-176">&lt;p&gt;&amp;copie ; &lt;% : DateTime. Now. Year%&gt;-My ASP.NET application Rocks !&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="4f26b-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="4f26b-177">Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre du navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Mon application ASP.NET Rocks !](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="4f26b-179">Vous avez peut-être pensé que le pied de page était sur la page *default. aspx* , mais il se trouvait dans la page de disposition maître et inspecteur de page l’a trouvé pour vous.</span><span class="sxs-lookup"><span data-stu-id="4f26b-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="4f26b-180">Mode d’inspection et la fenêtre HTML</span><span class="sxs-lookup"><span data-stu-id="4f26b-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="4f26b-181">Ensuite, vous aurez un aperçu rapide de la fenêtre HTML et de la façon dont elle mappe les éléments pour vous.</span><span class="sxs-lookup"><span data-stu-id="4f26b-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="4f26b-182">Placez Inspecteur de page dans Mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="4f26b-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="4f26b-184">Cliquez sur la partie supérieure de la page qui indique « votre logo ici ».</span><span class="sxs-lookup"><span data-stu-id="4f26b-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="4f26b-185">Vous examinez un élément particulier plus en détail. par conséquent, l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.</span><span class="sxs-lookup"><span data-stu-id="4f26b-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="4f26b-186">Déplacez maintenant le pointeur de la souris vers la fenêtre **HTML** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="4f26b-187">Lorsque vous déplacez le pointeur de la souris, Inspecteur de page présente l’élément dans la fenêtre **HTML** et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4f26b-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Fenêtre HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="4f26b-189">Comme précédemment, Inspecteur de page ouvre le fichier *site. Master* pour vous dans un onglet temporaire. cliquez sur l’onglet site. Master et le balisage correspondant est mis en surbrillance dans la section &lt;en-tête&gt; :</span><span class="sxs-lookup"><span data-stu-id="4f26b-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Balisage en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="4f26b-191">Aperçu des modifications CSS dans la fenêtre styles</span><span class="sxs-lookup"><span data-stu-id="4f26b-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="4f26b-192">Vous verrez ensuite comment vous pouvez utiliser la fenêtre Inspecteur de page **styles** pour afficher un aperçu des modifications apportées à CSS.</span><span class="sxs-lookup"><span data-stu-id="4f26b-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="4f26b-193">Cliquez sur le bouton **inspecter** pour placer Inspecteur de page dans mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="4f26b-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="4f26b-194">Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse.</span><span class="sxs-lookup"><span data-stu-id="4f26b-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Pointage sur les éléments](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="4f26b-196">Cliquez une fois dans la section div. Content-Wrapper, puis placez le pointeur de la souris dans la fenêtre **styles** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="4f26b-197">Sous le sélecteur de classe. featured. Content-Wrapper, désactivez et activez la case à cocher de la propriété Background-color.</span><span class="sxs-lookup"><span data-stu-id="4f26b-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Effacer la couleur d’arrière-plan](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="4f26b-199">Notez la manière dont les préversions changent instantanément dans la fenêtre du navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="4f26b-200">Activez à nouveau la case à cocher, puis double-cliquez sur la valeur de la propriété et remplacez-la par `red`.</span><span class="sxs-lookup"><span data-stu-id="4f26b-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="4f26b-201">La modification apparaît immédiatement :</span><span class="sxs-lookup"><span data-stu-id="4f26b-201">The change shows immediately:</span></span>

![Couleur d’arrière-plan rouge](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="4f26b-203">La fenêtre **styles** facilite le test et l’aperçu des modifications CSS avant de valider les modifications apportées à la feuille de style elle-même.</span><span class="sxs-lookup"><span data-stu-id="4f26b-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="4f26b-204">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="4f26b-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="4f26b-205">Cette fonctionnalité nécessite la version 1,3 de Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-205">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="4f26b-206">La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et de voir immédiatement les modifications dans le navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="4f26b-207">Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.</span><span class="sxs-lookup"><span data-stu-id="4f26b-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="4f26b-208">Dans le navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse.</span><span class="sxs-lookup"><span data-stu-id="4f26b-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="4f26b-209">Cliquez une fois pour sélectionner cet élément.</span><span class="sxs-lookup"><span data-stu-id="4f26b-209">Click once to select this element.</span></span>

<span data-ttu-id="4f26b-210">La fenêtre **styles** affiche toutes les règles CSS pour cet élément.</span><span class="sxs-lookup"><span data-stu-id="4f26b-210">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="4f26b-211">Faites défiler la liste pour rechercher le sélecteur de classe. featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="4f26b-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="4f26b-212">Cliquez sur « . featured. Content-wrapper ».</span><span class="sxs-lookup"><span data-stu-id="4f26b-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="4f26b-213">Inspecteur de page ouvre le fichier CSS qui définit ce style (site. CSS) et met en surbrillance le style CSS correspondant.</span><span class="sxs-lookup"><span data-stu-id="4f26b-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Fichier CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="4f26b-215">À présent, remplacez la valeur de `background-color` par « Red ».</span><span class="sxs-lookup"><span data-stu-id="4f26b-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="4f26b-216">La modification s’affiche immédiatement dans le navigateur Inspecteur de page.</span><span class="sxs-lookup"><span data-stu-id="4f26b-216">The change appears immediately in the Page Inspector browser.</span></span>

![Navigateur de Inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="4f26b-218">Utilisation du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="4f26b-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="4f26b-219">Ensuite, vous allez apprendre à utiliser Inspecteur de page pour rechercher et modifier rapidement le CSS pour le texte mis en surbrillance dans l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="4f26b-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="4f26b-220">Dans cet exemple, vous avez décidé que vous n’aimez pas la mise en surbrillance bleue et que vous souhaitez la remplacer par une autre couleur.</span><span class="sxs-lookup"><span data-stu-id="4f26b-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="4f26b-221">Cliquez sur le bouton **inspecter** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-221">Click the **Inspect** button.</span></span>

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="4f26b-223">Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur le texte « vidéos, didacticiels et exemples » en surbrillance pour afficher l’étiquette « marque » CSS.</span><span class="sxs-lookup"><span data-stu-id="4f26b-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Pointage sur l’élément Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="4f26b-225">Cliquez sur le texte pour le sélectionner.</span><span class="sxs-lookup"><span data-stu-id="4f26b-225">Click the text to select it.</span></span> <span data-ttu-id="4f26b-226">Le sélecteur de marque CSS correspondant apparaît en bas de la fenêtre **styles** .</span><span class="sxs-lookup"><span data-stu-id="4f26b-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![marquer le sélecteur dans la fenêtre styles](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="4f26b-228">Cliquez sur le sélecteur de marque.</span><span class="sxs-lookup"><span data-stu-id="4f26b-228">Click the mark selector.</span></span> <span data-ttu-id="4f26b-229">Cela ouvre le fichier *site. CSS* pour l’application Web.</span><span class="sxs-lookup"><span data-stu-id="4f26b-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="4f26b-230">Cliquez sur l’onglet site. CSS pour mettre en surbrillance le CSS correspondant du sélecteur :</span><span class="sxs-lookup"><span data-stu-id="4f26b-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![marquer le sélecteur dans la feuille de style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="4f26b-232">Sélectionnez et supprimez la ligne avec la propriété Background-color.</span><span class="sxs-lookup"><span data-stu-id="4f26b-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="4f26b-233">Vous allez maintenant utiliser le nouveau sélecteur de couleurs CSS de Visual Studio 2012 pour choisir une nouvelle couleur pour la propriété **marquer** la couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="4f26b-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="4f26b-234">Utilisation du sélecteur de couleurs CSS de Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4f26b-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="4f26b-235">L’éditeur CSS dans Visual Studio 2012 possède un sélecteur de couleurs qui facilite la sélection et l’insertion de couleurs.</span><span class="sxs-lookup"><span data-stu-id="4f26b-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="4f26b-236">Il possède une barre de couleurs simple et un sélecteur « indépendant » qui offre un contrôle plus précis.</span><span class="sxs-lookup"><span data-stu-id="4f26b-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="4f26b-237">Le sélecteur de couleurs contient une palette de couleurs standard, prend en charge les couleurs standard, les codes de hachage, les couleurs RVB, RVBA, TSL et HSLA, et conserve une liste des couleurs que vous avez utilisées le plus récemment dans le document.</span><span class="sxs-lookup"><span data-stu-id="4f26b-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="4f26b-238">Sur la ligne où la propriété Background-color était, tapez « BC » et appuyez une fois sur la flèche bas.</span><span class="sxs-lookup"><span data-stu-id="4f26b-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="4f26b-239">Quand vous tapez le premier caractère de chaque mot dans une propriété séparée par un trait d’Union comme « background-color », IntelliSense filtre la liste pour afficher uniquement les propriétés correspondantes :</span><span class="sxs-lookup"><span data-stu-id="4f26b-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valeurs filtrées IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="4f26b-241">À présent, tapez un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="4f26b-241">Now type a colon.</span></span> <span data-ttu-id="4f26b-242">Dans ce cas, le nom complet de la propriété Background-color est inséré.</span><span class="sxs-lookup"><span data-stu-id="4f26b-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="4f26b-243">Tapez **#** ou **RVB (** , et la barre du sélecteur de couleurs s’affiche :</span><span class="sxs-lookup"><span data-stu-id="4f26b-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Barre du sélecteur de couleurs CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="4f26b-245">Pour voir comment fonctionne la barre du sélecteur de couleurs, cliquez sur ses couleurs à l’aide du pointeur de la souris, ou appuyez sur la touche de direction bas, puis utilisez les touches de direction gauche et droite pour parcourir les couleurs.</span><span class="sxs-lookup"><span data-stu-id="4f26b-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="4f26b-246">Lorsque vous visitez une couleur, la valeur correspondante pour la propriété Background-color est aperçu :</span><span class="sxs-lookup"><span data-stu-id="4f26b-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![couleur d’arrière-plan-aperçu de la valeur de propriété](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="4f26b-248">À ce stade, vous pouvez appuyer sur entrée pour sélectionner la valeur, puis un point-virgule (;) pour terminer l’entrée CSS.</span><span class="sxs-lookup"><span data-stu-id="4f26b-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="4f26b-249">Pour le moment, passez à la section suivante pour voir comment fonctionne la fenêtre contextuelle du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="4f26b-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="4f26b-250">Utilisation de la fenêtre contextuelle du sélecteur de couleurs</span><span class="sxs-lookup"><span data-stu-id="4f26b-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="4f26b-251">Si la barre de couleurs n’a pas la couleur exacte que vous recherchez, vous pouvez utiliser le menu contextuel du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="4f26b-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="4f26b-252">Pour l’ouvrir, cliquez sur le double Chevron à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche bas une fois ou deux fois sur le clavier.</span><span class="sxs-lookup"><span data-stu-id="4f26b-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Menu contextuel du sélecteur de couleurs CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="4f26b-254">Cliquez sur une couleur à partir de la barre verticale à droite.</span><span class="sxs-lookup"><span data-stu-id="4f26b-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="4f26b-255">Cela montre un dégradé pour cette couleur dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="4f26b-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="4f26b-256">Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée ou en cliquant sur n’importe quel point dans la fenêtre principale pour choisir avec une plus grande précision.</span><span class="sxs-lookup"><span data-stu-id="4f26b-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="4f26b-257">S’il y a une couleur sur l’écran de l’ordinateur que vous souhaitez utiliser (il n’est pas nécessaire qu’il se trouve à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil pipette situé en bas à droite.</span><span class="sxs-lookup"><span data-stu-id="4f26b-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="4f26b-258">Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="4f26b-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="4f26b-259">Cela modifie les valeurs de couleur en valeurs RVBA, car le format RVBA peut représenter l’opacité.</span><span class="sxs-lookup"><span data-stu-id="4f26b-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="4f26b-260">Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de couleur d’arrière-plan dans le fichier *site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="4f26b-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="4f26b-261">Barre de mise à jour Inspecteur de page</span><span class="sxs-lookup"><span data-stu-id="4f26b-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="4f26b-262">Inspecteur de page détecte immédiatement la modification apportée au fichier *site. CSS* (ou à n’importe quel fichier de l’application) et affiche une alerte dans une barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="4f26b-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barre de mise à jour](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="4f26b-264">Pour enregistrer tous vos fichiers et actualiser le navigateur Inspecteur de page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="4f26b-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="4f26b-265">La modification de la couleur de surbrillance s’affiche dans le navigateur :</span><span class="sxs-lookup"><span data-stu-id="4f26b-265">The change in the highlight color appears in the browser:</span></span>

![Couleur de surbrillance modifiée](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="4f26b-267">Notez que vous avez facilement actualisé le Inspecteur de page navigateur à partir de l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f26b-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="4f26b-268">L’utilisation de Inspecteur de page au lieu d’un navigateur externe vous permet de rester dans l’éditeur lorsque vous développez vos applications Web.</span><span class="sxs-lookup"><span data-stu-id="4f26b-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="4f26b-269">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="4f26b-269">See Also</span></span>

<span data-ttu-id="4f26b-270">[Présentation de inspecteur de page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vidéo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="4f26b-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="4f26b-271">[Inspecteur de page des messages d’erreur](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="4f26b-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
