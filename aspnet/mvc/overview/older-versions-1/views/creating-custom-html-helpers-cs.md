---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création d’applications auxiliaires HTMLC#personnalisées () | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est de vous montrer comment créer des applications auxiliaires HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En tirant parti du programme d’assistance HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594529"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="62f06-104">Création de helpers HTML personnalisés (C#)</span><span class="sxs-lookup"><span data-stu-id="62f06-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="62f06-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="62f06-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="62f06-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="62f06-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="62f06-107">L’objectif de ce didacticiel est de vous montrer comment créer des applications auxiliaires HTML personnalisées que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="62f06-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="62f06-108">En tirant parti des applications auxiliaires HTML, vous pouvez réduire la quantité de frappe fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="62f06-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="62f06-109">L’objectif de ce didacticiel est de vous montrer comment créer des applications auxiliaires HTML personnalisées que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="62f06-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="62f06-110">En tirant parti des applications auxiliaires HTML, vous pouvez réduire la quantité de frappe fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="62f06-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="62f06-111">Dans la première partie de ce didacticiel, je décrirai certaines des applications auxiliaires HTML existantes incluses dans l’infrastructure MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62f06-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="62f06-112">Je décrirai ensuite deux méthodes pour créer des applications auxiliaires HTML personnalisées : j’explique comment créer des applications auxiliaires HTML personnalisées en créant une méthode statique et en créant une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="62f06-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="62f06-113">Fonctionnement des applications auxiliaires HTML</span><span class="sxs-lookup"><span data-stu-id="62f06-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="62f06-114">Une application auxiliaire HTML est simplement une méthode qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="62f06-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="62f06-115">La chaîne peut représenter n’importe quel type de contenu souhaité.</span><span class="sxs-lookup"><span data-stu-id="62f06-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="62f06-116">Par exemple, vous pouvez utiliser des applications auxiliaires HTML pour restituer des balises HTML standard, telles que des balises HTML `<input>` et `<img>`.</span><span class="sxs-lookup"><span data-stu-id="62f06-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="62f06-117">Vous pouvez également utiliser des applications auxiliaires HTML pour restituer un contenu plus complexe, tel qu’une bande d’onglets ou une table HTML de données de base de données.</span><span class="sxs-lookup"><span data-stu-id="62f06-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="62f06-118">L’infrastructure MVC ASP.NET comprend l’ensemble suivant de programme d’assistance HTML standard (il ne s’agit pas d’une liste complète) :</span><span class="sxs-lookup"><span data-stu-id="62f06-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="62f06-119">Html. ActionLink ()</span><span class="sxs-lookup"><span data-stu-id="62f06-119">Html.ActionLink()</span></span>
- <span data-ttu-id="62f06-120">Html. BeginForm ()</span><span class="sxs-lookup"><span data-stu-id="62f06-120">Html.BeginForm()</span></span>
- <span data-ttu-id="62f06-121">Html. CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="62f06-121">Html.CheckBox()</span></span>
- <span data-ttu-id="62f06-122">Html. DropDownList ()</span><span class="sxs-lookup"><span data-stu-id="62f06-122">Html.DropDownList()</span></span>
- <span data-ttu-id="62f06-123">Html. EndForm ()</span><span class="sxs-lookup"><span data-stu-id="62f06-123">Html.EndForm()</span></span>
- <span data-ttu-id="62f06-124">Html. Hidden ()</span><span class="sxs-lookup"><span data-stu-id="62f06-124">Html.Hidden()</span></span>
- <span data-ttu-id="62f06-125">Html. ListBox ()</span><span class="sxs-lookup"><span data-stu-id="62f06-125">Html.ListBox()</span></span>
- <span data-ttu-id="62f06-126">Html. mot de passe ()</span><span class="sxs-lookup"><span data-stu-id="62f06-126">Html.Password()</span></span>
- <span data-ttu-id="62f06-127">Html. RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="62f06-127">Html.RadioButton()</span></span>
- <span data-ttu-id="62f06-128">Html. TextArea ()</span><span class="sxs-lookup"><span data-stu-id="62f06-128">Html.TextArea()</span></span>
- <span data-ttu-id="62f06-129">Html. TextBox ()</span><span class="sxs-lookup"><span data-stu-id="62f06-129">Html.TextBox()</span></span>

