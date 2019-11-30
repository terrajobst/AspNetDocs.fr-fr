---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animation d’un contrôle UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599931"
---
# <a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="7d3b8-104">Animation d’un contrôle UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="7d3b8-104">Animating an UpdatePanel Control (C#)</span></span>

<span data-ttu-id="7d3b8-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7d3b8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7d3b8-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7d3b8-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="7d3b8-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7d3b8-108">Pour le contenu d’un UpdatePanel, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="7d3b8-109">Ce didacticiel montre comment configurer une telle animation pour un UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="7d3b8-110">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="7d3b8-110">Overview</span></span>

<span data-ttu-id="7d3b8-111">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7d3b8-112">Pour le contenu d’un `UpdatePanel`, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="7d3b8-113">Ce didacticiel montre comment configurer une telle animation pour un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="7d3b8-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="7d3b8-114">Steps</span></span>

<span data-ttu-id="7d3b8-115">La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :</span><span class="sxs-lookup"><span data-stu-id="7d3b8-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="7d3b8-116">L’animation dans ce scénario sera appliquée à un contrôle Web `Wizard` ASP.NET résidant dans une `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="7d3b8-117">Trois étapes (arbitraires) fournissent suffisamment d’options pour déclencher des publications (postback) :</span><span class="sxs-lookup"><span data-stu-id="7d3b8-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="7d3b8-118">Le balisage nécessaire pour le contrôle `UpdatePanelAnimationExtender` est assez similaire à celui utilisé pour le `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="7d3b8-119">Dans l’attribut `TargetControlID`, nous fournissons le `ID` du `UpdatePanel` à animer. dans le contrôle `UpdatePanelAnimationExtender`, l’élément `<Animations>` contient le balisage XML pour la ou les animations.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="7d3b8-120">Toutefois, il existe une différence : la quantité d’événements et de gestionnaires d’événements est limitée par rapport à `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="7d3b8-121">Pour `UpdatePanels`, seuls deux d’entre eux existent :</span><span class="sxs-lookup"><span data-stu-id="7d3b8-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="7d3b8-122">`<OnUpdated>` lorsque UpdatePanel a été mis à jour</span><span class="sxs-lookup"><span data-stu-id="7d3b8-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="7d3b8-123">`<OnUpdating>` lors du démarrage de la mise à jour de UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="7d3b8-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="7d3b8-124">Dans ce scénario, le nouveau contenu du `UpdatePanel` (après la publication) s’estompe.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="7d3b8-125">Il s’agit du balisage nécessaire pour ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="7d3b8-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="7d3b8-126">Maintenant, chaque fois qu’une publication (postback) se produit dans le UpdatePanel, le nouveau contenu du panneau s’estompe en douceur.</span><span class="sxs-lookup"><span data-stu-id="7d3b8-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="7d3b8-127">[![l’étape suivante de l’Assistant est en fondu](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d3b8-127">[![The next wizard step is fading in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="7d3b8-128">L’étape suivante de l’Assistant est en fondu ([cliquez pour afficher l’image en taille réelle](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7d3b8-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d3b8-129">[Précédent](changing-an-animation-using-client-side-code-cs.md)
> [Suivant](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7d3b8-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>
