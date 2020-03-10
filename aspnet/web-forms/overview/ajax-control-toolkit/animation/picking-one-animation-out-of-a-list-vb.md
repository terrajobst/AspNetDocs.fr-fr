---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Sélection d’une animation dans une liste (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’infrastructure impeux également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 694416532b558291ff6ab57a442058b53d6167e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536112"
---
# <a name="picking-one-animation-out-of-a-list-vb"></a>Sélection d’une animation dans une liste (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’infrastructure permet également au programmeur de sélectionner une animation dans une liste d’animations, en fonction de l’évaluation de certains codes JavaScript.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’infrastructure permet également au programmeur de sélectionner une animation dans une liste d’animations, en fonction de l’évaluation de certains codes JavaScript.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server":` obligatoire

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée. Au lieu de l’une des animations régulières, l’élément `<Case>` entre en lecture. La valeur de son attribut SelectScript est évaluée ; la valeur de retour doit être numérique. En fonction de ce nombre, l’une des sous-animations dans &lt;cas&gt; est exécutée. Par exemple, si SelectScript a la valeur 2, le Control Toolkit exécute la troisième animation dans &lt;&gt; de cas (le comptage commence à 0).

Le balisage suivant définit trois sous-animations : le redimensionnement de la largeur, le redimensionnement de la hauteur et le fondu. Le code JavaScript (`Math.floor(3 * Math.random())`) sélectionne ensuite un nombre compris entre 0 et 2, ce qui signifie que l’une des trois animations est exécutée :

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]

[![l’une des trois animations possibles : le panneau est plus grand](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

L’une des trois animations possibles : le panneau est plus large ([cliquez pour afficher l’image en taille réelle](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animation-depending-on-a-condition-vb.md)
> [Suivant](animating-in-response-to-user-interaction-vb.md)
