---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Utilisation de la publication automatique avec CascadingDropDownC#() | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574492"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a>Utilisation de la publication (postback) automatique avec CascadingDropDown (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. Toutefois, lors de l’utilisation du contrôle CascadingDropDown, ASP. La fonctionnalité AutoPostBack du contrôle DropDownList de NET ne fonctionne pas, car le chargement asynchrone des données dans la liste génère une publication (postback) (inutile) proprement dite. Avec du code JavaScript, cet effet peut être évité.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) Toutefois, lors de l’utilisation du contrôle CascadingDropDown, ASP. La fonctionnalité AutoPostBack du contrôle DropDownList de NET ne fonctionne pas, car le chargement asynchrone des données dans la liste génère une publication (postback) (inutile) proprement dite. Avec du code JavaScript, cet effet peut être évité.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans le &lt;`form`élément &gt;) :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Un contrôle DropDownList est ensuite requis :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté, fournissant des informations sur l’URL et la méthode du service Web :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

L’extendeur CascadingDropDown appelle ensuite de manière asynchrone un service Web avec la signature de méthode suivante :

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

La méthode retourne un tableau de type CascadingDropDown. Le constructeur du type attend la légende de la première entrée de liste, puis la valeur (attribut de `value` HTML).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Le chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, la seconde étant présélectionnée. En outre, ASP.NET définit la méthode JavaScript `__doPostBack()`. Une fois la page chargée, cet appel JavaScript est ajouté à la liste déroulante, mais uniquement s’il existe des éléments. S’il n’y a aucun élément dans la liste, la boîte à outils de contrôle les charge actuellement, le code JavaScript utilise donc un délai d’attente et tente à nouveau de s’exécuter en une demi-seconde.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

De cette façon, une publication (postback) est exécutée uniquement lorsqu’il y a réellement des éléments dans la liste et que l’utilisateur sélectionne une entrée.

[![la sélection d’un élément de liste provoque une publication (postback)](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

La sélection d’un élément de liste entraîne une publication ([cliquez pour afficher l’image en taille réelle](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Suivant](filling-a-list-using-cascadingdropdown-vb.md)
