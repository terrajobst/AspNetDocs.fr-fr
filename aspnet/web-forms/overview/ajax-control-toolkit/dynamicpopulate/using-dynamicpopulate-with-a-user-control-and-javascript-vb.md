---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Utilisation de DynamicPopulate avec un contrôle utilisateur et JavaScript (VB) | Microsoft Docs
author: wenz
description: Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: ee5923ad6d8b101f689a0564aef8b1e0e00a7639
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613672"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Utilisation de DynamicPopulate avec un contrôle utilisateur et JavaScript (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page. Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé. Toutefois, une attention particulière doit être prise lorsque l’extendeur réside dans un contrôle utilisateur.

## <a name="overview"></a>Présentation

Le contrôle `DynamicPopulate` de la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page. Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé. Toutefois, une attention particulière doit être prise lorsque l’extendeur réside dans un contrôle utilisateur.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin d’un service Web ASP.NET qui implémente la méthode à appeler par le contrôle `DynamicPopulateExtender`. Le service Web implémente la méthode `getDate()` qui attend un argument de type String, appelée `contextKey`, puisque le contrôle `DynamicPopulate` envoie une partie des informations de contexte avec chaque appel de service Web. Voici le code (fichiers `DynamicPopulate.vb.asmx`) qui récupère la date actuelle dans l’un des trois formats suivants :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

À l’étape suivante, créez un nouveau contrôle utilisateur (fichier`.ascx`), indiqué par la déclaration suivante sur sa première ligne :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Un &lt;`label`élément &gt; sera utilisé pour afficher les données provenant du serveur.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Dans le fichier de contrôle utilisateur, nous allons également utiliser trois cases d’option, chacune représentant l’un des trois formats de date possibles pris en charge par le service Web. Quand l’utilisateur clique sur l’une des cases d’option, le navigateur exécute du code JavaScript qui ressemble à ceci :

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Ce code permet d’accéder à la `DynamicPopulateExtender` (ne vous inquiétez pas encore de l’ID étrange, cette opération sera traitée plus tard) et déclenchera le remplissage dynamique avec les données. Dans le contexte de la case d’option active, `this.value` fait référence à sa valeur qui est `format1`, `format2` ou `format3` exactement ce que la méthode Web attend.

La seule chose manquante dans le contrôle utilisateur est le contrôle `DynamicPopulateExtender` qui lie les cases d’option au service Web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Là encore, vous pouvez noter l’ID étrange utilisé dans le contrôle : `mcd1$myDate` au lieu de `myDate`. Précédemment, le code JavaScript utilisé `mcd1_dpe1` pour accéder au `DynamicPopulateExtender` au lieu de `dpe1`. Cette stratégie de nommage est une exigence spéciale lors de l’utilisation de `DynamicPopulateExtender` dans un contrôle utilisateur. En outre, vous devez incorporer le contrôle utilisateur d’une manière spécifique pour le faire fonctionner. Créez une nouvelle page ASP.NET et enregistrez un préfixe de balise pour le contrôle utilisateur que vous venez d’implémenter :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Incluez ensuite le contrôle de `ScriptManager` AJAX ASP.NET sur la nouvelle page :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Enfin, ajoutez le contrôle utilisateur à la page. Il vous suffit de définir son attribut `ID` (et `runat="server"`, bien évidemment), mais vous devez également le définir sur un nom spécifique : `mcd1` puisque c’est le préfixe utilisé dans le contrôle utilisateur pour y accéder à l’aide de JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

Et voilà ! La page se comporte comme prévu : un utilisateur clique sur l’une des cases d’option, le contrôle de la boîte à outils appelle le service Web et affiche la date actuelle au format souhaité.

[![les cases d’option résident dans un contrôle utilisateur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Les cases d’option résident dans un contrôle utilisateur ([cliquez pour afficher l’image en taille réelle](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](dynamically-populating-a-control-using-javascript-code-vb.md)
