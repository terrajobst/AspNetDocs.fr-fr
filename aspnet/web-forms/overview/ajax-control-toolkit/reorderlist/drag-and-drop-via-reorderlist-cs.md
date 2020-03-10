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
# <a name="drag-and-drop-via-reorderlist-c"></a>Glisser-déplacer via ReorderList (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. L’ordre actuel de la liste doit être conservé sur le serveur.

## <a name="overview"></a>Présentation

Le contrôle `ReorderList` dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. L’ordre actuel de la liste doit être conservé sur le serveur.

## <a name="steps"></a>Étapes

Le contrôle `ReorderList` prend en charge la liaison de données d’une base de données à la liste. Mieux encore, il prend également en charge l’écriture des modifications apportées à l’ordre de l’élément de liste dans le magasin de données.

Cet exemple utilise Microsoft SQL Server édition Express 2005 comme magasin de données. La base de données est une partie facultative (et gratuite) d’une installation Visual Studio, y compris l’édition Express. Il est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut. Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.

Le moyen le plus simple de configurer la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) en). Connectez-vous au serveur, double-cliquez sur `Databases` et créez une nouvelle base de données (cliquez avec le bouton droit et choisissez `New Database`) appelée `Tutorials`.

Dans cette base de données, créez une nouvelle table nommée `AJAX` avec les quatre colonnes suivantes :

- `id` (clé primaire, entier, identité, et non NULL)
- `char` (Char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)

[![la disposition de la table AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Disposition de la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image3.png))

Ensuite, remplissez la table avec deux valeurs. Notez que la colonne `position` contient l’ordre de tri des éléments.

[![les données initiales dans la table AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

Données initiales dans la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image6.png))

L’étape suivante nécessite la génération d’un contrôle de `SqlDataSource` pour communiquer avec la nouvelle base de données et sa table. La source de données doit prendre en charge les commandes SQL `SELECT` et `UPDATE`. Lorsque l’ordre des éléments de liste est modifié par la suite, le contrôle `ReorderList` envoie automatiquement deux valeurs à la commande `Update` de la source de données : la nouvelle position et l’ID de l’élément. Par conséquent, la source de données a besoin d’une section `<UpdateParameters>` pour ces deux valeurs :

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

Le contrôle `ReorderList` doit définir les attributs suivants :

- `AllowReorder`: indique si les éléments de la liste peuvent être réorganisés
- `DataSourceID`: ID de la source de données
- `DataKeyField`: nom de la colonne de clé primaire dans la source de données
- `SortOrderField`: colonne de source de données qui fournit l’ordre de tri des éléments de liste

Dans les sections `<DragHandleTemplate>` et `<ItemTemplate>`, la disposition de la liste peut être ajustée. En outre, la liaison de liaison est possible à l’aide de la méthode `Eval()`, comme illustré ici :

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Les informations de style CSS suivantes (référencées dans la section `<DragHandleTemplate>` du contrôle `ReorderList`) permettent de s’assurer que le pointeur de la souris change de manière appropriée lorsqu’il pointe sur la poignée de glissement :

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Enfin, un contrôle de `ScriptManager` Initialise ASP.NET AJAX pour la page :

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Exécutez cet exemple dans le navigateur et réorganisez les éléments de liste un peu. Ensuite, rechargez la page et/ou consultez la base de données. Les positions modifiées ont été conservées et sont également reflétées par les valeurs dans la colonne `position` de la base de données et celles qui ne sont pas sans code, simplement à l’aide du balisage.

[![les données de la base de données changent en fonction du nouvel ordre des éléments de liste](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Les données de la base de données sont modifiées en fonction du nouvel ordre des éléments de liste ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image9.png))

> [!div class="step-by-step"]
> [Précédent](using-postbacks-with-reorderlist-cs.md)
> [Suivant](using-postbacks-with-reorderlist-vb.md)
