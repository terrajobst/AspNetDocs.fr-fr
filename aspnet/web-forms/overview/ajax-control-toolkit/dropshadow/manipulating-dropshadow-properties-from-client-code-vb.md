---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulation des propriétés DropShadow à partir du code client (VB) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Les propriétés de cet extendeur peuvent également être modifiées à l’aide du client JavaScrip...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613798"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="a32cd-104">Manipulation des propriétés de DropShadow à partir de code client (VB)</span><span class="sxs-lookup"><span data-stu-id="a32cd-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>

<span data-ttu-id="a32cd-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a32cd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a32cd-106">[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a32cd-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="a32cd-107">Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="a32cd-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a32cd-108">Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.</span><span class="sxs-lookup"><span data-stu-id="a32cd-108">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="a32cd-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="a32cd-109">Overview</span></span>

<span data-ttu-id="a32cd-110">Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="a32cd-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a32cd-111">Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.</span><span class="sxs-lookup"><span data-stu-id="a32cd-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a32cd-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="a32cd-112">Steps</span></span>

<span data-ttu-id="a32cd-113">Le code commence par un panneau contenant des lignes de texte :</span><span class="sxs-lookup"><span data-stu-id="a32cd-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="a32cd-114">La classe CSS associée donne au panneau une couleur d’arrière-plan intéressante :</span><span class="sxs-lookup"><span data-stu-id="a32cd-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="a32cd-115">Le `DropShadowExtender` est ajouté pour étendre le panneau avec un effet d’ombre portée, l’opacité est définie sur 50% :</span><span class="sxs-lookup"><span data-stu-id="a32cd-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="a32cd-116">Ensuite, le contrôle de `ScriptManager` AJAX ASP.NET permet à Control Toolkit de fonctionner :</span><span class="sxs-lookup"><span data-stu-id="a32cd-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="a32cd-117">Un autre panneau contient deux liens JavaScript pour définir l’opacité de l’ombre portée : le lien moins diminue l’opacité de l’ombre, le lien plus l’augmente.</span><span class="sxs-lookup"><span data-stu-id="a32cd-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="a32cd-118">La fonction JavaScript `changeOpacity()` doit alors rechercher d’abord le contrôle `DropShadowExtender` sur la page.</span><span class="sxs-lookup"><span data-stu-id="a32cd-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="a32cd-119">ASP.NET AJAX définit la méthode `$find()` pour cette tâche.</span><span class="sxs-lookup"><span data-stu-id="a32cd-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="a32cd-120">Ensuite, la méthode `get_Opacity()` récupère l’opacité actuelle, la méthode `set_Opacity()` la définit.</span><span class="sxs-lookup"><span data-stu-id="a32cd-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="a32cd-121">Le code JavaScript place ensuite la valeur d’opacité actuelle dans l’élément `<label>` :</span><span class="sxs-lookup"><span data-stu-id="a32cd-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

<span data-ttu-id="a32cd-122">[![l’opacité est modifiée côté client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a32cd-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="a32cd-123">L’opacité est modifiée côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a32cd-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a32cd-124">Précédent</span><span class="sxs-lookup"><span data-stu-id="a32cd-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
