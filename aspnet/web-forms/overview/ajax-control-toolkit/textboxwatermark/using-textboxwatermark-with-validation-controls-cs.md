---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Utilisation de TextBoxWatermark avec des contrôlesC#de validation () | Microsoft Docs
author: wenz
description: Le contrôle TextBoxWatermark dans la boîte à outils du contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Quand un utilisateur clique sur la zone, je vais...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577839"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="717a5-104">Utilisation de TextBoxWatermark avec des contrôles Validation (C#)</span><span class="sxs-lookup"><span data-stu-id="717a5-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="717a5-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="717a5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="717a5-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="717a5-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="717a5-107">Le contrôle TextBoxWatermark dans la boîte à outils du contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone.</span><span class="sxs-lookup"><span data-stu-id="717a5-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="717a5-108">Lorsqu’un utilisateur clique sur la zone, il est vidé.</span><span class="sxs-lookup"><span data-stu-id="717a5-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="717a5-109">Si l’utilisateur quitte la zone sans entrer de texte, le texte prérempli réapparaît.</span><span class="sxs-lookup"><span data-stu-id="717a5-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="717a5-110">Cela peut entrer en conflit avec des contrôles de validation ASP.NET sur la même page, mais ces problèmes peuvent être résolus.</span><span class="sxs-lookup"><span data-stu-id="717a5-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="717a5-111">Présentation</span><span class="sxs-lookup"><span data-stu-id="717a5-111">Overview</span></span>

<span data-ttu-id="717a5-112">Le contrôle `TextBoxWatermark` dans le kit d’outils de contrôle AJAX étend une zone de texte pour qu’un texte s’affiche dans la zone.</span><span class="sxs-lookup"><span data-stu-id="717a5-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="717a5-113">Lorsqu’un utilisateur clique sur la zone, il est vidé.</span><span class="sxs-lookup"><span data-stu-id="717a5-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="717a5-114">Si l’utilisateur quitte la zone sans entrer de texte, le texte prérempli réapparaît.</span><span class="sxs-lookup"><span data-stu-id="717a5-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="717a5-115">Cela peut entrer en conflit avec des contrôles de validation ASP.NET sur la même page, mais ces problèmes peuvent être résolus.</span><span class="sxs-lookup"><span data-stu-id="717a5-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="717a5-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="717a5-116">Steps</span></span>

<span data-ttu-id="717a5-117">Le paramétrage de base de l’exemple est le suivant : un contrôle de `TextBox` est en filigrane à l’aide d’un contrôle `TextBoxWatermarkExtender`.</span><span class="sxs-lookup"><span data-stu-id="717a5-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="717a5-118">Un bouton déclenche une publication (postback) et sera utilisé ultérieurement pour déclencher les contrôles de validation sur la page.</span><span class="sxs-lookup"><span data-stu-id="717a5-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="717a5-119">En outre, un contrôle de `ScriptManager` est requis pour initialiser ASP.NET AJAX :</span><span class="sxs-lookup"><span data-stu-id="717a5-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="717a5-120">Ajoutez maintenant un contrôle de `RequiredFieldValidator` qui vérifie s’il existe un texte dans le champ lorsque le formulaire est envoyé.</span><span class="sxs-lookup"><span data-stu-id="717a5-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="717a5-121">La valeur de la propriété `InitialValue` du validateur doit être identique à celle utilisée dans le contrôle `TextBoxWatermarkExtender` : lorsque le formulaire est envoyé, la valeur d’une zone de texte inchangée est la valeur de filigrane dans celle-ci :</span><span class="sxs-lookup"><span data-stu-id="717a5-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="717a5-122">Toutefois, il existe un problème avec cette approche : si le client désactive JavaScript, le champ de texte n’est pas prérempli avec le texte de filigrane, par conséquent le `RequiredFieldValidator` ne déclenche pas de message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="717a5-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="717a5-123">Par conséquent, un deuxième `RequiredFieldValidator` contrôle est requis, qui recherche une zone de texte vide (en omettant l’attribut `InitialValue`).</span><span class="sxs-lookup"><span data-stu-id="717a5-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="717a5-124">Étant donné que les deux validateurs utilisent `Display`=`"Dynamic"`, l’utilisateur final ne peut pas distinguer de l’apparence visuelle laquelle des deux validateurs ont été déclenchés ; au lieu de cela, il semblerait qu’il n’y en avait qu’un seul.</span><span class="sxs-lookup"><span data-stu-id="717a5-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="717a5-125">Enfin, ajoutez du code côté serveur pour générer le texte dans le champ si aucun validateur n’a émis de message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="717a5-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="717a5-126">[![le validateur se plaint qu’il n’y a aucun texte dans le champ](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="717a5-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="717a5-127">Le validateur se plaint qu’il n’y a pas de texte dans le champ ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="717a5-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="717a5-128">[Précédent](using-textboxwatermark-in-a-formview-cs.md)
> [Suivant](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="717a5-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
