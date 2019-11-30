---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Déclenchement d’une animation dans un autre contrôle (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En général, le lancement d’une...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a4af2324afab7519170c123b6ea7c57ab3e03fb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575036"
---
# <a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="9e3b6-104">Déclenchement d’une animation dans un autre contrôle (VB)</span><span class="sxs-lookup"><span data-stu-id="9e3b6-104">Triggering an Animation in another Control (VB)</span></span>

<span data-ttu-id="9e3b6-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9e3b6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9e3b6-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9e3b6-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="9e3b6-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9e3b6-108">En général, le lancement d’une animation est déclenché par l’interaction de l’utilisateur avec le même contrôle.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="9e3b6-109">Toutefois, il est également possible d’interagir avec un contrôle, puis d’effectuer l’animation d’un autre contrôle.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-109">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="overview"></a><span data-ttu-id="9e3b6-110">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="9e3b6-110">Overview</span></span>

<span data-ttu-id="9e3b6-111">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9e3b6-112">En général, le lancement d’une animation est déclenché par l’interaction de l’utilisateur avec le même contrôle.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="9e3b6-113">Toutefois, il est également possible d’interagir avec un contrôle, puis d’effectuer l’animation d’un autre contrôle.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="9e3b6-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="9e3b6-114">Steps</span></span>

<span data-ttu-id="9e3b6-115">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="9e3b6-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="9e3b6-116">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="9e3b6-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="9e3b6-117">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="9e3b6-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="9e3b6-118">Pour que vous puissiez commencer à animer le panneau, un bouton HTML est utilisé.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="9e3b6-119">Notez que `<input type="button" />` est favorisé sur `<asp:Button />` puisque nous ne voulons pas une publication (postback) quand l’utilisateur clique sur ce bouton.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="9e3b6-120">Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="9e3b6-121">Il est important de définir `TargetControlID` sur l’ID du bouton (l’élément qui déclenche l’animation), et non sur l’ID du panneau (l’élément animé).</span><span class="sxs-lookup"><span data-stu-id="9e3b6-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="9e3b6-122">Dans le nœud `<Animations>`, placez les animations comme d’habitude.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="9e3b6-123">Pour qu’ils modifient le panneau, pas le bouton, définissez l’attribut `AnimationTarget` pour chaque élément d’animation dans `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="9e3b6-124">La valeur de `AnimationTarget` est l’ID du panneau, bien sûr.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="9e3b6-125">De cette façon, les animations se produisent avec le panneau, et non avec le bouton de déclenchement.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="9e3b6-126">Voici le balisage `AnimationExtender` pour ce scénario :</span><span class="sxs-lookup"><span data-stu-id="9e3b6-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="9e3b6-127">Notez l’ordre spécial dans lequel les différentes animations apparaissent.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="9e3b6-128">Tout d’abord, le bouton est désactivé une fois l’animation exécutée.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="9e3b6-129">Étant donné qu’il n’y a pas d’attribut `AnimationTarget` dans l’élément `<EnableAction>`, cette animation est appliquée au contrôle d’origine : le bouton.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="9e3b6-130">Les deux étapes suivantes de l’animation doivent être effectuées en parallèle (élément`<Parallel>`).</span><span class="sxs-lookup"><span data-stu-id="9e3b6-130">The next two animation steps shall be carried out in parallel (`<Parallel>` element).</span></span> <span data-ttu-id="9e3b6-131">Leurs attributs `AnimationTarget` sont tous deux définis sur `"Panel1"`, ce qui permet d’animer le panneau, et non le bouton.</span><span class="sxs-lookup"><span data-stu-id="9e3b6-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>

<span data-ttu-id="9e3b6-132">[![un clic de souris sur le bouton démarre l’animation du panneau](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9e3b6-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="9e3b6-133">Un clic de souris sur le bouton démarre l’animation du panneau ([cliquez pour afficher l’image en taille réelle](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9e3b6-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9e3b6-134">[Précédent](disabling-actions-during-animation-vb.md)
> [Suivant](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9e3b6-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
