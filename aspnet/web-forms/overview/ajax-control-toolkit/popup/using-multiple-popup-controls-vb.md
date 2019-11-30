---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Utilisation de plusieurs contrôles Popup (VB) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Il est également possible d’utiliser m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611590"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="31fba-104">Utilisation de plusieurs contrôles de fenêtre contextuelle (VB)</span><span class="sxs-lookup"><span data-stu-id="31fba-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="31fba-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="31fba-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="31fba-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="31fba-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="31fba-107">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="31fba-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="31fba-108">Il est également possible d’utiliser plusieurs contrôles Popup sur une page.</span><span class="sxs-lookup"><span data-stu-id="31fba-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="31fba-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="31fba-109">Overview</span></span>

<span data-ttu-id="31fba-110">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="31fba-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="31fba-111">Il est également possible d’utiliser plusieurs contrôles Popup sur une page.</span><span class="sxs-lookup"><span data-stu-id="31fba-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="31fba-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="31fba-112">Steps</span></span>

<span data-ttu-id="31fba-113">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="31fba-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="31fba-114">Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="31fba-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="31fba-115">Dans le scénario actuel, le panneau contient un contrôle de `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="31fba-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="31fba-116">Afin d’éviter l’actualisation des pages provoquée par les publications du calendrier, le panneau est placé dans un contrôle `UpdatePanel` :</span><span class="sxs-lookup"><span data-stu-id="31fba-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="31fba-117">La page contient également deux zones de texte.</span><span class="sxs-lookup"><span data-stu-id="31fba-117">The page also contains two text boxes.</span></span> <span data-ttu-id="31fba-118">Pour chaque zone de texte, la fenêtre contextuelle du calendrier s’affiche une fois que la zone de texte est activée.</span><span class="sxs-lookup"><span data-stu-id="31fba-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="31fba-119">À présent, étendez chacune des deux zones de texte avec une `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="31fba-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="31fba-120">L’attribut `TargetControlID` fournit l’ID du contrôle lié à l’extendeur.</span><span class="sxs-lookup"><span data-stu-id="31fba-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="31fba-121">L’attribut `PopupControlID` contient l’ID du panneau contextuel.</span><span class="sxs-lookup"><span data-stu-id="31fba-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="31fba-122">Dans ce cas, les deux extendeurs affichent le même panneau, mais des panneaux différents sont également possibles.</span><span class="sxs-lookup"><span data-stu-id="31fba-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="31fba-123">À présent, lorsque vous cliquez dans un champ de texte, un calendrier apparaît sous le champ, ce qui vous permet de sélectionner une date.</span><span class="sxs-lookup"><span data-stu-id="31fba-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="31fba-124">(L’extraction de la date sélectionnée dans les zones de texte sera traitée dans un autre didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="31fba-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="31fba-125">[![le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="31fba-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="31fba-126">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="31fba-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="31fba-127">[Précédent](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Suivant](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="31fba-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
