---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Liaison de liaison à un accordéon (VB) | Microsoft Docs
author: wenz
description: Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: bfb31940c0395c7ed1d5d471fb8fb686b66c59ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614519"
---
# <a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="0877d-104">Liaison de données à un Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="0877d-104">Databinding to an Accordion (VB)</span></span>

<span data-ttu-id="0877d-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0877d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0877d-106">[Télécharger le code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0877d-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="0877d-107">Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois.</span><span class="sxs-lookup"><span data-stu-id="0877d-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="0877d-108">Les panneaux sont généralement déclarés dans la page elle-même, mais la liaison à une source de données offre plus de flexibilité.</span><span class="sxs-lookup"><span data-stu-id="0877d-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="0877d-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="0877d-109">Overview</span></span>

<span data-ttu-id="0877d-110">Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois.</span><span class="sxs-lookup"><span data-stu-id="0877d-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="0877d-111">Les panneaux sont généralement déclarés dans la page elle-même, mais la liaison à une source de données offre plus de flexibilité.</span><span class="sxs-lookup"><span data-stu-id="0877d-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="0877d-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="0877d-112">Steps</span></span>

<span data-ttu-id="0877d-113">Tout d’abord, une source de données est requise.</span><span class="sxs-lookup"><span data-stu-id="0877d-113">First of all, a data source is required.</span></span> <span data-ttu-id="0877d-114">Cet exemple utilise la base de données AdventureWorks et la Microsoft SQL Server édition Express 2005.</span><span class="sxs-lookup"><span data-stu-id="0877d-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="0877d-115">La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition Express) et est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="0877d-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0877d-116">La base de données AdventureWorks fait partie des exemples SQL Server 2005 et des exemples de bases de données (téléchargez à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)en).</span><span class="sxs-lookup"><span data-stu-id="0877d-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="0877d-117">Le moyen le plus simple de définir la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)en) et à attacher le fichier de base de données de `AdventureWorks.mdf`.</span><span class="sxs-lookup"><span data-stu-id="0877d-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="0877d-118">Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut.</span><span class="sxs-lookup"><span data-stu-id="0877d-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0877d-119">Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.</span><span class="sxs-lookup"><span data-stu-id="0877d-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0877d-120">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="0877d-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="0877d-121">Ajoutez ensuite une source de données à la page.</span><span class="sxs-lookup"><span data-stu-id="0877d-121">Then, add a data source to the page.</span></span> <span data-ttu-id="0877d-122">Afin d’utiliser une quantité limitée de données, seules les cinq premières entrées de la table Vendor de la base de données AdventureWorks sont sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="0877d-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="0877d-123">Si vous utilisez l’Assistant Visual Studio pour créer la source de données, n’oubliez pas qu’un bogue dans la version actuelle ne précède pas le nom de la table (`Vendor`) avec `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="0877d-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="0877d-124">Le balisage suivant illustre la syntaxe correcte :</span><span class="sxs-lookup"><span data-stu-id="0877d-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="0877d-125">N’oubliez pas le nom (ID) de la source de données.</span><span class="sxs-lookup"><span data-stu-id="0877d-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="0877d-126">Cette identification doit ensuite être utilisée dans la propriété `DataSourceID` du contrôle accordéon :</span><span class="sxs-lookup"><span data-stu-id="0877d-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="0877d-127">Dans le contrôle accordéon, vous pouvez fournir des modèles pour différentes parties du contrôle, y compris l’en-tête (`<HeaderTemplate>`) et le contenu (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="0877d-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="0877d-128">Au sein de ces éléments, il suffit de sortir les données de la source de données à l’aide de la méthode `DataBinder.Eval()` :</span><span class="sxs-lookup"><span data-stu-id="0877d-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="0877d-129">Lorsque la page est chargée, la source de données doit être liée à l’accordéon avec ce code côté serveur :</span><span class="sxs-lookup"><span data-stu-id="0877d-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="0877d-130">Pour conclure cet exemple, vous devez définir les deux classes CSS qui sont référencées dans le contrôle accordéon (dans ses propriétés `HeaderCssClass` et `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="0877d-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="0877d-131">Placez le balisage suivant dans la section `<head>` de la page :</span><span class="sxs-lookup"><span data-stu-id="0877d-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]

<span data-ttu-id="0877d-132">[![les données de l’accordéon proviennent directement de la source de données](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0877d-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="0877d-133">Les données de l’accordéon proviennent directement de la source de données ([cliquez pour afficher l’image en taille réelle](databinding-to-an-accordion-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0877d-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0877d-134">[Précédent](dynamically-adding-an-accordion-pane-cs.md)
> [Suivant](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0877d-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
