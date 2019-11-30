---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Exécution d’animations à l’aide du code côté client (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Exécution de l’animation...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575481"
---
# <a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="576c9-104">Exécution d’animations avec du code côté client (VB)</span><span class="sxs-lookup"><span data-stu-id="576c9-104">Executing Animations Using Client-Side Code (VB)</span></span>

<span data-ttu-id="576c9-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="576c9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="576c9-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="576c9-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="576c9-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="576c9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="576c9-108">L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="576c9-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="576c9-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="576c9-109">Overview</span></span>

<span data-ttu-id="576c9-110">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="576c9-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="576c9-111">L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="576c9-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="576c9-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="576c9-112">Steps</span></span>

<span data-ttu-id="576c9-113">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="576c9-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="576c9-114">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="576c9-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="576c9-115">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="576c9-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="576c9-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, l’attribut `TargetControlID` et le `runat="server"`obligatoire :</span><span class="sxs-lookup"><span data-stu-id="576c9-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="576c9-117">Dans le nœud `<Animations>`, utilisez `<OnClick>` pour exécuter les animations une fois que l’utilisateur a cliqué sur le panneau.</span><span class="sxs-lookup"><span data-stu-id="576c9-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="576c9-118">Ajoutez deux animations à exécuter en parallèle :</span><span class="sxs-lookup"><span data-stu-id="576c9-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="576c9-119">À des fins de démonstration, cette animation (et toute autre animation créée à l’aide de Control Toolkit) est exécutée à l’aide de code JavaScript, une fois la page exécutée.</span><span class="sxs-lookup"><span data-stu-id="576c9-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="576c9-120">Tout d’abord, nous avons besoin d’accéder au contrôle `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="576c9-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="576c9-121">La bibliothèque AJAX ASP.NET fournit la fonction `$find()` pour cette tâche :</span><span class="sxs-lookup"><span data-stu-id="576c9-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="576c9-122">Le contrôle `AnimationExtender` expose une API riche, y compris des méthodes dont les noms sont identiques aux gestionnaires d’événements utilisés dans le balisage XML : `OnClick()`, `OnLoad()`, etc.</span><span class="sxs-lookup"><span data-stu-id="576c9-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="576c9-123">Par exemple, un appel de la méthode `OnClick()` exécute l’animation dans l’élément `<OnClick>` du contrôle `AnimationExtender` :</span><span class="sxs-lookup"><span data-stu-id="576c9-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="576c9-124">Voici le code JavaScript côté client complet qui émule le clic sur le panneau une fois que la page a été entièrement chargée. Notez que le nom de la fonction de `pageLoad()` est utilisé, ce qui est appelé par ASP.NET AJAX une fois que la page et toutes les bibliothèques JavaScript incluses ont été chargées.</span><span class="sxs-lookup"><span data-stu-id="576c9-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

<span data-ttu-id="576c9-125">[![l’animation s’exécute immédiatement, sans clic de souris](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="576c9-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="576c9-126">L’animation s’exécute immédiatement, sans clic de souris ([Cliquer pour afficher l’image en taille réelle](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="576c9-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="576c9-127">[Précédent](modifying-animations-from-the-server-side-vb.md)
> [Suivant](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="576c9-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
