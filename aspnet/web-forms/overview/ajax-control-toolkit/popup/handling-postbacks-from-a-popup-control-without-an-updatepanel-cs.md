---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Gestion des publications (postback) à partir d’un contrôleC#Popup sans UpdatePanel () | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Lorsqu’une publication (postback) se produit dans la variable su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598727"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Gestion des publications (postback) à partir d’un contrôle de fenêtre contextuelle sans un UpdatePanel (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Lorsqu’une publication (postback) se produit dans un tel panneau et qu’il y a plusieurs panneaux sur la page, il est difficile de déterminer le panneau sur lequel l’utilisateur a cliqué.

## <a name="overview"></a>Vue d'ensemble de

L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Lorsqu’une publication (postback) se produit dans un tel panneau et qu’il y a plusieurs panneaux sur la page, il est difficile de déterminer le panneau sur lequel l’utilisateur a cliqué.

## <a name="steps"></a>Étapes

Lors de l’utilisation d’un `PopupControl` avec une publication (postback), mais sans avoir d' `UpdatePanel` sur la page, le Control Toolkit n’offre aucun moyen de déterminer quel élément client a déclenché la fenêtre contextuelle qui a à son tour provoqué la publication (postback). Toutefois, une petite astuce fournit une solution de contournement pour ce scénario.

Tout d’abord, voici la configuration de base : deux zones de texte qui déclenchent la même fenêtre contextuelle, un calendrier. Deux `PopupControlExtenders` regroupent des zones de texte et des fenêtres contextuelles.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

L’idée de base est d’ajouter un champ de formulaire masqué dans le &lt;`form`&gt; élément qui contient la zone de texte qui a lancé la fenêtre contextuelle :

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Lorsque la page est chargée, le code JavaScript ajoute un gestionnaire d’événements aux deux zones de texte : chaque fois que l’utilisateur clique sur une zone de texte, son nom est écrit dans le champ de formulaire masqué :

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

Dans le code côté serveur, la valeur du champ masqué doit être lue. Étant donné que les champs de formulaire masqués sont faciles à manipuler, une approche de liste blanche pour valider la valeur cachée est requise. Une fois que la zone de texte correcte a été identifiée, la date du calendrier est écrite dans celle-ci.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

[![le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))

[![cliquant sur une date l’insère dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Cliquez sur une date pour la placer dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Suivant](using-multiple-popup-controls-vb.md)
