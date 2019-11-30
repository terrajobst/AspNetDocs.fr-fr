---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Désactivation d’actions pendant l'C#animation () | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge l’action...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599732"
---
# <a name="disabling-actions-during-animation-c"></a><span data-ttu-id="32c2e-104">Désactivation d’actions pendant une animation (C#)</span><span class="sxs-lookup"><span data-stu-id="32c2e-104">Disabling Actions during Animation (C#)</span></span>

<span data-ttu-id="32c2e-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="32c2e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="32c2e-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="32c2e-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="32c2e-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="32c2e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="32c2e-108">Il prend également en charge les actions, telles que les clics de souris.</span><span class="sxs-lookup"><span data-stu-id="32c2e-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="32c2e-109">Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.</span><span class="sxs-lookup"><span data-stu-id="32c2e-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="32c2e-110">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="32c2e-110">Overview</span></span>

<span data-ttu-id="32c2e-111">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="32c2e-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="32c2e-112">Il prend également en charge les actions, telles que les clics de souris.</span><span class="sxs-lookup"><span data-stu-id="32c2e-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="32c2e-113">Toutefois, quand un clic de souris démarre une animation, il est préférable de désactiver les clics de souris pendant l’animation.</span><span class="sxs-lookup"><span data-stu-id="32c2e-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="32c2e-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="32c2e-114">Steps</span></span>

<span data-ttu-id="32c2e-115">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="32c2e-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="32c2e-116">L’animation sera appliquée à un bouton HTML comme suit :</span><span class="sxs-lookup"><span data-stu-id="32c2e-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="32c2e-117">Notez qu’un contrôle HTML est utilisé à la place d’un contrôle Web dans la mesure où nous ne voulons pas que le bouton crée une publication (postback); il lancera simplement l’animation côté client pour nous.</span><span class="sxs-lookup"><span data-stu-id="32c2e-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="32c2e-118">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :</span><span class="sxs-lookup"><span data-stu-id="32c2e-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="32c2e-119">Dans le nœud `<Animations>`, `<OnClick>` est l’élément approprié pour gérer le clic de souris.</span><span class="sxs-lookup"><span data-stu-id="32c2e-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="32c2e-120">Toutefois, vous pouvez également cliquer sur le bouton pendant l’animation.</span><span class="sxs-lookup"><span data-stu-id="32c2e-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="32c2e-121">L’élément `<EnableAction>` peut s’en occuper.</span><span class="sxs-lookup"><span data-stu-id="32c2e-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="32c2e-122">La définition de `Enabled="false"` désactive le bouton dans le cadre de l’animation.</span><span class="sxs-lookup"><span data-stu-id="32c2e-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="32c2e-123">Étant donné que nous utilisons plusieurs animations individuelles (désactivation du bouton et des animations réelles), l’élément `<Parallel>` est requis pour coller les deux animations ensemble dans un.</span><span class="sxs-lookup"><span data-stu-id="32c2e-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="32c2e-124">Voici le balisage complet pour `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="32c2e-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="32c2e-125">Il serait également possible d’activer à nouveau le bouton après l’animation, en utilisant l’élément XML suivant à la fin de la liste :</span><span class="sxs-lookup"><span data-stu-id="32c2e-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="32c2e-126">Toutefois, dans le scénario donné, cela serait inutile, car le bouton disparaît et n’est pas visible à la fin de l’animation.</span><span class="sxs-lookup"><span data-stu-id="32c2e-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="32c2e-127">[![le bouton est désactivé dès l’exécution de l’animation](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32c2e-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="32c2e-128">Le bouton est désactivé dès l’exécution de l’animation ([cliquez pour afficher l’image en taille réelle](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="32c2e-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32c2e-129">[Précédent](animating-in-response-to-user-interaction-cs.md)
> [Suivant](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="32c2e-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
