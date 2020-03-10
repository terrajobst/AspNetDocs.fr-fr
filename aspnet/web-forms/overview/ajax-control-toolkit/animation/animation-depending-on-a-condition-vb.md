---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animation selon une condition (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Si une animation est...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614344"
---
# <a name="animation-depending-on-a-condition-vb"></a>Animation dépendant d’une condition (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le fait qu’une animation soit exécutée ou non peut également dépendre d’une condition sous forme de code JavaScript.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le fait qu’une animation soit exécutée ou non peut également dépendre d’une condition sous forme de code JavaScript.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server":` obligatoire

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée. Au lieu de l’une des animations régulières, l’élément `<Condition>` entre en lecture. Le code JavaScript fourni comme valeur de l’attribut `ConditionScript` est exécuté au moment de l’exécution. Si elle prend la valeur true, l’animation est exécutée, sinon. Le balisage suivant fournit deux animations, chacune étant exécutée dans 50% des cas sur Random. Étant donné qu’il ne peut y avoir qu’une seule animation dans `<OnLoad>`, les deux animations `<Condition>` sont jointes à l’aide de l’élément `<Sequence>` :

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Notez que le signe inférieur à (`<`) dans l’attribut `ConditionScript` doit être placé dans une séquence d’échappement (). Quand vous exécutez ce script, aucune animation n’est exécutée, ou l’une des deux, ou les deux.

[![le panneau s’exclut sans le redimensionnement, la deuxième animation s’exécute, la première ne l’a pas fait.](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Le panneau s’affiche sans redimensionnement, de sorte que la deuxième animation s’exécute, la première ne l’a pas fait ([cliquez pour afficher l’image en taille réelle](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](executing-several-animations-after-each-other-vb.md)
> [Suivant](picking-one-animation-out-of-a-list-vb.md)
