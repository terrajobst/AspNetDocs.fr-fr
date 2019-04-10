---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animation en réponse à une Interaction utilisateur (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent étoiles...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: c38160ffa9965384cf4eae2ebda52bd62b766bba
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59396230"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="a2f9d-104">Animation en réponse à une interaction utilisateur (VB)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="a2f9d-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a2f9d-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="a2f9d-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a2f9d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a2f9d-108">Les animations peuvent démarrer automatiquement, ou peuvent être déclenchées par l’utilisateur, par exemple, en cliquant avec la souris.</span><span class="sxs-lookup"><span data-stu-id="a2f9d-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="a2f9d-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a2f9d-109">Overview</span></span>

<span data-ttu-id="a2f9d-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a2f9d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a2f9d-111">Les animations peuvent démarrer automatiquement, ou peuvent être déclenchées par l’utilisateur, par exemple, en cliquant avec la souris.</span><span class="sxs-lookup"><span data-stu-id="a2f9d-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="a2f9d-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="a2f9d-112">Steps</span></span>

<span data-ttu-id="a2f9d-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="a2f9d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="a2f9d-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="a2f9d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="a2f9d-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="a2f9d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="a2f9d-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a2f9d-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="a2f9d-117">Dans le `<Animations>` nœud, il existe cinq façons de démarrer l’animation par le biais de l’interaction utilisateur (l’élément manquant est `<OnLoad>` qui est exécuté une fois que la page entière a été entièrement chargée) :</span><span class="sxs-lookup"><span data-stu-id="a2f9d-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- `<OnClick>` <span data-ttu-id="a2f9d-118">(clic de souris sur le contrôle)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-118">(mouse click on the control)</span></span>
- `<OnHoverOut>` <span data-ttu-id="a2f9d-119">(la souris s’écarte du contrôle)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-119">(mouse leaves the control)</span></span>
- `<OnHoverOver>` <span data-ttu-id="a2f9d-120">(la souris pointe sur un contrôle, l’arrêt du `<OnHoverOut>` animation)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-120">(mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- `<OnMouseOut>` <span data-ttu-id="a2f9d-121">(la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-121">(mouse leaves a control)</span></span>
- `<OnMouseOver>` <span data-ttu-id="a2f9d-122">(la souris pointe sur un contrôle, ne pas l’arrêt du `<OnMouseOut>` animation)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-122">(mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="a2f9d-123">Dans ce scénario, `<OnClick>` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a2f9d-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="a2f9d-124">Lorsque l’utilisateur clique sur le panneau de configuration, il est redimensionné et fondu en même temps.</span><span class="sxs-lookup"><span data-stu-id="a2f9d-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![A <span data-ttu-id="a2f9d-125">clic de souris démarre l’animation]</span><span class="sxs-lookup"><span data-stu-id="a2f9d-125">mouse click starts the animation]</span></span>(animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

<span data-ttu-id="a2f9d-126">Un clic de souris démarre l’animation ([cliquez pour afficher l’image en taille réelle](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a2f9d-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2f9d-127">[Précédent](picking-one-animation-out-of-a-list-vb.md)
> [Suivant](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a2f9d-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
