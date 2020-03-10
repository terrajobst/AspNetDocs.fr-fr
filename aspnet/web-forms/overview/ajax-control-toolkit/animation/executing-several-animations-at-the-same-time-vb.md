---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Exécution de plusieurs animations en même temps (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter Severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: fc11debd06c897c29db56d42167d483f5608cdf8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614253"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="a357a-104">Exécution de plusieurs animations en même temps (VB)</span><span class="sxs-lookup"><span data-stu-id="a357a-104">Executing Several Animations at The Same Time (VB)</span></span>

<span data-ttu-id="a357a-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a357a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a357a-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a357a-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="a357a-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a357a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a357a-108">Il permet d’exécuter plusieurs animations de manière parallèle.</span><span class="sxs-lookup"><span data-stu-id="a357a-108">It allows to run several animations in a parallel fashion.</span></span>

## <a name="overview"></a><span data-ttu-id="a357a-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="a357a-109">Overview</span></span>

<span data-ttu-id="a357a-110">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a357a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a357a-111">Il permet d’exécuter plusieurs animations de manière parallèle.</span><span class="sxs-lookup"><span data-stu-id="a357a-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="a357a-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="a357a-112">Steps</span></span>

<span data-ttu-id="a357a-113">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="a357a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="a357a-114">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="a357a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="a357a-115">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="a357a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="a357a-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :</span><span class="sxs-lookup"><span data-stu-id="a357a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="a357a-117">Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="a357a-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="a357a-118">En règle générale, `<OnLoad>` n’accepte qu’une seule animation.</span><span class="sxs-lookup"><span data-stu-id="a357a-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="a357a-119">L’infrastructure d’animation vous permet de joindre plusieurs animations à un seul à l’aide de l’élément `<Parallel>`.</span><span class="sxs-lookup"><span data-stu-id="a357a-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="a357a-120">Toutes les animations au sein de `<Parallel>` sont exécutées en même temps.</span><span class="sxs-lookup"><span data-stu-id="a357a-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="a357a-121">Voici le balisage possible pour le contrôle `AnimationExtender`, le fondu et le redimensionnement du panneau en même temps :</span><span class="sxs-lookup"><span data-stu-id="a357a-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="a357a-122">Et en effet : lorsque vous exécutez ce script, le panneau s’affiche, puis le redimensionnement (plus que la largeur et la moitié de sa hauteur) et disparaît en fondu en même temps.</span><span class="sxs-lookup"><span data-stu-id="a357a-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>

<span data-ttu-id="a357a-123">[![le panneau est en fondu et en redimensionnement (y compris son contenu, grâce au moteur de rendu du navigateur)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a357a-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="a357a-124">Le panneau est en fondu et en redimensionnement (y compris son contenu, grâce au moteur de rendu du navigateur) ([cliquez pour afficher l’image en plein écran](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a357a-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a357a-125">[Précédent](adding-animation-to-a-control-vb.md)
> [Suivant](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a357a-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
