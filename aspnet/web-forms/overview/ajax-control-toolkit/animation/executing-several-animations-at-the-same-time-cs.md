---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Exécution de plusieurs animations en même temps (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter Severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: fe71feccbcbc4ee8e9cdc09d6220de6a53dd2d2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598118"
---
# <a name="executing-several-animations-at-the-same-time-c"></a>Exécution de plusieurs animations en même temps (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations de manière parallèle.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations de manière parallèle.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée. En règle générale, `<OnLoad>` n’accepte qu’une seule animation. L’infrastructure d’animation vous permet de joindre plusieurs animations à un seul à l’aide de l’élément `<Parallel>`. Toutes les animations au sein de `<Parallel>` sont exécutées en même temps.

Voici le balisage possible pour le contrôle `AnimationExtender`, le fondu et le redimensionnement du panneau en même temps :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

Et en effet : lorsque vous exécutez ce script, le panneau s’affiche, puis le redimensionnement (plus que la largeur et la moitié de sa hauteur) et disparaît en fondu en même temps.

[![le panneau est en fondu et en redimensionnement (y compris son contenu, grâce au moteur de rendu du navigateur)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Le panneau est en fondu et en redimensionnement (y compris son contenu, grâce au moteur de rendu du navigateur) ([cliquez pour afficher l’image en plein écran](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](adding-animation-to-a-control-cs.md)
> [Suivant](executing-several-animations-after-each-other-cs.md)
