---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Utilisation du calendrier contextuel de la fenêtre contextuelle du sélecteur de dates de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 2 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 325cc90eb6e717c47863eda6253e0d48d796386b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614974"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Utilisation du calendrier contextuel de la fenêtre contextuelle de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 2

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans une application Web ASP.NET MVC.

## <a name="adding-an-automatic-datetime-template"></a>Ajout d’un modèle de date et d’heure automatique

Dans la première partie de ce didacticiel, vous avez vu comment vous pouvez ajouter des attributs au modèle pour spécifier explicitement la mise en forme, et comment vous pouvez spécifier explicitement le modèle utilisé pour afficher le modèle. Par exemple, l’attribut [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) dans le code suivant spécifie explicitement la mise en forme de la propriété `ReleaseDate`.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Dans l’exemple suivant, l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , à l’aide de l’énumération `Date`, spécifie que le modèle de date doit être utilisé pour restituer le modèle. Si votre projet ne contient pas de modèle de date, le modèle de date intégré est utilisé.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Toutefois, ASP. MVC peut effectuer une correspondance de type à l’aide de la Convention-sur-Configuration, en recherchant un modèle qui correspond au nom d’un type. Cela vous permet de créer un modèle qui met automatiquement en forme les données sans utiliser d’attributs ou de code. Pour cette partie du didacticiel, vous allez créer un modèle qui est appliqué automatiquement aux propriétés de modèle de type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Vous n’avez pas besoin d’utiliser un attribut ou une autre configuration pour spécifier que le modèle doit être utilisé pour afficher toutes les propriétés de modèle de type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Vous découvrirez également un moyen de personnaliser l’affichage des propriétés individuelles, voire des champs individuels.

Pour commencer, nous allons supprimer les informations de mise en forme existantes et afficher les dates complètes dans l’application.

Ouvrez le fichier *Movie.cs* et commentez l’attribut `DataType` sur la propriété `ReleaseDate` :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Appuyez sur CTRL+F5 pour exécuter l'application.

Notez que la propriété `ReleaseDate` affiche à présent la date et l’heure, car il s’agit de la valeur par défaut quand aucune information de mise en forme n’est fournie.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Ajout de styles CSS pour tester de nouveaux modèles

Avant de créer un modèle pour mettre en forme des dates, vous devez ajouter quelques règles de style CSS que vous pouvez appliquer aux nouveaux modèles. Celles-ci vous aideront à vérifier que la page rendue utilise le nouveau modèle.

Ouvrez le fichier *Content\Site.cs*s et ajoutez les règles CSS suivantes au bas du fichier :

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Ajout de modèles d’affichage DateTime

Vous pouvez maintenant créer le nouveau modèle. Dans le dossier *Views\Movies* , créez un dossier *DisplayTemplates* .

Dans le dossier *Views\Shared* , créez un dossier *DisplayTemplates* et un dossier *EditorTemplates* .

Les modèles d’affichage dans le dossier *Views\Shared\DisplayTemplates.* seront utilisés par tous les contrôleurs. Les modèles d’affichage dans le dossier *Views\Movie\DisplayTemplates* seront utilisés uniquement par le contrôleur `Movie`. (Si un modèle portant le même nom apparaît dans les deux dossiers, le modèle dans le dossier *Views\Movie\DisplayTemplates* , autrement dit, le modèle plus spécifique, est prioritaire pour les affichages retournés par le contrôleur `Movie`.)

Dans **Explorateur de solutions**, développez le dossier *views* , développez le dossier *Shared* , puis cliquez avec le bouton droit sur le dossier *Views\Shared\DisplayTemplates.* .

Cliquez sur **Ajouter** , puis sur **affichage**. La boîte de dialogue **Ajouter une vue** s’affiche.

Dans la zone nom de la **vue** , tapez `DateTime`. (Vous devez utiliser ce nom pour faire correspondre le nom du type.)

Activez la case à cocher **créer comme vue partielle** . Vérifiez que les cases à cocher **utiliser une disposition ou une page maître** et **créer un affichage fortement typé** ne sont pas activées.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Cliquez sur **Ajouter**. Un modèle *DateTime. cshtml* est créé dans *Views\Shared\DisplayTemplates.* .

