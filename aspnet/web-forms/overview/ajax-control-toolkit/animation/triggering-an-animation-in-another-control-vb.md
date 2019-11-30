---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Déclenchement d’une animation dans un autre contrôle (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En général, le lancement d’une...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a4af2324afab7519170c123b6ea7c57ab3e03fb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575036"
---
# <a name="triggering-an-animation-in-another-control-vb"></a>Déclenchement d’une animation dans un autre contrôle (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En général, le lancement d’une animation est déclenché par l’interaction de l’utilisateur avec le même contrôle. Toutefois, il est également possible d’interagir avec un contrôle, puis d’effectuer l’animation d’un autre contrôle.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En général, le lancement d’une animation est déclenché par l’interaction de l’utilisateur avec le même contrôle. Toutefois, il est également possible d’interagir avec un contrôle, puis d’effectuer l’animation d’un autre contrôle.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Pour que vous puissiez commencer à animer le panneau, un bouton HTML est utilisé. Notez que `<input type="button" />` est favorisé sur `<asp:Button />` puisque nous ne voulons pas une publication (postback) quand l’utilisateur clique sur ce bouton.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire. Il est important de définir `TargetControlID` sur l’ID du bouton (l’élément qui déclenche l’animation), et non sur l’ID du panneau (l’élément animé).

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

Dans le nœud `<Animations>`, placez les animations comme d’habitude. Pour qu’ils modifient le panneau, pas le bouton, définissez l’attribut `AnimationTarget` pour chaque élément d’animation dans `AnimationExtender`. La valeur de `AnimationTarget` est l’ID du panneau, bien sûr. De cette façon, les animations se produisent avec le panneau, et non avec le bouton de déclenchement. Voici le balisage `AnimationExtender` pour ce scénario :

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Notez l’ordre spécial dans lequel les différentes animations apparaissent. Tout d’abord, le bouton est désactivé une fois l’animation exécutée. Étant donné qu’il n’y a pas d’attribut `AnimationTarget` dans l’élément `<EnableAction>`, cette animation est appliquée au contrôle d’origine : le bouton. Les deux étapes suivantes de l’animation doivent être effectuées en parallèle (élément`<Parallel>`). Leurs attributs `AnimationTarget` sont tous deux définis sur `"Panel1"`, ce qui permet d’animer le panneau, et non le bouton.

[![un clic de souris sur le bouton démarre l’animation du panneau](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Un clic de souris sur le bouton démarre l’animation du panneau ([cliquez pour afficher l’image en taille réelle](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](disabling-actions-during-animation-vb.md)
> [Suivant](modifying-animations-from-the-server-side-vb.md)
