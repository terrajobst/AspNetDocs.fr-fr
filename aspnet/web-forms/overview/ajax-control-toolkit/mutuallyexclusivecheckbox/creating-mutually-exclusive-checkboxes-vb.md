---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Création de cases à cocher mutuellement exclusives (VB) | Microsoft Docs
author: wenz
description: 'Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées. Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554011"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="81a71-104">Création de cases à cocher mutuellement exclusives (VB)</span><span class="sxs-lookup"><span data-stu-id="81a71-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="81a71-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="81a71-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="81a71-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="81a71-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="81a71-107">Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="81a71-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="81a71-108">Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe, il n’est pas possible de décocher toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="81a71-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="81a71-109">Les cases à cocher peuvent être désactivées à tout moment, mais ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="81a71-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="81a71-110">Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="81a71-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="81a71-111">Présentation</span><span class="sxs-lookup"><span data-stu-id="81a71-111">Overview</span></span>

<span data-ttu-id="81a71-112">Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="81a71-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="81a71-113">Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe, il n’est pas possible de décocher toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="81a71-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="81a71-114">Les cases à cocher peuvent être désactivées à tout moment, mais ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="81a71-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="81a71-115">Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="81a71-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="81a71-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="81a71-116">Steps</span></span>

<span data-ttu-id="81a71-117">ASP.NET AJAX Control Toolkit contient l’extendeur MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="81a71-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="81a71-118">Cela permet aux programmeurs d’attribuer n’importe quelle case à un nom de groupe (attribut`Key`).</span><span class="sxs-lookup"><span data-stu-id="81a71-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="81a71-119">À partir de toutes les cases au sein du même groupe, une seule peut être sélectionnée à la fois.</span><span class="sxs-lookup"><span data-stu-id="81a71-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="81a71-120">Commençons par placer deux cases à cocher sur une nouvelle page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81a71-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="81a71-121">Il peut y en avoir davantage, mais deux d’entre eux suffisent à démontrer le principe :</span><span class="sxs-lookup"><span data-stu-id="81a71-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="81a71-122">Pour les deux cases à cocher, un contrôle MutuallyExclusiveCheckBoxExtender doit être placé sur la page.</span><span class="sxs-lookup"><span data-stu-id="81a71-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="81a71-123">Les deux attributs de clé doivent avoir la même valeur, tout comme les attributs de valeur des éléments de case d’option HTML doivent être identiques pour désigner le groupe auquel ils appartiennent.</span><span class="sxs-lookup"><span data-stu-id="81a71-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="81a71-124">La propriété TargetControlID de l’extendeur pointe sur l’ID de la case à cocher.</span><span class="sxs-lookup"><span data-stu-id="81a71-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="81a71-125">Enfin, incluez le `ScriptManager` AJAX ASP.NET qui est requis par tous les éléments de ASP.NET AJAX Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="81a71-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="81a71-126">Enregistrer et exécuter la page : vous pouvez activer ou désactiver les deux cases à cocher. Toutefois, à aucun moment, les deux cases à cocher peuvent être activées.</span><span class="sxs-lookup"><span data-stu-id="81a71-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="81a71-127">[![une seule case à cocher peut être activée à la fois](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="81a71-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="81a71-128">Une seule case à cocher peut être activée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="81a71-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="81a71-129">Précédent</span><span class="sxs-lookup"><span data-stu-id="81a71-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
