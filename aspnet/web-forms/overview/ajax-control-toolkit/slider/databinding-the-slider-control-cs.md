---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Liaison de liaison du contrôle SliderC#() | Microsoft Docs
author: wenz
description: Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier le positio actuel...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553717"
---
# <a name="databinding-the-slider-control-c"></a>Liaison de données du contrôle Slider (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier la position actuelle du curseur à un autre contrôle ASP.NET.

## <a name="overview"></a>Présentation

Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier la position actuelle du curseur à un autre contrôle ASP.NET.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Ensuite, ajoutez deux contrôles `TextBox` à la page. L’une est transformée en curseur graphique, tandis que l’autre contiendra la position du curseur.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

L’étape suivante est déjà la dernière étape. Le contrôle `SliderExtender` de la boîte à outils de contrôle AJAX ASP.NET fait glisser un curseur de la première zone de texte et met automatiquement à jour la deuxième zone de texte lorsque la position du curseur est modifiée. Pour que cela fonctionne, l’attribut `TargetControlID` de `SliderExtender`doit être défini sur l’ID de la première zone de texte. l’attribut `BoundControlID` doit avoir pour valeur l’ID de la deuxième zone de texte.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Comme vous pouvez le voir dans le navigateur, la liaison de données fonctionne dans les deux sens : l’entrée d’une nouvelle valeur dans la zone de texte met à jour la position du curseur. Si vous définissez la deuxième zone de texte en lecture seule, vous pouvez ajouter une protection faible au champ de texte afin qu’il soit plus difficile pour l’utilisateur de mettre à jour manuellement la valeur.

[le curseur et la zone de texte de ![sont synchronisés](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Le curseur et la zone de texte sont synchronisés ([cliquez pour afficher l’image en taille réelle](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-the-slider-control-with-auto-postback-cs.md)
> [Suivant](using-the-slider-control-with-auto-postback-vb.md)
