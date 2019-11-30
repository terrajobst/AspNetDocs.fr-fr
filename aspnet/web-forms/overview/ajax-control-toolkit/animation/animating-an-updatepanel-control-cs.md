---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animation d’un contrôle UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599931"
---
# <a name="animating-an-updatepanel-control-c"></a>Animation d’un contrôle UpdatePanel (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un UpdatePanel, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation. Ce didacticiel montre comment configurer une telle animation pour un UpdatePanel.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un `UpdatePanel`, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`. Ce didacticiel montre comment configurer une telle animation pour un `UpdatePanel`.

## <a name="steps"></a>Étapes

La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à un contrôle Web `Wizard` ASP.NET résidant dans une `UpdatePanel`. Trois étapes (arbitraires) fournissent suffisamment d’options pour déclencher des publications (postback) :

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

Le balisage nécessaire pour le contrôle `UpdatePanelAnimationExtender` est assez similaire à celui utilisé pour le `AnimationExtender`. Dans l’attribut `TargetControlID`, nous fournissons le `ID` du `UpdatePanel` à animer. dans le contrôle `UpdatePanelAnimationExtender`, l’élément `<Animations>` contient le balisage XML pour la ou les animations. Toutefois, il existe une différence : la quantité d’événements et de gestionnaires d’événements est limitée par rapport à `AnimationExtender`. Pour `UpdatePanels`, seuls deux d’entre eux existent :

- `<OnUpdated>` lorsque UpdatePanel a été mis à jour
- `<OnUpdating>` lors du démarrage de la mise à jour de UpdatePanel

Dans ce scénario, le nouveau contenu du `UpdatePanel` (après la publication) s’estompe. Il s’agit du balisage nécessaire pour ce qui suit :

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Maintenant, chaque fois qu’une publication (postback) se produit dans le UpdatePanel, le nouveau contenu du panneau s’estompe en douceur.

[![l’étape suivante de l’Assistant est en fondu](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

L’étape suivante de l’Assistant est en fondu ([cliquez pour afficher l’image en taille réelle](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](changing-an-animation-using-client-side-code-cs.md)
> [Suivant](dynamically-controlling-updatepanel-animations-cs.md)
