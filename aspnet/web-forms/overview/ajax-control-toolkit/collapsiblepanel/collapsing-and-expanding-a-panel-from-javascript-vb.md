---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Réduction et développement d’un panneau à partir de JavaScript (VB) | Microsoft Docs
author: wenz
description: Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: aa9779c65fb587193dbabde55cc6900283ce239d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599361"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Réduction et développement d’un panneau à partir de JavaScript (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, créez une nouvelle page ASP.NET et incluez la `ScriptManager` dans l’élément `<form>`. Cela charge la bibliothèque AJAX ASP.NET requise par Control Toolkit :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Ensuite, créez un panneau avec du texte afin que l’effet de réduction/développement puisse être affiché :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Comme vous pouvez le voir, le panneau fait référence à une classe CSS qui est présentée ici (et définit fondamentalement une couleur d’arrière-plan et la largeur du panneau) :

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

Le contrôle `CollapsiblePanelExtender` nécessite l’attribut `TargetControlID` afin que la boîte à outils sache quel volet réduire ou développer à la demande :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Malheureusement, l’extendeur n’expose pas actuellement une API spécifique pour réduire ou développer le panneau, mais certaines méthodes non documentées en font. Tout d’abord, ajoutez trois boutons HTML à la page, ce qui déclenchera le JavaScript côté client pour réduire ou développer le contenu du panneau :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

Dans le code JavaScript côté client (démarré avec `<script type="text/javascript">`), la méthode `$find()` doit être utilisée pour accéder à la `CollapsiblePanelExtender`. `$find("cpe")` retourne une référence à celui-ci. À partir de là, des méthodes spécifiques résolvent la tâche en cours.

La méthode permettant d’ouvrir (développer) le panneau est appelée `_doOpen()`; le code suivant implémente la fonction `doOpen()` appelée suite à un clic sur le premier bouton :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Pour fermer ou réduire le panneau, la méthode `_doClose()` doit être exécutée. Ainsi, quand l’utilisateur clique sur le deuxième bouton, le code JavaScript suivant est appelé :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Le troisième bouton bascule l’état du panneau : de réduit à développé et vice versa. L' `CollapsiblePanelExtender` expose la méthode `toggle()` qui effectue exactement ce qui suit : inverse l’état du panneau. Toutefois, il existe également une autre approche (qui est utilisée en interne par la méthode `toggle()`) : la méthode `get_Collapsed()` de la `CollapsiblePanelExtender()` indique si le panneau est réduit ou non. Selon la valeur de retour de cette fonction, le panneau est ensuite développé (méthode`_doOpen()`) ou méthode réduite (`_doClose()`) :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]

[![le troisième bouton modifie l’état du panneau : de réduit à développé et de retour](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Le troisième bouton modifie l’état du panneau : de réduit à développé et de retour ([cliquez pour afficher l’image en taille réelle](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](collapsing-and-expanding-a-panel-from-javascript-cs.md)
