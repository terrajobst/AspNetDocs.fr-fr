---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Création d’un contrôle Numeric up/UpDown avec un backend de serviceC#Web () | Microsoft Docs
author: wenz
description: Au lieu de laisser un utilisateur taper une valeur dans une case à cocher, un contrôle numérique up/UpDown (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598908"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="c91bc-103">Création d’un contrôle Haut/bas numérique avec un backend de service web (C#)</span><span class="sxs-lookup"><span data-stu-id="c91bc-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>

<span data-ttu-id="c91bc-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c91bc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c91bc-105">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c91bc-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="c91bc-106">Au lieu de laisser un utilisateur taper une valeur dans une case à cocher, un contrôle numérique haut/PG. (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus confortable.</span><span class="sxs-lookup"><span data-stu-id="c91bc-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="c91bc-107">Par défaut, le contrôle NumericUpDown augmente ou diminue toujours une valeur de 1, mais un service Web prouve plus de flexibilité.</span><span class="sxs-lookup"><span data-stu-id="c91bc-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="c91bc-108">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="c91bc-108">Overview</span></span>

<span data-ttu-id="c91bc-109">Au lieu de laisser un utilisateur taper une valeur dans une case à cocher, un contrôle numérique haut/PG. (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus confortable.</span><span class="sxs-lookup"><span data-stu-id="c91bc-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="c91bc-110">Par défaut, le contrôle de `NumericUpDown` augmente ou diminue toujours une valeur de 1, mais un service Web prouve plus de flexibilité.</span><span class="sxs-lookup"><span data-stu-id="c91bc-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="c91bc-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="c91bc-111">Steps</span></span>

<span data-ttu-id="c91bc-112">ASP.NET AJAX Control Toolkit contient l’extendeur `NumericUpDown` qui ajoute automatiquement deux boutons à une zone de texte : un pour l’augmentation de sa valeur, un pour le réduire.</span><span class="sxs-lookup"><span data-stu-id="c91bc-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="c91bc-113">Toutefois, le contrôle prend également en charge un appel de service Web (ou un appel de méthode de page).</span><span class="sxs-lookup"><span data-stu-id="c91bc-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="c91bc-114">Chaque fois que l’utilisateur clique sur le bouton haut ou haut, le code JavaScript se connecte au serveur Web et y exécute une méthode.</span><span class="sxs-lookup"><span data-stu-id="c91bc-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="c91bc-115">La signature de la méthode est la suivante :</span><span class="sxs-lookup"><span data-stu-id="c91bc-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="c91bc-116">L’argument `current` est la valeur actuelle dans la zone de texte ; l’attribut `tag` est des données de contexte supplémentaires qui peuvent être définies en tant que propriété de l’extendeur `NumericUpDown` (mais n’est pas obligatoire).</span><span class="sxs-lookup"><span data-stu-id="c91bc-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="c91bc-117">Pour cet exemple, le contrôle Numeric up/UpDown doit autoriser uniquement les valeurs qui sont des puissances de deux : 1, 2, 4, 8, 16, 32, 64, etc.</span><span class="sxs-lookup"><span data-stu-id="c91bc-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="c91bc-118">Par conséquent, la méthode exécutée lorsque l’utilisateur souhaite augmenter la valeur doit doubler l’ancienne valeur ; l’autre méthode doit diviser value par deux.</span><span class="sxs-lookup"><span data-stu-id="c91bc-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="c91bc-119">Voici le service Web complet :</span><span class="sxs-lookup"><span data-stu-id="c91bc-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="c91bc-120">Enfin, créez une nouvelle page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c91bc-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="c91bc-121">Comme d’habitude, vous avez besoin d’un contrôle `ScriptManager`, d’un contrôle `TextBox` et d’un contrôle `NumericUpDownExtender`.</span><span class="sxs-lookup"><span data-stu-id="c91bc-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="c91bc-122">Pour ce dernier, vous devez fournir les informations sur le service Web :</span><span class="sxs-lookup"><span data-stu-id="c91bc-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="c91bc-123">`ServiceDownMethod` le nom de la méthode Web ou de la méthode de page en aval</span><span class="sxs-lookup"><span data-stu-id="c91bc-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="c91bc-124">`ServiceDownPath` chemin d’accès au service Web avec la méthode de service inactive ; omettre si vous utilisez une méthode de page</span><span class="sxs-lookup"><span data-stu-id="c91bc-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="c91bc-125">`ServiceUpMethod` le nom de la méthode Web ou de la méthode de page précédente</span><span class="sxs-lookup"><span data-stu-id="c91bc-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="c91bc-126">`ServiceUpPath` chemin d’accès au service Web avec la méthode de service up ; omettre si vous utilisez une méthode de page</span><span class="sxs-lookup"><span data-stu-id="c91bc-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="c91bc-127">Voici le balisage complet de la page :</span><span class="sxs-lookup"><span data-stu-id="c91bc-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="c91bc-128">Si vous exécutez la page, vous remarquez que la valeur de la zone de texte se double toujours lorsque vous cliquez sur le bouton supérieur, et est divisée en deux lorsque vous cliquez sur le bouton du bas.</span><span class="sxs-lookup"><span data-stu-id="c91bc-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="c91bc-129">[![uniquement les nombres qui sont une puissance de 2 apparaissent](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c91bc-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span></span>

<span data-ttu-id="c91bc-130">Seuls les nombres qui sont une puissance de 2 s’affichent ([cliquez pour afficher l’image en taille réelle](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c91bc-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c91bc-131">Suivant</span><span class="sxs-lookup"><span data-stu-id="c91bc-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
