---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Création de cases à cocher mutuellement exclusives (VB) | Microsoft Docs
author: wenz
description: 'Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient : Une fois une case d’option dans un groupe est sélectionnée...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd338b622779b64dd59f9cf6f3e2365ef5cb3ffb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045486"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="e5b45-104">Création de cases à cocher mutuellement exclusives (VB)</span><span class="sxs-lookup"><span data-stu-id="e5b45-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="e5b45-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e5b45-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e5b45-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e5b45-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="e5b45-107">Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="e5b45-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="e5b45-108">Il y a cependant un inconvénient : Une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de décocher toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="e5b45-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="e5b45-109">Cases à cocher peut être désactivées à tout moment, toutefois, ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="e5b45-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="e5b45-110">Ce didacticiel offre le meilleur des deux approches : cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="e5b45-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="e5b45-111">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e5b45-111">Overview</span></span>

<span data-ttu-id="e5b45-112">Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="e5b45-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="e5b45-113">Il y a cependant un inconvénient : Une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de décocher toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="e5b45-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="e5b45-114">Cases à cocher peut être désactivées à tout moment, toutefois, ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="e5b45-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="e5b45-115">Ce didacticiel offre le meilleur des deux approches : cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="e5b45-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="e5b45-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="e5b45-116">Steps</span></span>

<span data-ttu-id="e5b45-117">ASP.NET AJAX Control Toolkit contient les extendeurs MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="e5b45-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="e5b45-118">Cela permet aux programmeurs d’affecter n’importe quel case à cocher à un nom de groupe (`Key` attribut).</span><span class="sxs-lookup"><span data-stu-id="e5b45-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="e5b45-119">À partir de toutes les cases à cocher dans le même groupe, seul l’un peut être sélectionné à la fois.</span><span class="sxs-lookup"><span data-stu-id="e5b45-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="e5b45-120">Commençons par placer les deux cases à cocher sur une page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5b45-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="e5b45-121">Il peut y avoir plus, mais deux d'entre eux suffit pour illustrer le principe :</span><span class="sxs-lookup"><span data-stu-id="e5b45-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="e5b45-122">Pour les deux cases à cocher, vous devez placer un contrôle MutuallyExclusiveCheckBoxExtender sur la page.</span><span class="sxs-lookup"><span data-stu-id="e5b45-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="e5b45-123">Les deux attributs de clé doivent ont la même valeur, tout comme la valeur d’attributs des éléments de bouton de case d’option HTML doivent être identiques à indiquer le groupe qu'auquel ils appartiennent.</span><span class="sxs-lookup"><span data-stu-id="e5b45-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="e5b45-124">La propriété TargetControlID de l’extendeur pointe vers l’ID de la case à cocher.</span><span class="sxs-lookup"><span data-stu-id="e5b45-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="e5b45-125">Enfin, inclure ASP.NET AJAX `ScriptManager` qui est requis par tous les éléments d’ASP.NET AJAX Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="e5b45-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="e5b45-126">Enregistrez et exécutez la page : Vous pouvez vérifier et décochez les deux cases à cocher, toutefois à aucun moment peut les deux cases à cocher être vérifiées.</span><span class="sxs-lookup"><span data-stu-id="e5b45-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="e5b45-127">[![Case à cocher qu’une seule peut être vérifiée à la fois](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e5b45-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="e5b45-128">Case à cocher qu’une seule peut être vérifiée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e5b45-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e5b45-129">Précédent</span><span class="sxs-lookup"><span data-stu-id="e5b45-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
