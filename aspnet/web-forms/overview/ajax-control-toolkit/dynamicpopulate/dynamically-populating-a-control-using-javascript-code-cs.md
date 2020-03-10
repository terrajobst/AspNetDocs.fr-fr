---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Remplissage dynamique d’un contrôle à l’aide deC#code JavaScript () | Microsoft Docs
author: wenz
description: Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535741"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a>Remplissage dynamique d’un contrôle avec du code JavaScript (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page. Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé.

## <a name="overview"></a>Présentation

Le contrôle `DynamicPopulate` de la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page. Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin d’un service Web ASP.NET qui implémente la méthode à appeler par le contrôle `DynamicPopulateExtender`. Le service Web implémente la méthode `getDate()` qui attend un argument de type String, appelée `contextKey`, puisque le contrôle `DynamicPopulate` envoie une partie des informations de contexte avec chaque appel de service Web. Voici le code (`DynamicPopulate.cs.asmx`de fichier) qui récupère la date actuelle dans l’un des trois formats suivants :

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

À l’étape suivante, créez un nouveau site ASP.NET et commencez avec le contrôle ASP.NET AJAX ScriptManager :

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Ensuite, ajoutez un contrôle Label (par exemple à l’aide du contrôle HTML du même nom ou du contrôle Web `<asp:Label />`) qui affichera par la suite le résultat de l’appel du service Web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Ensuite, incluez un contrôle de `DynamicPopulateExtender` et fournissez des informations sur le service Web, le contrôle cible, mais pas le nom du contrôle qui déclenche le remplissage que cette opération sera effectuée ultérieurement, à l’aide de JavaScript personnalisé !

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Maintenant, à la partie JavaScript. La fonction `$find()`, définie par la bibliothèque AJAX ASP.NET, retourne une référence aux objets côté serveur de la boîte à outils de contrôle ASP.NET AJAX, comme `DynamicPopulateExtender`. Dans le fichier actif, `$find("dpe")` retourne une référence à l’un des `DynamicPopulateExtender` contrôle dans la page. Il expose une méthode appelée `populate()` qui déclenche le processus de remplissage dynamique. La méthode `populate()` nécessite un argument : la clé de contexte qui servira d’argument à la méthode Web `getDate()`. Par exemple, `$find("dpe").populate("format1")` remplit l’étiquette avec la date actuelle au format mois-jour-année.

Pour que l’exemple soit un peu plus flexible, l’utilisateur peut désormais choisir entre plusieurs formats de date. Pour chacun d’eux, une case d’option est affichée. Une fois que l’utilisateur clique sur une case d’option, le code JavaScript remplit dynamiquement l’étiquette avec le format de date sélectionné. Les cases d’option sont les suivantes :

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Notez que dans le contexte d’une case d’option, l’expression JavaScript `this.value` fait référence à la valeur du bouton actuel, qui se présente exactement les mêmes informations que celles que la méthode `getDate()` peut utiliser.

[![un clic sur le bouton récupère la date du serveur, au format spécifié](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Un clic sur le bouton permet de récupérer la date du serveur, au format spécifié ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](dynamically-populating-a-control-cs.md)
> [Suivant](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
