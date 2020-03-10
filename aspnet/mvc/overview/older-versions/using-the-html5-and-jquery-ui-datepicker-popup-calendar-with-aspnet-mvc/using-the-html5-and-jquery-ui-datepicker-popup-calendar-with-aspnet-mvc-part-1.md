---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Utilisation du calendrier contextuel de la fenêtre contextuelle du sélecteur de dates de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 1 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c1c2380f24c72f6aabaaacaf975e95288a384ff1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538989"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Utilisation du calendrier contextuel HTML5 et de jQuery UI DatePicker avec ASP.NET MVC-partie 1

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans une application Web ASP.NET MVC.

Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du [calendrier contextuel du sélecteur de dates de l’interface utilisateur](http://plugins.jquery.com/project/datepicker) jQuery dans une application Web ASP.NET MVC. Pour ce didacticiel, vous pouvez utiliser Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), qui est une version gratuite de Microsoft Visual Studio, ou vous pouvez utiliser Visual Studio 2010 SP1 si vous en avez déjà besoin.

Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les logiciels requis à l’aide des liens suivants :

- [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)

Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Ce didacticiel part du principe que vous avez terminé le didacticiel [prise en main avec MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou que vous êtes familiarisé avec le développement ASP.NET MVC. Ce didacticiel commence par le projet terminé du didacticiel [prise en main avec MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .

Ce didacticiel présente le code C#dans. Toutefois, le [projet de démarrage](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) et le projet terminé sont également disponibles dans Visual Basic.

Un projet Visual Studio avec C# et Visual Basic code source est disponible pour accompagner cette rubrique : [Download](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Contenu

Vous allez ajouter des modèles (plus précisément, des modèles de modification et d’affichage) à l’application de liste de films simple créée dans le didacticiel [prise en main avec MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) . Vous allez également ajouter un calendrier contextuel du sélecteur de dates de l' [interface utilisateur jQuery](http://jqueryui.com/demos/datepicker/) pour simplifier le processus d’entrée des dates. La capture d’écran suivante montre l’application modifiée avec la fenêtre contextuelle du sélecteur de dates de l’interface utilisateur jQuery affichée.

![jQuery terminé](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Compétences

Vous apprendrez les compétences suivantes :

- Comment utiliser des attributs de l’espace de noms [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) pour contrôler le format des données lorsqu’elles sont affichées et lorsqu’elles sont en mode édition.
- Comment créer des modèles (modifier et afficher des modèles) pour contrôler la mise en forme des données.
- Comment ajouter le DatePicker de l' [interface utilisateur jQuery](http://jqueryui.com/demos/datepicker/) comme moyen d’entrer des champs de date.

### <a name="getting-started"></a>Commencer

Si vous n’avez pas encore l’application de liste de films du projet de démarrage, téléchargez-la : 

* [Téléchargez](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).
* Dans l’Explorateur Windows, cliquez avec le bouton droit sur le fichier *MvcMovie. zip* , puis sélectionnez **Propriétés**. 
* Dans la boîte de dialogue **Propriétés de MvcMovie. zip** , sélectionnez **débloquer**. Le déblocage empêche l’apparition d’un avertissement de sécurité, qui s’affiche normalement lorsque vous essayez d’utiliser un fichier *.zip* téléchargé à partir d’Internet.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Cliquez avec le bouton droit sur le fichier *MvcMovie. zip* et sélectionnez **extraire tout** pour décompresser le fichier. Dans Visual Web Developer ou Visual Studio 2010, ouvrez le fichier *MvcMovieCS\_tu. sln* .

Dans **Explorateur de solutions**, double-cliquez sur le *Views\Shared\\_Layout. cshtml* pour l’ouvrir. Remplacez l’en-tête `H1` de l' **application de film MVC** par **Movie jQuery**. Appuyez sur CTRL + F5 pour exécuter l’application, puis cliquez sur l’onglet dossier de **démarrage** pour accéder à la méthode `Index` du contrôleur de film. Pour tester l’application, sélectionnez le lien **modifier** et le lien **Détails** pour l’un des films. Notez que dans les vues index, Edit et Details, la date de publication et le prix sont correctement mis en forme :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

La mise en forme de la date et du prix est le résultat de l’utilisation de l’attribut [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) sur les propriétés de la classe `Movie`.

Ouvrez le fichier *Movie.cs* et commentez l’attribut `DisplayFormat` sur les propriétés `ReleaseDate` et `Price`. La classe de `Movie` résultante ressemble à ceci :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Appuyez de nouveau sur CTRL + F5 pour exécuter l’application et sélectionnez l’onglet dossier de **démarrage** pour afficher la liste des films. Cette fois-ci, la date de publication indique la date et l’heure, et le champ Price n’affiche plus le symbole monétaire. La modification apportée à la classe `Movie` a annulé la mise en forme que vous avez notée précédemment, mais vous allez le résoudre dans un moment.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Utilisation de l’attribut de type de données DataAnnotations pour spécifier le type de données

Remplacez l’attribut `DisplayFormat` commenté pour la propriété `ReleaseDate` par l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , à l’aide de l’énumération `Date`. Remplacez l’attribut `DisplayFormat` pour la propriété `Price` par l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , cette fois à l’aide de l’énumération `Currency`. Voici à quoi ressemble le code complet :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Exécutez l'application. Désormais, les propriétés date de publication et prix sont correctement mises en forme (c’est-à-dire en utilisant des formats de date et de devise appropriés). L’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fournit des métadonnées de type pour les modèles MVC ASP.net intégrés afin que les champs s’affichent dans le format correct. L’utilisation de l’attribut `DataType` est préférable à l’utilisation de l’attribut `DisplayFormat` qui se trouvait à l’origine dans le code, car l’attribut `DataType` rend le modèle plus propre et plus flexible à des fins telles que l’internationalisation.

Dans la section suivante, vous allez apprendre à créer des modèles personnalisés pour afficher les champs de date.

> [!div class="step-by-step"]
> [Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
