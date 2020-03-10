---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animation en réponse à l’interaction de l'C#utilisateur () | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent être en étoile...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614372"
---
# <a name="animating-in-response-to-user-interaction-c"></a>Animation en réponse à une interaction utilisateur (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent démarrer automatiquement ou peuvent être déclenchées par l’intervention de l’utilisateur, par exemple en cliquant avec la souris.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent démarrer automatiquement ou peuvent être déclenchées par l’intervention de l’utilisateur, par exemple en cliquant avec la souris.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

Dans le nœud `<Animations>`, il existe cinq façons de démarrer l’animation par le biais de l’intervention de l’utilisateur (l’élément manquant est `<OnLoad>` qui est exécuté une fois que la page entière a été entièrement chargée) :

- `<OnClick>` (clic de souris sur le contrôle)
- `<OnHoverOut>` (la souris quitte le contrôle)
- `<OnHoverOver>` (pointage de la souris sur un contrôle, arrêt de l’animation `<OnHoverOut>`)
- `<OnMouseOut>` (la souris quitte un contrôle)
- `<OnMouseOver>` (pointage de la souris sur un contrôle, sans arrêter l’animation `<OnMouseOut>`)

Dans ce scénario, `<OnClick>` est utilisé. Quand l’utilisateur clique sur le panneau, il est redimensionné et disparaît en fondu en même temps.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

[![un clic de souris démarre l’animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Un clic de souris démarre l’animation ([cliquez pour afficher l’image en taille réelle](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](picking-one-animation-out-of-a-list-cs.md)
> [Suivant](disabling-actions-during-animation-cs.md)
