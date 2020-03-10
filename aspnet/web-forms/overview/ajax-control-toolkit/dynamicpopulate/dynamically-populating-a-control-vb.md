---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Remplissage dynamique d’un contrôle (VB) | Microsoft Docs
author: wenz
description: Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d11320f1f89bb69afe5f62751574079716124da0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535685"
---
# <a name="dynamically-populating-a-control-vb"></a>Remplissage dynamique d’un contrôle (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.

## <a name="overview"></a>Présentation

Le contrôle `DynamicPopulate` de la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page. Ce didacticiel montre comment le configurer.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin d’un service Web ASP.NET qui implémente la méthode à appeler par `DynamicPopulate`. La classe de service Web requiert l’attribut `ScriptService` qui est défini dans `Microsoft.Web.Script.Services`; Sinon, ASP.NET AJAX ne peut pas créer le proxy JavaScript côté client pour le service Web, qui est à son tour requis par `DynamicPopulate`.

La méthode Web doit s’attendre à un argument de type String, appelé `contextKey`, puisque le contrôle `DynamicPopulate` envoie une partie des informations de contexte avec chaque appel de service Web. Le service Web suivant retourne la date actuelle dans un format représenté par l’argument `contextKey` :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Le service Web est ensuite enregistré en tant que `DynamicPopulate.vb.asmx`. Vous pouvez également implémenter la méthode `getDate()` en tant que méthode de page dans la page ASP.NET réelle avec le contrôle `DynamicPopulate`.

À l’étape suivante, créez un nouveau fichier ASP.NET. Comme toujours, la première étape consiste à inclure la `ScriptManager` dans la page actuelle pour charger la bibliothèque AJAX ASP.NET et pour faire fonctionner le jeu d’outils de contrôle :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Ensuite, ajoutez un contrôle Label (par exemple à l’aide du contrôle HTML du même nom, ou le &lt;`asp:Label` /&gt; contrôle Web) qui affichera ultérieurement le résultat de l’appel de service Web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Un bouton HTML (comme un contrôle HTML, puisque nous n’avons pas besoin d’une publication sur le serveur) sera ensuite utilisé pour déclencher le remplissage dynamique :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Enfin, nous avons besoin du contrôle `DynamicPopulateExtender` pour relier les choses. Les attributs suivants seront définis (en plus de ceux qui sont évidents, `ID` et `runat`=`"server"`) :

- `TargetControlID` où placer le résultat de l’appel de service Web
- `ServicePath` chemin d’accès au service Web (omettre si vous souhaitez utiliser une méthode de page)
- `ServiceMethod` le nom de la méthode Web ou de la méthode de page
- `ContextKey` les informations de contexte à envoyer au service Web
- `PopulateTriggerControlID` élément qui déclenche l’appel de service Web
- `ClearContentsDuringUpdate` s’il faut vider l’élément cible pendant l’appel du service Web

Comme vous pouvez le voir, le contrôle nécessite des informations, mais tout mettre en place est tout à fait simple. Voici le balisage du contrôle `DynamicPopulateExtender` dans le scénario actuel :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Exécutez la page ASP.NET dans le navigateur, puis cliquez sur le bouton. vous recevrez la date actuelle au format mois-jour-année.

[![un clic sur le bouton récupère la date à partir du serveur](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Un clic sur le bouton permet de récupérer la date à partir du serveur ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Suivant](dynamically-populating-a-control-using-javascript-code-vb.md)
