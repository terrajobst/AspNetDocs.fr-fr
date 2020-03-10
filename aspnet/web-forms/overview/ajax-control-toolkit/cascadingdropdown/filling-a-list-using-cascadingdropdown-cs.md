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
# <a name="filling-a-list-using-cascadingdropdown-c"></a>Remplissage d’une liste avec CascadingDropDown (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.

## <a name="overview"></a>Présentation

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) La première difficulté à résoudre consiste à remplir une liste déroulante à l’aide de ce contrôle.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Un contrôle DropDownList est ensuite requis :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté. Il envoie une requête asynchrone à un service Web qui renverra ensuite une liste d’entrées à afficher dans la liste. Pour que cela fonctionne, les attributs CascadingDropDown suivants doivent être définis :

- `ServicePath`: URL d’un service Web qui fournit les entrées de liste
- `ServiceMethod`: méthode Web qui fournit les entrées de liste
- `TargetControlID`: ID de la liste déroulante
- `Category`: informations de catégorie soumises à la méthode Web quand elles sont appelées
- `PromptText`: texte affiché lors du chargement asynchrone de données de liste à partir du serveur

Voici le balisage de l’élément `CascadingDropDown`. La seule différence entre C# et VB est le nom du service Web associé :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Le code JavaScript provenant de l’extendeur `CascadingDropDown` appelle une méthode de service Web avec la signature suivante :

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

L’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (défini par ASP.NET AJAX Control Toolkit). Dans le constructeur `CascadingDropDownNameValue`, le texte de l’entrée de liste est d’abord fourni, puis sa valeur doit être fournie, tout comme `<option value="VALUE">NAME</option>` le ferait en HTML. Voici quelques exemples de données :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Le chargement de la page dans le navigateur déclenche l’remplissage de la liste avec trois fournisseurs.

[![la liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

La liste est renseignée automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-cascadingdropdown-with-a-database-cs.md)
