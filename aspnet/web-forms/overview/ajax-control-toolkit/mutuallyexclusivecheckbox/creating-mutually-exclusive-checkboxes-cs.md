---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Création de cases à cocher mutuellement exclusives (C#) | Microsoft Docs
author: wenz
description: 'Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées. Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613070"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a>Création de cases à cocher mutuellement exclusives (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées. Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe, il n’est pas possible de décocher toutes les cases d’option. Les cases à cocher peuvent être désactivées à tout moment, mais ne sont pas mutuellement exclusives. Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.

## <a name="overview"></a>Présentation

Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées. Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe, il n’est pas possible de décocher toutes les cases d’option. Les cases à cocher peuvent être désactivées à tout moment, mais ne sont pas mutuellement exclusives. Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient l’extendeur MutuallyExclusiveCheckBox. Cela permet aux programmeurs d’attribuer n’importe quelle case à un nom de groupe (attribut`Key`). À partir de toutes les cases au sein du même groupe, une seule peut être sélectionnée à la fois.

Commençons par placer deux cases à cocher sur une nouvelle page ASP.NET. Il peut y en avoir davantage, mais deux d’entre eux suffisent à démontrer le principe :

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Pour les deux cases à cocher, un contrôle MutuallyExclusiveCheckBoxExtender doit être placé sur la page. Les deux attributs de clé doivent avoir la même valeur, tout comme les attributs de valeur des éléments de case d’option HTML doivent être identiques pour désigner le groupe auquel ils appartiennent. La propriété TargetControlID de l’extendeur pointe sur l’ID de la case à cocher.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Enfin, incluez le `ScriptManager` AJAX ASP.NET qui est requis par tous les éléments de ASP.NET AJAX Control Toolkit :

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Enregistrer et exécuter la page : vous pouvez activer ou désactiver les deux cases à cocher. Toutefois, à aucun moment, les deux cases à cocher peuvent être activées.

[![une seule case à cocher peut être activée à la fois](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Une seule case à cocher peut être activée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-mutually-exclusive-checkboxes-vb.md)
