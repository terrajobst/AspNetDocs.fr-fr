---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Utilisation de l’application auxiliaire DropDownList avec ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6375bb2be158cea18309ffa71c71ac3e67bc91ed
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457867"
---
# <a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Utilisation du helper DropDownList avec ASP.NET MVC

par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel vous apprend les bases de l’utilisation de l’application auxiliaire [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) et du programme d’assistance [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) dans une application Web ASP.NET MVC. Vous pouvez utiliser Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio pour suivre le didacticiel. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :

- [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)

Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Ce didacticiel part du principe que vous avez terminé le didacticiel [d’introduction à ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou le didacticiel du[magasin de musique ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) ou que vous connaissez le développement ASP.NET MVC. Ce didacticiel commence par un projet modifié du didacticiel [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) . Vous pouvez télécharger le projet de démarrage avec le lien suivant pour [Télécharger la C# version](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Un projet Visual Web Developer avec le code C# source du didacticiel terminé est disponible pour accompagner cette rubrique. [Téléchargez](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Contenu

Vous allez créer des méthodes d’action et des vues qui utilisent l’application auxiliaire [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pour sélectionner une catégorie. Vous allez également utiliser **jQuery** pour ajouter une boîte de dialogue Insérer une catégorie qui peut être utilisée lorsqu’une nouvelle catégorie (telle que genre ou artiste) est nécessaire. Vous trouverez ci-dessous une capture d’écran de la vue créer montrant des liens permettant d’ajouter un nouveau genre et d’ajouter un nouvel artiste.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Compétences

Vous apprendrez les compétences suivantes :

- Comment utiliser le programme d’assistance [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pour sélectionner des données de catégorie.
- Comment ajouter une boîte de dialogue **jQuery** pour ajouter de nouvelles catégories.

### <a name="getting-started"></a>Mise en route

Commencez par Télécharger le projet de démarrage avec le lien suivant, [Télécharger](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). Dans l’Explorateur Windows, cliquez avec le bouton droit sur le fichier *DDL\_Starter. zip* , puis sélectionnez Propriétés. Dans la boîte de dialogue **Propriétés DDL\_Starter. zip** , sélectionnez débloquer.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Cliquez avec le bouton droit sur le fichier DDL\_Starter. zip, puis sélectionnez **extraire tout** pour décompresser le fichier. Ouvrez le fichier *StartMusicStore. sln* avec Visual Web developer 2010 Express (« Visual Web Developer » ou « VWD existant » pour Short) ou visual studio 2010.

Appuyez sur CTRL + F5 pour exécuter l’application et cliquez sur le lien **test** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Sélectionnez le lien **Sélectionner une catégorie de films (simple)** . Une liste de sélection de type de film s’affiche, avec comédie la valeur sélectionnée.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Cliquez avec le bouton droit dans le navigateur, puis sélectionnez Afficher la source. Le code HTML de la page s’affiche. Le code ci-dessous montre le code HTML de l’élément SELECT.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Vous pouvez voir que chaque élément de la liste de sélection a une valeur (0 pour action, 1 pour la fiction, 2 pour comédie et 3 pour science-fiction) et un nom d’affichage (action, fiction, comédie et science-fiction). Le code ci-dessus est du code HTML standard pour une liste de sélection.

Modifiez la liste de sélection en « fiction », puis appuyez sur le bouton **Envoyer** . L’URL dans le navigateur est `http://localhost:2468/Home/CategoryChosen?MovieType=1` et la page affiche la **sélection : 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Ouvrez le fichier *Controllers\HomeController.cs* et examinez la méthode `SelectCategory`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

L’application auxiliaire [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) utilisée pour créer une liste de sélection html requiert un **IEnumerable&lt;SelectListItem &gt;** , explicitement ou implicitement. Autrement dit, vous pouvez passer le **SelectListItem ienumerable&lt;&gt;** explicitement à l’application auxiliaire [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) ou vous pouvez ajouter le **&gt;IEnumerable&lt;SelectListItem** à [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) en utilisant le même nom pour le **SelectListItem** que la propriété de modèle. Le passage de **SelectListItem** implicitement et explicitement est abordé dans la partie suivante du didacticiel. Le code ci-dessus montre la manière la plus simple possible de créer un **IEnumerable&lt;SelectListItem &gt;** et de le remplir avec du texte et des valeurs. Notez que la propriété [sélectionnée](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) de la `Comedy`[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) a la valeur **true ;** la liste de sélection rendue affiche alors **comédie** en tant qu’élément sélectionné dans la liste.

Le **SelectListItem IEnumerable&lt;&gt;** créé ci-dessus est ajouté au [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) avec le nom MovieType. C’est ainsi que nous passons l' **IEnumerable&lt;SelectListItem &gt;** implicitement à l’application auxiliaire [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) indiquée ci-dessous.

Ouvrez le fichier *Views\Home\SelectCategory.cshtml* et examinez le balisage.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Sur la troisième ligne, nous définissons la disposition sur Views/Shared/\_simple\_Layout. cshtml, qui est une version simplifiée du fichier de disposition standard. Nous faisons cela pour simplifier l’affichage et le rendu du code HTML.

Dans cet exemple, nous ne modifions pas l’état de l’application. nous allons donc envoyer les données à l’aide d’une requête **http**et non **http**. Consultez la [liste de vérification rapide de la section W3C pour choisir http : obtenir ou poster](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Comme nous ne modifions pas l’application et en publiant le formulaire, nous utilisons la surcharge [html. BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) qui nous permet de spécifier la méthode d’action, le contrôleur et la méthode de formulaire (**http post** ou **http**). En général, les vues contiennent la surcharge [html. BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) qui ne prend aucun paramètre. La version de paramètre no est par défaut la publication des données de formulaire dans la version POST de la même méthode d’action et du même contrôleur.

La ligne suivante

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passe un argument de chaîne à l’application auxiliaire **DropDownList** . Cette chaîne, « MovieType » dans notre exemple, fait deux choses :

- Il fournit la clé pour que le programme d’assistance **DropDownList** recherche un **IEnumerable&lt;SelectListItem &gt;** dans **ViewBag**.
- Elle est liée aux données de l’élément de formulaire MovieType. Si la méthode Submit est **http obtenir**, `MovieType` est une chaîne de requête. Si la méthode Submit est **http postal**, `MovieType` sera ajouté dans le corps du message. L’illustration suivante montre la chaîne de requête avec la valeur 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Le code suivant illustre la méthode `CategoryChosen` à laquelle le formulaire a été envoyé.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Revenez à la page test et sélectionnez le lien **SelectList html** . La page HTML affiche un élément Select similaire à la page de test ASP.NET MVC simple. Cliquez avec le bouton droit sur la fenêtre du navigateur, puis sélectionnez **afficher la source**. Le balisage HTML de la liste de sélection est fondamentalement identique. Testez la page HTML, elle fonctionne comme la méthode d’action MVC ASP.NET et la vue que nous avons testée précédemment.

### <a name="improving-the-movie-select-list-with-enums"></a>Amélioration de la liste de sélection de films avec des énumérations

Si les catégories de votre application sont fixes et ne changent pas, vous pouvez tirer parti des enums pour rendre votre code plus robuste et plus simple à étendre. Lorsque vous ajoutez une nouvelle catégorie, la valeur de catégorie correcte est générée. Le évite les erreurs de copie et de collage lorsque vous ajoutez une nouvelle catégorie, mais que vous oubliez de mettre à jour la valeur de la catégorie.

Ouvrez le fichier *Controllers\HomeController.cs* et examinez le code suivant :

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

L' [énumération](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` capture les quatre types de films. La méthode `SetViewBagMovieType` crée le **&gt;IEnumerable&lt;SelectListItem** à partir de l' **énumération**`eMovieCategories`et définit la propriété `Selected` à partir du paramètre `selectedMovie`. La méthode d’action `SelectCategoryEnum` utilise la même vue que la méthode d’action `SelectCategory`.

Accédez à la page test, puis cliquez sur le lien `Select Movie Category (Enum)`. Cette fois, au lieu d’afficher une valeur (nombre), une chaîne représentant l’énumération est affichée.

### <a name="posting-enum-values"></a>Publication des valeurs enum

Les formulaires HTML sont généralement utilisés pour la publication de données sur le serveur. Le code suivant illustre les versions `HTTP GET` et `HTTP POST` de la méthode `SelectCategoryEnumPost`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

En passant une `eMovieCategories` enum à la méthode `POST`, nous pouvons extraire à la fois la valeur enum et la chaîne Enum. Exécutez l’exemple et accédez à la page de test. Cliquez sur le lien `Select Movie Category(Enum Post)`. Sélectionnez un type de film, puis appuyez sur le bouton Envoyer. L’affichage montre à la fois la valeur et le nom du type de film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Création d’un élément Select à plusieurs sections

Le programme d’assistance HTML [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) restitue l’élément HTML `<select>` avec l’attribut `multiple`, qui permet aux utilisateurs d’effectuer plusieurs sélections. Accédez au lien test, puis sélectionnez le lien **MultiSelect Country** . L’interface utilisateur rendue vous permet de sélectionner plusieurs pays. Dans l’image ci-dessous, le Canada et la Chine sont sélectionnés.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examen du code MultiSelectCountry

Examinez le code suivant à partir du fichier *Controllers\HomeController.cs* .

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

La méthode `GetCountries` crée une liste de pays, puis la passe au constructeur `MultiSelectList`. La surcharge du constructeur `MultiSelectList` utilisée dans la méthode `GetCountries` ci-dessus accepte quatre paramètres :

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *Items*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenant les éléments de la liste. Dans l’exemple ci-dessus, la liste des pays.
2. *dataValueField*: nom de la propriété dans la liste **IEnumerable** qui contient la valeur. Dans l’exemple ci-dessus, la propriété `ID`.
3. *dataTextField*: nom de la propriété dans la liste **IEnumerable** qui contient les informations à afficher. Dans l’exemple ci-dessus, la propriété `name`.
4. *selectedValues*: liste des valeurs sélectionnées.

Dans l’exemple ci-dessus, la méthode `MultiSelectCountry` passe une valeur `null` pour les pays sélectionnés, si bien qu’aucun pays n’est sélectionné lors de l’affichage de l’interface utilisateur. Le code suivant montre le balisage Razor utilisé pour restituer la vue `MultiSelectCountry`.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

La méthode [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) du programme d’assistance HTML utilisée ci-dessus prend deux paramètres, le nom de la propriété pour modeler la liaison et le [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) contenant les options et valeurs Select. Le code `ViewBag.YouSelected` ci-dessus est utilisé pour afficher les valeurs des pays que vous avez sélectionnés lors de l’envoi du formulaire. Examinez la surcharge HTTP Après la méthode `MultiSelectCountry`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

La propriété dynamique `ViewBag.YouSelected` contient les pays sélectionnés, obtenus pour l’entrée de `Countries` dans la collection de formulaires. Dans cette version, la méthode GetCountries est passée à une liste des pays sélectionnés. par conséquent, lorsque la vue `MultiSelectCountry` s’affiche, les pays sélectionnés sont sélectionnés dans l’interface utilisateur.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Rendre un élément Select convivial avec le plug-in jQuery choisi par la récolte

Le plug-in jQuery [choisi](https://harvesthq.github.com/chosen/) pour la récolte peut être ajouté à un élément HTML &lt;sélectionnez&gt; élément pour créer une interface utilisateur conviviale. Les images ci-dessous illustrent le plug-in jQuery [choisi](https://harvesthq.github.com/chosen/) par la vue `MultiSelectCountry`.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Dans les deux images ci-dessous, le **Canada** est sélectionné.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Dans l’image ci-dessus, le Canada est sélectionné et contient un **x** sur lequel vous pouvez cliquer pour supprimer la sélection. L’image ci-dessous indique le Canada, la Chine et le Japon sélectionnés.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Raccordement du plug-in jQuery choisi par la récolte

La section suivante est plus facile à suivre si vous avez une certaine expérience avec jQuery. Si vous n’avez jamais utilisé jQuery auparavant, vous souhaiterez peut-être essayer l’un des didacticiels jQuery suivants.

- [Fonctionnement de jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) par [John Resig](http://ejohn.org/)
- [Prise en main avec jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) par [Jörn Zaefferer](http://bassistance.de/)
- [Exemples en direct de jQuery](http://codylindley.com/blogstuff/js/jquery/#) par [Cody Lindley](http://codylindley.com/)

Le plug-in choisi est inclus dans les exemples de projets de démarrage et terminés qui accompagnent ce didacticiel. Pour ce didacticiel, vous devez uniquement utiliser jQuery pour le raccorder à l’interface utilisateur. Pour utiliser le plug-in jQuery choisi par RECOLTE dans un projet MVC ASP.NET, vous devez :

1. Téléchargez le plug-in choisi à partir de [GitHub](https://github.com/harvesthq/chosen/). Cette étape a été effectuée pour vous.
2. Ajoutez le dossier choisi à votre projet MVC ASP.NET. Ajoutez les éléments multimédias du plug-in choisi que vous avez téléchargé à l’étape précédente dans le dossier choisi. Cette étape a été effectuée pour vous.
3. Raccorder le plug-in choisi au programme d’assistance HTML **DropDownList** ou **ListBox** .

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Raccordement du plug-in choisi à la vue MultiSelectCountry.

Ouvrez le fichier *Views\Home\MultiSelectCountry.cshtml* et ajoutez un paramètre `htmlAttributes` au `Html.ListBox`. Le paramètre que vous ajouterez contient un nom de classe pour la liste de sélection (`@class = "chzn-select"`). Le code complet est indiqué ci-dessous :

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Dans le code ci-dessus, nous ajoutons l’attribut HTML et la valeur d’attribut `class = "chzn-select"`. Le \@ caractère précédent de la classe n’a rien à voir avec le moteur d’affichage Razor. `class` est un [ C# mot clé](https://msdn.microsoft.com/library/x53a06bb.aspx). C#les mots clés ne peuvent pas être utilisés comme identificateurs, sauf s’ils incluent \@ en tant que préfixe. Dans l’exemple ci-dessus, `@class` est un identificateur valide, mais la **classe** n’est pas parce que la **classe** est un mot clé.

Ajoutez des références aux fichiers choisis */choisis. jQuery. js* et *choisis/choisis. CSS* . Le *choisi/choisi. jQuery. js* et implémente la fonction du plug-in choisi. Le fichier *. CSS choisi/choisi* fournit le style. Ajoutez ces références au bas du fichier *Views\Home\MultiSelectCountry.cshtml* . Le code suivant montre comment référencer le plug-in choisi.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Activez le plug-in choisi à l’aide du nom de classe utilisé dans le code **html. ListBox** . Dans l’exemple ci-dessus, le nom de la classe est `chzn-select`. Ajoutez la ligne suivante au bas du fichier de vue *Views\Home\MultiSelectCountry.cshtml* . Cette ligne active le plug-in choisi.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La ligne suivante est la syntaxe permettant d’appeler la fonction jQuery Ready, qui sélectionne l’élément DOM avec le nom de classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Le jeu encapsulé retourné par l’appel ci-dessus applique ensuite la méthode choisie (`.chosen();`), qui raccorde le plug-in choisi.

Le code suivant montre le fichier de vue *Views\Home\MultiSelectCountry.cshtml* terminé.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Exécutez l’application et accédez à la vue `MultiSelectCountry`. Essayez d’ajouter et de supprimer des pays. L’exemple de téléchargement fourni contient également une méthode et une vue `MultiCountryVM` qui implémentent la fonctionnalité MultiSelectCountry à l’aide d’un modèle de vue au lieu d’un **ViewBag**.

Dans la section suivante, vous verrez comment le mécanisme de génération de modèles automatique ASP.NET MVC fonctionne avec l’application auxiliaire **DropDownList** .

> [!div class="step-by-step"]
> [Next](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
