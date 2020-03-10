---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Utilisation de publications (postback)C#avec ReorderList () | Microsoft Docs
author: wenz
description: Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. Chaque fois que la liste est réorganisée, un bon de commande...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627231"
---
# <a name="using-postbacks-with-reorderlist-c"></a>Utilisation de publications (postback) avec ReorderList (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Le contrôle ReorderList dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. Chaque fois que la liste est réorganisée, une publication (postback) informe le serveur de la modification.

## <a name="overview"></a>Présentation

Le contrôle `ReorderList` dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur par glisser-déplacer. Chaque fois que la liste est réorganisée, une publication (postback) informe le serveur de la modification.

## <a name="steps"></a>Étapes

Il existe plusieurs sources de données possibles pour le contrôle `ReorderList`. La première consiste à utiliser un contrôle de `XmlDataSource` :

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Pour lier ce code XML à un contrôle de `ReorderList` et activer les publications, les attributs suivants doivent être définis :

- `DataSourceID`: ID de la source de données
- `SortOrderField`: la propriété sur laquelle effectuer le tri
- `AllowReorder`: indique s’il faut autoriser l’utilisateur à réorganiser les éléments de liste
- `PostBackOnReorder`: indique s’il faut créer une publication (postback) à chaque réorganisation de la liste

Voici le balisage approprié pour le contrôle :

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

Dans le contrôle `ReorderList`, les données spécifiques de la source de données peuvent être liées à l’aide de la méthode `Eval()` :

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

À une position arbitraire sur la page, une étiquette contiendra les informations lorsque la dernière réorganisation s’est produite :

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Cette étiquette est remplie avec du texte dans le code côté serveur, ce qui gère la publication (postback) :

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Enfin, pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé sur la page :

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

[![chaque réorganisation déclenche une publication (postback)](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Chaque réorganisation déclenche une publication ([cliquez pour afficher l’image en taille réelle](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](drag-and-drop-via-reorderlist-cs.md)
