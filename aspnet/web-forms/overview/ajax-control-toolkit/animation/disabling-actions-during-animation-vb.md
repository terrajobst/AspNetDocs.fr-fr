---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Désactivation d’actions pendant une animation (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge l’action...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606843"
---
# <a name="disabling-actions-during-animation-vb"></a>Désactivation d’actions pendant une animation (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge les actions, telles que les clics de souris. Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge les actions, telles que les clics de souris. Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

L’animation sera appliquée à un bouton HTML comme suit :

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Notez qu’un contrôle HTML est utilisé à la place d’un contrôle Web dans la mesure où nous ne voulons pas que le bouton crée une publication (postback); il lancera simplement l’animation côté client pour nous.

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Dans le nœud `<Animations>`, `<OnClick>` est l’élément approprié pour gérer le clic de souris. Toutefois, vous pouvez également cliquer sur le bouton pendant l’animation. L’élément `<EnableAction>` peut s’en occuper. La définition de `Enabled="false"` désactive le bouton dans le cadre de l’animation. Étant donné que nous utilisons plusieurs animations individuelles (désactivation du bouton et des animations réelles), l’élément `<Parallel>` est requis pour coller les deux animations ensemble dans un. Voici le balisage complet pour `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Il serait également possible d’activer à nouveau le bouton après l’animation, en utilisant l’élément XML suivant à la fin de la liste :

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Toutefois, dans le scénario donné, cela serait inutile, car le bouton disparaît et n’est pas visible à la fin de l’animation.

[![le bouton est désactivé dès l’exécution de l’animation](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Le bouton est désactivé dès l’exécution de l’animation ([cliquez pour afficher l’image en taille réelle](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animating-in-response-to-user-interaction-vb.md)
> [Suivant](triggering-an-animation-in-another-control-vb.md)
