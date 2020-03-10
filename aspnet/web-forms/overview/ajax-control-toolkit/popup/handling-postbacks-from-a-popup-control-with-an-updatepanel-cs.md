---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Gestion des publications (postback) à partir d’un contrôleC#Popup avec un UpdatePanel () | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Une attention particulière doit être prise...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b9e58d68b3d6c5d01ceaba6c01653e9574b541b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612930"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Gestion des publications (postback) à partir d’un contrôle de fenêtre contextuelle avec un UpdatePanel (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Une attention particulière doit être prise lorsqu’une publication (postback) se produit dans une telle fenêtre contextuelle.

## <a name="overview"></a>Présentation

L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Une attention particulière doit être prise lorsqu’une publication (postback) se produit dans une telle fenêtre contextuelle.

## <a name="steps"></a>Étapes

Lors de l’utilisation d’un `PopupControl` avec une publication (postback), un `UpdatePanel` peut empêcher l’actualisation de la page causée par la publication (postback). Le balisage suivant définit quelques éléments importants :

- Un contrôle `ScriptManager` pour que la boîte à outils de contrôle ASP.NET AJAX fonctionne
- Deux contrôles `TextBox` qui déclenchent tous deux un popup
- Contrôle de `Panel` qui fera office de fenêtre contextuelle
- Dans le panneau, un contrôle de `Calendar` est incorporé dans un contrôle `UpdatePanel`
- Deux contrôles `PopupControlExtender` qui affectent le panneau aux zones de texte

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Notez que l’attribut `OnSelectionChanged` du contrôle `Calendar` est défini. Par conséquent, lorsque l’utilisateur sélectionne une date dans le calendrier, une publication (postback) se produit et la méthode côté serveur `c1_SelectionChanged()` est exécutée. Dans cette méthode, la date actuelle doit être extraite et réécrite dans la zone de texte.

La syntaxe de est la suivante : tout d’abord, un objet proxy pour l' `PopupControlExtender` sur la page doit être généré. ASP.NET AJAX Control Toolkit offre la méthode `GetProxyForCurrentPopup()`. L’objet retourné par cette méthode prend en charge la méthode `Commit()` qui renvoie une valeur au contrôle qui a déclenché la fenêtre contextuelle (et non le contrôle qui a déclenché l’appel de méthode !). Le code suivant fournit la date sélectionnée comme argument pour la méthode `Commit()`, ce qui amène le code à réécrire la date sélectionnée dans la zone de texte :

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

À présent, lorsque vous cliquez sur une date du calendrier, la date sélectionnée apparaît dans la zone de texte associée et crée un contrôle de sélecteur de dates qui se trouve actuellement sur de nombreux sites Web.

[![le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))

[![cliquant sur une date l’insère dans la zone de texte](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Cliquez sur une date pour la placer dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](using-multiple-popup-controls-cs.md)
> [Suivant](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
