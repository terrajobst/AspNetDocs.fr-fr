---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Autoriser uniquement certains caractères dans une zone de texte (VB) | Microsoft Docs
author: wenz
description: Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur. Toutefois, cela n’empêche toujours pas les utilisateurs de taper du texte non valide...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613511"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="df948-104">Autoriser seulement certains caractères dans une zone de texte (VB)</span><span class="sxs-lookup"><span data-stu-id="df948-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>

<span data-ttu-id="df948-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="df948-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="df948-106">[Télécharger le code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="df948-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="df948-107">Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="df948-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="df948-108">Toutefois, cela n’empêche toujours pas les utilisateurs de taper des caractères non valides et de tenter d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="df948-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="df948-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="df948-109">Overview</span></span>

<span data-ttu-id="df948-110">Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="df948-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="df948-111">Toutefois, cela n’empêche toujours pas les utilisateurs de taper des caractères non valides et de tenter d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="df948-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="df948-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="df948-112">Steps</span></span>

<span data-ttu-id="df948-113">ASP.NET AJAX Control Toolkit contient le contrôle `FilteredTextBox` qui étend une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="df948-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="df948-114">Une fois activé, seul un certain jeu de caractères peut être entré dans le champ.</span><span class="sxs-lookup"><span data-stu-id="df948-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="df948-115">Pour que cela fonctionne, nous avons tout d’abord besoin de la `ScriptManager` AJAX ASP.NET qui charge les bibliothèques JavaScript qui sont également utilisées par ASP.NET AJAX Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="df948-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="df948-116">Ensuite, nous avons besoin d’une zone de texte :</span><span class="sxs-lookup"><span data-stu-id="df948-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="df948-117">Enfin, le contrôle `FilteredTextBoxExtender` s’occupe de limiter les caractères que l’utilisateur est autorisé à taper.</span><span class="sxs-lookup"><span data-stu-id="df948-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="df948-118">Tout d’abord, affectez à l’attribut `TargetControlID` la `ID` du contrôle `TextBox`.</span><span class="sxs-lookup"><span data-stu-id="df948-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="df948-119">Ensuite, choisissez l’une des valeurs de `FilterType` disponibles :</span><span class="sxs-lookup"><span data-stu-id="df948-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="df948-120">`Custom` par défaut ; vous devez fournir une liste de caractères valides</span><span class="sxs-lookup"><span data-stu-id="df948-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="df948-121">`LowercaseLetters` les lettres minuscules uniquement</span><span class="sxs-lookup"><span data-stu-id="df948-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="df948-122">`Numbers` chiffres uniquement</span><span class="sxs-lookup"><span data-stu-id="df948-122">`Numbers` digits only</span></span>
- <span data-ttu-id="df948-123">`UppercaseLetters` lettres majuscules uniquement</span><span class="sxs-lookup"><span data-stu-id="df948-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="df948-124">Si le `Custom FilterType` est utilisé, la propriété `ValidChars` doit être définie et fournir une liste de caractères pouvant être tapés.</span><span class="sxs-lookup"><span data-stu-id="df948-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="df948-125">À la façon : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="df948-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="df948-126">Voici le balisage pour le contrôle `FilteredTextBoxExtender` qui autorise uniquement les chiffres (ce qui serait également possible avec `FilterType="Numbers"`) :</span><span class="sxs-lookup"><span data-stu-id="df948-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="df948-127">Exécutez la page et essayez d’entrer une lettre si JavaScript est activé, mais cela ne fonctionnera pas. Toutefois, les chiffres apparaissent sur la page.</span><span class="sxs-lookup"><span data-stu-id="df948-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="df948-128">Toutefois, Notez que le `FilteredTextBox` de protection fourni n’est pas une preuve de la puce : si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte. vous devez donc utiliser des moyens de validation supplémentaires, par exemple, ASP. Contrôles de validation du réseau.</span><span class="sxs-lookup"><span data-stu-id="df948-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="df948-129">[![seuls les chiffres peuvent être entrés](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df948-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="df948-130">Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="df948-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="df948-131">Précédent</span><span class="sxs-lookup"><span data-stu-id="df948-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
