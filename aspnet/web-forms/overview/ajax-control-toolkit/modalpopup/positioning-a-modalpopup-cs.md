---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Positionnement d’un ModalPopupC#() | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, le contrôle ne propose pas de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613224"
---
# <a name="positioning-a-modalpopup-c"></a>Positionnement d’un ModalPopup (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, le contrôle n’offre pas de fonctionnalité intégrée permettant de positionner la fenêtre contextuelle.

## <a name="overview"></a>Présentation

Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, le contrôle n’offre pas de fonctionnalité intégrée permettant de positionner la fenêtre contextuelle.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le `ScriptManager`. le contrôle doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle modale. Un bouton est utilisé pour fermer la fenêtre contextuelle :

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Chaque fois que la fenêtre contextuelle est affichée, elle doit être positionnée à un certain endroit de la page. Pour cette tâche, une fonction JavaScript côté client est créée. Il tente d’abord d’accéder au panneau. Si elle est réussie, la position du panneau est définie à l’aide de CSS et JavaScript (modifier la position de la fenêtre contextuelle à la place). Toutefois, le contrôle `ModalPopupExtender` tente également de positionner la fenêtre contextuelle. Par conséquent, le code JavaScript positionne à plusieurs reprises la fenêtre contextuelle, tous les dixièmes de seconde.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Comme vous pouvez le voir, la valeur de retour de la méthode JavaScript `setTimeout()` est enregistrée dans une variable globale. Cela permet d’arrêter la position répétée de la fenêtre contextuelle à la demande, à l’aide de la méthode `clearTimeout()` :

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Maintenant, il ne reste plus qu’à faire appel à ces fonctions par le navigateur chaque fois que cela est nécessaire. La fonction JavaScript `movePanel()` doit être appelée lorsque l’utilisateur clique sur le bouton pour déclencher le panneau :

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

Et la fonction `stopMoving()` entre en lecture lorsque la fenêtre contextuelle est fermée, vous pouvez le déclencher dans le contrôle `ModalPopupExtender` :

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

[![la fenêtre contextuelle modale apparaît à la position désignée](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

La fenêtre contextuelle modale apparaît à la position désignée ([cliquez pour afficher l’image en taille réelle](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-modalpopup-cs.md)
> [Suivant](launching-a-modal-popup-window-from-server-code-vb.md)