<span data-ttu-id="62f06-130">Par exemple, prenons le formulaire de la liste 1.</span><span class="sxs-lookup"><span data-stu-id="62f06-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="62f06-131">Ce formulaire est rendu à l’aide de deux des applications auxiliaires HTML standard (voir la figure 1).</span><span class="sxs-lookup"><span data-stu-id="62f06-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="62f06-132">Ce formulaire utilise les méthodes d’assistance `Html.BeginForm()` et `Html.TextBox()` pour restituer un formulaire HTML simple.</span><span class="sxs-lookup"><span data-stu-id="62f06-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="62f06-133">[Page ![affichée avec les applications auxiliaires HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="62f06-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="62f06-134">**Figure 01**: page affichée avec les applications auxiliaires html ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="62f06-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="62f06-135">**Liste 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="62f06-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="62f06-136">La méthode d’assistance HTML. BeginForm () est utilisée pour créer les balises d’ouverture et de fermeture de `<form>` HTML.</span><span class="sxs-lookup"><span data-stu-id="62f06-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="62f06-137">Notez que la méthode `Html.BeginForm()` est appelée dans une instruction using.</span><span class="sxs-lookup"><span data-stu-id="62f06-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="62f06-138">L’instruction Using garantit que la balise `<form>` est fermée à la fin du bloc using.</span><span class="sxs-lookup"><span data-stu-id="62f06-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="62f06-139">Si vous préférez, au lieu de créer un bloc using, vous pouvez appeler la méthode d’assistance HTML. EndForm () pour fermer la balise `<form>`.</span><span class="sxs-lookup"><span data-stu-id="62f06-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="62f06-140">Utilisez l’approche la plus simple pour créer une balise d’ouverture et de fermeture `<form>` qui semble la plus intuitive pour vous.</span><span class="sxs-lookup"><span data-stu-id="62f06-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="62f06-141">Les méthodes d’assistance `Html.TextBox()` sont utilisées dans la liste 1 pour afficher les balises de `<input>` HTML.</span><span class="sxs-lookup"><span data-stu-id="62f06-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="62f06-142">Si vous sélectionnez Afficher la source dans votre navigateur, vous voyez la source HTML dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="62f06-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="62f06-143">Notez que la source contient des balises HTML standard.</span><span class="sxs-lookup"><span data-stu-id="62f06-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62f06-144">Notez que le programme d’assistance `Html.TextBox()`-HTML est rendu avec des balises `<%= %>` au lieu de balises `<% %>`.</span><span class="sxs-lookup"><span data-stu-id="62f06-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="62f06-145">Si vous n’incluez pas le signe égal, rien n’est restitué dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="62f06-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="62f06-146">L’infrastructure MVC ASP.NET contient un petit ensemble d’applications d’assistance.</span><span class="sxs-lookup"><span data-stu-id="62f06-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="62f06-147">Il est très probable que vous deviez étendre l’infrastructure MVC avec des applications auxiliaires HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="62f06-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="62f06-148">Dans le reste de ce didacticiel, vous apprendrez deux méthodes pour créer des applications auxiliaires HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="62f06-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="62f06-149">**Liste 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="62f06-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="62f06-150">Création d’applications auxiliaires HTML avec des méthodes statiques</span><span class="sxs-lookup"><span data-stu-id="62f06-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="62f06-151">Le moyen le plus simple de créer une nouvelle application d’assistance HTML consiste à créer une méthode statique qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="62f06-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="62f06-152">Imaginez, par exemple, que vous décidiez de créer un programme d’assistance HTML qui restitue une balise de `<label>` HTML.</span><span class="sxs-lookup"><span data-stu-id="62f06-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="62f06-153">Vous pouvez utiliser la classe de la liste 2 pour afficher un `<label>`.</span><span class="sxs-lookup"><span data-stu-id="62f06-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="62f06-154">**Liste 2 – `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="62f06-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="62f06-155">Il n’y a rien de spécial concernant la classe dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="62f06-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="62f06-156">La méthode `Label()` retourne simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="62f06-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="62f06-157">La vue d’index modifiée dans la liste 3 utilise la `LabelHelper` pour afficher les balises de `<label>` HTML.</span><span class="sxs-lookup"><span data-stu-id="62f06-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="62f06-158">Notez que la vue comprend une directive `<%@ imports %>` qui importe l’espace de noms `Application1.Helpers`.</span><span class="sxs-lookup"><span data-stu-id="62f06-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="62f06-159">**Liste 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="62f06-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="62f06-160">Création d’applications auxiliaires HTML à l’aide de méthodes d’extension</span><span class="sxs-lookup"><span data-stu-id="62f06-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="62f06-161">Si vous souhaitez créer des applications auxiliaires HTML qui fonctionnent comme les applications auxiliaires HTML standard incluses dans l’infrastructure MVC ASP.NET, vous devez créer des méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="62f06-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="62f06-162">Les méthodes d’extension vous permettent d’ajouter de nouvelles méthodes à une classe existante.</span><span class="sxs-lookup"><span data-stu-id="62f06-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="62f06-163">Lorsque vous créez une méthode d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété HTML d’une vue.</span><span class="sxs-lookup"><span data-stu-id="62f06-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="62f06-164">La classe de la liste 3 ajoute une méthode d’extension à la classe `HtmlHelper` nommée `Label()`.</span><span class="sxs-lookup"><span data-stu-id="62f06-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="62f06-165">Il y a deux choses que vous devez remarquer à propos de cette classe.</span><span class="sxs-lookup"><span data-stu-id="62f06-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="62f06-166">Tout d’abord, Notez que la classe est une classe statique.</span><span class="sxs-lookup"><span data-stu-id="62f06-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="62f06-167">Vous devez définir une méthode d’extension avec une classe statique.</span><span class="sxs-lookup"><span data-stu-id="62f06-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="62f06-168">Deuxièmement, Notez que le premier paramètre de la méthode `Label()` est précédé du mot clé `this`.</span><span class="sxs-lookup"><span data-stu-id="62f06-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="62f06-169">Le premier paramètre d’une méthode d’extension indique la classe que la méthode d’extension étend.</span><span class="sxs-lookup"><span data-stu-id="62f06-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="62f06-170">**Liste 3 – `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="62f06-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="62f06-171">Après avoir créé une méthode d’extension et correctement généré votre application, la méthode d’extension apparaît dans Visual Studio IntelliSense, comme toutes les autres méthodes d’une classe (voir la figure 2).</span><span class="sxs-lookup"><span data-stu-id="62f06-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="62f06-172">La seule différence est que les méthodes d’extension s’affichent avec un symbole spécial en regard de celles-ci (une icône représentant une flèche vers le bas).</span><span class="sxs-lookup"><span data-stu-id="62f06-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="62f06-173">[![à l’aide de la méthode d’extension html. Label ()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="62f06-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="62f06-174">**Figure 02**: utilisation de la méthode d’extension html. Label () ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="62f06-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="62f06-175">La vue d’index modifiée de la liste 4 utilise la méthode d’extension html. Label () pour afficher toutes ses balises de `<label>`.</span><span class="sxs-lookup"><span data-stu-id="62f06-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="62f06-176">**Liste 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="62f06-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="62f06-177">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="62f06-177">Summary</span></span>

<span data-ttu-id="62f06-178">Dans ce didacticiel, vous avez appris deux méthodes pour créer des applications auxiliaires HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="62f06-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="62f06-179">Tout d’abord, vous avez appris à créer un programme d’assistance HTML `Label()` personnalisé en créant une méthode statique qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="62f06-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="62f06-180">Ensuite, vous avez appris à créer une méthode d’assistance HTML `Label()` personnalisée en créant une méthode d’extension sur la classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="62f06-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="62f06-181">Dans ce didacticiel, je me suis concentré sur la création d’une méthode d’assistance HTML extrêmement simple.</span><span class="sxs-lookup"><span data-stu-id="62f06-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="62f06-182">Sachez qu’une application auxiliaire HTML peut être aussi compliquée que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="62f06-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="62f06-183">Vous pouvez générer des applications auxiliaires HTML qui restituent du contenu riche, comme des arborescences, des menus ou des tables de données de base de données.</span><span class="sxs-lookup"><span data-stu-id="62f06-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62f06-184">[Précédent](asp-net-mvc-views-overview-cs.md)
> [Suivant](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="62f06-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
