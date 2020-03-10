---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Prédéfinition des entrées de liste avec CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597929"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Prédéfinition des entrées de liste avec CascadingDropDown (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. Avec un peu de code, il est possible qu’un élément de liste soit présélectionné une fois que les données ont été chargées dynamiquement.

## <a name="overview"></a>Présentation

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) Avec un peu de code, il est possible qu’un élément de liste soit présélectionné une fois que les données ont été chargées dynamiquement.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Un contrôle DropDownList est ensuite requis :

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté, fournissant des informations sur l’URL et la méthode du service Web :

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

L’extendeur CascadingDropDown appelle ensuite de manière asynchrone un service Web avec la signature de méthode suivante :

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

La méthode retourne un tableau de type CascadingDropDown. Le constructeur du type attend la légende de la première entrée de liste, puis la valeur (attribut de `value` HTML). Si le troisième argument est défini sur true, l’élément de liste est automatiquement sélectionné dans le navigateur.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Le chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, la seconde étant présélectionnée.

[![la liste est remplie et présélectionnée automatiquement](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

La liste est remplie et présélectionnée automatiquement ([cliquez pour afficher l’image en taille réelle](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-cascadingdropdown-with-a-database-vb.md)
> [Suivant](using-auto-postback-with-cascadingdropdown-vb.md)
