---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Contrôle dynamique des animations UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 97a52bd75fdf63ad62282afd9df772f0a9e4f931
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599686"
---
# <a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="2236c-104">Contrôle dynamique des animations UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="2236c-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>

<span data-ttu-id="2236c-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2236c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2236c-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2236c-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="2236c-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2236c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2236c-108">Pour le contenu d’un UpdatePanel, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="2236c-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="2236c-109">Il peut également fonctionner conjointement avec les déclencheurs UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="2236c-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="2236c-110">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="2236c-110">Overview</span></span>

<span data-ttu-id="2236c-111">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2236c-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2236c-112">Pour le contenu d’un `UpdatePanel`, il existe un extendeur spécial qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="2236c-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="2236c-113">Il peut également fonctionner conjointement avec les déclencheurs `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="2236c-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="2236c-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="2236c-114">Steps</span></span>

<span data-ttu-id="2236c-115">La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :</span><span class="sxs-lookup"><span data-stu-id="2236c-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="2236c-116">L’animation dans ce scénario sera appliquée à l’affichage de l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="2236c-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="2236c-117">Ces informations peuvent être écrites dans une étiquette à l’aide de la méthode `Page_Load()`, ou (par souci de simplicité) le code inline suivant est utilisé :</span><span class="sxs-lookup"><span data-stu-id="2236c-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="2236c-118">En outre, un bouton pour déclencher la mise à jour de l’heure est créé :</span><span class="sxs-lookup"><span data-stu-id="2236c-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="2236c-119">Ce code est ensuite placé dans la section `<ContentTemplate>` d’un élément `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="2236c-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="2236c-120">L’attribut `UpdateMode` du panneau doit être défini sur `"Conditional"`, car seuls les déclencheurs peuvent mettre à jour le contenu du panneau.</span><span class="sxs-lookup"><span data-stu-id="2236c-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="2236c-121">Dans la section `<Triggers>` de l' `UpdatePanel`, un déclencheur de publication (postback) asynchrone est créé et lié à l’événement `Click` du bouton.</span><span class="sxs-lookup"><span data-stu-id="2236c-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="2236c-122">Ainsi, si l’utilisateur clique sur le bouton, la `UpdatePanel` est actualisée.</span><span class="sxs-lookup"><span data-stu-id="2236c-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="2236c-123">Voici le balisage pour le contrôle `UpdatePanel` :</span><span class="sxs-lookup"><span data-stu-id="2236c-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="2236c-124">Enfin, le `UpdatePanelAnimationExtender` doit être configuré : affectez à l’attribut `TargetControlID` l’ID du panneau et définissez une animation dans l’extendeur.</span><span class="sxs-lookup"><span data-stu-id="2236c-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="2236c-125">Le fondu est judicieux, ce qui crée un aspect visuel agréable sur l’heure mise à jour.</span><span class="sxs-lookup"><span data-stu-id="2236c-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="2236c-126">Le balisage de votre extendeur peut alors ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="2236c-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="2236c-127">Exécutez le fichier dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="2236c-127">Run the file in the browser.</span></span> <span data-ttu-id="2236c-128">Chaque fois que vous cliquez sur le bouton, l’heure actuelle est affichée dans le panneau, ce qui fait toujours l’objet d’un fondu pour la durée d’une seconde.</span><span class="sxs-lookup"><span data-stu-id="2236c-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="2236c-129">[![l’heure actuelle est en fondu](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2236c-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="2236c-130">L’heure actuelle est en fondu ([cliquez pour afficher l’image en taille réelle](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2236c-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2236c-131">Précédent</span><span class="sxs-lookup"><span data-stu-id="2236c-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
