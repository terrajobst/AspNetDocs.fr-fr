---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Ajout d’une Animation à un contrôle (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aac6e97ae5d3d777c3644515628d2669076a88c4
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58421919"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="226c4-104">Ajout d’une animation à un contrôle (C#)</span><span class="sxs-lookup"><span data-stu-id="226c4-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="226c4-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="226c4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="226c4-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="226c4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="226c4-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="226c4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="226c4-108">Ce didacticiel montre comment configurer une animation de ce type.</span><span class="sxs-lookup"><span data-stu-id="226c4-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="226c4-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="226c4-109">Overview</span></span>

<span data-ttu-id="226c4-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="226c4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="226c4-111">Ce didacticiel montre comment configurer une animation de ce type.</span><span class="sxs-lookup"><span data-stu-id="226c4-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="226c4-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="226c4-112">Steps</span></span>

<span data-ttu-id="226c4-113">La première étape consiste comme d’habitude à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="226c4-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="226c4-114">L’animation dans ce scénario sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="226c4-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="226c4-115">La classe CSS associée pour le panneau définit une couleur d’arrière-plan et une largeur :</span><span class="sxs-lookup"><span data-stu-id="226c4-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="226c4-116">Ensuite, nous devons le `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="226c4-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="226c4-117">Après avoir entré un `ID` et habituelles `runat="server"`, le `TargetControlID` attribut doit être défini pour le contrôle pour animer dans notre cas, le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="226c4-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="226c4-118">L’animation entière est appliquée de manière déclarative, à l’aide d’une syntaxe XML, malheureusement actuellement pas entièrement pris en charge par IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="226c4-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="226c4-119">Le nœud racine est `<Animations>;` au sein de ce nœud, plusieurs événements sont autorisées qui déterminent quand l’animation prennent place :</span><span class="sxs-lookup"><span data-stu-id="226c4-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="226c4-120">`OnClick` (clic de souris)</span><span class="sxs-lookup"><span data-stu-id="226c4-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="226c4-121">`OnHoverOut` (lorsque la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="226c4-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="226c4-122">`OnHoverOver` (lorsque la souris pointe sur un contrôle, l’arrêt du `OnHoverOut` animation)</span><span class="sxs-lookup"><span data-stu-id="226c4-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="226c4-123">`OnLoad` (lorsque la page a été chargée)</span><span class="sxs-lookup"><span data-stu-id="226c4-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="226c4-124">`OnMouseOut` (lorsque la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="226c4-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="226c4-125">`OnMouseOver` (lorsque la souris pointe sur un contrôle, ne pas l’arrêt du `OnMouseOut` animation)</span><span class="sxs-lookup"><span data-stu-id="226c4-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="226c4-126">Le framework est fourni avec un ensemble d’animations, chacun d’eux représenté par son propre élément XML.</span><span class="sxs-lookup"><span data-stu-id="226c4-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="226c4-127">Voici une sélection :</span><span class="sxs-lookup"><span data-stu-id="226c4-127">Here is a selection:</span></span>

- <span data-ttu-id="226c4-128">`<Color>` (modification d’une couleur)</span><span class="sxs-lookup"><span data-stu-id="226c4-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="226c4-129">`<FadeIn>` (fondu dans)</span><span class="sxs-lookup"><span data-stu-id="226c4-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="226c4-130">`<FadeOut>` (fondu)</span><span class="sxs-lookup"><span data-stu-id="226c4-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="226c4-131">`<Property>` (modification de propriété d’un contrôle)</span><span class="sxs-lookup"><span data-stu-id="226c4-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="226c4-132">`<Pulse>` (impulsions)</span><span class="sxs-lookup"><span data-stu-id="226c4-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="226c4-133">`<Resize>` (modification de la taille)</span><span class="sxs-lookup"><span data-stu-id="226c4-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="226c4-134">`<Scale>` (proportionnellement à la taille de la variation)</span><span class="sxs-lookup"><span data-stu-id="226c4-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="226c4-135">Dans cet exemple, le panneau est disparition en fondu. L’animation prend 1,5 secondes (`Duration` attribut), affichage des 24 images (étapes d’animation) par seconde (`Fps` attribut).</span><span class="sxs-lookup"><span data-stu-id="226c4-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="226c4-136">Voici le balisage complet pour le `AnimationExtender` contrôle :</span><span class="sxs-lookup"><span data-stu-id="226c4-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="226c4-137">Lorsque vous exécutez ce script, le panneau s’affiche et fondu en quelques secondes et un demi.</span><span class="sxs-lookup"><span data-stu-id="226c4-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="226c4-138">[![Le panneau est fondu](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="226c4-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="226c4-139">Le panneau est fondu ([cliquez pour afficher l’image en taille réelle](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="226c4-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="226c4-140">Next</span><span class="sxs-lookup"><span data-stu-id="226c4-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
