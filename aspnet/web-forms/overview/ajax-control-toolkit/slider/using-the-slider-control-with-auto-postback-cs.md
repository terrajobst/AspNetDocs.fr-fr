---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Utilisation du contrôle Slider avec la publication automatique (C#) | Microsoft Docs
author: wenz
description: Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible d’effectuer la publication par le curseur...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553619"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a>Utilisation du contrôle Slider avec publication automatique (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible de faire de la publication automatique du curseur une fois que sa valeur est modifiée.

## <a name="overview"></a>Présentation

Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible de faire de la publication automatique du curseur une fois que sa valeur est modifiée.

## <a name="steps"></a>Étapes

Pour que le curseur effectue automatiquement une publication automatique sur une modification, les deux zones de texte ont besoin de l’attribut `AutoPostBack="true"`: la zone de texte qui deviendra le curseur lui-même et la zone de texte qui contient la position du curseur. Voici le balisage requis pour ce qui suit :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Le contrôle `SliderExtender` de la boîte à outils de contrôle AJAX ASP.NET affecte la fonctionnalité de curseur aux deux zones de texte :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Un élément label supplémentaire sera utilisé ultérieurement pour informer l’utilisateur d’une publication (postback) :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Enfin, le contrôle `ScriptManager` de ASP.NET AJAX charge le code JavaScript requis pour que Control Toolkit fonctionne :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Maintenant, le curseur est publié. côté serveur, cet événement peut être intercepté et agi sur :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

[![le déplacement du curseur déclenche une publication (postback)](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Le déplacement du curseur déclenche une publication ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image3.png))

[![par la suite, la date de cette modification est écrite dans l’étiquette](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Ensuite, la date de cette modification est écrite dans l’étiquette ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](databinding-the-slider-control-cs.md)
