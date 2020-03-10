---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Ajustement de l’index Z d’un DropShadow (C#) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, pour Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613882"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Ajustement de l’index-Z d’un DropShadow (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, par exemple le contrôle de menu ASP.NET. Lorsqu’une entrée de menu s’affiche, elle apparaît derrière l’ombre portée.

## <a name="overview"></a>Présentation

Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, par exemple le contrôle de menu ASP.NET. Lorsqu’une entrée de menu s’affiche, elle apparaît derrière l’ombre portée.

## <a name="steps"></a>Étapes

Le code commence par le panneau lui-même, contenant suffisamment de texte, de sorte que le panneau contient suffisamment de texte pour que l’effet soit visible :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Un autre panneau est placé directement avant le panneau `panelShadow`. Elle contient un menu avec une orientation horizontale afin que les entrées de menu s’affichent au-dessus (ou plutôt : sous) du panneau `dropShadow`) :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Ensuite, le `DropShadowExtender` est ajouté pour étendre le panneau `panelShadow` avec un effet d’ombre portée :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Enfin, le contrôle de `ScriptManager` AJAX ASP.NET permet au Control Toolkit de fonctionner :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Lorsque vous exécutez ce script, les entrées de menu s’affichent sous le panneau. Toutefois, le menu utilise la classe CSS `panel` où vous devez simplement définir deux choses pour faire apparaître les éléments devant l’autre panneau :

- Positionnement relatif
- Z-index positif

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Ensuite, le contrôle `DropShadowExtender` n’est plus en conflit avec le contrôle Menu.

[![avant : l’entrée de menu n’est pas visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Avant : l’entrée de menu n’est pas visible ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))

[![après : l’entrée de menu s’affiche](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Après : l’entrée de menu s’affiche ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](manipulating-dropshadow-properties-from-client-code-cs.md)
