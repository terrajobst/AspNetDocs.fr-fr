---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modification des animations du côté serveur (C#) | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 0594efea9598a6c2461a72f789b5bd5f8ece23da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575184"
---
# <a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="5cdb5-104">Modification des animations du côté serveur (C#)</span><span class="sxs-lookup"><span data-stu-id="5cdb5-104">Modifying Animations From The Server Side (C#)</span></span>

<span data-ttu-id="5cdb5-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5cdb5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5cdb5-106">[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5cdb5-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="5cdb5-107">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="5cdb5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5cdb5-108">Les animations peuvent également être modifiées côté serveur</span><span class="sxs-lookup"><span data-stu-id="5cdb5-108">The animations may also be changed on the server-side</span></span>

## <a name="overview"></a><span data-ttu-id="5cdb5-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="5cdb5-109">Overview</span></span>

<span data-ttu-id="5cdb5-110">Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="5cdb5-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5cdb5-111">Les animations peuvent également être modifiées côté serveur</span><span class="sxs-lookup"><span data-stu-id="5cdb5-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="5cdb5-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="5cdb5-112">Steps</span></span>

<span data-ttu-id="5cdb5-113">Tout d’abord, incluez la `ScriptManager` dans la page ; Ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="5cdb5-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="5cdb5-114">L’animation sera appliquée à un panneau de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="5cdb5-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="5cdb5-115">Dans la classe CSS associée du panneau, définissez une couleur d’arrière-plan intéressante et définissez également une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="5cdb5-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="5cdb5-116">Le reste du code s’exécute côté serveur et n’utilise pas le balisage ; au lieu de cela, il utilise du code pour créer le contrôle `AnimationExtender` :</span><span class="sxs-lookup"><span data-stu-id="5cdb5-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="5cdb5-117">Toutefois, le kit d’outils de contrôle ne fournit pas actuellement d’accès d’API pour créer les animations individuelles.</span><span class="sxs-lookup"><span data-stu-id="5cdb5-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="5cdb5-118">Il est toutefois possible de définir la propriété animations de `AnimationExtender`sur une chaîne contenant le balisage XML utilisé pour assigner les animations de façon déclarative.</span><span class="sxs-lookup"><span data-stu-id="5cdb5-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="5cdb5-119">Pour créer le fichier XML qui ne doit pas contenir l’élément `<Animations>` vous pouvez utiliser la prise en charge XML de .NET Framework ou, comme dans le code suivant, il vous suffit de fournir la chaîne :</span><span class="sxs-lookup"><span data-stu-id="5cdb5-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="5cdb5-120">Enfin, ajoutez le contrôle `AnimationExtender` à la page active, au sein de l’élément `<form runat="server">`, en vous assurant que l’animation est incluse et s’exécute :</span><span class="sxs-lookup"><span data-stu-id="5cdb5-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]

<span data-ttu-id="5cdb5-121">[![l’animation est créée à l’aide du C#code/VB côté serveur](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5cdb5-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="5cdb5-122">L’animation est créée à l’aide du C#code/VB côté serveur ([cliquez pour afficher l’image en taille réelle](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5cdb5-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cdb5-123">[Précédent](triggering-an-animation-in-another-control-cs.md)
> [Suivant](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5cdb5-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
