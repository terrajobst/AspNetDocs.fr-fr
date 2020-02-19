---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examen de la façon dont ASP.NET MVC génère le modèle de structure du programme d’assistance DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457607"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="ebe04-102">Examen de la façon dont ASP.NET MVC génère un modèle automatique du helper DropDownList</span><span class="sxs-lookup"><span data-stu-id="ebe04-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>

<span data-ttu-id="ebe04-103">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ebe04-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ebe04-104">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Controllers* , puis sélectionnez **Ajouter un contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="ebe04-105">Nommez le contrôleur **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="ebe04-106">Définissez les options de la boîte de dialogue **Ajouter un contrôleur** comme indiqué dans l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="ebe04-107">Modifiez la vue *StoreManager\Index.cshtml* et supprimez `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="ebe04-108">Si vous supprimez `AlbumArtUrl`, la présentation sera plus lisible.</span><span class="sxs-lookup"><span data-stu-id="ebe04-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="ebe04-109">Le code terminé est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="ebe04-110">Ouvrez le fichier *Controllers\StoreManagerController.cs* et recherchez la méthode `Index`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="ebe04-111">Ajoutez la clause `OrderBy` pour que les albums soient triés par prix.</span><span class="sxs-lookup"><span data-stu-id="ebe04-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="ebe04-112">Le code complet est présenté ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="ebe04-113">Le tri par prix facilite le test des modifications apportées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="ebe04-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="ebe04-114">Lorsque vous testez les méthodes de modification et de création, vous pouvez utiliser un prix faible afin que les données enregistrées s’affichent en premier.</span><span class="sxs-lookup"><span data-stu-id="ebe04-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="ebe04-115">Ouvrez le fichier *StoreManager\Edit.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ebe04-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="ebe04-116">Ajoutez la ligne suivante juste après la balise de légende.</span><span class="sxs-lookup"><span data-stu-id="ebe04-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="ebe04-117">Le code suivant montre le contexte de cette modification :</span><span class="sxs-lookup"><span data-stu-id="ebe04-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="ebe04-118">Le `AlbumId` est requis pour apporter des modifications à un enregistrement d’album.</span><span class="sxs-lookup"><span data-stu-id="ebe04-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="ebe04-119">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="ebe04-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="ebe04-120">Sélectionnez le lien **administrateur** , puis cliquez sur le lien **créer** pour créer un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="ebe04-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="ebe04-121">Vérifiez que les informations de l’album ont été enregistrées.</span><span class="sxs-lookup"><span data-stu-id="ebe04-121">Verify the album information was saved.</span></span> <span data-ttu-id="ebe04-122">Modifiez un album et vérifiez que les modifications que vous avez apportées sont conservées.</span><span class="sxs-lookup"><span data-stu-id="ebe04-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="ebe04-123">Schéma de l’album</span><span class="sxs-lookup"><span data-stu-id="ebe04-123">The Album Schema</span></span>

<span data-ttu-id="ebe04-124">Le contrôleur de `StoreManager` créé par le mécanisme de génération de modèles automatique MVC autorise l’accès à CRUD (création, lecture, mise à jour, suppression) aux albums dans la base de données du magasin musical.</span><span class="sxs-lookup"><span data-stu-id="ebe04-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="ebe04-125">Le schéma des informations relatives aux albums est illustré ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="ebe04-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="ebe04-126">La table `Albums` ne stocke pas le genre et la description de l’album. elle stocke une clé étrangère dans la table `Genres`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="ebe04-127">La table `Genres` contient le nom et la description du genre.</span><span class="sxs-lookup"><span data-stu-id="ebe04-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="ebe04-128">De même, la table `Albums` ne contient pas le nom des artistes d’albums, mais une clé étrangère de la table `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ebe04-129">La table `Artists` contient le nom de l’artiste.</span><span class="sxs-lookup"><span data-stu-id="ebe04-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="ebe04-130">Si vous examinez les données de la table `Albums`, vous pouvez voir que chaque ligne contient une clé étrangère vers la table `Genres` et une clé étrangère vers la table `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ebe04-131">L’image ci-dessous montre certaines données de table de la table `Albums`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="ebe04-132">Balise de sélection HTML</span><span class="sxs-lookup"><span data-stu-id="ebe04-132">The HTML Select Tag</span></span>

