---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Réduction et développement d’un panneau à partir deC#JavaScript () | Microsoft Docs
author: wenz
description: Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599429"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="6df80-103">Réduction et développement d’un panneau à partir de JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="6df80-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>

<span data-ttu-id="6df80-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6df80-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6df80-105">[Télécharger le code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6df80-105">[Download Code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="6df80-106">Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer à nouveau.</span><span class="sxs-lookup"><span data-stu-id="6df80-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="6df80-107">Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6df80-107">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="6df80-108">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="6df80-108">Overview</span></span>

<span data-ttu-id="6df80-109">Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer à nouveau.</span><span class="sxs-lookup"><span data-stu-id="6df80-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="6df80-110">Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6df80-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="6df80-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="6df80-111">Steps</span></span>

<span data-ttu-id="6df80-112">Tout d’abord, créez une nouvelle page ASP.NET et incluez la `ScriptManager` dans l’élément `<form>`.</span><span class="sxs-lookup"><span data-stu-id="6df80-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="6df80-113">Cela charge la bibliothèque AJAX ASP.NET requise par Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="6df80-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="6df80-114">Ensuite, créez un panneau avec du texte afin que l’effet de réduction/développement puisse être affiché :</span><span class="sxs-lookup"><span data-stu-id="6df80-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="6df80-115">Comme vous pouvez le voir, le panneau fait référence à une classe CSS qui est présentée ici (et définit fondamentalement une couleur d’arrière-plan et la largeur du panneau) :</span><span class="sxs-lookup"><span data-stu-id="6df80-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="6df80-116">Le contrôle `CollapsiblePanelExtender` nécessite l’attribut `TargetControlID` afin que la boîte à outils sache quel volet réduire ou développer à la demande :</span><span class="sxs-lookup"><span data-stu-id="6df80-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="6df80-117">Malheureusement, l’extendeur n’expose pas actuellement une API spécifique pour réduire ou développer le panneau, mais certaines méthodes non documentées en font.</span><span class="sxs-lookup"><span data-stu-id="6df80-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="6df80-118">Tout d’abord, ajoutez trois boutons HTML à la page, ce qui déclenchera le JavaScript côté client pour réduire ou développer le contenu du panneau :</span><span class="sxs-lookup"><span data-stu-id="6df80-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="6df80-119">Dans le code JavaScript côté client (démarré avec `<script type="text/javascript">`), la méthode `$find()` doit être utilisée pour accéder à la `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="6df80-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="6df80-120">`$find("cpe")` retourne une référence à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="6df80-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="6df80-121">À partir de là, des méthodes spécifiques résolvent la tâche en cours.</span><span class="sxs-lookup"><span data-stu-id="6df80-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="6df80-122">La méthode permettant d’ouvrir (développer) le panneau est appelée `_doOpen()`; le code suivant implémente la fonction `doOpen()` appelée suite à un clic sur le premier bouton :</span><span class="sxs-lookup"><span data-stu-id="6df80-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="6df80-123">Pour fermer ou réduire le panneau, la méthode `_doClose()` doit être exécutée.</span><span class="sxs-lookup"><span data-stu-id="6df80-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="6df80-124">Ainsi, quand l’utilisateur clique sur le deuxième bouton, le code JavaScript suivant est appelé :</span><span class="sxs-lookup"><span data-stu-id="6df80-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="6df80-125">Le troisième bouton bascule l’état du panneau : de réduit à développé et vice versa.</span><span class="sxs-lookup"><span data-stu-id="6df80-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="6df80-126">L' `CollapsiblePanelExtender` expose la méthode `toggle()` qui effectue exactement ce qui suit : inverse l’état du panneau.</span><span class="sxs-lookup"><span data-stu-id="6df80-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="6df80-127">Toutefois, il existe également une autre approche (qui est utilisée en interne par la méthode `toggle()`) : la méthode `get_Collapsed()` de la `CollapsiblePanelExtender()` indique si le panneau est réduit ou non.</span><span class="sxs-lookup"><span data-stu-id="6df80-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="6df80-128">Selon la valeur de retour de cette fonction, le panneau est ensuite développé (méthode`_doOpen()`) ou méthode réduite (`_doClose()`) :</span><span class="sxs-lookup"><span data-stu-id="6df80-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

<span data-ttu-id="6df80-129">[![le troisième bouton modifie l’état du panneau : de réduit à développé et de retour](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6df80-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="6df80-130">Le troisième bouton modifie l’état du panneau : de réduit à développé et de retour ([cliquez pour afficher l’image en taille réelle](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6df80-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6df80-131">Suivant</span><span class="sxs-lookup"><span data-stu-id="6df80-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
