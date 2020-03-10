---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Désactivation d’actions pendant l'C#animation () | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge l’action...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536266"
---
# <a name="disabling-actions-during-animation-c"></a>Désactivation d’actions pendant une animation (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge les actions, telles que les clics de souris. Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge les actions, telles que les clics de souris. Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

L’animation sera appliquée à un bouton HTML comme suit :

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Notez qu’un contrôle HTML est utilisé à la place d’un contrôle Web dans la mesure où nous ne voulons pas que le bouton crée une publication (postback); il lancera simplement l’animation côté client pour nous.

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

Dans le nœud `<Animations>`, `<OnClick>` est l’élément approprié pour gérer le clic de souris. Toutefois, vous pouvez également cliquer sur le bouton pendant l’animation. L’élément `<EnableAction>` peut s’en occuper. La définition de `Enabled="false"` désactive le bouton dans le cadre de l’animation. Étant donné que nous utilisons plusieurs animations individuelles (désactivation du bouton et des animations réelles), l’élément `<Parallel>` est requis pour coller les deux animations ensemble dans un. Voici le balisage complet pour `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Il serait également possible d’activer à nouveau le bouton après l’animation, en utilisant l’élément XML suivant à la fin de la liste :

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Toutefois, dans le scénario donné, cela serait inutile, car le bouton disparaît et n’est pas visible à la fin de l’animation.

[![le bouton est désactivé dès l’exécution de l’animation](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Le bouton est désactivé dès l’exécution de l’animation ([cliquez pour afficher l’image en taille réelle](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animating-in-response-to-user-interaction-cs.md)
> [Suivant](triggering-an-animation-in-another-control-cs.md)
