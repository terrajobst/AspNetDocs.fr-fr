---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Partie 10 : mises à jour finales de la navigation et de la conception de site, conclusion | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 10 couvre les dernières mises à jour de la navigation et des...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539367"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Partie 10 : mises à jour finales de la navigation et de la conception de site, conclusion

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 10 couvre les dernières mises à jour de la navigation et de la conception de site, conclusion.

Nous avons effectué toutes les fonctionnalités majeures de notre site, mais nous avons toujours des fonctionnalités à ajouter à la navigation sur le site, à la page d’hébergement et à la page de navigation dans le Store.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Création de la vue partielle du résumé du panier d’achat

Nous souhaitons exposer le nombre d’éléments dans le panier d’achat de l’utilisateur sur l’ensemble du site.

![](mvc-music-store-part-10/_static/image1.png)

Nous pouvons facilement implémenter cela en créant une vue partielle qui est ajoutée à notre site. Master.

Comme indiqué précédemment, le contrôleur ShoppingCart comprend une méthode d’action CartSummary qui retourne une vue partielle :

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Pour créer la vue partielle CartSummary, cliquez avec le bouton droit sur le dossier views/ShoppingCart et sélectionnez Ajouter une vue. Nommez la vue CartSummary et activez la case à cocher créer une vue partielle, comme indiqué ci-dessous.

![](mvc-music-store-part-10/_static/image2.png)

La vue partielle CartSummary est vraiment simple : il s’agit simplement d’un lien vers la vue d’index ShoppingCart qui affiche le nombre d’éléments dans le panier. Le code complet pour CartSummary. cshtml est le suivant :

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Nous pouvons inclure une vue partielle dans n’importe quelle page du site, y compris le site principal, à l’aide de la méthode html. RenderAction. RenderAction requiert que nous spécifiions le nom de l’action (« CartSummary ») et le nom du contrôleur (« ShoppingCart ») comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Avant de l’ajouter à la disposition du site, nous allons également créer le menu genre pour que nous puissions effectuer toutes nos mises à jour de site. Master à un moment donné.

## <a name="creating-the-genre-menu-partial-view"></a>Création de la vue partielle du menu genre

Nous pouvons simplifier considérablement la navigation dans le magasin en ajoutant un menu genre qui répertorie tous les genres disponibles dans notre magasin.

![](mvc-music-store-part-10/_static/image3.png)

Nous suivons également les mêmes étapes pour créer une vue partielle GenreMenu, puis nous pouvons les ajouter à la fois sur le site maître. Tout d’abord, ajoutez l’action de contrôleur GenreMenu suivante à StoreController :

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Cette action renvoie la liste des genres qui seront affichés par la vue partielle, que nous allons ensuite créer.

*Remarque : nous avons ajouté l’attribut [ChildActionOnly] à cette action de contrôleur, ce qui indique que cette action doit uniquement être utilisée à partir d’une vue partielle. Cet attribut empêchera l’exécution de l’action du contrôleur en accédant à/Store/GenreMenu. Cela n’est pas obligatoire pour les vues partielles, mais c’est une bonne pratique, car nous voulons nous assurer que nos actions de contrôleur sont utilisées comme nous l’avons prévu. Nous revenons également à PartialView plutôt qu’à View, ce qui permet au moteur de vue de savoir qu’il ne doit pas utiliser la disposition pour cette vue, car elle est incluse dans d’autres vues.*

Cliquez avec le bouton droit sur l’action du contrôleur GenreMenu et créez une vue partielle nommée GenreMenu, qui est fortement typée à l’aide de la classe de données de la vue de genre, comme indiqué ci-dessous.

![](mvc-music-store-part-10/_static/image4.png)

Mettez à jour le code de vue pour la vue partielle GenreMenu pour afficher les éléments à l’aide d’une liste non triée comme suit.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Mise à jour de la disposition du site pour afficher nos vues partielles

Nous pouvons ajouter des vues partielles à la disposition du site (/Views/Shared/\_Layout. cshtml) en appelant html. RenderAction (). Nous les ajouterons dans, ainsi que des balises supplémentaires pour les afficher, comme indiqué ci-dessous :

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Maintenant, lorsque nous exécutons l’application, nous verrons le genre dans la zone de navigation gauche et le résumé du panier en haut.

## <a name="update-to-the-store-browse-page"></a>Mettre à jour vers la page de navigation du Store

La page de navigation dans le magasin est fonctionnelle, mais ne semble pas très bonne. Nous pouvons mettre à jour la page pour afficher les albums dans une meilleure disposition en mettant à jour le code de vue (disponible dans/Views/Store/Browse.cshtml) comme suit :

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Ici, nous utilisons URL. action plutôt que html. ActionLink afin de pouvoir appliquer une mise en forme spéciale au lien pour inclure l’illustration de l’album.

*Remarque : nous affichons une couverture d’album générique pour ces albums. Ces informations sont stockées dans la base de données et sont modifiables via le responsable du magasin. Vous êtes invité à ajouter votre propre illustration.*

Maintenant, lorsque nous parcourons un genre, nous verrons les albums affichés dans une grille avec l’illustration de l’album.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Mise à jour de la page d’hébergement pour afficher les meilleurs albums de vente

Nous souhaitons utiliser nos principaux Albums de vente sur la page d’hébergement pour augmenter les ventes. Nous allons apporter des mises à jour à notre HomeController pour les gérer, et ajouter également des graphiques supplémentaires.

Tout d’abord, nous allons ajouter une propriété de navigation à notre classe album afin que l’EntityFramework sache qu’ils sont associés. Les dernières lignes de notre classe **album** doivent maintenant ressembler à ceci :

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Remarque : cette opération nécessite l’ajout d’une instruction using pour importer l’espace de noms System. Collections. Generic.*

Tout d’abord, nous allons ajouter un champ storeDB et les instructions MvcMusicStore. Models à l’aide d’instructions, comme dans nos autres contrôleurs. Ensuite, nous allons ajouter la méthode suivante au HomeController qui interroge notre base de données pour trouver les meilleurs albums de vente en fonction de OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Il s’agit d’une méthode privée, car nous ne voulons pas la rendre disponible en tant qu’action de contrôleur. Nous l’incluons dans le HomeController pour des raisons de simplicité, mais il est recommandé de déplacer votre logique métier dans des classes de service distinctes, le cas échéant.

Cela étant en place, nous pouvons mettre à jour l’action du contrôleur d’index pour interroger les 5 premiers albums de vente et les renvoyer à la vue.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Le code complet pour le HomeController mis à jour est comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Enfin, nous devrons mettre à jour notre vue d’index de base afin de pouvoir afficher une liste d’albums en mettant à jour le type de modèle et en ajoutant la liste d’albums en bas. Nous allons également ajouter un titre et une section de promotion à la page.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Maintenant, lorsque nous exécutons l’application, nous verrons notre page d’hébergement mise à jour avec les meilleurs albums de vente et notre message promotionnel.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusion

Nous avons vu que ASP.NET MVC facilite la création d’un site Web sophistiqué avec accès aux bases de données, appartenance, AJAX, etc. assez rapidement. Nous espérons que ce didacticiel vous a donné les outils dont vous avez besoin pour commencer à créer vos propres applications ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-9.md)
