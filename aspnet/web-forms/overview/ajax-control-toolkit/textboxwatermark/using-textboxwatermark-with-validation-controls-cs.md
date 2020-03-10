---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Utilisation de TextBoxWatermark avec des contrôlesC#de validation () | Microsoft Docs
author: wenz
description: Le contrôle TextBoxWatermark dans la boîte à outils du contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Quand un utilisateur clique sur la zone, je vais...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577839"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a>Utilisation de TextBoxWatermark avec des contrôles Validation (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Le contrôle TextBoxWatermark dans la boîte à outils du contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique sur la zone, il est vidé. Si l’utilisateur quitte la zone sans entrer de texte, le texte prérempli réapparaît. Cela peut entrer en conflit avec des contrôles de validation ASP.NET sur la même page, mais ces problèmes peuvent être résolus.

## <a name="overview"></a>Présentation

Le contrôle `TextBoxWatermark` dans le kit d’outils de contrôle AJAX étend une zone de texte pour qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique sur la zone, il est vidé. Si l’utilisateur quitte la zone sans entrer de texte, le texte prérempli réapparaît. Cela peut entrer en conflit avec des contrôles de validation ASP.NET sur la même page, mais ces problèmes peuvent être résolus.

## <a name="steps"></a>Étapes

Le paramétrage de base de l’exemple est le suivant : un contrôle de `TextBox` est en filigrane à l’aide d’un contrôle `TextBoxWatermarkExtender`. Un bouton déclenche une publication (postback) et sera utilisé ultérieurement pour déclencher les contrôles de validation sur la page. En outre, un contrôle de `ScriptManager` est requis pour initialiser ASP.NET AJAX :

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Ajoutez maintenant un contrôle de `RequiredFieldValidator` qui vérifie s’il existe un texte dans le champ lorsque le formulaire est envoyé. La valeur de la propriété `InitialValue` du validateur doit être identique à celle utilisée dans le contrôle `TextBoxWatermarkExtender` : lorsque le formulaire est envoyé, la valeur d’une zone de texte inchangée est la valeur de filigrane dans celle-ci :

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Toutefois, il existe un problème avec cette approche : si le client désactive JavaScript, le champ de texte n’est pas prérempli avec le texte de filigrane, par conséquent le `RequiredFieldValidator` ne déclenche pas de message d’erreur. Par conséquent, un deuxième `RequiredFieldValidator` contrôle est requis, qui recherche une zone de texte vide (en omettant l’attribut `InitialValue`).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Étant donné que les deux validateurs utilisent `Display`=`"Dynamic"`, l’utilisateur final ne peut pas distinguer de l’apparence visuelle laquelle des deux validateurs ont été déclenchés ; au lieu de cela, il semblerait qu’il n’y en avait qu’un seul.

Enfin, ajoutez du code côté serveur pour générer le texte dans le champ si aucun validateur n’a émis de message d’erreur :

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

[![le validateur se plaint qu’il n’y a aucun texte dans le champ](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Le validateur se plaint qu’il n’y a pas de texte dans le champ ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-textboxwatermark-in-a-formview-cs.md)
> [Suivant](using-textboxwatermark-in-a-formview-vb.md)
