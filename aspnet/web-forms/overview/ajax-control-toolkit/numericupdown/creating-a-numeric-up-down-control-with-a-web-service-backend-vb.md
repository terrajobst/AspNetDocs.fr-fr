---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Création d’un contrôle Numeric up/UpDown avec un backend de service Web (VB) | Microsoft Docs
author: wenz
description: Au lieu de laisser un utilisateur taper une valeur dans une case à cocher, un contrôle numérique up/UpDown (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bf6e1b27180589d39e308de62b5be1f47fa8fe2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612993"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Création d’un contrôle Haut/bas numérique avec un backend de service web (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Au lieu de laisser un utilisateur taper une valeur dans une case à cocher, un contrôle numérique haut/PG. (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus confortable. Par défaut, le contrôle NumericUpDown augmente ou diminue toujours une valeur de 1, mais un service Web prouve plus de flexibilité.

## <a name="overview"></a>Présentation

Au lieu de laisser un utilisateur taper une valeur dans une case à cocher, un contrôle numérique haut/PG. (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus confortable. Par défaut, le contrôle de `NumericUpDown` augmente ou diminue toujours une valeur de 1, mais un service Web prouve plus de flexibilité.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient l’extendeur `NumericUpDown` qui ajoute automatiquement deux boutons à une zone de texte : un pour l’augmentation de sa valeur, un pour le réduire. Toutefois, le contrôle prend également en charge un appel de service Web (ou un appel de méthode de page). Chaque fois que l’utilisateur clique sur le bouton haut ou haut, le code JavaScript se connecte au serveur Web et y exécute une méthode. La signature de la méthode est la suivante :

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

L’argument `current` est la valeur actuelle dans la zone de texte ; l’attribut `tag` est des données de contexte supplémentaires qui peuvent être définies en tant que propriété de l’extendeur `NumericUpDown` (mais n’est pas obligatoire).

Pour cet exemple, le contrôle Numeric up/UpDown doit autoriser uniquement les valeurs qui sont des puissances de deux : 1, 2, 4, 8, 16, 32, 64, etc. Par conséquent, la méthode exécutée lorsque l’utilisateur souhaite augmenter la valeur doit doubler l’ancienne valeur ; l’autre méthode doit diviser value par deux. Voici le service Web complet :

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Enfin, créez une nouvelle page ASP.NET. Comme d’habitude, vous avez besoin d’un contrôle `ScriptManager`, d’un contrôle `TextBox` et d’un contrôle `NumericUpDownExtender`. Pour ce dernier, vous devez fournir les informations sur le service Web :

- `ServiceDownMethod` le nom de la méthode Web ou de la méthode de page en aval
- `ServiceDownPath` chemin d’accès au service Web avec la méthode de service inactive ; omettre si vous utilisez une méthode de page
- `ServiceUpMethod` le nom de la méthode Web ou de la méthode de page précédente
- `ServiceUpPath` chemin d’accès au service Web avec la méthode de service up ; omettre si vous utilisez une méthode de page

Voici le balisage complet de la page :

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Si vous exécutez la page, vous remarquez que la valeur de la zone de texte se double toujours lorsque vous cliquez sur le bouton supérieur, et est divisée en deux lorsque vous cliquez sur le bouton du bas.

[![uniquement les nombres qui sont une puissance de 2 apparaissent](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Seuls les nombres qui sont une puissance de 2 s’affichent ([cliquez pour afficher l’image en taille réelle](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
