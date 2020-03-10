---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Ajout d’une animation à un contrôle (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598300"
---
# <a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="ff6fe-104">Ajout d’une animation à un contrôle (VB)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-104">Adding Animation to a Control (VB)</span></span>

<span data-ttu-id="ff6fe-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ff6fe-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="ff6fe-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff6fe-108">Ce didacticiel montre comment configurer une telle animation.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-108">This tutorial shows how to set up such an animation.</span></span>

## <a name="overview"></a><span data-ttu-id="ff6fe-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="ff6fe-109">Overview</span></span>

<span data-ttu-id="ff6fe-110">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff6fe-111">Ce didacticiel montre comment configurer une telle animation.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ff6fe-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="ff6fe-112">Steps</span></span>

<span data-ttu-id="ff6fe-113">La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="ff6fe-114">L’animation dans ce scénario sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="ff6fe-115">La classe CSS associée pour le panneau définit une couleur d’arrière-plan et une largeur :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="ff6fe-116">Ensuite, nous avons besoin de la `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="ff6fe-117">Après avoir fourni un `ID` et le `runat="server"`habituel, l’attribut `TargetControlID` doit être défini sur le contrôle à animer dans notre cas, le panneau :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="ff6fe-118">L’animation entière est appliquée de manière déclarative, à l’aide d’une syntaxe XML, malheureusement pas entièrement prise en charge par IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="ff6fe-119">Le nœud racine est `<Animations>;` dans ce nœud, plusieurs événements sont autorisés, qui déterminent à quel moment la ou les animations prennent en place :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="ff6fe-120">`OnClick` (clic de souris)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="ff6fe-121">`OnHoverOut` (lorsque la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ff6fe-122">`OnHoverOver` (lorsque la souris pointe sur un contrôle, ce qui arrête l’animation `OnHoverOut`)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="ff6fe-123">`OnLoad` (lorsque la page a été chargée)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="ff6fe-124">`OnMouseOut` (lorsque la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ff6fe-125">`OnMouseOver` (lorsque la souris pointe sur un contrôle, sans arrêter l’animation `OnMouseOut`)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="ff6fe-126">Le Framework est fourni avec un ensemble d’animations, chacun représenté par son propre élément XML.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="ff6fe-127">Voici une sélection :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-127">Here is a selection:</span></span>

- <span data-ttu-id="ff6fe-128">`<Color>` (modification d’une couleur)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="ff6fe-129">`<FadeIn>` (fondu)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="ff6fe-130">`<FadeOut>` (fondu)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="ff6fe-131">`<Property>` (modification de la propriété d’un contrôle)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="ff6fe-132">`<Pulse>` (Pulsating)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="ff6fe-133">`<Resize>` (modification de la taille)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="ff6fe-134">`<Scale>` (modification proportionnelle de la taille)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="ff6fe-135">Dans cet exemple, le panneau s’estompe. L’animation prend 1,5 secondes (attribut`Duration`), en affichant 24 images (étapes d’animation) par seconde (attribut`Fps`).</span><span class="sxs-lookup"><span data-stu-id="ff6fe-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="ff6fe-136">Voici le balisage complet pour le contrôle `AnimationExtender` :</span><span class="sxs-lookup"><span data-stu-id="ff6fe-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="ff6fe-137">Lorsque vous exécutez ce script, le panneau s’affiche et disparaît en fondu d’une à deux secondes.</span><span class="sxs-lookup"><span data-stu-id="ff6fe-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>

<span data-ttu-id="ff6fe-138">[![le panneau est en fondu](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="ff6fe-139">Le panneau est en fondu ([cliquez pour afficher l’image en taille réelle](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ff6fe-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ff6fe-140">[Précédent](dynamically-controlling-updatepanel-animations-cs.md)
> [Suivant](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ff6fe-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
