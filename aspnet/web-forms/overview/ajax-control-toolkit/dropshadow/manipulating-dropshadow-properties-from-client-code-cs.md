---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulation des propriétés DropShadow à partir du codeC#client () | Microsoft Docs
author: wenz
description: Personnalisation de l’interface de modification du contrôle DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574086"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulation des propriétés de DropShadow à partir de code client (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.

## <a name="steps"></a>Étapes

Le code commence par un panneau contenant des lignes de texte :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

La classe CSS associée donne au panneau une couleur d’arrière-plan intéressante :

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

Le `DropShadowExtender` est ajouté pour étendre le panneau avec un effet d’ombre portée, l’opacité est définie sur 50% :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Ensuite, le contrôle de `ScriptManager` AJAX ASP.NET permet à Control Toolkit de fonctionner :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Un autre panneau contient deux liens JavaScript pour définir l’opacité de l’ombre portée : le lien moins diminue l’opacité de l’ombre, le lien plus l’augmente.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

La fonction JavaScript `changeOpacity()` doit alors rechercher d’abord le contrôle `DropShadowExtender` sur la page. ASP.NET AJAX définit la méthode `$find()` pour cette tâche. Ensuite, la méthode `get_Opacity()` récupère l’opacité actuelle, la méthode `set_Opacity()` la définit. Le code JavaScript place ensuite la valeur d’opacité actuelle dans l’élément `<label>` :

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

[![l’opacité est modifiée côté client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

L’opacité est modifiée côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Suivant](adjusting-the-z-index-of-a-dropshadow-vb.md)
