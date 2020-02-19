---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Utilisation du calendrier contextuel de la fenêtre contextuelle de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 3 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457893"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Utilisation du calendrier contextuel de la fenêtre contextuelle du sélecteur de dates de l’interface utilisateur HTML5 et jQuery avec ASP.NET MVC-partie 3

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de l’utilisation des modèles de l’éditeur, de l’affichage des modèles et du calendrier contextuel du sélecteur de dates de l’interface utilisateur jQuery dans une application Web ASP.NET MVC.

## <a name="working-with-complex-types"></a>Utilisation des types complexes

Dans cette section, vous allez créer une classe d’adresses et apprendre à créer un modèle pour l’afficher.

Dans le dossier *Models* , créez un fichier de classe nommé *Person.cs* dans lequel vous allez placer deux types : une classe `Person` et une classe `Address`. La classe `Person` contient une propriété de type `Address`. Le type de `Address` est un type complexe, ce qui signifie qu’il ne s’agit pas de l’un des types intégrés comme `int`, `string`ou `double`. Au lieu de cela, elle a plusieurs propriétés. Le code pour les nouvelles classes se présente comme suit :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

Dans le contrôleur `Movie`, ajoutez l’action `PersonDetail` suivante pour afficher une instance person :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Ajoutez ensuite le code suivant au contrôleur de `Movie` pour remplir le modèle de `Person` avec des exemples de données :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Ouvrez le fichier *Views\Movies\PersonDetail.cshtml* et ajoutez le balisage suivant pour la vue `PersonDetail`.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Appuyez sur CTRL + F5 pour exécuter l’application et accédez à *movies/PersonDetail*.

La vue `PersonDetail` ne contient pas le type complexe `Address`, comme vous pouvez le voir dans cette capture d’écran. (Aucune adresse n’est indiquée.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Les données du modèle de `Address` ne sont pas affichées, car il s’agit d’un type complexe. Pour afficher les informations d’adresse, ouvrez de nouveau le fichier *Views\Movies\PersonDetail.cshtml* et ajoutez le balisage suivant.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Le balisage complet de la vue `PersonDetail` Now se présente comme suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Réexécutez l’application et affichez la vue `PersonDetail`. Les informations d’adresse s’affichent désormais :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Création d’un modèle pour un type complexe

Dans cette section, vous allez créer un modèle qui sera utilisé pour restituer le type complexe `Address`. Lorsque vous créez un modèle pour le type de `Address`, ASP.NET MVC peut l’utiliser automatiquement pour mettre en forme un modèle d’adresse n’importe où dans l’application. Cela vous donne un moyen de contrôler le rendu du type de `Address` à partir d’un seul emplacement dans l’application.

Dans le dossier *Views\Shared\DisplayTemplates.* , créez une vue partielle fortement typée nommée **Address**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Cliquez sur **Ajouter**, puis ouvrez le nouveau fichier *Views\Shared\DisplayTemplates\Address.cshtml* . La nouvelle vue contient le balisage généré suivant :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Exécutez l’application et affichez la vue `PersonDetail`. Cette fois, le modèle de `Address` que vous venez de créer est utilisé pour afficher le type complexe `Address`, de sorte que l’affichage ressemble à ce qui suit :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Résumé : méthodes pour spécifier le format et le modèle d’affichage du modèle

Vous avez vu que vous pouvez spécifier le format ou le modèle d’une propriété de modèle à l’aide des approches suivantes :

- Application de l’attribut `DisplayFormat` à une propriété dans le modèle. Par exemple, le code suivant entraîne l’affichage de la date sans l’heure :

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Application d’un attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) à une propriété dans le modèle et spécification du type de données. Par exemple, le code suivant entraîne l’affichage de la date sans l’heure.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Si l’application contient un modèle *date. cshtml* dans le dossier *Views\Shared\DisplayTemplates.* ou le dossier *Views\Movies\DisplayTemplates* , ce modèle sera utilisé pour restituer la propriété `DateTime`. Dans le cas contraire, le système de création de modèles ASP.NET intégré affiche la propriété en tant que date.
- Création d’un modèle d’affichage dans le dossier *Views\Shared\DisplayTemplates.* ou *Views\Movies\DisplayTemplates* dont le nom correspond au type de données que vous souhaitez mettre en forme. Par exemple, vous avez vu que le *Views\Shared\DisplayTemplates\DateTime.cshtml* a été utilisé pour restituer des propriétés `DateTime` dans un modèle, sans ajouter un attribut au modèle et sans ajouter de balisage aux vues.
- Utilisation de l’attribut [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) sur le modèle pour spécifier le modèle pour l’affichage de la propriété de modèle.
- Ajout explicite du nom du modèle d’affichage à l’appel [html. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) dans une vue.

L’approche que vous utilisez dépend de ce que vous devez faire dans votre application. Il n’est pas rare de combiner ces approches pour atteindre exactement le type de mise en forme dont vous avez besoin.

Dans la section suivante, vous allez changer les engrenages et passer de la personnalisation de l’affichage des données pour personnaliser la façon dont elles sont entrées. Vous allez raccorder le sélecteur de modifications jQuery aux vues de modification dans l’application afin de fournir un moyen de spécifier des dates.

> [!div class="step-by-step"]
> [Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Suivant](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
