---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Ajout dynamique d’un volet accordéon (VB) | Microsoft Docs
author: wenz
description: Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607213"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="27dbf-104">Ajout dynamique d’un volet accordéon (VB)</span><span class="sxs-lookup"><span data-stu-id="27dbf-104">Dynamically Adding An Accordion Pane (VB)</span></span>

<span data-ttu-id="27dbf-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="27dbf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="27dbf-106">[Télécharger le code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="27dbf-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="27dbf-107">Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois.</span><span class="sxs-lookup"><span data-stu-id="27dbf-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="27dbf-108">Les panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.</span><span class="sxs-lookup"><span data-stu-id="27dbf-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="27dbf-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="27dbf-109">Overview</span></span>

<span data-ttu-id="27dbf-110">Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois.</span><span class="sxs-lookup"><span data-stu-id="27dbf-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="27dbf-111">Les panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.</span><span class="sxs-lookup"><span data-stu-id="27dbf-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="27dbf-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="27dbf-112">Steps</span></span>

<span data-ttu-id="27dbf-113">Le contrôle accordéon expose toutes les propriétés importantes au code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="27dbf-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="27dbf-114">Entre autres choses, la propriété `Panes` accorde l’accès à la collection de volets qui composent l’accordéon.</span><span class="sxs-lookup"><span data-stu-id="27dbf-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="27dbf-115">Chaque volet est de type `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="27dbf-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="27dbf-116">Il est donc trivial de créer un tel volet :</span><span class="sxs-lookup"><span data-stu-id="27dbf-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="27dbf-117">La propriété `HeaderContainer` de `AccordionPane` permet d’accéder aux contrôles ASP.NET dans la section d’en-tête du volet. la propriété `ContentContainer` de `AccordionPane` fait de même pour la section content du volet.</span><span class="sxs-lookup"><span data-stu-id="27dbf-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="27dbf-118">Cela permet au code ASP.NET d’ajouter du contenu aux volets :</span><span class="sxs-lookup"><span data-stu-id="27dbf-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="27dbf-119">Enfin, le ou les volets doivent être ajoutés à la collection `Panes` de l’accordéon :</span><span class="sxs-lookup"><span data-stu-id="27dbf-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="27dbf-120">Voici un code complet côté serveur qui ajoute deux volets à un contrôle accordéon :</span><span class="sxs-lookup"><span data-stu-id="27dbf-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="27dbf-121">Le seul élément manquant est l’accordéon lui-même, qui dépend de la présence du contrôle de `ScriptManager` ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="27dbf-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="27dbf-122">Pour terminer l’exemple, les deux classes CSS référencées dans le contrôle accordéon fournissent des informations de style pour le navigateur :</span><span class="sxs-lookup"><span data-stu-id="27dbf-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

<span data-ttu-id="27dbf-123">[![les données de l’accordéon ont été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="27dbf-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="27dbf-124">Les données de l’accordéon ont été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="27dbf-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27dbf-125">Précédent</span><span class="sxs-lookup"><span data-stu-id="27dbf-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
