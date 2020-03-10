---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Exécution de plusieurs animations après l’autre (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter Severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: e378ac038796dbd44e89d099532b9e186dcf673f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614281"
---
# <a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="2c6a4-104">Exécution de plusieurs animations l’une après l’autre (C#)</span><span class="sxs-lookup"><span data-stu-id="2c6a4-104">Executing Several Animations after Each Other (C#)</span></span>

<span data-ttu-id="2c6a4-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2c6a4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2c6a4-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2c6a4-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="2c6a4-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2c6a4-108">Il permet d’exécuter plusieurs animations l’une après l’autre.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-108">It allows to run several animations one after the other.</span></span>

<span data-ttu-id="2c6a4-109">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2c6a4-110">Il permet d’exécuter plusieurs animations l’une après l’autre.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="2c6a4-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="2c6a4-111">Steps</span></span>

<span data-ttu-id="2c6a4-112">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="2c6a4-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="2c6a4-113">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2c6a4-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="2c6a4-114">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="2c6a4-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="2c6a4-115">Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server":` obligatoire</span><span class="sxs-lookup"><span data-stu-id="2c6a4-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="2c6a4-116">Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="2c6a4-117">En règle générale, `<OnLoad>` n’accepte qu’une seule animation.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="2c6a4-118">L’infrastructure d’animation vous permet de joindre plusieurs animations à un seul à l’aide de l’élément `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="2c6a4-119">Toutes les animations au sein de `<Sequence>` sont exécutées l’une après l’autre.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="2c6a4-120">Voici le balisage possible pour le contrôle `AnimationExtender`, en commençant par rendre le volet plus grand, puis en diminuant sa hauteur :</span><span class="sxs-lookup"><span data-stu-id="2c6a4-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="2c6a4-121">Lorsque vous exécutez ce script, le panneau est d’abord plus grand, puis plus petit.</span><span class="sxs-lookup"><span data-stu-id="2c6a4-121">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="2c6a4-122">[![première la largeur est augmentée](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2c6a4-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="2c6a4-123">La largeur est augmentée ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2c6a4-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>

<span data-ttu-id="2c6a4-124">[![la hauteur est diminuée](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2c6a4-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="2c6a4-125">La hauteur est alors réduite ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2c6a4-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2c6a4-126">[Précédent](executing-several-animations-at-the-same-time-cs.md)
> [Suivant](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2c6a4-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
