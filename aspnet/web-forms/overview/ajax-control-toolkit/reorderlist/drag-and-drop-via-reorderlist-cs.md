---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Glisser-déplacer via ReorderList (C#) | Microsoft Docs
author: wenz
description: Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. L’ordre actuel de la liste doit être...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 2fc6d55a290cbb58bea36d8145d814e337bbd931
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553829"
---
# <a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="fe8de-104">Glisser-déplacer via ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="fe8de-104">Drag and Drop via ReorderList (C#)</span></span>

<span data-ttu-id="fe8de-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fe8de-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fe8de-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fe8de-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="fe8de-107">Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer.</span><span class="sxs-lookup"><span data-stu-id="fe8de-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="fe8de-108">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="fe8de-108">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="fe8de-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="fe8de-109">Overview</span></span>

<span data-ttu-id="fe8de-110">Le contrôle `ReorderList` dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer.</span><span class="sxs-lookup"><span data-stu-id="fe8de-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="fe8de-111">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="fe8de-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="fe8de-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="fe8de-112">Steps</span></span>

<span data-ttu-id="fe8de-113">Le contrôle `ReorderList` prend en charge la liaison de données d’une base de données à la liste.</span><span class="sxs-lookup"><span data-stu-id="fe8de-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="fe8de-114">Mieux encore, il prend également en charge l’écriture des modifications apportées à l’ordre de l’élément de liste dans le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="fe8de-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="fe8de-115">Cet exemple utilise Microsoft SQL Server édition Express 2005 comme magasin de données.</span><span class="sxs-lookup"><span data-stu-id="fe8de-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="fe8de-116">La base de données est une partie facultative (et gratuite) d’une installation Visual Studio, y compris l’édition Express.</span><span class="sxs-lookup"><span data-stu-id="fe8de-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="fe8de-117">Il est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="fe8de-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="fe8de-118">Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut.</span><span class="sxs-lookup"><span data-stu-id="fe8de-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="fe8de-119">Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.</span><span class="sxs-lookup"><span data-stu-id="fe8de-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="fe8de-120">Le moyen le plus simple de configurer la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) en).</span><span class="sxs-lookup"><span data-stu-id="fe8de-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="fe8de-121">Connectez-vous au serveur, double-cliquez sur `Databases` et créez une nouvelle base de données (cliquez avec le bouton droit et choisissez `New Database`) appelée `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="fe8de-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="fe8de-122">Dans cette base de données, créez une nouvelle table nommée `AJAX` avec les quatre colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="fe8de-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="fe8de-123">`id` (clé primaire, entier, identité, et non NULL)</span><span class="sxs-lookup"><span data-stu-id="fe8de-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="fe8de-124">`char` (Char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="fe8de-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="fe8de-125">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="fe8de-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="fe8de-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="fe8de-126">`position` (int, NULL)</span></span>

<span data-ttu-id="fe8de-127">[![la disposition de la table AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fe8de-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="fe8de-128">Disposition de la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fe8de-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>

<span data-ttu-id="fe8de-129">Ensuite, remplissez la table avec deux valeurs.</span><span class="sxs-lookup"><span data-stu-id="fe8de-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="fe8de-130">Notez que la colonne `position` contient l’ordre de tri des éléments.</span><span class="sxs-lookup"><span data-stu-id="fe8de-130">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="fe8de-131">[![les données initiales dans la table AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fe8de-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="fe8de-132">Données initiales dans la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fe8de-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>

<span data-ttu-id="fe8de-133">L’étape suivante nécessite la génération d’un contrôle de `SqlDataSource` pour communiquer avec la nouvelle base de données et sa table.</span><span class="sxs-lookup"><span data-stu-id="fe8de-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="fe8de-134">La source de données doit prendre en charge les commandes SQL `SELECT` et `UPDATE`.</span><span class="sxs-lookup"><span data-stu-id="fe8de-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="fe8de-135">Lorsque l’ordre des éléments de liste est modifié par la suite, le contrôle `ReorderList` envoie automatiquement deux valeurs à la commande `Update` de la source de données : la nouvelle position et l’ID de l’élément.</span><span class="sxs-lookup"><span data-stu-id="fe8de-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="fe8de-136">Par conséquent, la source de données a besoin d’une section `<UpdateParameters>` pour ces deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="fe8de-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="fe8de-137">Le contrôle `ReorderList` doit définir les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="fe8de-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="fe8de-138">`AllowReorder`: indique si les éléments de la liste peuvent être réorganisés</span><span class="sxs-lookup"><span data-stu-id="fe8de-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="fe8de-139">`DataSourceID`: ID de la source de données</span><span class="sxs-lookup"><span data-stu-id="fe8de-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="fe8de-140">`DataKeyField`: nom de la colonne de clé primaire dans la source de données</span><span class="sxs-lookup"><span data-stu-id="fe8de-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="fe8de-141">`SortOrderField`: colonne de source de données qui fournit l’ordre de tri des éléments de liste</span><span class="sxs-lookup"><span data-stu-id="fe8de-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="fe8de-142">Dans les sections `<DragHandleTemplate>` et `<ItemTemplate>`, la disposition de la liste peut être ajustée.</span><span class="sxs-lookup"><span data-stu-id="fe8de-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="fe8de-143">En outre, la liaison de liaison est possible à l’aide de la méthode `Eval()`, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="fe8de-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="fe8de-144">Les informations de style CSS suivantes (référencées dans la section `<DragHandleTemplate>` du contrôle `ReorderList`) permettent de s’assurer que le pointeur de la souris change de manière appropriée lorsqu’il pointe sur la poignée de glissement :</span><span class="sxs-lookup"><span data-stu-id="fe8de-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="fe8de-145">Enfin, un contrôle de `ScriptManager` Initialise ASP.NET AJAX pour la page :</span><span class="sxs-lookup"><span data-stu-id="fe8de-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="fe8de-146">Exécutez cet exemple dans le navigateur et réorganisez les éléments de liste un peu.</span><span class="sxs-lookup"><span data-stu-id="fe8de-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="fe8de-147">Ensuite, rechargez la page et/ou consultez la base de données.</span><span class="sxs-lookup"><span data-stu-id="fe8de-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="fe8de-148">Les positions modifiées ont été conservées et sont également reflétées par les valeurs dans la colonne `position` de la base de données et celles qui ne sont pas sans code, simplement à l’aide du balisage.</span><span class="sxs-lookup"><span data-stu-id="fe8de-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="fe8de-149">[![les données de la base de données changent en fonction du nouvel ordre des éléments de liste](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fe8de-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="fe8de-150">Les données de la base de données sont modifiées en fonction du nouvel ordre des éléments de liste ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="fe8de-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fe8de-151">[Précédent](using-postbacks-with-reorderlist-cs.md)
> [Suivant](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fe8de-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
