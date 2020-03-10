---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Création et utilisation d’un programme d’assistance dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment créer une application auxiliaire dans un site Web pages Web ASP.NET (Razor). Une application d’assistance est un composant réutilisable qui comprend le code et le balisage des performances...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563510"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="5e170-104">Création et utilisation d’un programme d’assistance dans un site pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="5e170-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="5e170-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5e170-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5e170-106">Cet article explique comment créer une application auxiliaire dans un site Web pages Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="5e170-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="5e170-107">Une *application auxiliaire* est un composant réutilisable qui comprend du code et des balises pour effectuer une tâche qui peut être fastidieuse ou complexe.</span><span class="sxs-lookup"><span data-stu-id="5e170-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="5e170-108">**Ce que vous allez apprendre :**</span><span class="sxs-lookup"><span data-stu-id="5e170-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="5e170-109">Comment créer et utiliser une application auxiliaire simple.</span><span class="sxs-lookup"><span data-stu-id="5e170-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="5e170-110">Voici les fonctionnalités ASP.NET présentées dans l’article :</span><span class="sxs-lookup"><span data-stu-id="5e170-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="5e170-111">Syntaxe de `@helper`.</span><span class="sxs-lookup"><span data-stu-id="5e170-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5e170-112">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5e170-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5e170-113">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5e170-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5e170-114">Ce didacticiel fonctionne également avec pages Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="5e170-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="5e170-115">Vue d’ensemble des applications auxiliaires</span><span class="sxs-lookup"><span data-stu-id="5e170-115">Overview of Helpers</span></span>

