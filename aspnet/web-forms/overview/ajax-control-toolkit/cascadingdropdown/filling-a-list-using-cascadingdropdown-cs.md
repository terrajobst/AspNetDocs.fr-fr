---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Remplissage d’une liste à l'C#aide de CascadingDropDown () | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: b5e9874fb5b6d3e55c8af5b85d12bf1ffacc116b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614211"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="28f5c-103">Remplissage d’une liste avec CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="28f5c-103">Filling a List Using CascadingDropDown (C#)</span></span>

<span data-ttu-id="28f5c-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="28f5c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="28f5c-105">[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="28f5c-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="28f5c-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="28f5c-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="28f5c-107">(Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="28f5c-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="28f5c-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="28f5c-108">Overview</span></span>

<span data-ttu-id="28f5c-109">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="28f5c-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="28f5c-110">(Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="28f5c-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="28f5c-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="28f5c-111">Steps</span></span>

<span data-ttu-id="28f5c-112">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="28f5c-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="28f5c-113">Un contrôle DropDownList est ensuite requis :</span><span class="sxs-lookup"><span data-stu-id="28f5c-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="28f5c-114">Pour cette liste, un extendeur CascadingDropDown est ajouté.</span><span class="sxs-lookup"><span data-stu-id="28f5c-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="28f5c-115">Il envoie une requête asynchrone à un service Web qui renverra ensuite une liste d’entrées à afficher dans la liste.</span><span class="sxs-lookup"><span data-stu-id="28f5c-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="28f5c-116">Pour que cela fonctionne, les attributs CascadingDropDown suivants doivent être définis :</span><span class="sxs-lookup"><span data-stu-id="28f5c-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="28f5c-117">`ServicePath`: URL d’un service Web qui fournit les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="28f5c-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="28f5c-118">`ServiceMethod`: méthode Web qui fournit les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="28f5c-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="28f5c-119">`TargetControlID`: ID de la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="28f5c-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="28f5c-120">`Category`: informations de catégorie soumises à la méthode Web quand elles sont appelées</span><span class="sxs-lookup"><span data-stu-id="28f5c-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="28f5c-121">`PromptText`: texte affiché lors du chargement asynchrone de données de liste à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="28f5c-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="28f5c-122">Voici le balisage de l’élément `CascadingDropDown`.</span><span class="sxs-lookup"><span data-stu-id="28f5c-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="28f5c-123">La seule différence entre C# et VB est le nom du service Web associé :</span><span class="sxs-lookup"><span data-stu-id="28f5c-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="28f5c-124">Le code JavaScript provenant de l’extendeur `CascadingDropDown` appelle une méthode de service Web avec la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="28f5c-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="28f5c-125">L’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (défini par ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="28f5c-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="28f5c-126">Dans le constructeur `CascadingDropDownNameValue`, le texte de l’entrée de liste est d’abord fourni, puis sa valeur doit être fournie, tout comme `<option value="VALUE">NAME</option>` le ferait en HTML.</span><span class="sxs-lookup"><span data-stu-id="28f5c-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="28f5c-127">Voici quelques exemples de données :</span><span class="sxs-lookup"><span data-stu-id="28f5c-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="28f5c-128">Le chargement de la page dans le navigateur déclenche l’remplissage de la liste avec trois fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="28f5c-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="28f5c-129">[![la liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="28f5c-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="28f5c-130">La liste est renseignée automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="28f5c-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="28f5c-131">Next</span><span class="sxs-lookup"><span data-stu-id="28f5c-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
