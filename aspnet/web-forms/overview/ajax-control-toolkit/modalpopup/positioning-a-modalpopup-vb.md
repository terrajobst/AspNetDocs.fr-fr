---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Positionnement d’un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, le contrôle ne propose pas de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: fb79a08a339588ed8adc4b4236911819ea9286b4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598957"
---
# <a name="positioning-a-modalpopup-vb"></a>Positionnement d’un ModalPopup (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, le contrôle n’offre pas de fonctionnalité intégrée permettant de positionner la fenêtre contextuelle.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, le contrôle n’offre pas de fonctionnalité intégrée permettant de positionner la fenêtre contextuelle.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le `ScriptManager`. le contrôle doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle modale. Un bouton est utilisé pour fermer la fenêtre contextuelle :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Chaque fois que la fenêtre contextuelle est affichée, elle doit être positionnée à un certain endroit de la page. Pour cette tâche, une fonction JavaScript côté client est créée. Il tente d’abord d’accéder au panneau. Si elle est réussie, la position du panneau est définie à l’aide de CSS et JavaScript (modifier la position de la fenêtre contextuelle à la place). Toutefois, le contrôle `ModalPopupExtender` tente également de positionner la fenêtre contextuelle. Par conséquent, le code JavaScript positionne à plusieurs reprises la fenêtre contextuelle, tous les dixièmes de seconde.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Comme vous pouvez le voir, la valeur de retour de la méthode JavaScript `setTimeout()` est enregistrée dans une variable globale. Cela permet d’arrêter la position répétée de la fenêtre contextuelle à la demande, à l’aide de la méthode `clearTimeout()` :

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Maintenant, il ne reste plus qu’à faire appel à ces fonctions par le navigateur chaque fois que cela est nécessaire. La fonction JavaScript `movePanel()` doit être appelée lorsque l’utilisateur clique sur le bouton pour déclencher le panneau :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

Et la fonction `stopMoving()` entre en lecture lorsque la fenêtre contextuelle est fermée, vous pouvez le déclencher dans le contrôle `ModalPopupExtender` :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]

[![la fenêtre contextuelle modale apparaît à la position désignée](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

La fenêtre contextuelle modale apparaît à la position désignée ([cliquez pour afficher l’image en taille réelle](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-modalpopup-vb.md)
