---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Contrôle dynamique des animations UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 183974564764aab9c0d8a4e577995f3c444bf2d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536203"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a>Contrôle dynamique des animations UpdatePanel (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un UpdatePanel, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation. Il peut également fonctionner conjointement avec les déclencheurs UpdatePanel.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un `UpdatePanel`, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`. Il peut également fonctionner conjointement avec les déclencheurs `UpdatePanel`.

## <a name="steps"></a>Étapes

La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à l’affichage de l’heure actuelle. Ces informations peuvent être écrites dans une étiquette à l’aide de la méthode `Page_Load()`, ou (par souci de simplicité) le code inline suivant est utilisé :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

En outre, un bouton pour déclencher la mise à jour de l’heure est créé :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Ce code est ensuite placé dans la section `<ContentTemplate>` d’un élément `UpdatePanel`. L’attribut `UpdateMode` du panneau doit être défini sur `"Conditional"`, car seuls les déclencheurs peuvent mettre à jour le contenu du panneau. Dans la section `<Triggers>` de l' `UpdatePanel`, un déclencheur de publication (postback) asynchrone est créé et lié à l’événement `Click` du bouton. Ainsi, si l’utilisateur clique sur le bouton, la `UpdatePanel` est actualisée. Voici le balisage pour le contrôle `UpdatePanel` :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Enfin, le `UpdatePanelAnimationExtender` doit être configuré : affectez à l’attribut `TargetControlID` l’ID du panneau et définissez une animation dans l’extendeur. Le fondu est judicieux, ce qui crée un aspect visuel agréable sur l’heure mise à jour. Le balisage de votre extendeur peut alors ressembler à ceci :

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Exécutez le fichier dans le navigateur. Chaque fois que vous cliquez sur le bouton, l’heure actuelle est affichée dans le panneau, ce qui fait toujours l’objet d’un fondu pour la durée d’une seconde.

[![l’heure actuelle est en fondu](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

L’heure actuelle est en fondu ([cliquez pour afficher l’image en taille réelle](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animating-an-updatepanel-control-cs.md)
> [Suivant](adding-animation-to-a-control-vb.md)
