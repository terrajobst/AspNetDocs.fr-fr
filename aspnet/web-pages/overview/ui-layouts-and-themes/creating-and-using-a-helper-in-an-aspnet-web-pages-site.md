---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Création et utilisation d’un programme d’assistance dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment créer une application auxiliaire dans un site Web pages Web ASP.NET (Razor). Une application d’assistance est un composant réutilisable qui comprend le code et le balisage des performances...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563510"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Création et utilisation d’un programme d’assistance dans un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment créer une application auxiliaire dans un site Web pages Web ASP.NET (Razor). Une *application auxiliaire* est un composant réutilisable qui comprend du code et des balises pour effectuer une tâche qui peut être fastidieuse ou complexe.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer et utiliser une application auxiliaire simple.
> 
> Voici les fonctionnalités ASP.NET présentées dans l’article :
> 
> - Syntaxe de `@helper`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

## <a name="overview-of-helpers"></a>Vue d’ensemble des applications auxiliaires

Si vous devez effectuer les mêmes tâches sur des pages différentes de votre site, vous pouvez utiliser une application auxiliaire. Pages Web ASP.NET inclut un certain nombre d’applications auxiliaires, et il existe de nombreux autres que vous pouvez télécharger et installer. (Une liste des applications d’assistance intégrées dans pages Web ASP.NET est répertoriée dans la [référence rapide de l’API ASP.net](https://go.microsoft.com/fwlink/?LinkId=202907).) Si aucune des applications d’assistance existantes ne répond à vos besoins, vous pouvez créer votre propre application d’assistance.

Une application d’assistance vous permet d’utiliser un bloc de code commun sur plusieurs pages. Supposons que, dans votre page, vous souhaitiez souvent créer un élément note qui est séparé des paragraphes normaux. Par exemple, la note est créée en tant qu’élément `<div>` qui a pour style une bordure. Au lieu d’ajouter ce même balisage à une page chaque fois que vous souhaitez afficher une note, vous pouvez empaqueter le balisage en tant qu’application d’assistance. Vous pouvez ensuite insérer la note avec une seule ligne de code partout où vous en avez besoin.

L’utilisation d’une application d’assistance de ce type rend le code dans chacune de vos pages plus simple et plus facile à lire. Il facilite également la maintenance de votre site, car si vous avez besoin de modifier l’apparence des notes, vous pouvez modifier le balisage à un seul endroit.

## <a name="creating-a-helper"></a>Création d’une application d’assistance

Cette procédure vous montre comment créer l’application auxiliaire qui crée la note, comme décrit précédemment. Il s’agit d’un exemple simple, mais le programme d’assistance personnalisé peut inclure tout code de balisage et ASP.NET dont vous avez besoin.

1. Dans le dossier racine du site Web, créez un dossier nommé *App\_code*. Il s’agit d’un nom de dossier réservé dans ASP.NET où vous pouvez placer du code pour des composants tels que des applications auxiliaires.
2. Dans le dossier *App\_code* , créez un nouveau fichier *. cshtml* , puis nommez-le *MyHelpers. cshtml*.
3. Remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Le code utilise la syntaxe `@helper` pour déclarer un nouveau programme d’assistance nommé `MakeNote`. Cette aide particulière vous permet de passer un paramètre nommé `content` qui peut contenir une combinaison de texte et de balises. Le programme d’assistance insère la chaîne dans le corps de la note à l’aide de la variable `@content`.

    Notez que le fichier est nommé *MyHelpers. cshtml*, mais que le programme d’assistance est nommé `MakeNote`. Vous pouvez placer plusieurs applications d’assistance personnalisées dans un seul fichier.
4. Enregistrez et fermez le fichier.

## <a name="using-the-helper-in-a-page"></a>Utilisation de l’application auxiliaire dans une page

1. Dans le dossier racine, créez un fichier vide nommé *TestHelper. cshtml*.
2. Ajoutez le code suivant au fichier :

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Pour appeler le programme d’assistance que vous avez créé, utilisez `@` suivi du nom du fichier d’assistance, d’un point, puis du nom de l’assistance. (Si vous avez plusieurs dossiers dans l' *application\_* dossier de code, vous pouvez utiliser la syntaxe `@FolderName.FileName.HelperName` pour appeler votre application auxiliaire dans n’importe quel niveau de dossier imbriqué). Le texte que vous ajoutez entre parenthèses est le texte que le programme d’assistance affichera dans le cadre de la note dans la page Web.
3. Enregistrez la page et exécutez-la dans un navigateur. L’application d’assistance génère l’élément note juste là où vous avez appelé le programme d’assistance : entre les deux paragraphes.

    ![Capture d’écran montrant la page dans le navigateur et comment le balisage généré par le programme d’assistance place une zone autour du texte spécifié.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Menu horizontal comme application auxiliaire Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Cette entrée de blog de Mike pape montre comment créer un menu horizontal en tant qu’application auxiliaire en utilisant le balisage, le code CSS et le code.

[Tirer parti de HTML5 dans pages Web ASP.net helpers pour WebMatrix et ASP.net MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Ce billet de blog de Sam Abraham affiche une assistance qui rend un élément `Canvas` HTML5.

[La différence entre @Helpers et @Functions dans WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Cette entrée de blog de Mike salée décrit `@helper` syntaxe et `@function` syntaxe et le moment où l’utiliser.