<span data-ttu-id="5e170-116">Si vous devez effectuer les mêmes tâches sur des pages différentes de votre site, vous pouvez utiliser une application auxiliaire.</span><span class="sxs-lookup"><span data-stu-id="5e170-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="5e170-117">Pages Web ASP.NET inclut un certain nombre d’applications auxiliaires, et il existe de nombreux autres que vous pouvez télécharger et installer.</span><span class="sxs-lookup"><span data-stu-id="5e170-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="5e170-118">(Une liste des applications d’assistance intégrées dans pages Web ASP.NET est répertoriée dans la [référence rapide de l’API ASP.net](https://go.microsoft.com/fwlink/?LinkId=202907).) Si aucune des applications d’assistance existantes ne répond à vos besoins, vous pouvez créer votre propre application d’assistance.</span><span class="sxs-lookup"><span data-stu-id="5e170-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="5e170-119">Une application d’assistance vous permet d’utiliser un bloc de code commun sur plusieurs pages.</span><span class="sxs-lookup"><span data-stu-id="5e170-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="5e170-120">Supposons que, dans votre page, vous souhaitiez souvent créer un élément note qui est séparé des paragraphes normaux.</span><span class="sxs-lookup"><span data-stu-id="5e170-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="5e170-121">Par exemple, la note est créée en tant qu’élément `<div>` qui a pour style une bordure.</span><span class="sxs-lookup"><span data-stu-id="5e170-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="5e170-122">Au lieu d’ajouter ce même balisage à une page chaque fois que vous souhaitez afficher une note, vous pouvez empaqueter le balisage en tant qu’application d’assistance.</span><span class="sxs-lookup"><span data-stu-id="5e170-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="5e170-123">Vous pouvez ensuite insérer la note avec une seule ligne de code partout où vous en avez besoin.</span><span class="sxs-lookup"><span data-stu-id="5e170-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="5e170-124">L’utilisation d’une application d’assistance de ce type rend le code dans chacune de vos pages plus simple et plus facile à lire.</span><span class="sxs-lookup"><span data-stu-id="5e170-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="5e170-125">Il facilite également la maintenance de votre site, car si vous avez besoin de modifier l’apparence des notes, vous pouvez modifier le balisage à un seul endroit.</span><span class="sxs-lookup"><span data-stu-id="5e170-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="5e170-126">Création d’une application d’assistance</span><span class="sxs-lookup"><span data-stu-id="5e170-126">Creating a Helper</span></span>

<span data-ttu-id="5e170-127">Cette procédure vous montre comment créer l’application auxiliaire qui crée la note, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="5e170-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="5e170-128">Il s’agit d’un exemple simple, mais le programme d’assistance personnalisé peut inclure tout code de balisage et ASP.NET dont vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="5e170-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="5e170-129">Dans le dossier racine du site Web, créez un dossier nommé *App\_code*.</span><span class="sxs-lookup"><span data-stu-id="5e170-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="5e170-130">Il s’agit d’un nom de dossier réservé dans ASP.NET où vous pouvez placer du code pour des composants tels que des applications auxiliaires.</span><span class="sxs-lookup"><span data-stu-id="5e170-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="5e170-131">Dans le dossier *App\_code* , créez un nouveau fichier *. cshtml* , puis nommez-le *MyHelpers. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5e170-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="5e170-132">Remplacez le contenu existant par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="5e170-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="5e170-133">Le code utilise la syntaxe `@helper` pour déclarer un nouveau programme d’assistance nommé `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="5e170-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="5e170-134">Cette aide particulière vous permet de passer un paramètre nommé `content` qui peut contenir une combinaison de texte et de balises.</span><span class="sxs-lookup"><span data-stu-id="5e170-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="5e170-135">Le programme d’assistance insère la chaîne dans le corps de la note à l’aide de la variable `@content`.</span><span class="sxs-lookup"><span data-stu-id="5e170-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="5e170-136">Notez que le fichier est nommé *MyHelpers. cshtml*, mais que le programme d’assistance est nommé `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="5e170-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="5e170-137">Vous pouvez placer plusieurs applications d’assistance personnalisées dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="5e170-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="5e170-138">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="5e170-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="5e170-139">Utilisation de l’application auxiliaire dans une page</span><span class="sxs-lookup"><span data-stu-id="5e170-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="5e170-140">Dans le dossier racine, créez un fichier vide nommé *TestHelper. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5e170-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="5e170-141">Ajoutez le code suivant au fichier :</span><span class="sxs-lookup"><span data-stu-id="5e170-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="5e170-142">Pour appeler le programme d’assistance que vous avez créé, utilisez `@` suivi du nom du fichier d’assistance, d’un point, puis du nom de l’assistance.</span><span class="sxs-lookup"><span data-stu-id="5e170-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="5e170-143">(Si vous avez plusieurs dossiers dans l' *application\_* dossier de code, vous pouvez utiliser la syntaxe `@FolderName.FileName.HelperName` pour appeler votre application auxiliaire dans n’importe quel niveau de dossier imbriqué).</span><span class="sxs-lookup"><span data-stu-id="5e170-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="5e170-144">Le texte que vous ajoutez entre parenthèses est le texte que le programme d’assistance affichera dans le cadre de la note dans la page Web.</span><span class="sxs-lookup"><span data-stu-id="5e170-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="5e170-145">Enregistrez la page et exécutez-la dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="5e170-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="5e170-146">L’application d’assistance génère l’élément note juste là où vous avez appelé le programme d’assistance : entre les deux paragraphes.</span><span class="sxs-lookup"><span data-stu-id="5e170-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Capture d’écran montrant la page dans le navigateur et comment le balisage généré par le programme d’assistance place une zone autour du texte spécifié.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="5e170-148">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5e170-148">Additional Resources</span></span>

<span data-ttu-id="5e170-149">[Menu horizontal comme application auxiliaire Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="5e170-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="5e170-150">Cette entrée de blog de Mike pape montre comment créer un menu horizontal en tant qu’application auxiliaire en utilisant le balisage, le code CSS et le code.</span><span class="sxs-lookup"><span data-stu-id="5e170-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="5e170-151">[Tirer parti de HTML5 dans pages Web ASP.net helpers pour WebMatrix et ASP.net MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e170-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="5e170-152">Ce billet de blog de Sam Abraham affiche une assistance qui rend un élément `Canvas` HTML5.</span><span class="sxs-lookup"><span data-stu-id="5e170-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="5e170-153">[La différence entre @Helpers et @Functions dans WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="5e170-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="5e170-154">Cette entrée de blog de Mike salée décrit `@helper` syntaxe et `@function` syntaxe et le moment où l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="5e170-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
