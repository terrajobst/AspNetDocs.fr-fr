---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustement de l’index Z d’un DropShadow (VB) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, pour Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 10495a9590ce1f25e9e3fa218ac5144268f50711
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574172"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Ajustement de l’index-Z d’un DropShadow (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, par exemple le contrôle de menu ASP.NET. Lorsqu’une entrée de menu s’affiche, elle apparaît derrière l’ombre portée.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, par exemple le contrôle de menu ASP.NET. Lorsqu’une entrée de menu s’affiche, elle apparaît derrière l’ombre portée.

## <a name="steps"></a>Étapes

Le code commence par le panneau lui-même, contenant suffisamment de texte, de sorte que le panneau contient suffisamment de texte pour que l’effet soit visible :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Un autre panneau est placé directement avant le panneau `panelShadow`. Elle contient un menu avec une orientation horizontale afin que les entrées de menu s’affichent au-dessus (ou plutôt : sous) du panneau `dropShadow`) :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Ensuite, le `DropShadowExtender` est ajouté pour étendre le panneau `panelShadow` avec un effet d’ombre portée :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Enfin, le contrôle de `ScriptManager` AJAX ASP.NET permet au Control Toolkit de fonctionner :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Lorsque vous exécutez ce script, les entrées de menu s’affichent sous le panneau. Toutefois, le menu utilise la classe CSS `panel` où vous devez simplement définir deux choses pour faire apparaître les éléments devant l’autre panneau :

- Positionnement relatif
- Z-index positif

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Ensuite, le contrôle `DropShadowExtender` n’est plus en conflit avec le contrôle Menu.

[![avant : l’entrée de menu n’est pas visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Avant : l’entrée de menu n’est pas visible ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))

[![après : l’entrée de menu s’affiche](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Après : l’entrée de menu s’affiche ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Suivant](manipulating-dropshadow-properties-from-client-code-vb.md)
