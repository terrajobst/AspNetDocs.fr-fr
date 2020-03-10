---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Autoriser uniquement certains caractères dans une zone de texte (VB) | Microsoft Docs
author: wenz
description: Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur. Toutefois, cela n’empêche toujours pas les utilisateurs de taper du texte non valide...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613511"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Autoriser seulement certains caractères dans une zone de texte (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur. Toutefois, cela n’empêche toujours pas les utilisateurs de taper des caractères non valides et de tenter d’envoyer le formulaire.

## <a name="overview"></a>Présentation

Les contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée utilisateur. Toutefois, cela n’empêche toujours pas les utilisateurs de taper des caractères non valides et de tenter d’envoyer le formulaire.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient le contrôle `FilteredTextBox` qui étend une zone de texte. Une fois activé, seul un certain jeu de caractères peut être entré dans le champ.

Pour que cela fonctionne, nous avons tout d’abord besoin de la `ScriptManager` AJAX ASP.NET qui charge les bibliothèques JavaScript qui sont également utilisées par ASP.NET AJAX Control Toolkit :

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Ensuite, nous avons besoin d’une zone de texte :

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Enfin, le contrôle `FilteredTextBoxExtender` s’occupe de limiter les caractères que l’utilisateur est autorisé à taper. Tout d’abord, affectez à l’attribut `TargetControlID` la `ID` du contrôle `TextBox`. Ensuite, choisissez l’une des valeurs de `FilterType` disponibles :

- `Custom` par défaut ; vous devez fournir une liste de caractères valides
- `LowercaseLetters` les lettres minuscules uniquement
- `Numbers` chiffres uniquement
- `UppercaseLetters` lettres majuscules uniquement

Si le `Custom FilterType` est utilisé, la propriété `ValidChars` doit être définie et fournir une liste de caractères pouvant être tapés. À la façon : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.

Voici le balisage pour le contrôle `FilteredTextBoxExtender` qui autorise uniquement les chiffres (ce qui serait également possible avec `FilterType="Numbers"`) :

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Exécutez la page et essayez d’entrer une lettre si JavaScript est activé, mais cela ne fonctionnera pas. Toutefois, les chiffres apparaissent sur la page. Toutefois, Notez que le `FilteredTextBox` de protection fourni n’est pas une preuve de la puce : si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte. vous devez donc utiliser des moyens de validation supplémentaires, par exemple, ASP. Contrôles de validation du réseau.

[![seuls les chiffres peuvent être entrés](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](allowing-only-certain-characters-in-a-text-box-cs.md)
