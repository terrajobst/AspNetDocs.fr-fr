---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Utilisation de la publication automatique avec CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5dea23a20aba00af5109f05f18365b89e409a131
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535923"
---
# <a name="using-auto-postback-with-cascadingdropdown-vb"></a>Utilisation de la publication (postback) automatique avec CascadingDropDown (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. Toutefois, lors de l’utilisation du contrôle CascadingDropDown, ASP. La fonctionnalité AutoPostBack du contrôle DropDownList de NET ne fonctionne pas, car le chargement asynchrone des données dans la liste génère une publication (postback) (inutile) proprement dite. Avec du code JavaScript, cet effet peut être évité.

## <a name="overview"></a>Présentation

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) Toutefois, lors de l’utilisation du contrôle CascadingDropDown, ASP. La fonctionnalité AutoPostBack du contrôle DropDownList de NET ne fonctionne pas, car le chargement asynchrone des données dans la liste génère une publication (postback) (inutile) proprement dite. Avec du code JavaScript, cet effet peut être évité.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans le &lt;`form`élément &gt;) :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Un contrôle DropDownList est ensuite requis :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté, fournissant des informations sur l’URL et la méthode du service Web :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

L’extendeur CascadingDropDown appelle ensuite de manière asynchrone un service Web avec la signature de méthode suivante :

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

La méthode retourne un tableau de type CascadingDropDown. Le constructeur du type attend la légende de la première entrée de liste, puis la valeur (attribut de `value` HTML).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Le chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, la seconde étant présélectionnée. En outre, ASP.NET définit la méthode JavaScript `__doPostBack()`. Une fois la page chargée, cet appel JavaScript est ajouté à la liste déroulante, mais uniquement s’il existe des éléments. S’il n’y a aucun élément dans la liste, la boîte à outils de contrôle les charge actuellement, le code JavaScript utilise donc un délai d’attente et tente à nouveau de s’exécuter en une demi-seconde.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

De cette façon, une publication (postback) est exécutée uniquement lorsqu’il y a réellement des éléments dans la liste et que l’utilisateur sélectionne une entrée.

[![la sélection d’un élément de liste provoque une publication (postback)](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

La sélection d’un élément de liste entraîne une publication ([cliquez pour afficher l’image en taille réelle](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](presetting-list-entries-with-cascadingdropdown-vb.md)
