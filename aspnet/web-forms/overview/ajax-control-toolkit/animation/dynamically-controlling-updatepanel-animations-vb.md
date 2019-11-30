---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Contrôle dynamique des animations UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 97a52bd75fdf63ad62282afd9df772f0a9e4f931
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599686"
---
# <a name="dynamically-controlling-updatepanel-animations-vb"></a>Contrôle dynamique des animations UpdatePanel (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un UpdatePanel, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation. Il peut également fonctionner conjointement avec les déclencheurs UpdatePanel.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un `UpdatePanel`, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`. Il peut également fonctionner conjointement avec les déclencheurs `UpdatePanel`.

## <a name="steps"></a>Étapes

La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à l’affichage de l’heure actuelle. Ces informations peuvent être écrites dans une étiquette à l’aide de la méthode `Page_Load()`, ou (par souci de simplicité) le code inline suivant est utilisé :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

En outre, un bouton pour déclencher la mise à jour de l’heure est créé :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Ce code est ensuite placé dans la section `<ContentTemplate>` d’un élément `UpdatePanel`. L’attribut `UpdateMode` du panneau doit être défini sur `"Conditional"`, car seuls les déclencheurs peuvent mettre à jour le contenu du panneau. Dans la section `<Triggers>` de l' `UpdatePanel`, un déclencheur de publication (postback) asynchrone est créé et lié à l’événement `Click` du bouton. Ainsi, si l’utilisateur clique sur le bouton, la `UpdatePanel` est actualisée. Voici le balisage pour le contrôle `UpdatePanel` :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Enfin, le `UpdatePanelAnimationExtender` doit être configuré : affectez à l’attribut `TargetControlID` l’ID du panneau et définissez une animation dans l’extendeur. Le fondu est judicieux, ce qui crée un aspect visuel agréable sur l’heure mise à jour. Le balisage de votre extendeur peut alors ressembler à ceci :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Exécutez le fichier dans le navigateur. Chaque fois que vous cliquez sur le bouton, l’heure actuelle est affichée dans le panneau, ce qui fait toujours l’objet d’un fondu pour la durée d’une seconde.

[![l’heure actuelle est en fondu](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

L’heure actuelle est en fondu ([cliquez pour afficher l’image en taille réelle](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animating-an-updatepanel-control-vb.md)
