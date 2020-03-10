---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Autoriser uniquement certains caractères dans une zone de texteC#() | Microsoft Docs
author: wenz
description: Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur. Toutefois, cela n’empêche toujours pas les utilisateurs de taper du texte non valide...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d1e367becd574e31d24fca8545f76b1ed3c4d85e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554235"
---
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="fcadd-104">Autoriser seulement certains caractères dans une zone de texte (C#)</span><span class="sxs-lookup"><span data-stu-id="fcadd-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>

<span data-ttu-id="fcadd-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fcadd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fcadd-106">[Télécharger le code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fcadd-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="fcadd-107">Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fcadd-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="fcadd-108">Toutefois, cela n’empêche toujours pas les utilisateurs de taper des caractères non valides et de tenter d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="fcadd-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="fcadd-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="fcadd-109">Overview</span></span>

<span data-ttu-id="fcadd-110">Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fcadd-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="fcadd-111">Toutefois, cela n’empêche toujours pas les utilisateurs de taper des caractères non valides et de tenter d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="fcadd-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="fcadd-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="fcadd-112">Steps</span></span>

<span data-ttu-id="fcadd-113">ASP.NET AJAX Control Toolkit contient le contrôle `FilteredTextBox` qui étend une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="fcadd-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="fcadd-114">Une fois activé, seul un certain jeu de caractères peut être entré dans le champ.</span><span class="sxs-lookup"><span data-stu-id="fcadd-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="fcadd-115">Pour que cela fonctionne, nous avons tout d’abord besoin de la `ScriptManager` AJAX ASP.NET qui charge les bibliothèques JavaScript qui sont également utilisées par ASP.NET AJAX Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="fcadd-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="fcadd-116">Ensuite, nous avons besoin d’une zone de texte :</span><span class="sxs-lookup"><span data-stu-id="fcadd-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="fcadd-117">Enfin, le contrôle `FilteredTextBoxExtender` s’occupe de limiter les caractères que l’utilisateur est autorisé à taper.</span><span class="sxs-lookup"><span data-stu-id="fcadd-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="fcadd-118">Tout d’abord, affectez à l’attribut `TargetControlID` la `ID` du contrôle `TextBox`.</span><span class="sxs-lookup"><span data-stu-id="fcadd-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="fcadd-119">Ensuite, choisissez l’une des valeurs de `FilterType` disponibles :</span><span class="sxs-lookup"><span data-stu-id="fcadd-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="fcadd-120">`Custom` par défaut ; vous devez fournir une liste de caractères valides</span><span class="sxs-lookup"><span data-stu-id="fcadd-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="fcadd-121">`LowercaseLetters` les lettres minuscules uniquement</span><span class="sxs-lookup"><span data-stu-id="fcadd-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="fcadd-122">`Numbers` chiffres uniquement</span><span class="sxs-lookup"><span data-stu-id="fcadd-122">`Numbers` digits only</span></span>
- <span data-ttu-id="fcadd-123">`UppercaseLetters` lettres majuscules uniquement</span><span class="sxs-lookup"><span data-stu-id="fcadd-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="fcadd-124">Si le `Custom FilterType` est utilisé, la propriété `ValidChars` doit être définie et fournir une liste de caractères pouvant être tapés.</span><span class="sxs-lookup"><span data-stu-id="fcadd-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="fcadd-125">À la façon : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="fcadd-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="fcadd-126">Voici le balisage pour le contrôle `FilteredTextBoxExtender` qui autorise uniquement les chiffres (ce qui serait également possible avec `FilterType="Numbers"`) :</span><span class="sxs-lookup"><span data-stu-id="fcadd-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="fcadd-127">Exécutez la page et essayez d’entrer une lettre si JavaScript est activé, mais cela ne fonctionnera pas. Toutefois, les chiffres apparaissent sur la page.</span><span class="sxs-lookup"><span data-stu-id="fcadd-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="fcadd-128">Toutefois, Notez que le `FilteredTextBox` de protection fourni n’est pas une preuve de la puce : si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte. vous devez donc utiliser des moyens de validation supplémentaires, par exemple, ASP. Contrôles de validation du réseau.</span><span class="sxs-lookup"><span data-stu-id="fcadd-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="fcadd-129">[![seuls les chiffres peuvent être entrés](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fcadd-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="fcadd-130">Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fcadd-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fcadd-131">Next</span><span class="sxs-lookup"><span data-stu-id="fcadd-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
