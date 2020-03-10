---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Création d’une application MVC 3 avec Razor et JavaScript discret | Microsoft Docs
author: microsoft
description: L’exemple d’application Web User list montre à quel degré il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor. L’exemple d’application...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540984"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="3cd78-104">Création d’une application MVC 3 Application avec Razor et JavaScript non obstrusif</span><span class="sxs-lookup"><span data-stu-id="3cd78-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="3cd78-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3cd78-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="3cd78-106">L’exemple d’application Web User list montre à quel degré il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor.</span><span class="sxs-lookup"><span data-stu-id="3cd78-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="3cd78-107">L’exemple d’application montre comment utiliser le nouveau moteur d’affichage Razor avec ASP.NET MVC version 3 et Visual Studio 2010 pour créer un site Web de liste d’utilisateurs fictifs qui comprend des fonctionnalités telles que la création, l’affichage, la modification et la suppression d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3cd78-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="3cd78-108">Ce didacticiel décrit les étapes qui ont été effectuées pour créer l’exemple d’application ASP.NET MVC 3 de la liste d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3cd78-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="3cd78-109">Un projet Visual Studio avec C# le code source vb et est disponible pour accompagner cette rubrique : [Télécharger](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="3cd78-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="3cd78-110">Si vous avez des questions sur ce didacticiel, envoyez-les sur le [Forum MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="3cd78-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="3cd78-111">Présentation</span><span class="sxs-lookup"><span data-stu-id="3cd78-111">Overview</span></span>

<span data-ttu-id="3cd78-112">L’application que vous allez générer est un site Web de liste d’utilisateurs simple.</span><span class="sxs-lookup"><span data-stu-id="3cd78-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="3cd78-113">Les utilisateurs peuvent entrer, afficher et mettre à jour les informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-113">Users can enter, view, and update user information.</span></span>

