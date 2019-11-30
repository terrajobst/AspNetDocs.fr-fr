---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustement de l’index Z d’un DropShadow (VB) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, pour Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 10495a9590ce1f25e9e3fa218ac5144268f50711
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574172"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="7ef30-104">Ajustement de l’index-Z d’un DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="7ef30-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>

<span data-ttu-id="7ef30-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7ef30-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7ef30-106">[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7ef30-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="7ef30-107">Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="7ef30-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7ef30-108">Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, par exemple le contrôle de menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7ef30-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="7ef30-109">Lorsqu’une entrée de menu s’affiche, elle apparaît derrière l’ombre portée.</span><span class="sxs-lookup"><span data-stu-id="7ef30-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="7ef30-110">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="7ef30-110">Overview</span></span>

<span data-ttu-id="7ef30-111">Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="7ef30-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7ef30-112">Toutefois, cette ombre est parfois en conflit avec d’autres contrôles, par exemple le contrôle de menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7ef30-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="7ef30-113">Lorsqu’une entrée de menu s’affiche, elle apparaît derrière l’ombre portée.</span><span class="sxs-lookup"><span data-stu-id="7ef30-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="7ef30-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="7ef30-114">Steps</span></span>

<span data-ttu-id="7ef30-115">Le code commence par le panneau lui-même, contenant suffisamment de texte, de sorte que le panneau contient suffisamment de texte pour que l’effet soit visible :</span><span class="sxs-lookup"><span data-stu-id="7ef30-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="7ef30-116">Un autre panneau est placé directement avant le panneau `panelShadow`.</span><span class="sxs-lookup"><span data-stu-id="7ef30-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="7ef30-117">Elle contient un menu avec une orientation horizontale afin que les entrées de menu s’affichent au-dessus (ou plutôt : sous) du panneau `dropShadow`) :</span><span class="sxs-lookup"><span data-stu-id="7ef30-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="7ef30-118">Ensuite, le `DropShadowExtender` est ajouté pour étendre le panneau `panelShadow` avec un effet d’ombre portée :</span><span class="sxs-lookup"><span data-stu-id="7ef30-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="7ef30-119">Enfin, le contrôle de `ScriptManager` AJAX ASP.NET permet au Control Toolkit de fonctionner :</span><span class="sxs-lookup"><span data-stu-id="7ef30-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="7ef30-120">Lorsque vous exécutez ce script, les entrées de menu s’affichent sous le panneau.</span><span class="sxs-lookup"><span data-stu-id="7ef30-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="7ef30-121">Toutefois, le menu utilise la classe CSS `panel` où vous devez simplement définir deux choses pour faire apparaître les éléments devant l’autre panneau :</span><span class="sxs-lookup"><span data-stu-id="7ef30-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="7ef30-122">Positionnement relatif</span><span class="sxs-lookup"><span data-stu-id="7ef30-122">Relative positioning</span></span>
- <span data-ttu-id="7ef30-123">Z-index positif</span><span class="sxs-lookup"><span data-stu-id="7ef30-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="7ef30-124">Ensuite, le contrôle `DropShadowExtender` n’est plus en conflit avec le contrôle Menu.</span><span class="sxs-lookup"><span data-stu-id="7ef30-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="7ef30-125">[![avant : l’entrée de menu n’est pas visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7ef30-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="7ef30-126">Avant : l’entrée de menu n’est pas visible ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7ef30-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>

<span data-ttu-id="7ef30-127">[![après : l’entrée de menu s’affiche](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7ef30-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="7ef30-128">Après : l’entrée de menu s’affiche ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7ef30-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7ef30-129">[Précédent](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Suivant](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7ef30-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