L’illustration suivante montre le dossier *views* dans **Explorateur de solutions** une fois que les modèles d’affichage et d’éditeur de `DateTime` sont créés.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Ouvrez le fichier *Views\Shared\DisplayTemplates\DateTime.cshtml* et ajoutez le balisage suivant, qui utilise la méthode [String. format](https://msdn.microsoft.com/library/system.string.format.aspx) pour mettre en forme la propriété en tant que date sans l’heure. (Le format de `{0:d}` spécifie un format de date abrégé.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Répétez cette étape pour créer un modèle de `DateTime` dans le dossier *Views\Movie\DisplayTemplates* . Utilisez le code suivant dans le fichier *Views\Movie\DisplayTemplates\DateTime.cshtml* .

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

La classe CSS `loud-1` entraîne l’affichage de la date en texte rouge gras. Vous avez ajouté la classe CSS `loud-1` de la même manière qu’une mesure temporaire afin de pouvoir voir facilement quand ce modèle particulier est utilisé.

Ce que vous avez fait, c’est créer des modèles personnalisés que ASP.NET utilisera pour afficher des dates. Le modèle plus général (dans le dossier *Views\Shared\DisplayTemplates.* ) affiche une date abrégée simple. Le modèle qui est spécifiquement pour le contrôleur `Movie` (dans le dossier *Views\Movies\DisplayTemplates* ) affiche une date brève qui est également mise en forme en texte rouge en gras.

Appuyez sur CTRL+F5 pour exécuter l'application. Le navigateur restitue la vue index pour l’application.

La propriété `ReleaseDate` affiche à présent la date dans une police rouge en gras sans l’heure. Cela vous permet de vérifier que le `DateTime` Helper basé sur un modèle dans le dossier *Views\Movies\DisplayTemplates* est sélectionné sur le `DateTime` Helper basé sur un modèle dans le dossier partagé (*Views\Shared\DisplayTemplates.* ).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Maintenant, renommez le fichier *Views\Movies\DisplayTemplates\DateTime.cshtml* en *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Appuyez sur CTRL+F5 pour exécuter l'application.

Cette fois, la propriété `ReleaseDate` affiche une date sans l’heure et sans la police rouge gras. Cela montre qu’un modèle qui porte le nom du type de données (dans ce cas `DateTime`) est utilisé automatiquement pour afficher toutes les propriétés de modèle de ce type. Une fois que vous avez renommé le fichier *DateTime. cshtml* en *LoudDateTime. cshtml*, ASP.net n’a plus trouvé de modèle dans le dossier *Views\Movies\DisplayTemplates* , donc il a utilisé le modèle *DateTime. cshtml* à partir du dossier * Views\Movies\Shared\*.

(La correspondance de modèle ne respecte pas la casse, vous pouvez donc avoir créé le nom du fichier de modèle avec n’importe quelle casse. Par exemple, *DateTime. cshtml, DateTime. cshtml*et *DateTime. cshtml* correspondront tous au type de `DateTime`.)

Pour réviser : à ce stade, le champ `ReleaseDate` s’affiche à l’aide du modèle *Views\Movies\DisplayTemplates\DateTime.cshtml* , qui affiche les données à l’aide d’un format de date abrégé, mais n’ajoute pas d’autre format spécial.

### <a name="using-uihint-to-specify-a-display-template"></a>Utilisation de UIHint pour spécifier un modèle d’affichage

Si votre application Web comporte de nombreux champs `DateTime` et que vous souhaitez afficher par défaut la totalité ou la plupart d’entre elles au format date uniquement, le modèle *DateTime. cshtml* est une bonne approche. Mais que se passe-t-il si vous souhaitez afficher la date et l’heure complètes ? Aucun problème. Vous pouvez créer un modèle supplémentaire et utiliser l’attribut [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) pour spécifier la mise en forme de la date et de l’heure complètes. Vous pouvez ensuite appliquer de manière sélective ce modèle. Vous pouvez utiliser l’attribut [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) au niveau du modèle ou vous pouvez spécifier le modèle à l’intérieur d’une vue. Dans cette section, vous allez apprendre à utiliser l’attribut `UIHint` pour modifier de manière sélective la mise en forme de certaines instances de champs date-heure.

Ouvrez le fichier *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* et remplacez le code existant par ce qui suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Cela entraîne l’affichage de la date et de l’heure complètes et ajoute la classe CSS qui rend le texte vert et grand.

Ouvrez le fichier *Movie.cs* et ajoutez l’attribut [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) à la propriété `ReleaseDate`, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Cela indique à ASP.NET MVC que lorsqu’il affiche la propriété `ReleaseDate` (plus précisément, et pas seulement un objet `DateTime`), il doit utiliser le modèle *LoudDateTime. cshtml* .

Appuyez sur CTRL+F5 pour exécuter l'application.

Notez que la propriété `ReleaseDate` affiche désormais la date et l’heure dans une grande police verte.

Revenez à l’attribut `UIHint` dans le fichier *Movie.cs* et commentez-le pour que le modèle *LoudDateTime. cshtml* ne soit pas utilisé. Exécutez de nouveau l'application. La date de sortie n’est pas affichée en grand et en vert. Cela permet de vérifier que le modèle *Views\Shared\DisplayTemplates\DateTime.cshtml* est utilisé dans les vues index et details.

Comme mentionné précédemment, vous pouvez également appliquer un modèle dans une vue, ce qui vous permet d’appliquer le modèle à une instance individuelle de certaines données. Ouvrez la vue *Views\Movies\Details.cshtml* . Ajoutez `"LoudDateTime"` comme deuxième paramètre de l’appel [html. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) pour le champ `ReleaseDate`. Le code complet se présente ainsi :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Cela spécifie que le modèle de `LoudDateTime` doit être utilisé pour afficher la propriété de modèle, quels que soient les attributs appliqués au modèle.

Appuyez sur CTRL+F5 pour exécuter l'application.

Vérifiez que la page d’index des films utilise le modèle *Views\Shared\DisplayTemplates\DateTime.cshtml* (rouge gras) et que la page *Movie\Details* utilise le modèle *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* (grand et vert).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Dans la section suivante, vous allez créer un modèle pour un type complexe.

> [!div class="step-by-step"]
> [Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Suivant](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
