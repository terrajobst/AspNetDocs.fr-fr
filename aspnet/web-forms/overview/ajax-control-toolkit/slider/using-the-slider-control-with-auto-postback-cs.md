---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Utilisation du contrôle Slider avec la publication automatique (C#) | Microsoft Docs
author: wenz
description: Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris. Il est possible d’effectuer la publication par le curseur...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553619"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="ee120-104">Utilisation du contrôle Slider avec publication automatique (C#)</span><span class="sxs-lookup"><span data-stu-id="ee120-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="ee120-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ee120-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ee120-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee120-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="ee120-107">Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="ee120-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="ee120-108">Il est possible de faire de la publication automatique du curseur une fois que sa valeur est modifiée.</span><span class="sxs-lookup"><span data-stu-id="ee120-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="ee120-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="ee120-109">Overview</span></span>

<span data-ttu-id="ee120-110">Le contrôle Slider dans la boîte à outils de contrôle AJAX fournit un curseur graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="ee120-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="ee120-111">Il est possible de faire de la publication automatique du curseur une fois que sa valeur est modifiée.</span><span class="sxs-lookup"><span data-stu-id="ee120-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="ee120-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="ee120-112">Steps</span></span>

<span data-ttu-id="ee120-113">Pour que le curseur effectue automatiquement une publication automatique sur une modification, les deux zones de texte ont besoin de l’attribut `AutoPostBack="true"`: la zone de texte qui deviendra le curseur lui-même et la zone de texte qui contient la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="ee120-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="ee120-114">Voici le balisage requis pour ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="ee120-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="ee120-115">Le contrôle `SliderExtender` de la boîte à outils de contrôle AJAX ASP.NET affecte la fonctionnalité de curseur aux deux zones de texte :</span><span class="sxs-lookup"><span data-stu-id="ee120-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="ee120-116">Un élément label supplémentaire sera utilisé ultérieurement pour informer l’utilisateur d’une publication (postback) :</span><span class="sxs-lookup"><span data-stu-id="ee120-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="ee120-117">Enfin, le contrôle `ScriptManager` de ASP.NET AJAX charge le code JavaScript requis pour que Control Toolkit fonctionne :</span><span class="sxs-lookup"><span data-stu-id="ee120-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="ee120-118">Maintenant, le curseur est publié. côté serveur, cet événement peut être intercepté et agi sur :</span><span class="sxs-lookup"><span data-stu-id="ee120-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

<span data-ttu-id="ee120-119">[![le déplacement du curseur déclenche une publication (postback)](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee120-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="ee120-120">Le déplacement du curseur déclenche une publication ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ee120-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>

<span data-ttu-id="ee120-121">[![par la suite, la date de cette modification est écrite dans l’étiquette](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ee120-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="ee120-122">Ensuite, la date de cette modification est écrite dans l’étiquette ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ee120-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ee120-123">Next</span><span class="sxs-lookup"><span data-stu-id="ee120-123">Next</span></span>](databinding-the-slider-control-cs.md)
