---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Utilisation de publications (postback)C#avec ReorderList () | Microsoft Docs
author: wenz
description: Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. Chaque fois que la liste est réorganisée, un bon de commande...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611385"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="75530-104">Utilisation de publications (postback) avec ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="75530-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="75530-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="75530-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="75530-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="75530-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="75530-107">Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer.</span><span class="sxs-lookup"><span data-stu-id="75530-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="75530-108">Chaque fois que la liste est réorganisée, une publication (postback) informe le serveur de la modification.</span><span class="sxs-lookup"><span data-stu-id="75530-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="75530-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="75530-109">Overview</span></span>

<span data-ttu-id="75530-110">Le contrôle `ReorderList` dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer.</span><span class="sxs-lookup"><span data-stu-id="75530-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="75530-111">Chaque fois que la liste est réorganisée, une publication (postback) informe le serveur de la modification.</span><span class="sxs-lookup"><span data-stu-id="75530-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="75530-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="75530-112">Steps</span></span>

<span data-ttu-id="75530-113">Il existe plusieurs sources de données possibles pour le contrôle `ReorderList`.</span><span class="sxs-lookup"><span data-stu-id="75530-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="75530-114">La première consiste à utiliser un contrôle de `XmlDataSource` :</span><span class="sxs-lookup"><span data-stu-id="75530-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="75530-115">Pour lier ce code XML à un contrôle de `ReorderList` et activer les publications, les attributs suivants doivent être définis :</span><span class="sxs-lookup"><span data-stu-id="75530-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="75530-116">`DataSourceID`: ID de la source de données</span><span class="sxs-lookup"><span data-stu-id="75530-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="75530-117">`SortOrderField`: la propriété sur laquelle effectuer le tri</span><span class="sxs-lookup"><span data-stu-id="75530-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="75530-118">`AllowReorder`: indique s’il faut autoriser l’utilisateur à réorganiser les éléments de liste</span><span class="sxs-lookup"><span data-stu-id="75530-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="75530-119">`PostBackOnReorder`: indique s’il faut créer une publication (postback) à chaque réorganisation de la liste</span><span class="sxs-lookup"><span data-stu-id="75530-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="75530-120">Voici le balisage approprié pour le contrôle :</span><span class="sxs-lookup"><span data-stu-id="75530-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="75530-121">Dans le contrôle `ReorderList`, les données spécifiques de la source de données peuvent être liées à l’aide de la méthode `Eval()` :</span><span class="sxs-lookup"><span data-stu-id="75530-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="75530-122">À une position arbitraire sur la page, une étiquette contiendra les informations lorsque la dernière réorganisation s’est produite :</span><span class="sxs-lookup"><span data-stu-id="75530-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="75530-123">Cette étiquette est remplie avec du texte dans le code côté serveur, ce qui gère la publication (postback) :</span><span class="sxs-lookup"><span data-stu-id="75530-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="75530-124">Enfin, pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé sur la page :</span><span class="sxs-lookup"><span data-stu-id="75530-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

<span data-ttu-id="75530-125">[![chaque réorganisation déclenche une publication (postback)](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="75530-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="75530-126">Chaque réorganisation déclenche une publication ([cliquez pour afficher l’image en taille réelle](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="75530-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="75530-127">Suivant</span><span class="sxs-lookup"><span data-stu-id="75530-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
