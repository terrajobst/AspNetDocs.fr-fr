---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Présentation de la pages Web ASP.NET de la création d’une disposition cohérente | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment utiliser les dispositions pour créer un aspect cohérent des pages sur un site qui utilise pages Web ASP.NET. Il part du principe que vous avez terminé...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526690"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Présentation de la pages Web ASP.NET de la création d’une disposition cohérente

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment utiliser les *dispositions* pour créer un aspect cohérent des pages sur un site qui utilise pages Web ASP.net. Elle suppose que vous avez terminé la série [en supprimant les données de la base de données dans pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Ce que vous allez apprendre :
> 
> - Ce qu’est une page de disposition.
> - Comment combiner des pages de disposition avec du contenu dynamique.
> - Comment passer des valeurs à une page de disposition.

## <a name="about-layouts"></a>À propos des dispositions

Les pages que vous avez créées jusqu’à présent ont toutes été des pages autonomes. Ils appartiennent tous au même site, mais ils n’ont pas d’éléments communs ou de présentation standard.

La plupart des sites ont une apparence et une disposition cohérentes. Par exemple, si vous accédez au site [Microsoft.com/Web](https://www.microsoft.com/web/) et que vous recherchez, vous constatez que les pages adhèrent à une disposition générale et à un thème visuel :

![Page de site Microsoft.com/web qui indique la disposition de l’en-tête, de la zone de navigation, de la zone de contenu et du pied de page](layouts/_static/image1.png)

Une méthode *inefficace* pour créer cette disposition consiste à définir séparément un en-tête, une barre de navigation et un pied de page sur chacune de vos pages. Vous devez dupliquer chaque fois le même balisage. Si vous souhaitez modifier une valeur (par exemple, mettre à jour le pied de page), vous devez modifier chaque page séparément.

C’est là qu’interviennent les *pages de disposition* . Dans pages Web ASP.NET, vous pouvez définir une page de disposition qui fournit un conteneur global pour les pages de votre site. Par exemple, la page de disposition peut contenir l’en-tête, la zone de navigation et le pied de page. La page de disposition comprend un espace réservé dans lequel se trouve le contenu principal.

Vous pouvez ensuite définir des pages de contenu individuelles qui contiennent le balisage et le code uniquement pour cette page. Les pages de contenu n’ont pas besoin d’être des pages HTML complètes ; ils n’ont même pas besoin d’un élément `<body>`. Ils ont également une ligne de code qui indique à ASP.NET la page de disposition dans laquelle vous souhaitez afficher le contenu. Voici une image qui montre le fonctionnement de cette relation de manière approximative :

![Diagramme conceptuel qui affiche deux pages de contenu et une page de disposition dans laquelle elles s’ajustent](layouts/_static/image2.png)

Cette interaction est facile à comprendre lorsque vous la voyez en action. Dans ce didacticiel, vous allez modifier vos pages de films pour utiliser une disposition.

## <a name="adding-a-layout-page"></a>Ajout d’une page de disposition

Vous allez commencer par créer une page de disposition qui définit une mise en page classique avec un en-tête, un pied de page et une zone pour le contenu principal. Sur le site WebPagesMovies, ajoutez une page CSHTML nommée *\_Layout. cshtml*.

Le caractère de trait de soulignement de début (`_`) est significatif. Si le nom d’une page commence par un trait de soulignement, ASP.NET n’envoie pas directement cette page au navigateur. Cette Convention vous permet de définir les pages requises pour votre site, mais que les utilisateurs ne doivent pas être en mesure de demander directement.

Remplacez le contenu de la page par ce qui suit :

[!code-html[Main](layouts/samples/sample1.html)]

Comme vous pouvez le voir, ce balisage est simplement du code HTML qui utilise `<div>` éléments pour définir trois sections dans la page plus un élément `<div>` pour contenir les trois sections. Le pied de page contient un peu de code Razor : `@DateTime.Now.Year`, qui affiche l’année en cours à cet emplacement dans la page.

Notez qu’il existe un lien vers une feuille de style nommée *movies. CSS*. La feuille de style est l’endroit où les détails de la disposition physique des éléments seront définis. Vous allez le créer dans un moment.

La seule fonctionnalité inhabituelle de cette page *\_Layout. cshtml* est la `@Render.Body()` ligne. Il s’agit de l’espace réservé où le contenu sera placé lorsque cette disposition sera fusionnée avec une autre page.

## <a name="adding-a-css-file"></a>Ajout d’un fichier. CSS

La meilleure façon de définir la disposition réelle (c’est-à-dire, l’apparence) des éléments sur la page consiste à utiliser des règles de feuille de style en cascade (CSS). Vous allez donc créer un fichier *. CSS* contenant les règles de votre nouvelle disposition.

Dans WebMatrix, sélectionnez la racine de votre site. Ensuite, sous l’onglet **fichiers** du ruban, cliquez sur la flèche sous le bouton **nouveau** , puis cliquez sur **nouveau dossier**.

![L’option « nouveau dossier » sous nouveau dans le ruban.](layouts/_static/image3.png)

Nommez les nouveaux *styles*de dossiers.

![Attribution d’un nom au nouveau dossier « styles »](layouts/_static/image4.png)

Dans le nouveau dossier *styles* , créez un fichier nommé *movies. CSS*.

![Création d’un nouveau fichier movies. CSS](layouts/_static/image5.png)

Remplacez le contenu du nouveau fichier *. CSS* par ce qui suit :

[!code-css[Main](layouts/samples/sample2.css)]

Nous n’indiquons pas en grande partie ces règles CSS, sauf pour noter deux choses. L’un est qu’en plus de la définition des polices et des tailles, les règles utilisent le positionnement absolu pour établir l’emplacement de l’en-tête, du pied de page et de la zone de contenu principale. Si vous ne connaissez pas le positionnement en CSS, vous pouvez lire le didacticiel sur le [positionnement CSS](http://www.w3schools.com/css/css_positioning.asp) sur le site W3Schools.

L’autre chose à noter est que, en bas, nous avons copié les règles de style qui ont été initialement définies individuellement dans le fichier *movies. cshtml* . Ces règles ont été utilisées dans l' [Introduction à l’affichage de données à l’aide d’pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251580) didacticiel permettant de rendre le balisage d' `WebGrid` qui a ajouté des bandes à la table. (Si vous envisagez d’utiliser un fichier *. CSS* pour les définitions de style, vous pouvez également placer les règles de style pour l’ensemble du site.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Mise à jour du fichier movies pour utiliser la mise en page

Vous pouvez maintenant mettre à jour les fichiers existants de votre site pour utiliser la nouvelle disposition. Ouvrez le fichier *movies. cshtml* . En haut, comme première ligne de code, ajoutez ce qui suit :

[!code-csharp[Main](layouts/samples/sample3.cs)]

La page commence maintenant de la façon suivante :

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Cette ligne de code indique à ASP.NET que lorsque la page *movies* s’exécute, elle doit être fusionnée avec le fichier *\_Layout. cshtml* .

Étant donné que le fichier *movies. cshtml* utilise désormais une page de disposition, vous pouvez supprimer le balisage de la page *movies. cshtml* qui est pris en charge par le fichier *\_Layout. cshtml* . Prenez le `<!DOCTYPE>`, `<html>`et `<body>` balises d’ouverture et de fermeture. Prenez la totalité de l’élément `<head>` et son contenu, qui comprend les règles de style pour la grille, puisque vous disposez désormais de ces règles dans un fichier *. CSS* . Lorsque vous y êtes, remplacez l’élément `<h1>` existant par un élément `<h2>` ; vous avez déjà un élément `<h1>` dans la page de disposition. Remplacez le texte `<h2>` par « liste des films ».

Normalement, vous n’auriez pas à effectuer ces types de modifications dans une page de contenu. Lorsque vous démarrez votre site à l’aide d’une page de disposition, vous créez des pages de contenu sans que ces éléments commencent par. Dans ce cas, toutefois, vous convertissez une page autonome en une page qui utilise une disposition, ce qui signifie qu’il y a un peu de nettoyage.

Lorsque vous avez terminé, la page *movies. cshtml* ressemble à ce qui suit :

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Test de la disposition

À présent, vous pouvez voir à quoi ressemble la disposition. Dans WebMatrix, cliquez avec le bouton droit sur la page *movies. cshtml* et sélectionnez **lancer dans le navigateur**. Lorsque le navigateur affiche la page, il ressemble à cette page :

![Page films rendue à l’aide d’une disposition](layouts/_static/image6.png)

ASP.NET a fusionné le contenu de la page movies. cshtml dans\_la page *Layout. cshtml* juste à l’emplacement où la méthode `RenderBody` est. Et bien sûr, la page *Layout. cshtml de\_* fait référence à un fichier *. CSS* qui définit l’apparence de la page.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Mise à jour de la page AddMovie pour utiliser la disposition

L’avantage réel des dispositions est que vous pouvez les utiliser pour toutes les pages de votre site. Ouvrez la page *AddMovie. cshtml* .

Vous vous souvenez peut-être que la page *AddMovie. cshtml* contenait à l’origine des règles CSS pour définir l’apparence des messages d’erreur de validation. Dans la mesure où vous disposez d’un fichier *. CSS* pour votre site, vous pouvez déplacer ces règles vers le fichier *. CSS* . Supprimez-les du fichier *AddMovie. cshtml* et ajoutez-les au bas du fichier *movies. CSS* . Vous déplacez les règles suivantes :

[!code-css[Main](layouts/samples/sample6.css)]

Effectuez maintenant les mêmes types de modifications dans *AddMovie. cshtml* que pour *movies. cshtml* : ajoutez `Layout="~/_Layout.cshtml;` et supprimez le balisage HTML qui est désormais superflu. Remplacez l’élément `<h1>` par `<h2>`. Lorsque vous avez terminé, la page ressemblera à l’exemple suivant :

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Exécutez la page. À présent, il ressemble à l’illustration suivante :

![Page « Ajouter des films » rendue à l’aide d’une disposition](layouts/_static/image7.png)

Vous souhaitez apporter des modifications similaires aux pages du site, *EditMovie. cshtml* et *DeleteMovie. cshtml*. Toutefois, avant de procéder à cette opération, vous pouvez apporter une autre modification à la disposition qui le rend un peu plus flexible.

## <a name="passing-title-information-to-the-layout-page"></a>Passage des informations de titre à la page de disposition

\_la page *Layout. cshtml* que vous avez créée a un élément `<title>` défini sur « mon site de film ». La plupart des navigateurs affichent le contenu de cet élément sous forme de texte sur un onglet :

![Élément de&gt; de titre &lt;de la page affiché dans un onglet de navigateur](layouts/_static/image8.png)

Ces informations de titre sont génériques. Supposons que vous souhaitiez que le texte du titre soit plus spécifique à la page actuelle. (Le texte du titre est également utilisé par les moteurs de recherche pour déterminer l’objet de votre page.) Vous pouvez transmettre des informations à partir d’une page de contenu comme *movies. cshtml* ou *AddMovie. cshtml* à la page de disposition, puis utiliser ces informations pour personnaliser ce que la page de disposition rend.

Ouvrez de nouveau la page *movies. cshtml* . Dans le code en haut, ajoutez la ligne suivante :

[!code-csharp[Main](layouts/samples/sample8.cs)]

L’objet `Page` est disponible sur toutes les pages *. cshtml* et est à cet effet, à savoir, pour partager des informations entre une page et sa disposition.

Ouvrez la page *Layout. cshtml\_* . Modifiez l’élément `<title>` afin qu’il ressemble à ce balisage :

[!code-html[Main](layouts/samples/sample9.html)]

Ce code affiche tout ce qui se trouve dans la propriété `Page.Title` directement à cet emplacement dans la page.

Exécutez la page *movies. cshtml* . Cette fois, l’onglet Browser affiche ce que vous avez passé comme valeur de `Page.Title`:

![Onglet de navigateur qui indique le titre créé dynamiquement](layouts/_static/image9.png)

Si vous le souhaitez, affichez la page source dans le navigateur. Vous pouvez voir que l’élément `<title>` est restitué en tant que `<title>List Movies</title>`.

> [!TIP] 
> 
> **Objet page**
> 
> Une fonctionnalité utile de `Page` est qu’il s’agit d’un objet dynamique : la propriété `Title` n’est pas un nom fixe ou réservé. Vous pouvez utiliser *n’importe quel* nom pour une valeur de l’objet `Page`. Par exemple, vous pouvez aussi facilement avoir passé le titre à l’aide d’une propriété nommée `Page.CurrentName` ou `Page.MyPage`. La seule restriction est que le nom doit respecter les règles normales pour les propriétés qui peuvent être nommées. (Par exemple, le nom ne peut pas contenir d’espace.)
> 
> Vous pouvez passer n’importe quel nombre de valeurs à l’aide de l’objet `Page`. Si vous souhaitez transmettre des informations de film à la page de disposition, vous pouvez passer des valeurs à l’aide d’un `Page.MovieTitle` et `Page.Genre` et `Page.MovieYear`. (Ou tout autre nom que vous avez inventé pour stocker les informations.) La seule exigence, ce qui est probablement évidente, est que vous devez utiliser les mêmes noms dans la page de contenu et la page de disposition.
> 
> Les informations que vous transmettez à l’aide de l’objet `Page` ne sont pas limitées au simple texte à afficher sur la page de disposition. Vous pouvez passer une valeur à la page de disposition, puis le code dans la page de disposition peut utiliser la valeur pour décider s’il faut afficher une section de la page, le fichier *. CSS* à utiliser, etc. Les valeurs que vous transmettez dans l’objet `Page` ressemblent à toutes les autres valeurs que vous utilisez dans le code. Il s’agit simplement de faire en sorte que les valeurs proviennent de la page de contenu et soient passées à la page de disposition.

Ouvrez la page *AddMovie. cshtml* et ajoutez une ligne en haut du code qui fournit un titre pour la page *AddMovie. cshtml* :

[!code-csharp[Main](layouts/samples/sample10.cs)]

Exécutez la page *AddMovie. cshtml* . Vous voyez ici le nouveau titre :

![Onglet de navigateur présentant le titre « ajouter des films » créé dynamiquement](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Mise à jour des pages restantes pour utiliser la disposition

Vous pouvez maintenant terminer les pages restantes de votre site afin qu’elles utilisent la nouvelle disposition. Ouvrez *EditMovie. cshtml* et *DeleteMovie. cshtml* à tour de main et apportez les mêmes modifications dans chacun d’entre eux.

Ajoutez la ligne de code qui établit un lien vers la page de disposition :

[!code-csharp[Main](layouts/samples/sample11.cs)]

Ajoutez une ligne pour définir le titre de la page :

[!code-csharp[Main](layouts/samples/sample12.cs)]

ou :

[!code-csharp[Main](layouts/samples/sample13.cs)]

Supprimez tout le balisage HTML superflu, en fait, laissez uniquement les bits qui se trouvent à l’intérieur de l’élément `<body>` (plus le bloc de code en haut).

Remplacez l’élément `<h1>` par un élément `<h2>`.

Une fois ces modifications effectuées, testez chacune d’elles et assurez-vous qu’elles s’affichent correctement et que le titre est correct.

## <a name="parting-thoughts-about-layout-pages"></a>Commentaires sur les pages de disposition

Dans ce didacticiel, vous avez créé une page *Layout. cshtml\_* et utilisé la méthode `RenderBody` pour fusionner le contenu d’une autre page. Il s’agit du modèle de base pour l’utilisation de dispositions dans des pages Web.

Les pages de disposition ont des fonctionnalités supplémentaires que nous n’avions pas abordées ici. Par exemple, vous pouvez imbriquer des pages de disposition ; une page de disposition peut à son tour faire référence à une autre. Les dispositions imbriquées peuvent être utiles si vous utilisez des sous-sections d’un site qui nécessitent des dispositions différentes. Vous pouvez également utiliser des méthodes supplémentaires (par exemple, `RenderSection`) pour configurer des sections nommées dans la page de disposition.

La combinaison des pages de disposition et des fichiers *. CSS* est puissante. Comme vous le verrez dans la série de didacticiels suivante, dans WebMatrix, vous pouvez créer un site basé sur un *modèle*, ce qui vous donne un site avec des fonctionnalités prédéfinies. Les modèles utilisent correctement les pages de disposition et CSS pour créer des sites qui s’affichent parfaitement et qui possèdent des fonctionnalités telles que des menus. Voici une capture d’écran de la page d’hébergement d’un site basé sur un modèle, montrant des fonctionnalités qui utilisent des pages de disposition et CSS :

![Disposition créée par le modèle de site WebMatrix, avec l’en-tête, la zone de navigation, la zone de contenu, la section facultative et les liens de connexion](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Liste complète des pages de films (mise à jour pour utiliser une page de disposition)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Liste complète des pages pour la page Ajouter un film (mise à jour de la disposition)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Liste complète des pages pour la page de suppression de la vidéo (mise à jour pour la disposition)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Liste complète des pages pour la page de modification de la vidéo (mise à jour de la disposition)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>À venir suivant

Dans le didacticiel suivant, vous apprendrez à publier votre site sur Internet afin que tout le monde puisse le voir.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Création d’une apparence cohérente](https://go.microsoft.com/fwlink/?LinkID=202891) : un article qui fournit plus de détails sur l’utilisation des dispositions. Elle explique également comment passer une valeur à une page de disposition qui affiche ou masque une partie du contenu.
- [Pages de disposition imbriquées avec Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike saumured Blogs montre comment imbriquer des pages de disposition. (Comprend un téléchargement des pages.)

> [!div class="step-by-step"]
> [Précédent](deleting-data.md)
> [Suivant](publishing.md)
