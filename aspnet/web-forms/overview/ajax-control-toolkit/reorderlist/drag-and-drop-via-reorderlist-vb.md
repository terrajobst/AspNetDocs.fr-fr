---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Glisser-déplacer via ReorderList (VB) | Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f7c5749053d8bf587467fb1939fca05ce2872a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553920"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="ee2fe-103">Glisser-déplacer via ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="ee2fe-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ee2fe-105">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="ee2fe-106">Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="ee2fe-107">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-107">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="ee2fe-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="ee2fe-108">Overview</span></span>

<span data-ttu-id="ee2fe-109">Le contrôle `ReorderList` dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="ee2fe-110">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="ee2fe-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="ee2fe-111">Steps</span></span>

<span data-ttu-id="ee2fe-112">Le contrôle `ReorderList` prend en charge la liaison de données d’une base de données à la liste.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="ee2fe-113">Mieux encore, il prend également en charge l’écriture des modifications apportées à l’ordre de l’élément de liste dans le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="ee2fe-114">Cet exemple utilise Microsoft SQL Server édition Express 2005 comme magasin de données.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="ee2fe-115">La base de données est une partie facultative (et gratuite) d’une installation Visual Studio, y compris l’édition Express.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="ee2fe-116">Il est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="ee2fe-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="ee2fe-117">Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="ee2fe-118">Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="ee2fe-119">Le moyen le plus simple de configurer la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) en).</span><span class="sxs-lookup"><span data-stu-id="ee2fe-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="ee2fe-120">Connectez-vous au serveur, double-cliquez sur `Databases` et créez une nouvelle base de données (cliquez avec le bouton droit et choisissez `New Database`) appelée `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="ee2fe-121">Dans cette base de données, créez une nouvelle table nommée `AJAX` avec les quatre colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ee2fe-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="ee2fe-122">`id` (clé primaire, entier, identité, et non NULL)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="ee2fe-123">`char` (Char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="ee2fe-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="ee2fe-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-125">`position` (int, NULL)</span></span>

<span data-ttu-id="ee2fe-126">[![la disposition de la table AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="ee2fe-127">Disposition de la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ee2fe-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>

<span data-ttu-id="ee2fe-128">Ensuite, remplissez la table avec deux valeurs.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="ee2fe-129">Notez que la colonne `position` contient l’ordre de tri des éléments.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-129">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="ee2fe-130">[![les données initiales dans la table AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="ee2fe-131">Données initiales dans la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ee2fe-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>

<span data-ttu-id="ee2fe-132">L’étape suivante nécessite la génération d’un contrôle de `SqlDataSource` pour communiquer avec la nouvelle base de données et sa table.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="ee2fe-133">La source de données doit prendre en charge les commandes SQL `SELECT` et `UPDATE`.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="ee2fe-134">Lorsque l’ordre des éléments de liste est modifié par la suite, le contrôle `ReorderList` envoie automatiquement deux valeurs à la commande `Update` de la source de données : la nouvelle position et l’ID de l’élément.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="ee2fe-135">Par conséquent, la source de données a besoin d’une section `<UpdateParameters>` pour ces deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="ee2fe-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="ee2fe-136">Le contrôle `ReorderList` doit définir les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="ee2fe-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="ee2fe-137">`AllowReorder`: indique si les éléments de la liste peuvent être réorganisés</span><span class="sxs-lookup"><span data-stu-id="ee2fe-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="ee2fe-138">`DataSourceID`: ID de la source de données</span><span class="sxs-lookup"><span data-stu-id="ee2fe-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="ee2fe-139">`DataKeyField`: nom de la colonne de clé primaire dans la source de données</span><span class="sxs-lookup"><span data-stu-id="ee2fe-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="ee2fe-140">`SortOrderField`: colonne de source de données qui fournit l’ordre de tri des éléments de liste</span><span class="sxs-lookup"><span data-stu-id="ee2fe-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="ee2fe-141">Dans les sections `<DragHandleTemplate>` et `<ItemTemplate>`, la disposition de la liste peut être ajustée.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="ee2fe-142">En outre, la liaison de liaison est possible à l’aide de la méthode `Eval()`, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="ee2fe-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="ee2fe-143">Les informations de style CSS suivantes (référencées dans la section `<DragHandleTemplate>` du contrôle `ReorderList`) permettent de s’assurer que le pointeur de la souris change de manière appropriée lorsqu’il pointe sur la poignée de glissement :</span><span class="sxs-lookup"><span data-stu-id="ee2fe-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="ee2fe-144">Enfin, un contrôle de `ScriptManager` Initialise ASP.NET AJAX pour la page :</span><span class="sxs-lookup"><span data-stu-id="ee2fe-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="ee2fe-145">Exécutez cet exemple dans le navigateur et réorganisez les éléments de liste un peu.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="ee2fe-146">Ensuite, rechargez la page et/ou consultez la base de données.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="ee2fe-147">Les positions modifiées ont été conservées et sont également reflétées par les valeurs dans la colonne `position` de la base de données et celles qui ne sont pas sans code, simplement à l’aide du balisage.</span><span class="sxs-lookup"><span data-stu-id="ee2fe-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="ee2fe-148">[![les données de la base de données changent en fonction du nouvel ordre des éléments de liste](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ee2fe-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="ee2fe-149">Les données de la base de données sont modifiées en fonction du nouvel ordre des éléments de liste ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="ee2fe-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ee2fe-150">Précédent</span><span class="sxs-lookup"><span data-stu-id="ee2fe-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
