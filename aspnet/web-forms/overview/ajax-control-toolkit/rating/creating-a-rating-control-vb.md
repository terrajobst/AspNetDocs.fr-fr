---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Création d’un contrôle Rating (VB) | Microsoft Docs
author: wenz
description: De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles. Cela nécessite généralement un effort de codage, mais nous avons le...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612195"
---
# <a name="creating-a-rating-control-vb"></a>Création d’un contrôle Rating (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles. Cela nécessite généralement un effort de codage, mais nous avons la boîte à outils de contrôle à notre disposition.

## <a name="overview"></a>Présentation

De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles. Cela nécessite généralement un effort de codage, mais nous avons la boîte à outils de contrôle à notre disposition.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin (au moins) de deux types d’images : un pour un élément de notation rempli et un pour un élément d’évaluation vide. Un élément d’évaluation est généralement une étoile ou un Smiley. Pour ce scénario, vous trouverez trois fichiers, Smiley. png et Empty. png et Smiley-Done. png dans le cadre des téléchargements du code source pour ce didacticiel.

Créez ensuite un nouveau fichier ASP.NET et commencez par y ajouter un contrôle de `ScriptManager` :

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Ensuite, ajoutez le contrôle `Rating` à partir de la boîte à outils de contrôle AJAX ASP.NET. Les attributs suivants doivent être définis pour cet exemple :

- `CurrentRating` l’évaluation initiale à utiliser
- `MaxRating` le classement maximal
- `EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (en étoile) est vide
- `FilledStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (en étoile) est rempli
- `StarCssClass` la classe CSS à utiliser pour un stat visible
- `WaitingStarCssClass` la classe CSS à utiliser pendant qu’une évaluation par étoile est renvoyée au serveur

Et voici le balisage qui crée un contrôle d’évaluation avec cinq éléments (Smileys) dont aucun n’est rempli initialement :

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Les trois classes CSS référencées doivent maintenant afficher les fichiers image appropriés, ce qui est facile à utiliser avec CSS :

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut paraître un peu.

Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données). Ajoutez donc une étiquette pour la sortie d’un message texte et un bouton Envoyer pour valider le formulaire d’évaluation sur le serveur :

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Dans le code côté serveur, accédez au contrôle d’évaluation via son `ID` puis accédez à sa propriété `CurrentRating` qui est le nombre d’éléments d’évaluation sélectionnés, dans notre exemple, une valeur comprise entre 0 et 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Enregistrez la page et chargez-la dans votre navigateur. Quand vous pointez sur les éléments d’évaluation (initialement vides), un effet JavaScript se produit : l’évaluation change. Lorsque vous cliquez sur l’ensemble d’étoiles, l’évaluation actuelle est conservée. Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère l’évaluation sélectionnée.

[![de la création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-rating-control-cs.md)
