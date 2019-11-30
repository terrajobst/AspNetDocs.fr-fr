---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Liaison de liaison du contrôle SliderC#() | Microsoft Docs
author: wenz
description: Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier le positio actuel...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598588"
---
# <a name="databinding-the-slider-control-c"></a><span data-ttu-id="caa3c-104">Liaison de données du contrôle Slider (C#)</span><span class="sxs-lookup"><span data-stu-id="caa3c-104">Databinding the Slider Control (C#)</span></span>

<span data-ttu-id="caa3c-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="caa3c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="caa3c-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="caa3c-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="caa3c-107">Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="caa3c-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="caa3c-108">Il est possible de lier la position actuelle du curseur à un autre contrôle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="caa3c-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="caa3c-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="caa3c-109">Overview</span></span>

<span data-ttu-id="caa3c-110">Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="caa3c-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="caa3c-111">Il est possible de lier la position actuelle du curseur à un autre contrôle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="caa3c-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="caa3c-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="caa3c-112">Steps</span></span>

<span data-ttu-id="caa3c-113">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="caa3c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="caa3c-114">Ensuite, ajoutez deux contrôles `TextBox` à la page.</span><span class="sxs-lookup"><span data-stu-id="caa3c-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="caa3c-115">L’une est transformée en curseur graphique, tandis que l’autre contiendra la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="caa3c-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="caa3c-116">L’étape suivante est déjà la dernière étape.</span><span class="sxs-lookup"><span data-stu-id="caa3c-116">The next step is already the final step.</span></span> <span data-ttu-id="caa3c-117">Le contrôle `SliderExtender` de la boîte à outils de contrôle AJAX ASP.NET fait glisser un curseur de la première zone de texte et met automatiquement à jour la deuxième zone de texte lorsque la position du curseur est modifiée.</span><span class="sxs-lookup"><span data-stu-id="caa3c-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="caa3c-118">Pour que cela fonctionne, l’attribut `TargetControlID` de `SliderExtender`doit être défini sur l’ID de la première zone de texte. l’attribut `BoundControlID` doit avoir pour valeur l’ID de la deuxième zone de texte.</span><span class="sxs-lookup"><span data-stu-id="caa3c-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="caa3c-119">Comme vous pouvez le voir dans le navigateur, la liaison de données fonctionne dans les deux sens : l’entrée d’une nouvelle valeur dans la zone de texte met à jour la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="caa3c-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="caa3c-120">Si vous définissez la deuxième zone de texte en lecture seule, vous pouvez ajouter une protection faible au champ de texte afin qu’il soit plus difficile pour l’utilisateur de mettre à jour manuellement la valeur.</span><span class="sxs-lookup"><span data-stu-id="caa3c-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="caa3c-121">[le curseur et la zone de texte de ![sont synchronisés](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="caa3c-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="caa3c-122">Le curseur et la zone de texte sont synchronisés ([cliquez pour afficher l’image en taille réelle](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="caa3c-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="caa3c-123">[Précédent](using-the-slider-control-with-auto-postback-cs.md)
> [Suivant](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="caa3c-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
