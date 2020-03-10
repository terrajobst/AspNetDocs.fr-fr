---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Intégration de JQuery UI DatePicker avec la liaison de modèle et Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642477"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="317b2-104">Intégration de JQuery UI DatePicker avec la liaison de modèle et les Web Forms</span><span class="sxs-lookup"><span data-stu-id="317b2-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>

<span data-ttu-id="317b2-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="317b2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="317b2-106">Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="317b2-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="317b2-107">La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="317b2-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="317b2-108">Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="317b2-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="317b2-109">Ce didacticiel montre comment ajouter le [widget DatePicker](http://jqueryui.com/datepicker/) de l’interface utilisateur jQuery à un formulaire Web et comment utiliser la liaison de modèle pour mettre à jour la base de données avec la valeur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="317b2-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="317b2-110">Ce didacticiel s’appuie sur le projet créé dans la [première](retrieving-data.md) et la [deuxième](updating-deleting-and-creating-data.md) partie de la série.</span><span class="sxs-lookup"><span data-stu-id="317b2-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="317b2-111">Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB.</span><span class="sxs-lookup"><span data-stu-id="317b2-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="317b2-112">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="317b2-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="317b2-113">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="317b2-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="317b2-114">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="317b2-114">What you'll build</span></span>

<span data-ttu-id="317b2-115">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="317b2-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="317b2-116">Ajouter une propriété à votre modèle pour enregistrer la date d’inscription de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="317b2-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="317b2-117">Permettre à l’utilisateur de sélectionner la date d’inscription à l’aide du widget de DatePicker UI de JQuery</span><span class="sxs-lookup"><span data-stu-id="317b2-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="317b2-118">Appliquer les règles de validation pour la date d’inscription</span><span class="sxs-lookup"><span data-stu-id="317b2-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="317b2-119">Le widget de DatePicker de l’interface utilisateur JQuery permet aux utilisateurs de sélectionner facilement une date dans un calendrier qui s’affiche lorsque l’utilisateur interagit avec le champ.</span><span class="sxs-lookup"><span data-stu-id="317b2-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="317b2-120">L’utilisation de ce widget peut être plus pratique pour les utilisateurs que de taper manuellement une date.</span><span class="sxs-lookup"><span data-stu-id="317b2-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="317b2-121">L’intégration du widget DatePicker dans une page qui utilise la liaison de modèle pour les opérations de données nécessite uniquement une petite quantité de travail supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="317b2-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="317b2-122">Ajouter une nouvelle propriété au modèle</span><span class="sxs-lookup"><span data-stu-id="317b2-122">Add a new property to the model</span></span>

<span data-ttu-id="317b2-123">Tout d’abord, vous allez ajouter une propriété **DateTime** à votre modèle d’étudiant et migrer cette modification vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="317b2-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="317b2-124">Ouvrez **UniversityModels.cs**, puis ajoutez le code en surbrillance au modèle Student.</span><span class="sxs-lookup"><span data-stu-id="317b2-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="317b2-125">Le **RangeAttribute** est inclus pour appliquer des règles de validation pour la propriété.</span><span class="sxs-lookup"><span data-stu-id="317b2-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="317b2-126">Pour ce didacticiel, nous partons du principe que Contoso University a été créé le 1er janvier 2013 et que les dates d’inscription antérieures ne sont donc pas valides.</span><span class="sxs-lookup"><span data-stu-id="317b2-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="317b2-127">Dans la fenêtre Package Management, ajoutez une migration en exécutant la commande **Add-migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="317b2-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="317b2-128">Notez que le code de migration ajoute la nouvelle colonne DateTime à la table Student.</span><span class="sxs-lookup"><span data-stu-id="317b2-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="317b2-129">Pour correspondre à la valeur que vous avez spécifiée dans RangeAttribute, ajoutez une valeur par défaut pour la nouvelle colonne, comme indiqué dans le code en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="317b2-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="317b2-130">Enregistrez vos modifications dans le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="317b2-130">Save your change to the migration file.</span></span>

<span data-ttu-id="317b2-131">Vous n’avez pas besoin de réamorcer les données.</span><span class="sxs-lookup"><span data-stu-id="317b2-131">You do not need to seed the data again.</span></span> <span data-ttu-id="317b2-132">Par conséquent, ouvrez **Configuration.cs** dans le dossier migrations, puis supprimez ou commentez le code dans la méthode **Seed** .</span><span class="sxs-lookup"><span data-stu-id="317b2-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="317b2-133">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="317b2-133">Save and close the file.</span></span>

<span data-ttu-id="317b2-134">À présent, exécutez la commande **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="317b2-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="317b2-135">Notez que la colonne existe désormais dans la base de données et que tous les enregistrements existants ont la valeur par défaut pour EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="317b2-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="317b2-136">Ajouter des contrôles dynamiques pour la date d’inscription</span><span class="sxs-lookup"><span data-stu-id="317b2-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="317b2-137">Vous allez maintenant ajouter des contrôles pour afficher et modifier la date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="317b2-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="317b2-138">À ce stade, la valeur est modifiée à l’aide d’une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="317b2-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="317b2-139">Plus loin dans ce didacticiel, vous allez remplacer la zone de texte par le widget de DatePicker JQuery.</span><span class="sxs-lookup"><span data-stu-id="317b2-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="317b2-140">Tout d’abord, il est important de noter que vous n’avez pas besoin d’apporter des modifications au fichier **AddStudent. aspx** .</span><span class="sxs-lookup"><span data-stu-id="317b2-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="317b2-141">Le contrôle DynamicEntity affiche automatiquement la nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="317b2-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="317b2-142">Ouvrez **students. aspx**, puis ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="317b2-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="317b2-143">Exécutez l’application et notez que vous pouvez définir la valeur de la date d’inscription en tapant une date.</span><span class="sxs-lookup"><span data-stu-id="317b2-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="317b2-144">Quand vous ajoutez un nouvel étudiant :</span><span class="sxs-lookup"><span data-stu-id="317b2-144">When adding a new student:</span></span>

