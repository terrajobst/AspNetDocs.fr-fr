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
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examen de la façon dont ASP.NET MVC génère un modèle automatique du helper DropDownList

par [Rick Anderson](https://twitter.com/RickAndMSFT)

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Controllers* , puis sélectionnez **Ajouter un contrôleur**. Nommez le contrôleur **StoreManagerController**. Définissez les options de la boîte de dialogue **Ajouter un contrôleur** comme indiqué dans l’image ci-dessous.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Modifiez la vue *StoreManager\Index.cshtml* et supprimez `AlbumArtUrl`. Si vous supprimez `AlbumArtUrl`, la présentation sera plus lisible. Le code terminé est indiqué ci-dessous.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Ouvrez le fichier *Controllers\StoreManagerController.cs* et recherchez la méthode `Index`. Ajoutez la clause `OrderBy` pour que les albums soient triés par prix. Le code complet est présenté ci-dessous.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Le tri par prix facilite le test des modifications apportées à la base de données. Lorsque vous testez les méthodes de modification et de création, vous pouvez utiliser un prix faible afin que les données enregistrées s’affichent en premier.

Ouvrez le fichier *StoreManager\Edit.cshtml* . Ajoutez la ligne suivante juste après la balise de légende.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Le code suivant montre le contexte de cette modification :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Le `AlbumId` est requis pour apporter des modifications à un enregistrement d’album.

Appuyez sur Ctrl+F5 pour exécuter l’application. Sélectionnez le lien **administrateur** , puis cliquez sur le lien **créer** pour créer un nouvel album. Vérifiez que les informations de l’album ont été enregistrées. Modifiez un album et vérifiez que les modifications que vous avez apportées sont conservées.

### <a name="the-album-schema"></a>Schéma de l’album

Le contrôleur de `StoreManager` créé par le mécanisme de génération de modèles automatique MVC autorise l’accès à CRUD (création, lecture, mise à jour, suppression) aux albums dans la base de données du magasin musical. Le schéma des informations relatives aux albums est illustré ci-dessous :

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

La table `Albums` ne stocke pas le genre et la description de l’album. elle stocke une clé étrangère dans la table `Genres`. La table `Genres` contient le nom et la description du genre. De même, la table `Albums` ne contient pas le nom des artistes d’albums, mais une clé étrangère de la table `Artists`. La table `Artists` contient le nom de l’artiste. Si vous examinez les données de la table `Albums`, vous pouvez voir que chaque ligne contient une clé étrangère vers la table `Genres` et une clé étrangère vers la table `Artists`. L’image ci-dessous montre certaines données de table de la table `Albums`.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Balise de sélection HTML

L’élément `<select>` HTML (créé par le programme d’assistance [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) html) permet d’afficher une liste complète des valeurs (par exemple, la liste des genres). Pour les formulaires de modification, lorsque la valeur actuelle est connue, la liste de sélection peut afficher la valeur actuelle. Nous l’avons vu précédemment lorsque nous avons défini la valeur sélectionnée sur **comédie**. La liste de sélection est idéale pour l’affichage des données de catégorie ou de clé étrangère. L’élément `<select>` pour la clé étrangère genre affiche la liste des noms de genres possibles, mais lorsque vous enregistrez le formulaire, la propriété genre est mise à jour avec la valeur de clé étrangère genre, et non pas le nom de genre affiché. Dans l’image ci-dessous, le genre sélectionné est **Disco** et l’artiste est **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examen du code ASP.NET MVC

Ouvrez le fichier *Controllers\StoreManagerController.cs* et recherchez la méthode `HTTP GET Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

La méthode `Create` ajoute deux objets [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) à l' `ViewBag`, l’un pour contenir les informations de genre et l’autre pour contenir les informations sur l’artiste. La surcharge du constructeur [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) utilisée ci-dessus accepte trois arguments :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *Items*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenant les éléments de la liste. Dans l’exemple ci-dessus, la liste des genres retournés par `db.Genres`.
2. *dataValueField*: nom de la propriété dans la liste **IEnumerable** qui contient la valeur de clé. Dans l’exemple ci-dessus, `GenreId` et `ArtistId`.
3. *dataTextField*: nom de la propriété dans la liste **IEnumerable** qui contient les informations à afficher. Dans la table Artists et genre, le champ `name` est utilisé.

Ouvrez le fichier *Views\StoreManager\Create.cshtml* et examinez le balisage d’assistance `Html.DropDownList` pour le champ genre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La première ligne indique que la vue Create prend un modèle de `Album`. Dans la méthode `Create` présentée ci-dessus, aucun modèle n’a été passé, de sorte que la vue obtient un modèle de `Album` **null** . À ce stade, nous créons un nouvel album afin que nous n’ayons pas de données `Album` pour celui-ci.

La surcharge [html. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) présentée ci-dessus prend le nom du champ à lier au modèle. Il utilise également ce nom pour rechercher un objet **ViewBag** contenant un objet [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) . À l’aide de cette surcharge, vous devez nommer l’objet **ViewBag SelectList** `GenreId`. Le deuxième paramètre (`String.Empty`) est le texte à afficher quand aucun élément n’est sélectionné. C’est exactement ce que nous voulons lors de la création d’un album. Si vous avez supprimé le deuxième paramètre et utilisé le code suivant :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

La liste de sélection est définie par défaut sur le premier élément, ou Rock dans notre exemple.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examen de la méthode `HTTP POST Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Cette surcharge de la méthode `Create` prend un objet `album`, créé par le système de liaison de modèle MVC ASP.NET à partir des valeurs de formulaire publiées. Lorsque vous soumettez un nouvel album, si l’état du modèle est valide et qu’il n’y a aucune erreur de base de données, le nouvel album est ajouté à la base de données. L’illustration suivante montre la création d’un nouvel album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Vous pouvez utiliser l' [outil Fiddler](http://www.fiddler2.com/fiddler2/) pour examiner les valeurs de formulaire publiées utilisées par la liaison de modèle ASP.NET MVC pour créer l’objet album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactorisation de la création de SelectList ViewBag

Les méthodes `Edit` et la méthode `HTTP POST Create` ont le même code pour configurer **SelectList** dans **ViewBag**. Dans l’esprit de [Dry](http://en.wikipedia.org/wiki/Don't_repeat_yourself), nous refactorisons ce code. Nous utiliserons ce code refactorisé ultérieurement.

Créez une nouvelle méthode pour ajouter un genre et un artiste **SelectList** à **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Remplacez les deux lignes qui déplacent les `ViewBag` dans chacune des méthodes `Create` et `Edit` par un appel à la méthode `SetGenreArtistViewBag`. Le code terminé est indiqué ci-dessous.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Créez un nouvel album et modifiez un album pour vérifier le travail des modifications.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Passage explicite de SelectList à DropDownList

Les vues Create et Edit créées par la génération de modèles automatique ASP.NET MVC utilisent la surcharge **DropDownList** suivante :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Le balisage `DropDownList` pour la vue Create est illustré ci-dessous.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Étant donné que la propriété `ViewBag` du `SelectList` est nommée `GenreId`, l’application auxiliaire **DropDownList** utilise le `GenreId`**SelectList** dans **ViewBag**. Dans la surcharge **DropDownList** suivante, le `SelectList` est passé explicitement.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Ouvrez le fichier *Views\StoreManager\Edit.cshtml* et modifiez l’appel **DropDownList** pour passer explicitement le **SelectList**, à l’aide de la surcharge ci-dessus. Pour la catégorie genre, procédez comme suit. Le code complet est indiqué ci-dessous :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Exécutez l’application, cliquez sur le lien **administrateur** , puis accédez à un album jazz et sélectionnez le lien **modifier** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Au lieu d’afficher jazz comme genre actuellement sélectionné, le rock est affiché. Lorsque l’argument de chaîne (la propriété à lier) et l’objet **SelectList** ont le même nom, la valeur sélectionnée n’est pas utilisée. Lorsqu’il n’y a aucune valeur sélectionnée, Browser prend par défaut le premier élément du **SelectList**(qui est le **Rock** dans l’exemple ci-dessus). Il s’agit d’une limitation connue de l’application auxiliaire **DropDownList** .

Ouvrez le fichier *Controllers\StoreManagerController.cs* et modifiez les noms des objets **SelectList** en `Genres` et `Artists`. Le code complet est indiqué ci-dessous :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Les noms et les artistes sont de meilleurs noms pour les catégories, car ils contiennent plus que l’ID de chaque catégorie. La refactorisation que nous avons fait précédemment a été annulée. Au lieu de modifier le **ViewBag** en quatre méthodes, nos modifications étaient isolées dans la méthode `SetGenreArtistViewBag`.

Modifiez l’appel **DropDownList** dans les affichages créer et modifier pour utiliser les nouveaux noms **SelectList** . Le nouveau balisage de la vue Edit (édition) est illustré ci-dessous :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

La vue Create requiert une chaîne vide pour empêcher l’affichage du premier élément du SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Créez un nouvel album et modifiez un album pour vérifier le travail des modifications. Testez le code de modification en sélectionnant un album avec un genre autre que Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Utilisation d’un modèle de vue avec le programme d’assistance DropDownList

Créez une nouvelle classe dans le dossier ViewModels nommé `AlbumSelectListViewModel`. Remplacez le code de la classe `AlbumSelectListViewModel` par ce qui suit :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Le constructeur `AlbumSelectListViewModel` prend un album, une liste d’artistes et de genres, et crée un objet contenant l’album et un `SelectList` pour les genres et les artistes.

Générez le projet afin que le `AlbumSelectListViewModel` soit disponible lorsque nous créons une vue à l’étape suivante.

Ajoutez une méthode `EditVM` à la `StoreManagerController`. Le code terminé est indiqué ci-dessous.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Cliquez avec le bouton droit sur `AlbumSelectListViewModel`, sélectionnez **résoudre**, puis **Utilisez MvcMusicStore. ViewModels ;** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Vous pouvez également ajouter l’instruction using suivante :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Cliquez avec le bouton droit sur `EditVM`, puis sélectionnez **Ajouter une vue**. Utilisez les options indiquées ci-dessous.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Sélectionnez **Ajouter**, puis remplacez le contenu du fichier *Views\StoreManager\EditVM.cshtml* par le code suivant :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Le balisage `EditVM` est très similaire au balisage `Edit` d’origine, avec les exceptions suivantes.

- Les propriétés de modèle de la vue `Edit` se présentent sous la forme `model.property`(par exemple, `model.Title`). Les propriétés de modèle de la vue `EditVm` se présentent sous la forme `model.Album.property`(par exemple, `model.Album.Title`). Cela est dû au fait que la vue `EditVM` reçoit un conteneur pour un `Album`, et non un `Album` comme dans la vue `Edit`.
- Le deuxième paramètre **DropDownList** provient du modèle de vue, et non de **ViewBag**.
- L’application auxiliaire **BeginForm** dans la vue `EditVM` est publiée explicitement dans la méthode d’action `Edit`. En republiant sur l’action `Edit`, nous n’avons pas besoin d’écrire de `HTTP POST EditVM` action et de réutiliser le `HTTP POST` `Edit` action.

Exécutez l’application et modifiez un album. Modifiez l’URL pour utiliser `EditVM`. Modifiez un champ et cliquez sur le bouton **Enregistrer** pour vérifier que le code fonctionne.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Quelle approche devez-vous utiliser ?

Les trois approches affichées sont acceptables. De nombreux développeurs préfèrent passer explicitement le `SelectList` au `DropDownList` à l’aide de l' `ViewBag`. Cette approche présente l’avantage supplémentaire de vous donner la possibilité d’utiliser un nom plus approprié pour la collection. Le seul inconvénient est que vous ne pouvez pas nommer l’objet `ViewBag SelectList` le même nom que la propriété de modèle.

Certains développeurs préfèrent l’approche ViewModel. D’autres considèrent le balisage plus détaillé et le code HTML généré de l’approche ViewModel un inconvénient.

Dans cette section, nous avons appris trois approches pour utiliser le **DropDownList** avec les données de catégorie. Dans la section suivante, nous allons montrer comment ajouter une nouvelle catégorie.

> [!div class="step-by-step"]
> [Précédent](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Suivant](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