![Exemple de site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="3cd78-115">Vous pouvez télécharger le projet VB C# et le projet terminé [ici](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="3cd78-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="3cd78-116">Création de l’application Web</span><span class="sxs-lookup"><span data-stu-id="3cd78-116">Creating the Web Application</span></span>

<span data-ttu-id="3cd78-117">Pour démarrer le didacticiel, ouvrez Visual Studio 2010 et créez un nouveau projet à l’aide du modèle d' *application Web ASP.NET MVC 3* .</span><span class="sxs-lookup"><span data-stu-id="3cd78-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="3cd78-118">Nommez l’application &quot;&quot;Mvc3Razor.</span><span class="sxs-lookup"><span data-stu-id="3cd78-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="3cd78-119">[![un nouveau projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="3cd78-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="3cd78-120">Dans la boîte de dialogue **nouveau projet ASP.NET MVC 3** , sélectionnez **application Internet**, sélectionnez le moteur d’affichage Razor, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Boîte de dialogue Nouveau projet ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="3cd78-122">Dans ce didacticiel, vous n’utiliserez pas le fournisseur d’appartenances ASP.NET. vous pouvez donc supprimer tous les fichiers associés à l’ouverture de session et à l’appartenance.</span><span class="sxs-lookup"><span data-stu-id="3cd78-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="3cd78-123">Dans **Explorateur de solutions**, supprimez les fichiers et répertoires suivants :</span><span class="sxs-lookup"><span data-stu-id="3cd78-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="3cd78-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="3cd78-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="3cd78-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="3cd78-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="3cd78-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="3cd78-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="3cd78-127">*Views\Account* (et tous les fichiers de ce répertoire)</span><span class="sxs-lookup"><span data-stu-id="3cd78-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="3cd78-129">Modifiez le fichier <em>\_Layout. cshtml</em> et remplacez le balisage à l’intérieur de l’élément `<div>` nommé `logindisplay` par le message <em>&quot;</em>connexion désactivée&quot;.</span><span class="sxs-lookup"><span data-stu-id="3cd78-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="3cd78-130">L’exemple suivant montre le nouveau balisage :</span><span class="sxs-lookup"><span data-stu-id="3cd78-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="3cd78-131">Ajout du modèle</span><span class="sxs-lookup"><span data-stu-id="3cd78-131">Adding the Model</span></span>

<span data-ttu-id="3cd78-132">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis cliquez sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nouvelle classe de l’utilisateur MDL](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="3cd78-134">Nommez la classe `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="3cd78-134">Name the class `UserModel`.</span></span> <span data-ttu-id="3cd78-135">Remplacez le contenu du fichier *UserModel* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="3cd78-136">La classe `UserModel` représente des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3cd78-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="3cd78-137">Chaque membre de la classe est annoté avec l’attribut [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) de l’espace de noms [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) .</span><span class="sxs-lookup"><span data-stu-id="3cd78-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="3cd78-138">Les attributs de l’espace de noms [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournissent une validation automatique côté client et côté serveur pour les applications Web.</span><span class="sxs-lookup"><span data-stu-id="3cd78-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="3cd78-139">Ouvrez la classe `HomeController` et ajoutez une directive `using` pour pouvoir accéder aux classes `UserModel` et `Users` :</span><span class="sxs-lookup"><span data-stu-id="3cd78-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="3cd78-140">Juste après la déclaration de `HomeController`, ajoutez le commentaire suivant et la référence à une classe `Users` :</span><span class="sxs-lookup"><span data-stu-id="3cd78-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="3cd78-141">La classe `Users` est un magasin de données en mémoire simplifié que vous allez utiliser dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="3cd78-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="3cd78-142">Dans une application réelle, vous utilisez une base de données pour stocker les informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="3cd78-143">Les premières lignes du fichier `HomeController` sont présentées dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="3cd78-144">Générez l’application afin que le modèle utilisateur soit disponible pour l’Assistant de génération de modèles automatique à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="3cd78-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="3cd78-145">Création de la vue par défaut</span><span class="sxs-lookup"><span data-stu-id="3cd78-145">Creating the Default View</span></span>

<span data-ttu-id="3cd78-146">L’étape suivante consiste à ajouter une méthode d’action et une vue pour afficher les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3cd78-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="3cd78-147">Supprimez le fichier *Views\Home\Index* existant.</span><span class="sxs-lookup"><span data-stu-id="3cd78-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="3cd78-148">Vous allez créer un nouveau fichier d' *index* pour afficher les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3cd78-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="3cd78-149">Dans la classe `HomeController`, remplacez le contenu de la méthode `Index` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="3cd78-150">Cliquez avec le bouton droit à l’intérieur de la méthode `Index`, puis cliquez sur **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Ajouter une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="3cd78-152">Sélectionnez l’option **créer un affichage fortement typé** .</span><span class="sxs-lookup"><span data-stu-id="3cd78-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="3cd78-153">Pour la **classe de données d’affichage**, sélectionnez **Mvc3Razor. Models. UserModel**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="3cd78-154">(Si vous ne voyez pas **Mvc3Razor. Models. UserModel** dans la zone **afficher la classe de données** , vous devez générer le projet.) Assurez-vous que le moteur d’affichage est défini sur **Razor**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="3cd78-155">Définissez **afficher le contenu** sur **liste** , puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-155">Set **View content** to **List** and then click **Add**.</span></span>

![Ajouter une vue d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="3cd78-157">La nouvelle vue génère automatiquement une structure automatique des données utilisateur passées à la vue `Index`.</span><span class="sxs-lookup"><span data-stu-id="3cd78-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="3cd78-158">Examinez le fichier *Views\Home\Index* que vous venez de générer.</span><span class="sxs-lookup"><span data-stu-id="3cd78-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="3cd78-159">Les liens **créer**, **modifier**, **Détails**et **supprimer** ne fonctionnent pas, mais le reste de la page est fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="3cd78-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="3cd78-160">Exécutez la page.</span><span class="sxs-lookup"><span data-stu-id="3cd78-160">Run the page.</span></span> <span data-ttu-id="3cd78-161">La liste des utilisateurs s’affiche.</span><span class="sxs-lookup"><span data-stu-id="3cd78-161">You see a list of users.</span></span>

![Page d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="3cd78-163">Ouvrez le fichier *index. cshtml* et remplacez le balisage `ActionLink` pour **modifier**, **Détails**et **supprimer** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="3cd78-164">Le nom d’utilisateur est utilisé comme ID pour Rechercher l’enregistrement sélectionné dans les liens **modifier**, **Détails**et **supprimer** .</span><span class="sxs-lookup"><span data-stu-id="3cd78-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="3cd78-165">Création du mode Détails</span><span class="sxs-lookup"><span data-stu-id="3cd78-165">Creating the Details View</span></span>

<span data-ttu-id="3cd78-166">L’étape suivante consiste à ajouter une `Details` méthode et une vue d’action pour afficher les détails de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="3cd78-168">Ajoutez la méthode `Details` suivante au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="3cd78-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="3cd78-169">Cliquez avec le bouton droit à l’intérieur de la méthode `Details`, puis sélectionnez <strong>Ajouter une vue</strong>.</span><span class="sxs-lookup"><span data-stu-id="3cd78-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="3cd78-170">Vérifiez que la zone <strong>afficher la classe de données</strong> contient <strong>Mvc3Razor. Models. UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="3cd78-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="3cd78-171">Définissez <strong>afficher le contenu</strong> sur <strong>Détails</strong> , puis cliquez sur <strong>Ajouter</strong>.</span><span class="sxs-lookup"><span data-stu-id="3cd78-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Ajouter un affichage des détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="3cd78-173">Exécutez l’application et sélectionnez un lien Détails.</span><span class="sxs-lookup"><span data-stu-id="3cd78-173">Run the application and select a details link.</span></span> <span data-ttu-id="3cd78-174">La génération de modèles automatique affiche chaque propriété dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="3cd78-174">The automatic scaffolding shows each property in the model.</span></span>

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="3cd78-176">Création de la vue Edit</span><span class="sxs-lookup"><span data-stu-id="3cd78-176">Creating the Edit View</span></span>

<span data-ttu-id="3cd78-177">Ajoutez la méthode `Edit` suivante au contrôleur d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="3cd78-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="3cd78-178">Ajoutez une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **modifier**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Ajouter un affichage de modification](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="3cd78-180">Exécutez l’application et modifiez le prénom et le nom de l’un des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3cd78-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="3cd78-181">Si vous ne respectez pas les contraintes de `DataAnnotation` qui ont été appliquées à la classe `UserModel`, lorsque vous envoyez le formulaire, vous verrez des erreurs de validation générées par le code serveur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="3cd78-182">Par exemple, si vous modifiez le prénom &quot;Ann&quot; à &quot;un&quot;, lorsque vous envoyez le formulaire, l’erreur suivante s’affiche sur le formulaire :</span><span class="sxs-lookup"><span data-stu-id="3cd78-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="3cd78-183">Dans ce didacticiel, vous traitez le nom d’utilisateur en tant que clé primaire.</span><span class="sxs-lookup"><span data-stu-id="3cd78-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="3cd78-184">Par conséquent, la propriété nom d’utilisateur ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="3cd78-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="3cd78-185">Dans le fichier *Edit. cshtml* , juste après l’instruction `Html.BeginForm`, définissez le nom d’utilisateur en tant que champ masqué.</span><span class="sxs-lookup"><span data-stu-id="3cd78-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="3cd78-186">La propriété est alors passée dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="3cd78-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="3cd78-187">Le fragment de code suivant montre l’emplacement de l’instruction `Hidden` :</span><span class="sxs-lookup"><span data-stu-id="3cd78-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="3cd78-188">Remplacez le balisage `TextBoxFor` et `ValidationMessageFor` pour le nom d’utilisateur par un appel de `DisplayFor`.</span><span class="sxs-lookup"><span data-stu-id="3cd78-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="3cd78-189">La méthode `DisplayFor` affiche la propriété sous la forme d’un élément en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="3cd78-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="3cd78-190">L'exemple suivant montre le balisage complet.</span><span class="sxs-lookup"><span data-stu-id="3cd78-190">The following example shows the completed markup.</span></span> <span data-ttu-id="3cd78-191">Les appels de `TextBoxFor` et d' `ValidationMessageFor` d’origine sont commentés avec les caractères de début et de fin Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="3cd78-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="3cd78-192">Activation de la validation côté client</span><span class="sxs-lookup"><span data-stu-id="3cd78-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="3cd78-193">Pour activer la validation côté client dans ASP.NET MVC 3, vous devez définir deux indicateurs et vous devez inclure trois fichiers JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3cd78-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="3cd78-194">Ouvrez le fichier *Web. config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="3cd78-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="3cd78-195">Vérifiez `that ClientValidationEnabled` et `UnobtrusiveJavaScriptEnabled` avez la valeur true dans les paramètres de l’application.</span><span class="sxs-lookup"><span data-stu-id="3cd78-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="3cd78-196">Le fragment suivant du fichier *Web. config* racine affiche les paramètres corrects :</span><span class="sxs-lookup"><span data-stu-id="3cd78-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="3cd78-197">La définition de `UnobtrusiveJavaScriptEnabled` sur true active Ajax discrète et la validation client discrète.</span><span class="sxs-lookup"><span data-stu-id="3cd78-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="3cd78-198">Lorsque vous utilisez la validation discrète, les règles de validation sont converties en attributs HTML5.</span><span class="sxs-lookup"><span data-stu-id="3cd78-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="3cd78-199">Les noms d’attributs HTML5 ne peuvent comporter que des lettres minuscules, des chiffres et des tirets.</span><span class="sxs-lookup"><span data-stu-id="3cd78-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="3cd78-200">L’affectation de la valeur true à `ClientValidationEnabled` active la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="3cd78-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="3cd78-201">En définissant ces clés dans le fichier *Web. config* de l’application, vous activez la validation du client et le JavaScript discret pour l’application entière.</span><span class="sxs-lookup"><span data-stu-id="3cd78-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="3cd78-202">Vous pouvez également activer ou désactiver ces paramètres dans des vues individuelles ou dans des méthodes de contrôleur à l’aide du code suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="3cd78-203">Vous devez également inclure plusieurs fichiers JavaScript dans l’affichage rendu.</span><span class="sxs-lookup"><span data-stu-id="3cd78-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="3cd78-204">Un moyen simple d’inclure le JavaScript dans toutes les vues consiste à les ajouter au fichier *Views\Shared\\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="3cd78-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="3cd78-205">Remplacez l’élément `<head>` du fichier *\_Layout. cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="3cd78-206">Les deux premiers scripts jQuery sont hébergés par le CDN (Content Delivery Network) de Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="3cd78-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="3cd78-207">En tirant parti du CDN Microsoft Ajax, vous pouvez améliorer considérablement les performances de vos applications.</span><span class="sxs-lookup"><span data-stu-id="3cd78-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="3cd78-208">Exécutez l’application et cliquez sur un lien modifier.</span><span class="sxs-lookup"><span data-stu-id="3cd78-208">Run the application and click an edit link.</span></span> <span data-ttu-id="3cd78-209">Affichez la source de la page dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-209">View the page's source in the browser.</span></span> <span data-ttu-id="3cd78-210">La source du navigateur affiche de nombreux attributs sous la forme `data-val` (pour la validation des données).</span><span class="sxs-lookup"><span data-stu-id="3cd78-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="3cd78-211">Lorsque la validation du client et le JavaScript discret sont activés, les champs d’entrée avec une règle de validation du client contiennent l’attribut `data-val="true"` pour déclencher une validation client discrète.</span><span class="sxs-lookup"><span data-stu-id="3cd78-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="3cd78-212">Par exemple, le champ `City` du modèle a été décoré avec l’attribut [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , ce qui donne le code html illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="3cd78-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="3cd78-213">Pour chaque règle de validation client, un attribut ayant la forme `data-val-rulename="message"`est ajouté.</span><span class="sxs-lookup"><span data-stu-id="3cd78-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="3cd78-214">À l’aide de l’exemple de champ `City` indiqué plus haut, la règle de validation client requise génère l’attribut `data-val-required` et le message &quot;le champ City est obligatoire&quot;.</span><span class="sxs-lookup"><span data-stu-id="3cd78-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="3cd78-215">Exécutez l’application, modifiez l’un des utilisateurs et désactivez le champ `City`.</span><span class="sxs-lookup"><span data-stu-id="3cd78-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="3cd78-216">Lorsque vous quittez le champ, un message d’erreur de validation côté client s’affiche.</span><span class="sxs-lookup"><span data-stu-id="3cd78-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ville obligatoire](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="3cd78-218">De même, pour chaque paramètre de la règle de validation client, un attribut qui a la forme `data-val-rulename-paramname=paramvalue`est ajouté.</span><span class="sxs-lookup"><span data-stu-id="3cd78-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="3cd78-219">Par exemple, la propriété `FirstName` est annotée avec l’attribut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) et spécifie une longueur minimale de 3 et une longueur maximale de 8.</span><span class="sxs-lookup"><span data-stu-id="3cd78-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="3cd78-220">La règle de validation des données nommée `length` a le nom de paramètre `max` et la valeur de paramètre 8.</span><span class="sxs-lookup"><span data-stu-id="3cd78-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="3cd78-221">L’exemple suivant montre le code HTML généré pour le champ `FirstName` lorsque vous modifiez l’un des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="3cd78-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="3cd78-222">Pour plus d’informations sur la validation client discrète, consultez l’entrée « [validation client discrète » dans ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) dans le blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="3cd78-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="3cd78-223">Dans ASP.NET MVC 3 Beta, vous devez parfois envoyer le formulaire afin de démarrer la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="3cd78-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="3cd78-224">Cela peut être modifié pour la version finale.</span><span class="sxs-lookup"><span data-stu-id="3cd78-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="3cd78-225">Création de la vue Create</span><span class="sxs-lookup"><span data-stu-id="3cd78-225">Creating the Create View</span></span>

<span data-ttu-id="3cd78-226">L’étape suivante consiste à ajouter une `Create` méthode et une vue d’action pour permettre à l’utilisateur de créer un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="3cd78-227">Ajoutez la méthode `Create` suivante au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="3cd78-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="3cd78-228">Ajoutez une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **créer**.</span><span class="sxs-lookup"><span data-stu-id="3cd78-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Créer une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="3cd78-230">Exécutez l’application, sélectionnez le lien **créer** , puis ajoutez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="3cd78-231">La méthode `Create` tire automatiquement parti de la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="3cd78-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="3cd78-232">Essayez d’entrer un nom d’utilisateur qui contient un espace blanc, par exemple &quot;&quot;Ben X.</span><span class="sxs-lookup"><span data-stu-id="3cd78-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="3cd78-233">Quand vous quittez le champ nom d’utilisateur, une erreur de validation côté client (`White space is not allowed`) s’affiche.</span><span class="sxs-lookup"><span data-stu-id="3cd78-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="3cd78-234">Ajouter la méthode Delete</span><span class="sxs-lookup"><span data-stu-id="3cd78-234">Add the Delete method</span></span>

<span data-ttu-id="3cd78-235">Pour suivre le didacticiel, ajoutez la méthode `Delete` suivante au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="3cd78-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="3cd78-236">Ajoutez une `Delete` vue comme dans les étapes précédentes, en affectant la valeur **supprimer**au contenu de la **vue** .</span><span class="sxs-lookup"><span data-stu-id="3cd78-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Supprimer la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="3cd78-238">Vous disposez maintenant d’une application ASP.NET MVC 3 simple mais entièrement fonctionnelle avec validation.</span><span class="sxs-lookup"><span data-stu-id="3cd78-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
