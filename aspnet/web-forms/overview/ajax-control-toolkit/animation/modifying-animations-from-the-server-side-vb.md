---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modification des animations du côté serveur (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597999"
---
# <a name="modifying-animations-from-the-server-side-vb"></a>Modification des animations du côté serveur (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également être modifiées côté serveur

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également être modifiées côté serveur

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Le reste du code s’exécute côté serveur et n’utilise pas le balisage ; au lieu de cela, il utilise du code pour créer le contrôle `AnimationExtender` :

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Toutefois, le kit d’outils de contrôle ne fournit pas actuellement d’accès d’API pour créer les animations individuelles. Il est toutefois possible de définir la propriété animations de `AnimationExtender`sur une chaîne contenant le balisage XML utilisé pour assigner les animations de façon déclarative. Pour créer le fichier XML qui ne doit pas contenir l’élément `<Animations>` vous pouvez utiliser la prise en charge XML de .NET Framework ou, comme dans le code suivant, il vous suffit de fournir la chaîne :

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Enfin, ajoutez le contrôle `AnimationExtender` à la page active, au sein de l’élément `<form runat="server">`, en vous assurant que l’animation est incluse et s’exécute :

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

[![l’animation est créée à l’aide du C#code/VB côté serveur](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

L’animation est créée à l’aide du C#code/VB côté serveur ([cliquez pour afficher l’image en taille réelle](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](triggering-an-animation-in-another-control-vb.md)
> [Suivant](executing-animations-using-client-side-code-vb.md)
