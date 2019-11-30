---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Désactivation d’actions pendant une animation (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge l’action...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606843"
---
# <a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="9d36a-104">Désactivation d’actions pendant une animation (VB)</span><span class="sxs-lookup"><span data-stu-id="9d36a-104">Disabling Actions during Animation (VB)</span></span>

<span data-ttu-id="9d36a-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9d36a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9d36a-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d36a-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="9d36a-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="9d36a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9d36a-108">Il prend également en charge les actions, telles que les clics de souris.</span><span class="sxs-lookup"><span data-stu-id="9d36a-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="9d36a-109">Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.</span><span class="sxs-lookup"><span data-stu-id="9d36a-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="9d36a-110">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="9d36a-110">Overview</span></span>

<span data-ttu-id="9d36a-111">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="9d36a-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9d36a-112">Il prend également en charge les actions, telles que les clics de souris.</span><span class="sxs-lookup"><span data-stu-id="9d36a-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="9d36a-113">Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.</span><span class="sxs-lookup"><span data-stu-id="9d36a-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="9d36a-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="9d36a-114">Steps</span></span>

<span data-ttu-id="9d36a-115">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="9d36a-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="9d36a-116">L’animation sera appliquée à un bouton HTML comme suit :</span><span class="sxs-lookup"><span data-stu-id="9d36a-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="9d36a-117">Notez qu’un contrôle HTML est utilisé à la place d’un contrôle Web dans la mesure où nous ne voulons pas que le bouton crée une publication (postback); il lancera simplement l’animation côté client pour nous.</span><span class="sxs-lookup"><span data-stu-id="9d36a-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="9d36a-118">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :</span><span class="sxs-lookup"><span data-stu-id="9d36a-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="9d36a-119">Dans le nœud `<Animations>`, `<OnClick>` est l’élément approprié pour gérer le clic de souris.</span><span class="sxs-lookup"><span data-stu-id="9d36a-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="9d36a-120">Toutefois, vous pouvez également cliquer sur le bouton pendant l’animation.</span><span class="sxs-lookup"><span data-stu-id="9d36a-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="9d36a-121">L’élément `<EnableAction>` peut s’en occuper.</span><span class="sxs-lookup"><span data-stu-id="9d36a-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="9d36a-122">La définition de `Enabled="false"` désactive le bouton dans le cadre de l’animation.</span><span class="sxs-lookup"><span data-stu-id="9d36a-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="9d36a-123">Étant donné que nous utilisons plusieurs animations individuelles (désactivation du bouton et des animations réelles), l’élément `<Parallel>` est requis pour coller les deux animations ensemble dans un.</span><span class="sxs-lookup"><span data-stu-id="9d36a-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="9d36a-124">Voici le balisage complet pour `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="9d36a-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="9d36a-125">Il serait également possible d’activer à nouveau le bouton après l’animation, en utilisant l’élément XML suivant à la fin de la liste :</span><span class="sxs-lookup"><span data-stu-id="9d36a-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="9d36a-126">Toutefois, dans le scénario donné, cela serait inutile, car le bouton disparaît et n’est pas visible à la fin de l’animation.</span><span class="sxs-lookup"><span data-stu-id="9d36a-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="9d36a-127">[![le bouton est désactivé dès l’exécution de l’animation](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d36a-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="9d36a-128">Le bouton est désactivé dès l’exécution de l’animation ([cliquez pour afficher l’image en taille réelle](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9d36a-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d36a-129">[Précédent](animating-in-response-to-user-interaction-vb.md)
> [Suivant](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9d36a-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
