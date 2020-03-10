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
# <a name="filling-a-list-using-cascadingdropdown-vb"></a>Remplissage d’une liste avec CascadingDropDown (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.

## <a name="overview"></a>Présentation

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Un contrôle DropDownList est ensuite requis :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté. Il envoie une requête asynchrone à un service Web qui renverra ensuite une liste d’entrées à afficher dans la liste. Pour que cela fonctionne, les attributs CascadingDropDown suivants doivent être définis :

- `ServicePath`: URL d’un service Web qui fournit les entrées de liste
- `ServiceMethod`: méthode Web qui fournit les entrées de liste
- `TargetControlID`: ID de la liste déroulante
- `Category`: informations de catégorie soumises à la méthode Web quand elles sont appelées
- `PromptText`: texte affiché lors du chargement asynchrone de données de liste à partir du serveur

Voici le balisage de l’élément `CascadingDropDown`. La seule différence entre C# et VB est le nom du service Web associé :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Le code JavaScript provenant de l’extendeur `CascadingDropDown` appelle une méthode de service Web avec la signature suivante :

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

L’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (défini par ASP.NET AJAX Control Toolkit). Dans le constructeur `CascadingDropDownNameValue`, le texte de l’entrée de liste est d’abord fourni, puis sa valeur doit être fournie, tout comme `<option value="VALUE">NAME</option>` le ferait en HTML. Voici quelques exemples de données :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Le chargement de la page dans le navigateur déclenche l’remplissage de la liste avec trois fournisseurs.

[![la liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

La liste est renseignée automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-auto-postback-with-cascadingdropdown-cs.md)
> [Suivant](using-cascadingdropdown-with-a-database-vb.md)
