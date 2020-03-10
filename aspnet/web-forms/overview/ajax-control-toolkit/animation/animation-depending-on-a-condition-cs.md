---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animation selon une condition (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Si une animation est...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598349"
---
# <a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="1cb60-104">Animation dépendant d’une condition (C#)</span><span class="sxs-lookup"><span data-stu-id="1cb60-104">Animation Depending On a Condition (C#)</span></span>

<span data-ttu-id="1cb60-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1cb60-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1cb60-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1cb60-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="1cb60-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="1cb60-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1cb60-108">Le fait qu’une animation soit exécutée ou non peut également dépendre d’une condition sous forme de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cb60-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="1cb60-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="1cb60-109">Overview</span></span>

<span data-ttu-id="1cb60-110">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="1cb60-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1cb60-111">Le fait qu’une animation soit exécutée ou non peut également dépendre d’une condition sous forme de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cb60-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="1cb60-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="1cb60-112">Steps</span></span>

<span data-ttu-id="1cb60-113">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="1cb60-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="1cb60-114">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1cb60-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="1cb60-115">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="1cb60-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="1cb60-116">Ajoutez ensuite le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server":` obligatoire</span><span class="sxs-lookup"><span data-stu-id="1cb60-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="1cb60-117">Dans le nœud `<Animations>`, utilisez `<OnLoad>` pour exécuter les animations une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="1cb60-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="1cb60-118">Au lieu de l’une des animations régulières, l’élément `<Condition>` entre en lecture.</span><span class="sxs-lookup"><span data-stu-id="1cb60-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="1cb60-119">Le code JavaScript fourni comme valeur de l’attribut `ConditionScript` est exécuté au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="1cb60-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="1cb60-120">Si elle prend la valeur true, l’animation est exécutée, sinon.</span><span class="sxs-lookup"><span data-stu-id="1cb60-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="1cb60-121">Le balisage suivant fournit deux animations, chacune étant exécutée dans 50% des cas sur Random.</span><span class="sxs-lookup"><span data-stu-id="1cb60-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="1cb60-122">Étant donné qu’il ne peut y avoir qu’une seule animation dans `<OnLoad>`, les deux animations `<Condition>` sont jointes à l’aide de l’élément `<Sequence>` :</span><span class="sxs-lookup"><span data-stu-id="1cb60-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="1cb60-123">Notez que le signe inférieur à (`<`) dans l’attribut `ConditionScript` doit être placé dans une séquence d’échappement ().</span><span class="sxs-lookup"><span data-stu-id="1cb60-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="1cb60-124">Quand vous exécutez ce script, aucune animation n’est exécutée, ou l’une des deux, ou les deux.</span><span class="sxs-lookup"><span data-stu-id="1cb60-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="1cb60-125">[![le panneau s’exclut sans le redimensionnement, la deuxième animation s’exécute, la première ne l’a pas fait.](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1cb60-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="1cb60-126">Le panneau s’affiche sans redimensionnement, de sorte que la deuxième animation s’exécute, la première ne l’a pas fait ([cliquez pour afficher l’image en taille réelle](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1cb60-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1cb60-127">[Précédent](executing-several-animations-after-each-other-cs.md)
> [Suivant](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1cb60-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
