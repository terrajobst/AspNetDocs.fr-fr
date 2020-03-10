---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animation selon une condition (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Si une animation est...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598349"
---
# <a name="animation-depending-on-a-condition-c"></a>Animation dépendant d’une condition (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le fait qu’une animation soit exécutée ou non peut également dépendre d’une condition sous forme de code JavaScript.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le fait qu’une animation soit exécutée ou non peut également dépendre d’une condition sous forme de code JavaScript.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server":` obligatoire

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée. Au lieu de l’une des animations régulières, l’élément `<Condition>` entre en lecture. Le code JavaScript fourni comme valeur de l’attribut `ConditionScript` est exécuté au moment de l’exécution. Si elle prend la valeur true, l’animation est exécutée, sinon. Le balisage suivant fournit deux animations, chacune étant exécutée dans 50% des cas sur Random. Étant donné qu’il ne peut y avoir qu’une seule animation dans `<OnLoad>`, les deux animations `<Condition>` sont jointes à l’aide de l’élément `<Sequence>` :

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Notez que le signe inférieur à (`<`) dans l’attribut `ConditionScript` doit être placé dans une séquence d’échappement (). Quand vous exécutez ce script, aucune animation n’est exécutée, ou l’une des deux, ou les deux.

[![le panneau s’exclut sans le redimensionnement, la deuxième animation s’exécute, la première ne l’a pas fait.](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Le panneau s’affiche sans redimensionnement, de sorte que la deuxième animation s’exécute, la première ne l’a pas fait ([cliquez pour afficher l’image en taille réelle](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](executing-several-animations-after-each-other-cs.md)
> [Suivant](picking-one-animation-out-of-a-list-cs.md)
