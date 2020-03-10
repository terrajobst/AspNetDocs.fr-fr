---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulation des propriétés DropShadow à partir du codeC#client () | Microsoft Docs
author: wenz
description: Personnalisation de l’interface de modification du contrôle DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613819"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="83112-103">Manipulation des propriétés de DropShadow à partir de code client (C#)</span><span class="sxs-lookup"><span data-stu-id="83112-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>

<span data-ttu-id="83112-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="83112-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="83112-105">[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="83112-105">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="83112-106">Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="83112-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="83112-107">Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.</span><span class="sxs-lookup"><span data-stu-id="83112-107">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="83112-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="83112-108">Overview</span></span>

<span data-ttu-id="83112-109">Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="83112-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="83112-110">Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.</span><span class="sxs-lookup"><span data-stu-id="83112-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="83112-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="83112-111">Steps</span></span>

<span data-ttu-id="83112-112">Le code commence par un panneau contenant des lignes de texte :</span><span class="sxs-lookup"><span data-stu-id="83112-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="83112-113">La classe CSS associée donne au panneau une couleur d’arrière-plan intéressante :</span><span class="sxs-lookup"><span data-stu-id="83112-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="83112-114">Le `DropShadowExtender` est ajouté pour étendre le panneau avec un effet d’ombre portée, l’opacité est définie sur 50% :</span><span class="sxs-lookup"><span data-stu-id="83112-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="83112-115">Ensuite, le contrôle de `ScriptManager` AJAX ASP.NET permet à Control Toolkit de fonctionner :</span><span class="sxs-lookup"><span data-stu-id="83112-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="83112-116">Un autre panneau contient deux liens JavaScript pour définir l’opacité de l’ombre portée : le lien moins diminue l’opacité de l’ombre, le lien plus l’augmente.</span><span class="sxs-lookup"><span data-stu-id="83112-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="83112-117">La fonction JavaScript `changeOpacity()` doit alors rechercher d’abord le contrôle `DropShadowExtender` sur la page.</span><span class="sxs-lookup"><span data-stu-id="83112-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="83112-118">ASP.NET AJAX définit la méthode `$find()` pour cette tâche.</span><span class="sxs-lookup"><span data-stu-id="83112-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="83112-119">Ensuite, la méthode `get_Opacity()` récupère l’opacité actuelle, la méthode `set_Opacity()` la définit.</span><span class="sxs-lookup"><span data-stu-id="83112-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="83112-120">Le code JavaScript place ensuite la valeur d’opacité actuelle dans l’élément `<label>` :</span><span class="sxs-lookup"><span data-stu-id="83112-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

<span data-ttu-id="83112-121">[![l’opacité est modifiée côté client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="83112-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="83112-122">L’opacité est modifiée côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="83112-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="83112-123">[Précédent](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Suivant](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="83112-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
