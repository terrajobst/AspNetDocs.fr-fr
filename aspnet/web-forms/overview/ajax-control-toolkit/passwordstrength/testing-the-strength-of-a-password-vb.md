---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Test de la force d’un mot de passe (VB) | Microsoft Docs
author: wenz
description: Les mots de passe sont nécessaires presque n’importe où, de sorte que les utilisateurs paresseux ont tendance à choisir des mots de passe simples faciles à rompre. Contrôle PasswordStrength dans l’ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: b614e1788eeafc175dd792ec6d3e4619f9ea2b7a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606249"
---
# <a name="testing-the-strength-of-a-password-vb"></a>Test de la force d’un mot de passe (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Les mots de passe sont nécessaires presque n’importe où, de sorte que les utilisateurs paresseux ont tendance à choisir des mots de passe simples faciles à rompre. Le contrôle PasswordStrength dans ASP.NET AJAX Control Toolkit peut vérifier la qualité d’un mot de passe.

## <a name="overview"></a>Vue d'ensemble de

Les mots de passe sont nécessaires presque n’importe où, de sorte que les utilisateurs paresseux ont tendance à choisir des mots de passe simples faciles à rompre. Le contrôle `PasswordStrength` de la boîte à outils de contrôle AJAX ASP.NET peut vérifier la qualité d’un mot de passe.

## <a name="steps"></a>Étapes

Le contrôle `PasswordStrength` étend une zone de texte et vérifie si le mot de passe est suffisamment correct. Il offre une multitude d’options via les attributs. Voici quelques-unes d’entre elles :

- `MinimumNumericCharacters` nombre minimal de caractères numériques requis dans le mot de passe
- `MinimumSymbolCharacters` nombre minimal de caractères de symbole (et non de lettres et de chiffres) requis dans le mot de passe
- `PreferredPasswordLength` la longueur minimale du mot de passe
- `RequiresUpperAndLowerCaseCharacters` si le mot de passe doit utiliser des caractères majuscules et minuscules

Le `StrengthIndicatorType` fournit des informations sur la façon de présenter la force du mot de passe, sous forme de texte (valeur `"Text"`) ou sous la forme d’un type de barre de progression (valeur `"BarIndicator"`). Dans l’attribut `DisplayPosition`, vous configurez l’emplacement où les informations s’affichent. Voici un exemple complet, notamment le contrôle de `ScriptManager` AJAX ASP.NET, le contrôle `PasswordStrength` et, bien sûr, une zone de texte dans laquelle l’utilisateur peut entrer un mot de passe. À des fins de démonstration, le champ de formulaire est un champ de texte normal et non un champ de mot de passe, qui vous permet de voir pendant le développement ce que vous tapez.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Exécutez la page et tapez absent : une fois que vous avez entré des minuscules, des lettres majuscules, des chiffres et des symboles, le mot de passe est considéré comme non réutilisable.

[![maintenant, le mot de passe est (très) correct](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

Désormais, le mot de passe est (très) correct ([cliquez pour afficher l’image en taille réelle](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](testing-the-strength-of-a-password-cs.md)
