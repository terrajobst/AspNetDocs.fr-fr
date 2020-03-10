---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulation des propriétés DropShadow à partir du code client (VB) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Les propriétés de cet extendeur peuvent également être modifiées à l’aide du client JavaScrip...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613798"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipulation des propriétés de DropShadow à partir de code client (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.

## <a name="overview"></a>Présentation

Le contrôle DropShadow dans la boîte à outils de contrôle AJAX étend un panneau avec une ombre portée. Les propriétés de cet extendeur peuvent également être modifiées à l’aide du code JavaScript du client.

## <a name="steps"></a>Étapes

Le code commence par un panneau contenant des lignes de texte :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

La classe CSS associée donne au panneau une couleur d’arrière-plan intéressante :

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

Le `DropShadowExtender` est ajouté pour étendre le panneau avec un effet d’ombre portée, l’opacité est définie sur 50% :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Ensuite, le contrôle de `ScriptManager` AJAX ASP.NET permet à Control Toolkit de fonctionner :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Un autre panneau contient deux liens JavaScript pour définir l’opacité de l’ombre portée : le lien moins diminue l’opacité de l’ombre, le lien plus l’augmente.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

La fonction JavaScript `changeOpacity()` doit alors rechercher d’abord le contrôle `DropShadowExtender` sur la page. ASP.NET AJAX définit la méthode `$find()` pour cette tâche. Ensuite, la méthode `get_Opacity()` récupère l’opacité actuelle, la méthode `set_Opacity()` la définit. Le code JavaScript place ensuite la valeur d’opacité actuelle dans l’élément `<label>` :

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

[![l’opacité est modifiée côté client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

L’opacité est modifiée côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](adjusting-the-z-index-of-a-dropshadow-vb.md)