<span data-ttu-id="ebe04-133">L’élément `<select>` HTML (créé par le programme d’assistance [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) html) permet d’afficher une liste complète des valeurs (par exemple, la liste des genres).</span><span class="sxs-lookup"><span data-stu-id="ebe04-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="ebe04-134">Pour les formulaires de modification, lorsque la valeur actuelle est connue, la liste de sélection peut afficher la valeur actuelle.</span><span class="sxs-lookup"><span data-stu-id="ebe04-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="ebe04-135">Nous l’avons vu précédemment lorsque nous avons défini la valeur sélectionnée sur **comédie**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="ebe04-136">La liste de sélection est idéale pour l’affichage des données de catégorie ou de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="ebe04-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="ebe04-137">L’élément `<select>` pour la clé étrangère genre affiche la liste des noms de genres possibles, mais lorsque vous enregistrez le formulaire, la propriété genre est mise à jour avec la valeur de clé étrangère genre, et non pas le nom de genre affiché.</span><span class="sxs-lookup"><span data-stu-id="ebe04-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="ebe04-138">Dans l’image ci-dessous, le genre sélectionné est **Disco** et l’artiste est **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="ebe04-139">Examen du code ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ebe04-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="ebe04-140">Ouvrez le fichier *Controllers\StoreManagerController.cs* et recherchez la méthode `HTTP GET Create`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="ebe04-141">La méthode `Create` ajoute deux objets [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) à l' `ViewBag`, l’un pour contenir les informations de genre et l’autre pour contenir les informations sur l’artiste.</span><span class="sxs-lookup"><span data-stu-id="ebe04-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="ebe04-142">La surcharge du constructeur [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) utilisée ci-dessus accepte trois arguments :</span><span class="sxs-lookup"><span data-stu-id="ebe04-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="ebe04-143">*Items*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenant les éléments de la liste.</span><span class="sxs-lookup"><span data-stu-id="ebe04-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="ebe04-144">Dans l’exemple ci-dessus, la liste des genres retournés par `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="ebe04-145">*dataValueField*: nom de la propriété dans la liste **IEnumerable** qui contient la valeur de clé.</span><span class="sxs-lookup"><span data-stu-id="ebe04-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="ebe04-146">Dans l’exemple ci-dessus, `GenreId` et `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="ebe04-147">*dataTextField*: nom de la propriété dans la liste **IEnumerable** qui contient les informations à afficher.</span><span class="sxs-lookup"><span data-stu-id="ebe04-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="ebe04-148">Dans la table Artists et genre, le champ `name` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="ebe04-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="ebe04-149">Ouvrez le fichier *Views\StoreManager\Create.cshtml* et examinez le balisage d’assistance `Html.DropDownList` pour le champ genre.</span><span class="sxs-lookup"><span data-stu-id="ebe04-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="ebe04-150">La première ligne indique que la vue Create prend un modèle de `Album`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="ebe04-151">Dans la méthode `Create` présentée ci-dessus, aucun modèle n’a été passé, de sorte que la vue obtient un modèle de `Album` **null** .</span><span class="sxs-lookup"><span data-stu-id="ebe04-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="ebe04-152">À ce stade, nous créons un nouvel album afin que nous n’ayons pas de données `Album` pour celui-ci.</span><span class="sxs-lookup"><span data-stu-id="ebe04-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="ebe04-153">La surcharge [html. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) présentée ci-dessus prend le nom du champ à lier au modèle.</span><span class="sxs-lookup"><span data-stu-id="ebe04-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="ebe04-154">Il utilise également ce nom pour rechercher un objet **ViewBag** contenant un objet [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) .</span><span class="sxs-lookup"><span data-stu-id="ebe04-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="ebe04-155">À l’aide de cette surcharge, vous devez nommer l’objet **ViewBag SelectList** `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="ebe04-156">Le deuxième paramètre (`String.Empty`) est le texte à afficher quand aucun élément n’est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ebe04-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="ebe04-157">C’est exactement ce que nous voulons lors de la création d’un album.</span><span class="sxs-lookup"><span data-stu-id="ebe04-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="ebe04-158">Si vous avez supprimé le deuxième paramètre et utilisé le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ebe04-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="ebe04-159">La liste de sélection est définie par défaut sur le premier élément, ou Rock dans notre exemple.</span><span class="sxs-lookup"><span data-stu-id="ebe04-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="ebe04-160">Examen de la méthode `HTTP POST Create`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="ebe04-161">Cette surcharge de la méthode `Create` prend un objet `album`, créé par le système de liaison de modèle MVC ASP.NET à partir des valeurs de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="ebe04-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="ebe04-162">Lorsque vous soumettez un nouvel album, si l’état du modèle est valide et qu’il n’y a aucune erreur de base de données, le nouvel album est ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="ebe04-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="ebe04-163">L’illustration suivante montre la création d’un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="ebe04-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="ebe04-164">Vous pouvez utiliser l' [outil Fiddler](http://www.fiddler2.com/fiddler2/) pour examiner les valeurs de formulaire publiées utilisées par la liaison de modèle ASP.NET MVC pour créer l’objet album.</span><span class="sxs-lookup"><span data-stu-id="ebe04-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="ebe04-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="ebe04-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="ebe04-166">Refactorisation de la création de SelectList ViewBag</span><span class="sxs-lookup"><span data-stu-id="ebe04-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="ebe04-167">Les méthodes `Edit` et la méthode `HTTP POST Create` ont le même code pour configurer **SelectList** dans **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ebe04-168">Dans l’esprit de [Dry](http://en.wikipedia.org/wiki/Don't_repeat_yourself), nous refactorisons ce code.</span><span class="sxs-lookup"><span data-stu-id="ebe04-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="ebe04-169">Nous utiliserons ce code refactorisé ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="ebe04-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="ebe04-170">Créez une nouvelle méthode pour ajouter un genre et un artiste **SelectList** à **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="ebe04-171">Remplacez les deux lignes qui déplacent les `ViewBag` dans chacune des méthodes `Create` et `Edit` par un appel à la méthode `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="ebe04-172">Le code terminé est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="ebe04-173">Créez un nouvel album et modifiez un album pour vérifier le travail des modifications.</span><span class="sxs-lookup"><span data-stu-id="ebe04-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="ebe04-174">Passage explicite de SelectList à DropDownList</span><span class="sxs-lookup"><span data-stu-id="ebe04-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="ebe04-175">Les vues Create et Edit créées par la génération de modèles automatique ASP.NET MVC utilisent la surcharge **DropDownList** suivante :</span><span class="sxs-lookup"><span data-stu-id="ebe04-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="ebe04-176">Le balisage `DropDownList` pour la vue Create est illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="ebe04-177">Étant donné que la propriété `ViewBag` du `SelectList` est nommée `GenreId`, l’application auxiliaire **DropDownList** utilise le `GenreId`**SelectList** dans **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ebe04-178">Dans la surcharge **DropDownList** suivante, le `SelectList` est passé explicitement.</span><span class="sxs-lookup"><span data-stu-id="ebe04-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="ebe04-179">Ouvrez le fichier *Views\StoreManager\Edit.cshtml* et modifiez l’appel **DropDownList** pour passer explicitement le **SelectList**, à l’aide de la surcharge ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ebe04-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="ebe04-180">Pour la catégorie genre, procédez comme suit.</span><span class="sxs-lookup"><span data-stu-id="ebe04-180">Do this for the Genre category.</span></span> <span data-ttu-id="ebe04-181">Le code complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="ebe04-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="ebe04-182">Exécutez l’application, cliquez sur le lien **administrateur** , puis accédez à un album jazz et sélectionnez le lien **modifier** .</span><span class="sxs-lookup"><span data-stu-id="ebe04-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="ebe04-183">Au lieu d’afficher jazz comme genre actuellement sélectionné, le rock est affiché.</span><span class="sxs-lookup"><span data-stu-id="ebe04-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="ebe04-184">Lorsque l’argument de chaîne (la propriété à lier) et l’objet **SelectList** ont le même nom, la valeur sélectionnée n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="ebe04-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="ebe04-185">Lorsqu’il n’y a aucune valeur sélectionnée, Browser prend par défaut le premier élément du **SelectList**(qui est le **Rock** dans l’exemple ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="ebe04-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="ebe04-186">Il s’agit d’une limitation connue de l’application auxiliaire **DropDownList** .</span><span class="sxs-lookup"><span data-stu-id="ebe04-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="ebe04-187">Ouvrez le fichier *Controllers\StoreManagerController.cs* et modifiez les noms des objets **SelectList** en `Genres` et `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="ebe04-188">Le code complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="ebe04-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="ebe04-189">Les noms et les artistes sont de meilleurs noms pour les catégories, car ils contiennent plus que l’ID de chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="ebe04-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="ebe04-190">La refactorisation que nous avons fait précédemment a été annulée.</span><span class="sxs-lookup"><span data-stu-id="ebe04-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="ebe04-191">Au lieu de modifier le **ViewBag** en quatre méthodes, nos modifications étaient isolées dans la méthode `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="ebe04-192">Modifiez l’appel **DropDownList** dans les affichages créer et modifier pour utiliser les nouveaux noms **SelectList** .</span><span class="sxs-lookup"><span data-stu-id="ebe04-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="ebe04-193">Le nouveau balisage de la vue Edit (édition) est illustré ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="ebe04-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="ebe04-194">La vue Create requiert une chaîne vide pour empêcher l’affichage du premier élément du SelectList.</span><span class="sxs-lookup"><span data-stu-id="ebe04-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="ebe04-195">Créez un nouvel album et modifiez un album pour vérifier le travail des modifications.</span><span class="sxs-lookup"><span data-stu-id="ebe04-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="ebe04-196">Testez le code de modification en sélectionnant un album avec un genre autre que Rock.</span><span class="sxs-lookup"><span data-stu-id="ebe04-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="ebe04-197">Utilisation d’un modèle de vue avec le programme d’assistance DropDownList</span><span class="sxs-lookup"><span data-stu-id="ebe04-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="ebe04-198">Créez une nouvelle classe dans le dossier ViewModels nommé `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="ebe04-199">Remplacez le code de la classe `AlbumSelectListViewModel` par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="ebe04-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="ebe04-200">Le constructeur `AlbumSelectListViewModel` prend un album, une liste d’artistes et de genres, et crée un objet contenant l’album et un `SelectList` pour les genres et les artistes.</span><span class="sxs-lookup"><span data-stu-id="ebe04-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="ebe04-201">Générez le projet afin que le `AlbumSelectListViewModel` soit disponible lorsque nous créons une vue à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="ebe04-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="ebe04-202">Ajoutez une méthode `EditVM` à la `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="ebe04-203">Le code terminé est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="ebe04-204">Cliquez avec le bouton droit sur `AlbumSelectListViewModel`, sélectionnez **résoudre**, puis **Utilisez MvcMusicStore. ViewModels ;** .</span><span class="sxs-lookup"><span data-stu-id="ebe04-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="ebe04-205">Vous pouvez également ajouter l’instruction using suivante :</span><span class="sxs-lookup"><span data-stu-id="ebe04-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="ebe04-206">Cliquez avec le bouton droit sur `EditVM`, puis sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="ebe04-207">Utilisez les options indiquées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ebe04-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="ebe04-208">Sélectionnez **Ajouter**, puis remplacez le contenu du fichier *Views\StoreManager\EditVM.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ebe04-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="ebe04-209">Le balisage `EditVM` est très similaire au balisage `Edit` d’origine, avec les exceptions suivantes.</span><span class="sxs-lookup"><span data-stu-id="ebe04-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="ebe04-210">Les propriétés de modèle de la vue `Edit` se présentent sous la forme `model.property`(par exemple, `model.Title`).</span><span class="sxs-lookup"><span data-stu-id="ebe04-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="ebe04-211">Les propriétés de modèle de la vue `EditVm` se présentent sous la forme `model.Album.property`(par exemple, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="ebe04-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="ebe04-212">Cela est dû au fait que la vue `EditVM` reçoit un conteneur pour un `Album`, et non un `Album` comme dans la vue `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="ebe04-213">Le deuxième paramètre **DropDownList** provient du modèle de vue, et non de **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ebe04-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="ebe04-214">L’application auxiliaire **BeginForm** dans la vue `EditVM` est publiée explicitement dans la méthode d’action `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="ebe04-215">En republiant sur l’action `Edit`, nous n’avons pas besoin d’écrire de `HTTP POST EditVM` action et de réutiliser le `HTTP POST` `Edit` action.</span><span class="sxs-lookup"><span data-stu-id="ebe04-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="ebe04-216">Exécutez l’application et modifiez un album.</span><span class="sxs-lookup"><span data-stu-id="ebe04-216">Run the application and edit an album.</span></span> <span data-ttu-id="ebe04-217">Modifiez l’URL pour utiliser `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="ebe04-218">Modifiez un champ et cliquez sur le bouton **Enregistrer** pour vérifier que le code fonctionne.</span><span class="sxs-lookup"><span data-stu-id="ebe04-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="ebe04-219">Quelle approche devez-vous utiliser ?</span><span class="sxs-lookup"><span data-stu-id="ebe04-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="ebe04-220">Les trois approches affichées sont acceptables.</span><span class="sxs-lookup"><span data-stu-id="ebe04-220">All three approaches shown are acceptable.</span></span> <span data-ttu-id="ebe04-221">De nombreux développeurs préfèrent passer explicitement le `SelectList` au `DropDownList` à l’aide de l' `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ebe04-221">Many developers prefer to explicitly pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="ebe04-222">Cette approche présente l’avantage supplémentaire de vous donner la possibilité d’utiliser un nom plus approprié pour la collection.</span><span class="sxs-lookup"><span data-stu-id="ebe04-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="ebe04-223">Le seul inconvénient est que vous ne pouvez pas nommer l’objet `ViewBag SelectList` le même nom que la propriété de modèle.</span><span class="sxs-lookup"><span data-stu-id="ebe04-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="ebe04-224">Certains développeurs préfèrent l’approche ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ebe04-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="ebe04-225">D’autres considèrent le balisage plus détaillé et le code HTML généré de l’approche ViewModel un inconvénient.</span><span class="sxs-lookup"><span data-stu-id="ebe04-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="ebe04-226">Dans cette section, nous avons appris trois approches pour utiliser le **DropDownList** avec les données de catégorie.</span><span class="sxs-lookup"><span data-stu-id="ebe04-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="ebe04-227">Dans la section suivante, nous allons montrer comment ajouter une nouvelle catégorie.</span><span class="sxs-lookup"><span data-stu-id="ebe04-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ebe04-228">[Précédent](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Suivant](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="ebe04-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
