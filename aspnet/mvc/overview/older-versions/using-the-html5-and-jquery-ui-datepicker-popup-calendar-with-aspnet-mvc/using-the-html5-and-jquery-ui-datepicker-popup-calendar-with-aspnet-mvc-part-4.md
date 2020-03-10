---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Utilisation du calendrier contextuel de la fenêtre contextuelle de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 4 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 583e782641efea9a9517edb31f7718b28203d756
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538947"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Utilisation du calendrier contextuel de la fenêtre contextuelle de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 4

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans une application Web ASP.NET MVC.

### <a name="adding-a-template-for-editing-dates"></a>Ajout d’un modèle pour la modification des dates

Dans cette section, vous allez créer un modèle pour la modification des dates qui seront appliquées quand ASP.NET MVC affiche l’interface utilisateur pour la modification des propriétés du modèle qui sont marquées avec l’énumération **Date** de l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . Le modèle affiche uniquement la date. l’heure ne sera pas affichée. Dans le modèle, vous allez utiliser le calendrier contextuel de la fenêtre contextuelle [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) pour fournir un moyen de modifier les dates.

Pour commencer, ouvrez le fichier *Movie.cs* et ajoutez l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) avec l’énumération **Date** à la propriété `ReleaseDate`, comme indiqué dans le code suivant :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Ce code provoque l’affichage du champ `ReleaseDate` sans l’heure dans les modèles d’affichage et les modèles de modification. Si votre application contient un modèle *date. cshtml* dans le dossier *Views\Shared\EditorTemplates* ou dans le dossier *Views\Movies\EditorTemplates* , ce modèle sera utilisé pour afficher toute propriété de `DateTime` lors de la modification. Dans le cas contraire, le système de création de modèles ASP.NET intégré affiche la propriété en tant que date.

Appuyez sur CTRL+F5 pour exécuter l'application. Sélectionnez un lien Modifier pour vérifier que le champ d’entrée pour la date de publication n’indique que la date.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

Dans **Explorateur de solutions**, développez le dossier *views* , développez le dossier *Shared* , puis cliquez avec le bouton droit sur le dossier *Views\Shared\EditorTemplates* .

Cliquez sur **Ajouter**, puis sur **affichage**. La boîte de dialogue **Ajouter une vue** s’affiche.

Dans la zone nom de la **vue** , tapez &quot;date&quot;.

Activez la case à cocher **créer comme vue partielle** . Vérifiez que les cases à cocher **utiliser une disposition ou une page maître** et **créer un affichage fortement typé** ne sont pas activées.

Cliquez sur **Ajouter**. Le modèle *Views\Shared\EditorTemplates\Date.cshtml* est créé.

