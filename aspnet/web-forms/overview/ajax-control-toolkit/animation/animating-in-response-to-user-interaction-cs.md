---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animation en réponse à l’interaction de l'C#utilisateur () | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent être en étoile...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614372"
---
# <a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="7e545-104">Animation en réponse à une interaction utilisateur (C#)</span><span class="sxs-lookup"><span data-stu-id="7e545-104">Animating in Response To User Interaction (C#)</span></span>

<span data-ttu-id="7e545-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7e545-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7e545-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7e545-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="7e545-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="7e545-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7e545-108">Les animations peuvent démarrer automatiquement ou peuvent être déclenchées par l’intervention de l’utilisateur, par exemple en cliquant avec la souris.</span><span class="sxs-lookup"><span data-stu-id="7e545-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="overview"></a><span data-ttu-id="7e545-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="7e545-109">Overview</span></span>

<span data-ttu-id="7e545-110">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="7e545-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7e545-111">Les animations peuvent démarrer automatiquement ou peuvent être déclenchées par l’intervention de l’utilisateur, par exemple en cliquant avec la souris.</span><span class="sxs-lookup"><span data-stu-id="7e545-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="7e545-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="7e545-112">Steps</span></span>

<span data-ttu-id="7e545-113">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="7e545-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="7e545-114">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="7e545-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="7e545-115">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="7e545-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="7e545-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :</span><span class="sxs-lookup"><span data-stu-id="7e545-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="7e545-117">Dans le nœud `<Animations>`, il existe cinq façons de démarrer l’animation par le biais de l’intervention de l’utilisateur (l’élément manquant est `<OnLoad>` qui est exécuté une fois que la page entière a été entièrement chargée) :</span><span class="sxs-lookup"><span data-stu-id="7e545-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="7e545-118">`<OnClick>` (clic de souris sur le contrôle)</span><span class="sxs-lookup"><span data-stu-id="7e545-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="7e545-119">`<OnHoverOut>` (la souris quitte le contrôle)</span><span class="sxs-lookup"><span data-stu-id="7e545-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="7e545-120">`<OnHoverOver>` (pointage de la souris sur un contrôle, arrêt de l’animation `<OnHoverOut>`)</span><span class="sxs-lookup"><span data-stu-id="7e545-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="7e545-121">`<OnMouseOut>` (la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="7e545-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="7e545-122">`<OnMouseOver>` (pointage de la souris sur un contrôle, sans arrêter l’animation `<OnMouseOut>`)</span><span class="sxs-lookup"><span data-stu-id="7e545-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="7e545-123">Dans ce scénario, `<OnClick>` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7e545-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="7e545-124">Quand l’utilisateur clique sur le panneau, il est redimensionné et disparaît en fondu en même temps.</span><span class="sxs-lookup"><span data-stu-id="7e545-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

<span data-ttu-id="7e545-125">[![un clic de souris démarre l’animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7e545-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="7e545-126">Un clic de souris démarre l’animation ([cliquez pour afficher l’image en taille réelle](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7e545-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e545-127">[Précédent](picking-one-animation-out-of-a-list-cs.md)
> [Suivant](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7e545-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
