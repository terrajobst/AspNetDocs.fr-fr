---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Ajout d’une nouvelle catégorie au DropDownList à l’aide de jQuery UI | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: cb9053593e2ea788638aec063c845cb91121861b
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075110"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="ae42f-102">Ajout d’une nouvelle catégorie au contrôle DropDownList avec l’interface utilisateur jQuery</span><span class="sxs-lookup"><span data-stu-id="ae42f-102">Adding a New Category to the DropDownList using jQuery UI</span></span>

<span data-ttu-id="ae42f-103">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="ae42f-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="ae42f-104">La balise de `Select` HTML est idéale pour présenter une liste de données de catégorie fixe, mais souvent, vous devez ajouter une nouvelle catégorie.</span><span class="sxs-lookup"><span data-stu-id="ae42f-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="ae42f-105">Supposons que nous voulons ajouter le genre « Opera » aux catégories de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="ae42f-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="ae42f-106">Dans cette section, nous allons utiliser jQuery UI pour ajouter une boîte de dialogue que vous pouvez utiliser pour ajouter une nouvelle catégorie.</span><span class="sxs-lookup"><span data-stu-id="ae42f-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="ae42f-107">L’image ci-dessous montre comment l’interface utilisateur sera présente dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ae42f-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="ae42f-108">Lorsqu’un utilisateur sélectionne le lien **Ajouter un nouveau genre** , une boîte de dialogue contextuelle invite l’utilisateur à entrer un nouveau nom de genre (et éventuellement une description).</span><span class="sxs-lookup"><span data-stu-id="ae42f-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="ae42f-109">L’image ci-dessous montre la boîte de dialogue contextuelle **Ajouter un genre** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="ae42f-110">Quand un nouveau nom de genre est entré et que le bouton d' **enregistrement** fait l’objet d’un push, les événements suivants se produisent :</span><span class="sxs-lookup"><span data-stu-id="ae42f-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="ae42f-111">Un appel AJAX publie les données dans la méthode Create du contrôleur de genre, qui enregistre le nouveau genre dans la base de données et renvoie les nouvelles informations de genre (nom de genre et ID) au format JSON.</span><span class="sxs-lookup"><span data-stu-id="ae42f-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="ae42f-112">JavaScript ajoute les nouvelles données de genre à la liste de sélection.</span><span class="sxs-lookup"><span data-stu-id="ae42f-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="ae42f-113">JavaScript fait du nouveau genre l’élément sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ae42f-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="ae42f-114">Dans l’image ci-dessous, **Opera** a été ajouté à la base de données et sélectionné dans la liste déroulante **genre** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="ae42f-115">Ouvrez le fichier *Views\StoreManager\Create.cshtml* et remplacez le balisage de genre par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ae42f-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="ae42f-116">La `_ChooseGenre` vue partielle contient toute la logique permettant de raccorder le JavaScript et jQuery utilisé pour implémenter la fonctionnalité Ajouter un nouveau genre.</span><span class="sxs-lookup"><span data-stu-id="ae42f-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="ae42f-117">Une fois le code terminé, il est simple de faire de même avec l’interface utilisateur de l’artiste.</span><span class="sxs-lookup"><span data-stu-id="ae42f-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="ae42f-118">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Views\StoreManager* et sélectionnez **Ajouter**, puis **Afficher**.</span><span class="sxs-lookup"><span data-stu-id="ae42f-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="ae42f-119">Dans l’entrée nom de la **vue** , entrez `_ChooseGenre` puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ae42f-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="ae42f-120">Remplacez le balisage dans le fichier *Views\StoreManager\\_ChooseGenre. cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ae42f-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="ae42f-121">La première ligne déclare que nous passons un `Album` en tant que notre modèle, exactement la même instruction de modèle trouvée dans la vue Create.</span><span class="sxs-lookup"><span data-stu-id="ae42f-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="ae42f-122">Les lignes suivantes sont le balisage d’assistance d' **étiquette** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="ae42f-123">La ligne suivante est l’appel d’assistance **DropDownList** , exactement le même que dans la vue Create d’origine.</span><span class="sxs-lookup"><span data-stu-id="ae42f-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="ae42f-124">La ligne suivante ajoute un lien avec le nom `Add New Genre`et des styles comme un bouton.</span><span class="sxs-lookup"><span data-stu-id="ae42f-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="ae42f-125">La ligne qui contient `ValidationMessageFor` est copiée directement à partir de la vue Create.</span><span class="sxs-lookup"><span data-stu-id="ae42f-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="ae42f-126">Les lignes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ae42f-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="ae42f-127">crée une balise div masquée, avec l’ID de `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="ae42f-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="ae42f-128">Nous allons utiliser jQuery pour raccorder la boîte de dialogue **Ajouter un genre** à l’ID `genreDialog` dans ce div.</span><span class="sxs-lookup"><span data-stu-id="ae42f-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="ae42f-129">Les deux dernières balises de script contiennent des liens vers les fichiers JavaScript que nous allons utiliser pour implémenter la fonctionnalité Ajouter un nouveau genre.</span><span class="sxs-lookup"><span data-stu-id="ae42f-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="ae42f-130">Le fichier */scripts/chooseGenre.js* est fourni pour vous dans le projet, nous l’examinerons plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ae42f-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="ae42f-131">Exécutez l’application et cliquez sur le bouton **Ajouter un nouveau genre** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="ae42f-132">Dans la boîte de dialogue **Ajouter un genre** , entrez **Opera** dans la zone de texte **nom** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="ae42f-133">Cliquez sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-133">Click the **Save** button.</span></span> <span data-ttu-id="ae42f-134">Un appel AJAX crée la catégorie Opera, puis remplit la liste déroulante avec Opera et définit Opera comme genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ae42f-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="ae42f-135">Entrez un artiste, un titre et un prix, puis sélectionnez le bouton **créer** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="ae42f-136">Si vous entrez un prix inférieur à $8,99, le nouvel album s’affiche en haut de la vue index.</span><span class="sxs-lookup"><span data-stu-id="ae42f-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="ae42f-137">Vérifiez que la nouvelle entrée d’album a été enregistrée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="ae42f-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="ae42f-138">Essayez de créer un nouveau genre avec une seule lettre.</span><span class="sxs-lookup"><span data-stu-id="ae42f-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="ae42f-139">Le code suivant dans le fichier *Models\Genre.cs* définit la longueur minimale et la longueur maximale du nom de genre.</span><span class="sxs-lookup"><span data-stu-id="ae42f-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="ae42f-140">Rapports de validation côté client vous devez entrer une chaîne de 2 à 20 caractères.</span><span class="sxs-lookup"><span data-stu-id="ae42f-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="ae42f-141">Examiner comment un nouveau genre est ajouté à la base de données et à la liste de sélection.</span><span class="sxs-lookup"><span data-stu-id="ae42f-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="ae42f-142">Ouvrez le fichier *Scripts\chooseGenre.js* et examinez le code.</span><span class="sxs-lookup"><span data-stu-id="ae42f-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="ae42f-143">La deuxième ligne utilise l’ID `genreDialog` pour créer une boîte de dialogue sur la balise div dans le fichier *Views\StoreManager\\_ChooseGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ae42f-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="ae42f-144">La plupart des paramètres nommés sont explicites.</span><span class="sxs-lookup"><span data-stu-id="ae42f-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="ae42f-145">Le paramètre `autoOpen` a la valeur false, le fait de sélectionner le bouton **créer un genre** ouvre la boîte de dialogue explicitement (cette dernière est décrite dans).</span><span class="sxs-lookup"><span data-stu-id="ae42f-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="ae42f-146">La boîte de dialogue contient deux boutons : **Enregistrer** et **Annuler**.</span><span class="sxs-lookup"><span data-stu-id="ae42f-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="ae42f-147">Le bouton **Annuler** ferme la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="ae42f-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="ae42f-148">Le code suivant illustre la fonction bouton d' **enregistrement** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="ae42f-149">Le `var createGenreForm` est sélectionné à partir de l’ID de `createGenreForm`.</span><span class="sxs-lookup"><span data-stu-id="ae42f-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="ae42f-150">L’ID de `createGenreForm` a été défini dans le code suivant qui se trouve dans le fichier *Views\Genre\\_CreateGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ae42f-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="ae42f-151">La surcharge du programme d’assistance [html. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) utilisée dans le fichier *Views\Genre\\_CreateGenre. cshtml* génère du code HTML avec un attribut action contenant l’URL à laquelle envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="ae42f-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="ae42f-152">Pour le voir, vous pouvez afficher la page créer un album dans un navigateur et sélectionner Afficher la source dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ae42f-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="ae42f-153">Le balisage suivant montre le code HTML généré contenant la balise form.</span><span class="sxs-lookup"><span data-stu-id="ae42f-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="ae42f-154">La ligne de `$.post` jQuery effectue un appel AJAX à l’attribut action (`/StoreManager/Create`) et transmet les données à partir de la boîte de dialogue **créer un genre** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="ae42f-155">Les données se composent du nom du nouveau genre et d’une description facultative.</span><span class="sxs-lookup"><span data-stu-id="ae42f-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="ae42f-156">Si l’appel AJAX réussit, le nouveau nom de genre et la nouvelle valeur sont ajoutés à la balise Select, et le nouveau genre est défini sur la valeur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="ae42f-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="ae42f-157">Étant donné qu’il s’agit d’un balisage généré dynamiquement, vous ne pouvez pas voir la nouvelle option Select en affichant la source dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ae42f-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="ae42f-158">Vous pouvez voir le nouveau code HTML avec les outils de développement Internet Explorer 9 F12.</span><span class="sxs-lookup"><span data-stu-id="ae42f-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="ae42f-159">Pour afficher la nouvelle option Sélectionner, dans Internet Explorer 9, appuyez sur la touche F12 pour démarrer les outils de développement F12.</span><span class="sxs-lookup"><span data-stu-id="ae42f-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="ae42f-160">Accédez à la page créer et ajoutez un nouveau genre afin que le nouveau genre soit sélectionné dans la liste de sélection genre.</span><span class="sxs-lookup"><span data-stu-id="ae42f-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="ae42f-161">Dans les outils de développement F12 :</span><span class="sxs-lookup"><span data-stu-id="ae42f-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="ae42f-162">Sélectionnez l’onglet HTML.</span><span class="sxs-lookup"><span data-stu-id="ae42f-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="ae42f-163">Appuyez sur l’icône d’actualisation.</span><span class="sxs-lookup"><span data-stu-id="ae42f-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="ae42f-164">Dans la zone de recherche, entrez GenreID.</span><span class="sxs-lookup"><span data-stu-id="ae42f-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="ae42f-165">À l’aide de l’icône suivante,</span><span class="sxs-lookup"><span data-stu-id="ae42f-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="ae42f-166">Accédez à la balise SELECT suivante :</span><span class="sxs-lookup"><span data-stu-id="ae42f-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="ae42f-167">Développez la valeur de la dernière option.</span><span class="sxs-lookup"><span data-stu-id="ae42f-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="ae42f-168">Le code suivant dans le fichier *Scripts\chooseGenre.js* montre comment le bouton **Ajouter un nouveau genre** est connecté à l’événement Click et comment la boîte de dialogue **Ajouter un nouveau genre** est créée.</span><span class="sxs-lookup"><span data-stu-id="ae42f-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="ae42f-169">La première ligne crée une fonction Click attachée au bouton **Ajouter un nouveau genre** .</span><span class="sxs-lookup"><span data-stu-id="ae42f-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="ae42f-170">Le balisage suivant à partir du fichier Views\StoreManager\\_ChooseGenre. cshtml montre comment le bouton **Ajouter un nouveau genre** est créé :</span><span class="sxs-lookup"><span data-stu-id="ae42f-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="ae42f-171">La méthode Load crée et ouvre la boîte de dialogue Ajouter un genre et appelle la méthode jQuery `parse` pour que la validation du client se produise sur les données entrées dans la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="ae42f-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="ae42f-172">Dans cette section, vous avez appris à créer une boîte de dialogue qui peut être utilisée pour ajouter de nouvelles données de catégorie à une liste de sélection.</span><span class="sxs-lookup"><span data-stu-id="ae42f-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="ae42f-173">Vous pouvez suivre la même procédure pour créer une interface utilisateur afin d’ajouter un nouvel artiste à la liste de sélection de l’artiste.</span><span class="sxs-lookup"><span data-stu-id="ae42f-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="ae42f-174">Ce didacticiel a donné une vue d’ensemble de l’utilisation du **DropDownList**helper HTML ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ae42f-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="ae42f-175">Pour plus d’informations sur l’utilisation du **DropDownList**, consultez la section Références supplémentaires ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ae42f-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="ae42f-176">Faites-nous savoir si ce didacticiel vous a été utile.</span><span class="sxs-lookup"><span data-stu-id="ae42f-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="ae42f-177">Rick. Anderson [at] Microsoft. com</span><span class="sxs-lookup"><span data-stu-id="ae42f-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="ae42f-178">Références supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ae42f-178">Additional References</span></span>

- <span data-ttu-id="ae42f-179">[ASP.NET MVC – didacticiel sur les listes déroulantes en cascade](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) par [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="ae42f-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="ae42f-180">[Choisi](https://harvesthq.github.com/chosen/) Un plug-in JavaScript qui prend en charge la sélection multiple et le filtrage.</span><span class="sxs-lookup"><span data-stu-id="ae42f-180">[Chosen](https://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="ae42f-181">Contributeurs</span><span class="sxs-lookup"><span data-stu-id="ae42f-181">Contributors</span></span>

- [<span data-ttu-id="ae42f-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="ae42f-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="ae42f-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="ae42f-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="ae42f-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="ae42f-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="ae42f-185">Réviseurs</span><span class="sxs-lookup"><span data-stu-id="ae42f-185">Reviewers</span></span>

- <span data-ttu-id="ae42f-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="ae42f-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="ae42f-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="ae42f-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="ae42f-188">Mike pape</span><span class="sxs-lookup"><span data-stu-id="ae42f-188">Mike Pope</span></span>
- <span data-ttu-id="ae42f-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="ae42f-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ae42f-190">Précédent</span><span class="sxs-lookup"><span data-stu-id="ae42f-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
