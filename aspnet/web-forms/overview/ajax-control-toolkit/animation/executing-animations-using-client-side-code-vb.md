---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Exécution d’animations à l’aide du code côté client (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Exécution de l’animation...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575481"
---
# <a name="executing-animations-using-client-side-code-vb"></a>Exécution d’animations avec du code côté client (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

Dans le nœud `<Animations>`, utilisez `<OnClick>` pour exécuter les animations une fois que l’utilisateur a cliqué sur le panneau. Ajoutez deux animations à exécuter en parallèle :

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

À des fins de démonstration, cette animation (et toute autre animation créée à l’aide de Control Toolkit) est exécutée à l’aide de code JavaScript, une fois la page exécutée. Tout d’abord, nous avons besoin d’accéder au contrôle `AnimationExtender`. La bibliothèque AJAX ASP.NET fournit la fonction `$find()` pour cette tâche :

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

Le contrôle `AnimationExtender` expose une API riche, y compris des méthodes dont les noms sont identiques aux gestionnaires d’événements utilisés dans le balisage XML : `OnClick()`, `OnLoad()`, etc. Par exemple, un appel de la méthode `OnClick()` exécute l’animation dans l’élément `<OnClick>` du contrôle `AnimationExtender` :

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Voici le code JavaScript côté client complet qui émule le clic sur le panneau une fois que la page a été entièrement chargée. Notez que le nom de la fonction de `pageLoad()` est utilisé, ce qui est appelé par ASP.NET AJAX une fois que la page et toutes les bibliothèques JavaScript incluses ont été chargées.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

[![l’animation s’exécute immédiatement, sans clic de souris](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

L’animation s’exécute immédiatement, sans clic de souris ([Cliquer pour afficher l’image en taille réelle](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](modifying-animations-from-the-server-side-vb.md)
> [Suivant](changing-an-animation-using-client-side-code-vb.md)