Ajoutez le code suivant au modèle *Views\Shared\EditorTemplates\Date.cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La première ligne déclare que le modèle est un type de `DateTime`. Même si vous n’avez pas besoin de déclarer le type de modèle dans les modèles de modification et d’affichage, il est recommandé d’obtenir la vérification au moment de la compilation du modèle passé à la vue. (Un autre avantage est que vous recevez ensuite IntelliSense pour le modèle dans la vue dans Visual Studio.) Si le type de modèle n’est pas déclaré, ASP.NET MVC considère qu’il s’agit d’un type [dynamique](https://msdn.microsoft.com/library/dd264741.aspx) et qu’il n’y a aucune vérification de type au moment de la compilation. Si vous déclarez que le modèle est un type de `DateTime`, il est fortement typé.

La deuxième ligne est simplement une balise HTML littérale qui affiche &quot;à l’aide du modèle de date&quot; avant un champ de date. Vous allez utiliser cette ligne temporairement pour vérifier que ce modèle de date est utilisé.

La ligne suivante est une application d’assistance [html. TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) qui restitue un champ `input` qui est une zone de texte. Le troisième paramètre de l’application auxiliaire utilise un type anonyme pour définir la classe de la zone de texte sur `datefield` et le type à `date`. (Étant donné que `class` est un C#réservé dans, vous devez utiliser le caractère `@` pour échapper l’attribut `class` dans C# l’analyseur.)

Le type de `date` est un type d’entrée HTML5 qui permet aux navigateurs prenant en charge HTML5 d’afficher un contrôle de calendrier HTML5. Plus tard, vous ajouterez du JavaScript pour raccorder le sélecteur jQuery à l’élément `Html.TextBox` à l’aide de la classe `datefield`.

Appuyez sur CTRL+F5 pour exécuter l'application. Vous pouvez vérifier que la propriété `ReleaseDate` dans la vue Edit utilise le modèle Edit, car le modèle affiche &quot;à l’aide du modèle de date&quot; juste avant la zone de texte `ReleaseDate` Text, comme indiqué dans cette image :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Dans votre navigateur, affichez la source de la page. (Par exemple, cliquez avec le bouton droit sur la page et sélectionnez **afficher la source**.) L’exemple suivant montre une partie du balisage de la page, illustrant les attributs `class` et `type` dans le rendu HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Revenez au modèle *Views\Shared\EditorTemplates\Date.cshtml* et supprimez le &quot;à l’aide du modèle de date&quot; le balisage. Le modèle terminé ressemble maintenant à ce qui suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Ajout d’un calendrier contextuel du sélecteur de dates jQuery UI à l’aide de NuGet

Dans cette section, vous allez ajouter le calendrier contextuel du sélecteur de dates de l' [interface utilisateur jQuery](http://jqueryui.com/demos/datepicker/) au modèle date-édition. La bibliothèque [jQuery UI](http://jqueryui.com/) prend en charge l’animation, les effets avancés et les widgets personnalisables. Il s’appuie sur la bibliothèque Java jQuery. Le calendrier contextuel de DatePicker permet d’entrer facilement et en toute simplicité des dates à l’aide d’un calendrier au lieu d’entrer une chaîne. Le calendrier contextuel limite également les utilisateurs aux dates légales. une entrée de texte ordinaire pour une date vous permet de saisir des `2/33/1999` (février 33rd, 1999), mais le calendrier contextuel [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) ne l’autorise pas.

Tout d’abord, vous devez installer les bibliothèques de l’interface utilisateur jQuery. Pour ce faire, vous allez utiliser NuGet, qui est un gestionnaire de package inclus dans les versions SP1 de Visual Studio 2010 et Visual Web Developer.

Dans Visual Web Developer, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** , puis sélectionnez **gérer les packages NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Remarque : si le menu **Outils** n’affiche pas la commande **Gestionnaire de package NuGet** , vous devez installer NuGet en suivant les instructions de la page [installation de NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) du site Web NuGet.   
  
Si vous utilisez Visual Studio au lieu de Visual Web Developer, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** , puis sélectionnez Ajouter une référence de package de **bibliothèque**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Dans la boîte de dialogue **MVCMovie-Manage NuGet packages** , cliquez sur l’onglet **Online** situé à gauche, puis entrez &quot;jQuery. UI&quot; dans la zone de recherche. Sélectionnez j **query UI widgets : DatePicker**, puis cliquez sur le bouton **installer** .

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet ajoute ces versions Debug et les versions minimisés de jQuery UI Core et le sélecteur de date jQuery UI à votre projet :

- *jQuery. UI. Core. js*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. js*
- *jQuery. UI. DatePicker. min. js*

Remarque : les versions Debug (les fichiers sans l’extension *. min. js* ) sont utiles pour le débogage, mais dans un site de production, vous n’incluez que les versions minimisés.

Pour utiliser réellement le sélecteur de dates jQuery, vous devez créer un script jQuery qui associe le widget de calendrier au modèle d’édition. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *scripts* , puis sélectionnez **Ajouter**, **nouvel élément**et **fichier JScript**. Nommez le fichier *DatePickerReady. js*.

Ajoutez le code suivant au fichier *DatePickerReady. js* :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Si vous n’êtes pas familiarisé avec jQuery, voici une brève explication de ce que cela fait : la première ligne est la &quot;fonction jQuery Ready&quot;, qui est appelée lorsque tous les éléments DOM d’une page ont été chargés. La deuxième ligne sélectionne tous les éléments DOM qui ont le nom de la classe `datefield`, puis appelle la fonction `datepicker` pour chacun d’eux. (N’oubliez pas que vous avez ajouté la classe `datefield` au modèle *Views\Shared\EditorTemplates\Date.cshtml* plus haut dans le didacticiel.)

Ensuite, ouvrez le fichier *Views\Shared\\_Layout. cshtml* . Vous devez ajouter des références aux fichiers suivants, qui sont tous obligatoires afin que vous puissiez utiliser le sélecteur de dates :

- *Content/themes/base/jQuery. UI. Core. CSS*
- *Content/themes/base/jQuery. UI. DatePicker. CSS*
- *Content/themes/base/jQuery. UI. Theme. CSS*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. min. js*
- *DatePickerReady. js*

L’exemple suivant montre le code réel que vous devez ajouter en bas de l’élément `head` dans le fichier *Views\Shared\\_Layout. cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

La section `head` complète est présentée ici :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

La méthode d' [assistance de contenu d’URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) convertit le chemin d’accès de la ressource en chemin d’accès absolu. Vous devez utiliser `@URL.Content` pour référencer correctement ces ressources lorsque l’application s’exécute sur IIS.

Appuyez sur CTRL+F5 pour exécuter l'application. Sélectionnez un lien modifier, puis placez le point d’insertion dans le champ **libéré** . Le calendrier contextuel jQuery UI s’affiche.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Comme la plupart des contrôles jQuery, le DatePicker vous permet de le personnaliser de manière intensive. Pour plus d’informations, consultez [personnalisation visuelle : conception d’un thème de l’interface utilisateur jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) sur le site de l' [interface utilisateur jQuery](http://learn.jquery.com/jquery-ui/getting-started/) .

### <a name="supporting-the-html5-date-input-control"></a>Prise en charge du contrôle d’entrée de date HTML5

À mesure que de plus en plus de navigateurs prennent en charge HTML5, vous souhaiterez utiliser l’entrée HTML5 native, telle que l’élément d’entrée `date`, et ne pas utiliser le calendrier de l’interface utilisateur jQuery. Vous pouvez ajouter une logique à votre application pour qu’elle utilise automatiquement les contrôles HTML5 si le navigateur les prend en charge. Pour ce faire, remplacez le contenu du fichier *DatePickerReady. js* par ce qui suit :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La première ligne de ce script utilise Modernizr pour vérifier que l’entrée de date HTML5 est prise en charge. S’il n’est pas pris en charge, le sélecteur de dates jQuery UI est raccordé à la place. ([Modernizr](http://www.modernizr.com/docs/) est une bibliothèque JavaScript open source qui détecte la disponibilité des implémentations natives de HTML5 et CSS3. Modernizr est inclus dans tous les nouveaux projets MVC ASP.NET que vous créez.)

Une fois que vous avez apporté cette modification, vous pouvez la tester à l’aide d’un navigateur prenant en charge HTML5, tel que Opera 11. Exécutez l’application à l’aide d’un navigateur compatible HTML5 et modifiez une entrée de film. Le contrôle de date HTML5 est utilisé à la place du calendrier contextuel de l’interface utilisateur jQuery :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Étant donné que les nouvelles versions des navigateurs implémentent HTML5 de manière incrémentielle, une bonne approche pour l’instant est d’ajouter du code à votre site Web qui prend en charge un large éventail de prise en charge HTML5. Par exemple, un script *DatePickerReady. js* plus robuste est illustré ci-dessous, ce qui permet à votre site de prendre en charge des navigateurs qui ne prennent en charge que partiellement le contrôle de date HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Ce script sélectionne les éléments de `input` HTML5 de type `date` qui ne prennent pas entièrement en charge le contrôle de date HTML5. Pour ces éléments, elle raccorde le calendrier contextuel de l’interface utilisateur jQuery, puis remplace l’attribut `type` de `date` par `text`. En remplaçant l’attribut `type` de `date` par `text`, la prise en charge partielle des dates HTML5 est éliminée. Vous trouverez un script *DatePickerReady. js* encore plus robuste sur [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Ajout de dates Nullable aux modèles

Si vous utilisez l’un des modèles de date existants et que vous transmettez une date NULL, vous obtenez une erreur d’exécution. Pour rendre les modèles de date plus fiables, vous devez les modifier pour gérer les valeurs NULL. Pour prendre en charge les dates Nullable, remplacez le code dans *Views\Shared\DisplayTemplates\DateTime.cshtml* par ce qui suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Le code retourne une chaîne vide lorsque le modèle a la **valeur null**.

Remplacez le code du fichier *Views\Shared\EditorTemplates\Date.cshtml* par ce qui suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quand ce code s’exécute, si le modèle n’est pas null, la valeur de `DateTime` du modèle est utilisée. Si le modèle a la valeur null, la date actuelle est utilisée à la place.

### <a name="wrapup"></a>Wrapup

Ce didacticiel a abordé les principes de base des applications auxiliaires basées sur un modèle ASP.NET et vous montre comment utiliser le calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans une application ASP.NET MVC. Pour plus d’informations, consultez les ressources suivantes :

- Pour plus d’informations sur la localisation, consultez le billet de blog de Rajeesh [jQueryUI DatePicker dans ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Pour plus d’informations sur l’interface utilisateur jQuery, consultez [jQuery UI](http://docs.jquery.com/UI).
- Pour plus d’informations sur la localisation du contrôle DatePicker, consultez [UI/DatePicker/localization](http://docs.jquery.com/UI/Datepicker/Localization).
- Pour plus d’informations sur les modèles MVC ASP.NET, consultez la série de blogs de Brad Wilson sur les [modèles ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Bien que la série ait été écrite pour ASP.NET MVC 2, le matériel s’applique toujours à la version actuelle de ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
