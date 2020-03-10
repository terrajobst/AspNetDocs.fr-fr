---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Ajout de la validation au modèle (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 7f3f195bc30ed23a637b59f15e6fc8431e39e217
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615156"
---
# <a name="adding-validation-to-the-model-vb"></a>Ajout de la validation au modèle (VB)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez C#, passez à la [ C# version](../cs/adding-validation-to-the-model.md) de ce didacticiel.

Dans cette section, vous allez ajouter une logique de validation au modèle `Movie`, et vous vous assurerez que les règles de validation sont appliquées chaque fois qu’un utilisateur tente de créer ou de modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Conservation des éléments secs

L’un des principes de conception de base de ASP.NET MVC est DRY (« Ne vous répétez pas »). ASP.NET MVC vous encourage à spécifier des fonctionnalités ou un comportement une seule fois, puis à les répercuter dans une application. Cela réduit la quantité de code que vous devez écrire et facilite grandement la maintenance du code que vous écrivez.

La prise en charge de la validation fournie par ASP.NET MVC et Entity Framework Code First est un excellent exemple du principe DRY en action. Vous pouvez spécifier de façon déclarative des règles de validation à un seul emplacement (dans la classe de modèle), puis ces règles sont appliquées partout dans l’application.

Voyons comment vous pouvez tirer parti de cette prise en charge de validation dans l’application de film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de validation au modèle Movie

Vous commencerez par ajouter une logique de validation à la classe `Movie`.

Ouvrez le fichier *Movie. vb* . Ajoutez une instruction `Imports` en haut du fichier qui fait référence à l’espace de noms [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

L’espace de noms fait partie du .NET Framework. Il fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à toute classe ou propriété.

À présent, mettez à jour la classe `Movie` pour tirer parti des attributs de validation intégrés [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)et [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Utilisez le code suivant comme exemple d’application des attributs.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. L’attribut `Required` indique qu’une propriété doit avoir une valeur ; dans cet exemple, un film doit avoir des valeurs pour les propriétés `Title`, `ReleaseDate`, `Genre`et `Price` afin d’être valide. L’attribut `Range` contraint une valeur à une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale.

Code First garantit que les règles de validation que vous spécifiez sur une classe de modèle sont appliquées avant que l’application n’enregistre les modifications dans la base de données. Par exemple, le code ci-dessous lèvera une exception lorsque la méthode `SaveChanges` est appelée, car plusieurs valeurs de propriété obligatoires `Movie` sont manquantes et le prix est égal à zéro (ce qui est en dehors de la plage valide).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

L’application automatique des règles de validation par le .NET Framework aide à rendre votre application plus robuste. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

Voici une liste complète de code pour le fichier *Movie. vb* mis à jour :

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interface utilisateur d’erreur de validation dans ASP.NET MVC

Réexécutez l’application et accédez à l’URL */movies* .

Cliquez sur le lien **créer un film** pour ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides, puis cliquez sur le bouton **créer** .

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Notez que le formulaire a automatiquement utilisé une couleur d’arrière-plan pour mettre en surbrillance les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation approprié à côté de chacun d’eux. Les messages d’erreur correspondent aux chaînes d’erreur que vous avez spécifiées lors de l’annotation de la classe `Movie`. Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript) et côté serveur (si JavaScript est désactivé pour un utilisateur).

L’un des avantages réels est que vous n’avez pas besoin de modifier une seule ligne de code dans la classe `MoviesController` ou dans la vue *Create. vbhtml* afin d’activer cette interface utilisateur de validation. Le contrôleur et les vues que vous avez créés précédemment dans ce didacticiel ont automatiquement récupéré les règles de validation que vous avez spécifiées à l’aide d’attributs sur la classe de modèle `Movie`.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Comment la validation se produit dans la méthode Create View et Create action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante montre à quoi ressemblent les méthodes `Create` de la classe `MovieController`. Ils n’ont pas changé par rapport à la façon dont vous les avez créés précédemment dans ce didacticiel.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

La première méthode d’action affiche le formulaire de création initial. La seconde gère la publication de formulaire. La deuxième `Create` méthode appelle `ModelState.IsValid` pour vérifier si le film contient des erreurs de validation. L’appel de cette méthode évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, la méthode `Create` réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données.

Vous trouverez ci-dessous le modèle de vue *Create. vbhtml* que vous avez généré au préalable dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Notez que le code utilise un programme d’assistance `Html.EditorFor` pour générer la sortie de l’élément `<input>` pour chaque propriété `Movie`. En regard de cette application d’assistance, il s’agit d’un appel à la méthode d’assistance `Html.ValidationMessageFor`. Ces deux méthodes d’assistance fonctionnent avec l’objet de modèle passé par le contrôleur à la vue (dans ce cas, un objet `Movie`). Ils recherchent automatiquement les attributs de validation spécifiés sur le modèle et affichent les messages d’erreur appropriés.

L’avantage de cette approche est que ni le contrôleur, ni le modèle de vue Create ne savent quoi que ce soit sur les règles de validation réelles appliquées ou sur les messages d’erreur spécifiques affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`.

Si vous souhaitez modifier la logique de validation ultérieurement, vous pouvez le faire à un seul endroit. Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez entièrement le principe DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Ajout de la mise en forme au modèle Movie

Ouvrez le fichier *Movie. vb* . L’espace de noms [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Vous appliquerez l’attribut [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) et une valeur d’énumération [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) à la date de sortie et aux champs de prix. Le code suivant montre les propriétés `ReleaseDate` et `Price` avec l’attribut [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) approprié.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Vous pouvez également définir explicitement une valeur [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . Le code suivant montre la propriété release date avec une chaîne de format de date (c’est-à-dire « d »). Vous pouvez l’utiliser pour spécifier que vous ne souhaitez pas l’heure dans le cadre de la date de publication.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Le code suivant met en forme la propriété `Price` en tant que devise.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

La classe `Movie` complète est indiquée ci-dessous.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Exécutez l’application et accédez au contrôleur `Movies`.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux méthodes de `Details` et de `Delete` générées automatiquement.

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field.md)
> [Suivant](improving-the-details-and-delete-methods.md)