![définir la date](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="317b2-146">Ou modifiez une valeur existante :</span><span class="sxs-lookup"><span data-stu-id="317b2-146">Or, editing an existing value:</span></span>

![modifier la date](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="317b2-148">La saisie de la date fonctionne, mais elle peut ne pas être l’expérience du client que vous souhaitez fournir.</span><span class="sxs-lookup"><span data-stu-id="317b2-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="317b2-149">Dans la section suivante, vous allez activer la sélection d’une date dans un calendrier.</span><span class="sxs-lookup"><span data-stu-id="317b2-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="317b2-150">Installer le package NuGet pour fonctionner avec JQuery UI</span><span class="sxs-lookup"><span data-stu-id="317b2-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="317b2-151">Le package NuGet de l' **interface utilisateur de jus** permet d’intégrer facilement les widgets jQuery UI à votre application Web.</span><span class="sxs-lookup"><span data-stu-id="317b2-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="317b2-152">Pour utiliser ce package, installez-le via NuGet.</span><span class="sxs-lookup"><span data-stu-id="317b2-152">To use this package, install it through NuGet.</span></span>

![Ajouter une interface utilisateur de jus](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="317b2-154">La version de l’interface utilisateur de jus que vous installez peut être en conflit avec la version de JQuery dans votre application.</span><span class="sxs-lookup"><span data-stu-id="317b2-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="317b2-155">Avant de poursuivre ce didacticiel, essayez d’exécuter votre application.</span><span class="sxs-lookup"><span data-stu-id="317b2-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="317b2-156">Si vous rencontrez une erreur JavaScript, vous devez rapprocher la version de JQuery.</span><span class="sxs-lookup"><span data-stu-id="317b2-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="317b2-157">Vous pouvez ajouter la version attendue de JQuery à votre dossier de scripts (version 1.8.2 au moment de la rédaction de ce didacticiel), ou dans site. Master, spécifiez le chemin d’accès au fichier JQuery.</span><span class="sxs-lookup"><span data-stu-id="317b2-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="317b2-158">Personnaliser le modèle DateTime pour inclure le widget DatePicker</span><span class="sxs-lookup"><span data-stu-id="317b2-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="317b2-159">Vous allez ajouter le widget DatePicker au modèle Dynamic Data pour modifier une valeur DateTime.</span><span class="sxs-lookup"><span data-stu-id="317b2-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="317b2-160">En ajoutant le widget au modèle, il est automatiquement restitué dans le formulaire d’ajout d’un nouvel étudiant et dans la vue grille pour la modification des étudiants.</span><span class="sxs-lookup"><span data-stu-id="317b2-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="317b2-161">Ouvrez **DateTime\_Edit. ascx**, puis ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="317b2-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="317b2-162">Dans le fichier code-behind, vous allez définir les dates minimale et maximale du DatePicker.</span><span class="sxs-lookup"><span data-stu-id="317b2-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="317b2-163">En définissant ces valeurs, vous empêchez les utilisateurs de naviguer vers des dates non valides.</span><span class="sxs-lookup"><span data-stu-id="317b2-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="317b2-164">Vous allez récupérer les valeurs minimales et maximales de **RangeAttribute** sur la propriété DateTime, si une valeur est fournie.</span><span class="sxs-lookup"><span data-stu-id="317b2-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="317b2-165">Ouvrez **DateTime\_Edit.ascx.cs**, puis ajoutez le code en surbrillance suivant à la page\_méthode Load.</span><span class="sxs-lookup"><span data-stu-id="317b2-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="317b2-166">Exécutez l’application Web et accédez à la page AddStudent.</span><span class="sxs-lookup"><span data-stu-id="317b2-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="317b2-167">Fournissez des valeurs pour les champs et remarquez que lorsque vous cliquez sur la zone de texte pour la date d’inscription, le calendrier est affiché.</span><span class="sxs-lookup"><span data-stu-id="317b2-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![sélecteur de dates](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="317b2-169">Choisissez une date, puis cliquez sur **Insérer**.</span><span class="sxs-lookup"><span data-stu-id="317b2-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="317b2-170">RangeAttribute applique la validation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="317b2-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="317b2-171">En définissant la propriété MinDate sur le DatePicker, vous appliquez également la validation sur le client.</span><span class="sxs-lookup"><span data-stu-id="317b2-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="317b2-172">Le calendrier ne permet pas à l’utilisateur d’accéder à une date antérieure à la valeur de MinDate.</span><span class="sxs-lookup"><span data-stu-id="317b2-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="317b2-173">Lorsque vous modifiez un enregistrement dans la vue grille, le calendrier est également affiché.</span><span class="sxs-lookup"><span data-stu-id="317b2-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker dans GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="317b2-175">Conclusion</span><span class="sxs-lookup"><span data-stu-id="317b2-175">Conclusion</span></span>

<span data-ttu-id="317b2-176">Dans ce didacticiel, vous avez appris à incorporer un widget JQuery dans un formulaire Web qui utilise la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="317b2-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="317b2-177">Dans le [didacticiel](using-query-string-values-to-retrieve-data.md)suivant, vous allez utiliser une valeur de chaîne de requête lors de la sélection de données.</span><span class="sxs-lookup"><span data-stu-id="317b2-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="317b2-178">[Précédent](sorting-paging-and-filtering-data.md)
> [Suivant](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="317b2-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
