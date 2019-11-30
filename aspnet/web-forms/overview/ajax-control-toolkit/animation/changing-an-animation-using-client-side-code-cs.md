---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Modification d’une animation à l’aide d’unC#code côté client () | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606980"
---
# <a name="changing-an-animation-using-client-side-code-c"></a>Changement d’une animation avec du code côté client (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également être modifiée à l’aide de code JavaScript côté client personnalisé.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également être modifiée à l’aide de code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

L’animation sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

L’animation réelle est lancée par un bouton HTML :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Notez qu’il n’y a aucun nœud `<Animations>` dans le contrôle `AnimationExtender`. Le code JavaScript personnalisé est utilisé pour fournir les animations à utiliser avec le contrôle.

Comme avec l’API serveur de `AnimationExtender`, il n’existe pas encore de méthode simple pour assigner une animation à l’extendeur. Toutefois, l’extendeur expose plusieurs méthodes pour lire et écrire des animations inscrites avec les divers événements (`OnClick`, `OnLoad`, etc.). Voici quelques exemples :

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Le format de la valeur de retour des fonctions `get_*()` et le format de l’argument pour les fonctions de `set_*()` est une chaîne JSON, fournissant une représentation d’objet de ce que serait le balisage XML. Actuellement, il n’existe aucun moyen de passer un objet dans, mais il est possible de lire un objet à partir d’une animation donnée (méthodes`get_OnXXXBehavior()`).

Voici une chaîne JSON (sans les guillemets de délimitation et joliment mis en forme) représentant une animation déclenchée par le bouton, mais en animant le panneau en le redimensionnant et en le déposant en même temps :

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Le code JavaScript suivant assigne ce descripteur JSON à l’animation `OnClick` de l’extendeur actuel et l’exécute :

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

[![l’animation s’exécute immédiatement, sans clic de souris (et avec très peu de balises)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

L’animation s’exécute immédiatement, sans clic de souris (et avec très peu de balises) ([cliquez pour afficher l’image en taille réelle](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](executing-animations-using-client-side-code-cs.md)
> [Suivant](animating-an-updatepanel-control-cs.md)
