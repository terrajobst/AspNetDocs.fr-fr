---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Remplissage d’une liste à l’aide de CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536007"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="1f851-103">Remplissage d’une liste avec CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="1f851-103">Filling a List Using CascadingDropDown (VB)</span></span>

<span data-ttu-id="1f851-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1f851-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1f851-105">[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1f851-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="1f851-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="1f851-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="1f851-107">(Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="1f851-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="1f851-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="1f851-108">Overview</span></span>

<span data-ttu-id="1f851-109">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="1f851-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="1f851-110">(Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="1f851-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="1f851-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="1f851-111">Steps</span></span>

<span data-ttu-id="1f851-112">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="1f851-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="1f851-113">Un contrôle DropDownList est ensuite requis :</span><span class="sxs-lookup"><span data-stu-id="1f851-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="1f851-114">Pour cette liste, un extendeur CascadingDropDown est ajouté.</span><span class="sxs-lookup"><span data-stu-id="1f851-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="1f851-115">Il envoie une requête asynchrone à un service Web qui renverra ensuite une liste d’entrées à afficher dans la liste.</span><span class="sxs-lookup"><span data-stu-id="1f851-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="1f851-116">Pour que cela fonctionne, les attributs CascadingDropDown suivants doivent être définis :</span><span class="sxs-lookup"><span data-stu-id="1f851-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="1f851-117">`ServicePath`: URL d’un service Web qui fournit les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="1f851-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="1f851-118">`ServiceMethod`: méthode Web qui fournit les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="1f851-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="1f851-119">`TargetControlID`: ID de la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="1f851-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="1f851-120">`Category`: informations de catégorie soumises à la méthode Web quand elles sont appelées</span><span class="sxs-lookup"><span data-stu-id="1f851-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="1f851-121">`PromptText`: texte affiché lors du chargement asynchrone de données de liste à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="1f851-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="1f851-122">Voici le balisage de l’élément `CascadingDropDown`.</span><span class="sxs-lookup"><span data-stu-id="1f851-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="1f851-123">La seule différence entre C# et VB est le nom du service Web associé :</span><span class="sxs-lookup"><span data-stu-id="1f851-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="1f851-124">Le code JavaScript provenant de l’extendeur `CascadingDropDown` appelle une méthode de service Web avec la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="1f851-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="1f851-125">L’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (défini par ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="1f851-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="1f851-126">Dans le constructeur `CascadingDropDownNameValue`, le texte de l’entrée de liste est d’abord fourni, puis sa valeur doit être fournie, tout comme `<option value="VALUE">NAME</option>` le ferait en HTML.</span><span class="sxs-lookup"><span data-stu-id="1f851-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="1f851-127">Voici quelques exemples de données :</span><span class="sxs-lookup"><span data-stu-id="1f851-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="1f851-128">Le chargement de la page dans le navigateur déclenche l’remplissage de la liste avec trois fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1f851-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="1f851-129">[![la liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1f851-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="1f851-130">La liste est renseignée automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1f851-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1f851-131">[Précédent](using-auto-postback-with-cascadingdropdown-cs.md)
> [Suivant](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1f851-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
