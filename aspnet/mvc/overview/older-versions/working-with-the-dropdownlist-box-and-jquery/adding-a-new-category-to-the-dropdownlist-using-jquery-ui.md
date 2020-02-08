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
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Ajout d’une nouvelle catégorie au contrôle DropDownList avec l’interface utilisateur jQuery

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

La balise de `Select` HTML est idéale pour présenter une liste de données de catégorie fixe, mais souvent, vous devez ajouter une nouvelle catégorie. Supposons que nous voulons ajouter le genre « Opera » aux catégories de notre base de données. Dans cette section, nous allons utiliser jQuery UI pour ajouter une boîte de dialogue que vous pouvez utiliser pour ajouter une nouvelle catégorie. L’image ci-dessous montre comment l’interface utilisateur sera présente dans le navigateur.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Lorsqu’un utilisateur sélectionne le lien **Ajouter un nouveau genre** , une boîte de dialogue contextuelle invite l’utilisateur à entrer un nouveau nom de genre (et éventuellement une description). L’image ci-dessous montre la boîte de dialogue contextuelle **Ajouter un genre** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quand un nouveau nom de genre est entré et que le bouton d' **enregistrement** fait l’objet d’un push, les événements suivants se produisent :

1. Un appel AJAX publie les données dans la méthode Create du contrôleur de genre, qui enregistre le nouveau genre dans la base de données et renvoie les nouvelles informations de genre (nom de genre et ID) au format JSON.
2. JavaScript ajoute les nouvelles données de genre à la liste de sélection.
3. JavaScript fait du nouveau genre l’élément sélectionné.

   Dans l’image ci-dessous, **Opera** a été ajouté à la base de données et sélectionné dans la liste déroulante **genre** . 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Ouvrez le fichier *Views\StoreManager\Create.cshtml* et remplacez le balisage de genre par le code suivant :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

La `_ChooseGenre` vue partielle contient toute la logique permettant de raccorder le JavaScript et jQuery utilisé pour implémenter la fonctionnalité Ajouter un nouveau genre. Une fois le code terminé, il est simple de faire de même avec l’interface utilisateur de l’artiste.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Views\StoreManager* et sélectionnez **Ajouter**, puis **Afficher**. Dans l’entrée nom de la **vue** , entrez `_ChooseGenre` puis sélectionnez **Ajouter**. Remplacez le balisage dans le fichier *Views\StoreManager\\_ChooseGenre. cshtml* par le code suivant :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La première ligne déclare que nous passons un `Album` en tant que notre modèle, exactement la même instruction de modèle trouvée dans la vue Create. Les lignes suivantes sont le balisage d’assistance d' **étiquette** . La ligne suivante est l’appel d’assistance **DropDownList** , exactement le même que dans la vue Create d’origine. La ligne suivante ajoute un lien avec le nom `Add New Genre`et des styles comme un bouton. La ligne qui contient `ValidationMessageFor` est copiée directement à partir de la vue Create. Les lignes suivantes :

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

crée une balise div masquée, avec l’ID de `genreDialog`. Nous allons utiliser jQuery pour raccorder la boîte de dialogue **Ajouter un genre** à l’ID `genreDialog` dans ce div. Les deux dernières balises de script contiennent des liens vers les fichiers JavaScript que nous allons utiliser pour implémenter la fonctionnalité Ajouter un nouveau genre. Le fichier */scripts/chooseGenre.js* est fourni pour vous dans le projet, nous l’examinerons plus loin dans le didacticiel.

Exécutez l’application et cliquez sur le bouton **Ajouter un nouveau genre** . Dans la boîte de dialogue **Ajouter un genre** , entrez **Opera** dans la zone de texte **nom** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Cliquez sur le bouton **Enregistrer** . Un appel AJAX crée la catégorie Opera, puis remplit la liste déroulante avec Opera et définit Opera comme genre sélectionné.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Entrez un artiste, un titre et un prix, puis sélectionnez le bouton **créer** . Si vous entrez un prix inférieur à $8,99, le nouvel album s’affiche en haut de la vue index. Vérifiez que la nouvelle entrée d’album a été enregistrée dans la base de données.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Essayez de créer un nouveau genre avec une seule lettre. Le code suivant dans le fichier *Models\Genre.cs* définit la longueur minimale et la longueur maximale du nom de genre.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Rapports de validation côté client vous devez entrer une chaîne de 2 à 20 caractères.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examiner comment un nouveau genre est ajouté à la base de données et à la liste de sélection.

