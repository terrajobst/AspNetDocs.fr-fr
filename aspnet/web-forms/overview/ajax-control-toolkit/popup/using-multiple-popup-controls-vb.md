---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Utilisation de plusieurs contrôles Popup (VB) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Il est également possible d’utiliser m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612531"
---
# <a name="using-multiple-popup-controls-vb"></a>Utilisation de plusieurs contrôles de fenêtre contextuelle (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Il est également possible d’utiliser plusieurs contrôles Popup sur une page.

## <a name="overview"></a>Présentation

L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Il est également possible d’utiliser plusieurs contrôles Popup sur une page.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle. Dans le scénario actuel, le panneau contient un contrôle de `Calendar`. Afin d’éviter l’actualisation des pages provoquée par les publications du calendrier, le panneau est placé dans un contrôle `UpdatePanel` :

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

La page contient également deux zones de texte. Pour chaque zone de texte, la fenêtre contextuelle du calendrier s’affiche une fois que la zone de texte est activée.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

À présent, étendez chacune des deux zones de texte avec une `PopupControlExtender`. L’attribut `TargetControlID` fournit l’ID du contrôle lié à l’extendeur. L’attribut `PopupControlID` contient l’ID du panneau contextuel. Dans ce cas, les deux extendeurs affichent le même panneau, mais des panneaux différents sont également possibles.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

À présent, lorsque vous cliquez dans un champ de texte, un calendrier apparaît sous le champ, ce qui vous permet de sélectionner une date. (L’extraction de la date sélectionnée dans les zones de texte sera traitée dans un autre didacticiel.)

[![le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Suivant](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
