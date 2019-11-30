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
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599014"
---
# <a name="positioning-a-modalpopup-c"></a><span data-ttu-id="47fa3-104">Positionnement d’un ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="47fa3-104">Positioning a ModalPopup (C#)</span></span>

<span data-ttu-id="47fa3-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="47fa3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="47fa3-106">[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="47fa3-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="47fa3-107">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client.</span><span class="sxs-lookup"><span data-stu-id="47fa3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="47fa3-108">Toutefois, le contrôle n’offre pas de fonctionnalité intégrée permettant de positionner la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="47fa3-108">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="47fa3-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="47fa3-109">Overview</span></span>

<span data-ttu-id="47fa3-110">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client.</span><span class="sxs-lookup"><span data-stu-id="47fa3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="47fa3-111">Toutefois, le contrôle n’offre pas de fonctionnalité intégrée permettant de positionner la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="47fa3-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="47fa3-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="47fa3-112">Steps</span></span>

<span data-ttu-id="47fa3-113">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="47fa3-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="47fa3-114">le contrôle doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="47fa3-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="47fa3-115">Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="47fa3-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="47fa3-116">Un bouton est utilisé pour fermer la fenêtre contextuelle :</span><span class="sxs-lookup"><span data-stu-id="47fa3-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="47fa3-117">Chaque fois que la fenêtre contextuelle est affichée, elle doit être positionnée à un certain endroit de la page.</span><span class="sxs-lookup"><span data-stu-id="47fa3-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="47fa3-118">Pour cette tâche, une fonction JavaScript côté client est créée.</span><span class="sxs-lookup"><span data-stu-id="47fa3-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="47fa3-119">Il tente d’abord d’accéder au panneau.</span><span class="sxs-lookup"><span data-stu-id="47fa3-119">It first tries to access the panel.</span></span> <span data-ttu-id="47fa3-120">Si elle est réussie, la position du panneau est définie à l’aide de CSS et JavaScript (modifier la position de la fenêtre contextuelle à la place).</span><span class="sxs-lookup"><span data-stu-id="47fa3-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="47fa3-121">Toutefois, le contrôle `ModalPopupExtender` tente également de positionner la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="47fa3-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="47fa3-122">Par conséquent, le code JavaScript positionne à plusieurs reprises la fenêtre contextuelle, tous les dixièmes de seconde.</span><span class="sxs-lookup"><span data-stu-id="47fa3-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="47fa3-123">Comme vous pouvez le voir, la valeur de retour de la méthode JavaScript `setTimeout()` est enregistrée dans une variable globale.</span><span class="sxs-lookup"><span data-stu-id="47fa3-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="47fa3-124">Cela permet d’arrêter la position répétée de la fenêtre contextuelle à la demande, à l’aide de la méthode `clearTimeout()` :</span><span class="sxs-lookup"><span data-stu-id="47fa3-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="47fa3-125">Maintenant, il ne reste plus qu’à faire appel à ces fonctions par le navigateur chaque fois que cela est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="47fa3-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="47fa3-126">La fonction JavaScript `movePanel()` doit être appelée lorsque l’utilisateur clique sur le bouton pour déclencher le panneau :</span><span class="sxs-lookup"><span data-stu-id="47fa3-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="47fa3-127">Et la fonction `stopMoving()` entre en lecture lorsque la fenêtre contextuelle est fermée, vous pouvez le déclencher dans le contrôle `ModalPopupExtender` :</span><span class="sxs-lookup"><span data-stu-id="47fa3-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

<span data-ttu-id="47fa3-128">[![la fenêtre contextuelle modale apparaît à la position désignée](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="47fa3-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="47fa3-129">La fenêtre contextuelle modale apparaît à la position désignée ([cliquez pour afficher l’image en taille réelle](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="47fa3-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47fa3-130">[Précédent](handling-postbacks-from-a-modalpopup-cs.md)
> [Suivant](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="47fa3-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