Ouvrez le fichier *Scripts\chooseGenre.js* et examinez le code.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La deuxième ligne utilise l’ID `genreDialog` pour créer une boîte de dialogue sur la balise div dans le fichier *Views\StoreManager\\_ChooseGenre. cshtml* . La plupart des paramètres nommés sont explicites. Le paramètre `autoOpen` a la valeur false, le fait de sélectionner le bouton **créer un genre** ouvre la boîte de dialogue explicitement (cette dernière est décrite dans). La boîte de dialogue contient deux boutons : **Enregistrer** et **Annuler**. Le bouton **Annuler** ferme la boîte de dialogue. Le code suivant illustre la fonction bouton d' **enregistrement** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Le `var createGenreForm` est sélectionné à partir de l’ID de `createGenreForm`. L’ID de `createGenreForm` a été défini dans le code suivant qui se trouve dans le fichier *Views\Genre\\_CreateGenre. cshtml* .

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

La surcharge du programme d’assistance [html. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) utilisée dans le fichier *Views\Genre\\_CreateGenre. cshtml* génère du code HTML avec un attribut action contenant l’URL à laquelle envoyer le formulaire. Pour le voir, vous pouvez afficher la page créer un album dans un navigateur et sélectionner Afficher la source dans le navigateur. Le balisage suivant montre le code HTML généré contenant la balise form.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

La ligne de `$.post` jQuery effectue un appel AJAX à l’attribut action (`/StoreManager/Create`) et transmet les données à partir de la boîte de dialogue **créer un genre** . Les données se composent du nom du nouveau genre et d’une description facultative. Si l’appel AJAX réussit, le nouveau nom de genre et la nouvelle valeur sont ajoutés à la balise Select, et le nouveau genre est défini sur la valeur sélectionnée. Étant donné qu’il s’agit d’un balisage généré dynamiquement, vous ne pouvez pas voir la nouvelle option Select en affichant la source dans le navigateur. Vous pouvez voir le nouveau code HTML avec les outils de développement Internet Explorer 9 F12. Pour afficher la nouvelle option Sélectionner, dans Internet Explorer 9, appuyez sur la touche F12 pour démarrer les outils de développement F12. Accédez à la page créer et ajoutez un nouveau genre afin que le nouveau genre soit sélectionné dans la liste de sélection genre. Dans les outils de développement F12 :

1. Sélectionnez l’onglet HTML.
2. Appuyez sur l’icône d’actualisation.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Dans la zone de recherche, entrez GenreID.
4. À l’aide de l’icône suivante,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Accédez à la balise SELECT suivante :

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Développez la valeur de la dernière option.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Le code suivant dans le fichier *Scripts\chooseGenre.js* montre comment le bouton **Ajouter un nouveau genre** est connecté à l’événement Click et comment la boîte de dialogue **Ajouter un nouveau genre** est créée.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La première ligne crée une fonction Click attachée au bouton **Ajouter un nouveau genre** . Le balisage suivant à partir du fichier Views\StoreManager\\_ChooseGenre. cshtml montre comment le bouton **Ajouter un nouveau genre** est créé :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

La méthode Load crée et ouvre la boîte de dialogue Ajouter un genre et appelle la méthode jQuery `parse` pour que la validation du client se produise sur les données entrées dans la boîte de dialogue.

Dans cette section, vous avez appris à créer une boîte de dialogue qui peut être utilisée pour ajouter de nouvelles données de catégorie à une liste de sélection. Vous pouvez suivre la même procédure pour créer une interface utilisateur afin d’ajouter un nouvel artiste à la liste de sélection de l’artiste. Ce didacticiel a donné une vue d’ensemble de l’utilisation du **DropDownList**helper HTML ASP.NET MVC. Pour plus d’informations sur l’utilisation du **DropDownList**, consultez la section Références supplémentaires ci-dessous. Faites-nous savoir si ce didacticiel vous a été utile.

Rick. Anderson [at] Microsoft. com

### <a name="additional-references"></a>Références supplémentaires

- [ASP.NET MVC – didacticiel sur les listes déroulantes en cascade](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) par [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Choisi](https://harvesthq.github.com/chosen/) Un plug-in JavaScript qui prend en charge la sélection multiple et le filtrage.

### <a name="contributors"></a>Contributeurs

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Réviseurs

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike pape
- Tom Dykstra

> [!div class="step-by-step"]
> [Précédent](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
