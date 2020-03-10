---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Utilisation du calendrier contextuel de la fenêtre contextuelle de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 3 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538905"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="e36b5-103">Utilisation du calendrier contextuel de la fenêtre contextuelle du sélecteur de dates de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 3</span><span class="sxs-lookup"><span data-stu-id="e36b5-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>

<span data-ttu-id="e36b5-104">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e36b5-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="e36b5-105">Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans une application Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e36b5-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>

## <a name="working-with-complex-types"></a><span data-ttu-id="e36b5-106">Utilisation des types complexes</span><span class="sxs-lookup"><span data-stu-id="e36b5-106">Working with Complex Types</span></span>

<span data-ttu-id="e36b5-107">Dans cette section, vous allez créer une classe d’adresses et apprendre à créer un modèle pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="e36b5-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="e36b5-108">Dans le dossier *Models* , créez un fichier de classe nommé *Person.cs* dans lequel vous allez placer deux types : une classe `Person` et une classe `Address`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="e36b5-109">La classe `Person` contient une propriété de type `Address`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="e36b5-110">Le type de `Address` est un type complexe, ce qui signifie qu’il ne s’agit pas de l’un des types intégrés comme `int`, `string`ou `double`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="e36b5-111">Au lieu de cela, elle a plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="e36b5-111">Instead, it has several properties.</span></span> <span data-ttu-id="e36b5-112">Le code pour les nouvelles classes se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="e36b5-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="e36b5-113">Dans le contrôleur `Movie`, ajoutez l’action `PersonDetail` suivante pour afficher une instance person :</span><span class="sxs-lookup"><span data-stu-id="e36b5-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="e36b5-114">Ajoutez ensuite le code suivant au contrôleur de `Movie` pour remplir le modèle de `Person` avec des exemples de données :</span><span class="sxs-lookup"><span data-stu-id="e36b5-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="e36b5-115">Ouvrez le fichier *Views\Movies\PersonDetail.cshtml* et ajoutez le balisage suivant pour la vue `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="e36b5-116">Appuyez sur CTRL + F5 pour exécuter l’application et accédez à *movies/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="e36b5-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="e36b5-117">La vue `PersonDetail` ne contient pas le type complexe `Address`, comme vous pouvez le voir dans cette capture d’écran.</span><span class="sxs-lookup"><span data-stu-id="e36b5-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="e36b5-118">(Aucune adresse n’est indiquée.)</span><span class="sxs-lookup"><span data-stu-id="e36b5-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="e36b5-119">Les données du modèle de `Address` ne sont pas affichées, car il s’agit d’un type complexe.</span><span class="sxs-lookup"><span data-stu-id="e36b5-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="e36b5-120">Pour afficher les informations d’adresse, ouvrez de nouveau le fichier *Views\Movies\PersonDetail.cshtml* et ajoutez le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="e36b5-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="e36b5-121">Le balisage complet de la vue `PersonDetail` Now se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="e36b5-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="e36b5-122">Réexécutez l’application et affichez la vue `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="e36b5-123">Les informations d’adresse s’affichent désormais :</span><span class="sxs-lookup"><span data-stu-id="e36b5-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="e36b5-124">Création d’un modèle pour un type complexe</span><span class="sxs-lookup"><span data-stu-id="e36b5-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="e36b5-125">Dans cette section, vous allez créer un modèle qui sera utilisé pour restituer le type complexe `Address`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="e36b5-126">Lorsque vous créez un modèle pour le type de `Address`, ASP.NET MVC peut l’utiliser automatiquement pour mettre en forme un modèle d’adresse n’importe où dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e36b5-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="e36b5-127">Cela vous donne un moyen de contrôler le rendu du type de `Address` à partir d’un seul emplacement dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e36b5-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="e36b5-128">Dans le dossier *Views\Shared\DisplayTemplates.* , créez une vue partielle fortement typée nommée **Address**:</span><span class="sxs-lookup"><span data-stu-id="e36b5-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="e36b5-129">Cliquez sur **Ajouter**, puis ouvrez le nouveau fichier *Views\Shared\DisplayTemplates\Address.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e36b5-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="e36b5-130">La nouvelle vue contient le balisage généré suivant :</span><span class="sxs-lookup"><span data-stu-id="e36b5-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="e36b5-131">Exécutez l’application et affichez la vue `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="e36b5-132">Cette fois, le modèle de `Address` que vous venez de créer est utilisé pour afficher le type complexe `Address`, de sorte que l’affichage ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e36b5-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="e36b5-133">Résumé : méthodes pour spécifier le format et le modèle d’affichage du modèle</span><span class="sxs-lookup"><span data-stu-id="e36b5-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="e36b5-134">Vous avez vu que vous pouvez spécifier le format ou le modèle d’une propriété de modèle à l’aide des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e36b5-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="e36b5-135">Application de l’attribut `DisplayFormat` à une propriété dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="e36b5-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="e36b5-136">Par exemple, le code suivant entraîne l’affichage de la date sans l’heure :</span><span class="sxs-lookup"><span data-stu-id="e36b5-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="e36b5-137">Application d’un attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) à une propriété dans le modèle et spécification du type de données.</span><span class="sxs-lookup"><span data-stu-id="e36b5-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="e36b5-138">Par exemple, le code suivant entraîne l’affichage de la date sans l’heure.</span><span class="sxs-lookup"><span data-stu-id="e36b5-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="e36b5-139">Si l’application contient un modèle *date. cshtml* dans le dossier *Views\Shared\DisplayTemplates.* ou le dossier *Views\Movies\DisplayTemplates* , ce modèle sera utilisé pour restituer la propriété `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="e36b5-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="e36b5-140">Dans le cas contraire, le système de création de modèles ASP.NET intégré affiche la propriété en tant que date.</span><span class="sxs-lookup"><span data-stu-id="e36b5-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="e36b5-141">Création d’un modèle d’affichage dans le dossier *Views\Shared\DisplayTemplates.* ou *Views\Movies\DisplayTemplates* dont le nom correspond au type de données que vous souhaitez mettre en forme.</span><span class="sxs-lookup"><span data-stu-id="e36b5-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="e36b5-142">Par exemple, vous avez vu que le *Views\Shared\DisplayTemplates\DateTime.cshtml* a été utilisé pour restituer des propriétés `DateTime` dans un modèle, sans ajouter un attribut au modèle et sans ajouter de balisage aux vues.</span><span class="sxs-lookup"><span data-stu-id="e36b5-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="e36b5-143">Utilisation de l’attribut [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) sur le modèle pour spécifier le modèle pour l’affichage de la propriété de modèle.</span><span class="sxs-lookup"><span data-stu-id="e36b5-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="e36b5-144">Ajout explicite du nom du modèle d’affichage à l’appel [html. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) dans une vue.</span><span class="sxs-lookup"><span data-stu-id="e36b5-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="e36b5-145">L’approche que vous utilisez dépend de ce que vous devez faire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="e36b5-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="e36b5-146">Il n’est pas rare de combiner ces approches pour atteindre exactement le type de mise en forme dont vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="e36b5-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="e36b5-147">Dans la section suivante, vous allez changer les engrenages et passer de la personnalisation de l’affichage des données pour personnaliser la façon dont elles sont entrées.</span><span class="sxs-lookup"><span data-stu-id="e36b5-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="e36b5-148">Vous allez raccorder le sélecteur de modifications jQuery aux vues de modification dans l’application afin de fournir un moyen de spécifier des dates.</span><span class="sxs-lookup"><span data-stu-id="e36b5-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e36b5-149">[Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Suivant](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="e36b5-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
