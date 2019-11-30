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
# <a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="8a1e5-104">Test de la force d’un mot de passe (VB)</span><span class="sxs-lookup"><span data-stu-id="8a1e5-104">Testing the Strength of a Password (VB)</span></span>

<span data-ttu-id="8a1e5-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8a1e5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8a1e5-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8a1e5-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="8a1e5-107">Les mots de passe sont nécessaires presque n’importe où, de sorte que les utilisateurs paresseux ont tendance à choisir des mots de passe simples faciles à rompre.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="8a1e5-108">Le contrôle PasswordStrength dans ASP.NET AJAX Control Toolkit peut vérifier la qualité d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="overview"></a><span data-ttu-id="8a1e5-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="8a1e5-109">Overview</span></span>

<span data-ttu-id="8a1e5-110">Les mots de passe sont nécessaires presque n’importe où, de sorte que les utilisateurs paresseux ont tendance à choisir des mots de passe simples faciles à rompre.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="8a1e5-111">Le contrôle `PasswordStrength` de la boîte à outils de contrôle AJAX ASP.NET peut vérifier la qualité d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="8a1e5-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="8a1e5-112">Steps</span></span>

<span data-ttu-id="8a1e5-113">Le contrôle `PasswordStrength` étend une zone de texte et vérifie si le mot de passe est suffisamment correct.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="8a1e5-114">Il offre une multitude d’options via les attributs. Voici quelques-unes d’entre elles :</span><span class="sxs-lookup"><span data-stu-id="8a1e5-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="8a1e5-115">`MinimumNumericCharacters` nombre minimal de caractères numériques requis dans le mot de passe</span><span class="sxs-lookup"><span data-stu-id="8a1e5-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="8a1e5-116">`MinimumSymbolCharacters` nombre minimal de caractères de symbole (et non de lettres et de chiffres) requis dans le mot de passe</span><span class="sxs-lookup"><span data-stu-id="8a1e5-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="8a1e5-117">`PreferredPasswordLength` la longueur minimale du mot de passe</span><span class="sxs-lookup"><span data-stu-id="8a1e5-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="8a1e5-118">`RequiresUpperAndLowerCaseCharacters` si le mot de passe doit utiliser des caractères majuscules et minuscules</span><span class="sxs-lookup"><span data-stu-id="8a1e5-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="8a1e5-119">Le `StrengthIndicatorType` fournit des informations sur la façon de présenter la force du mot de passe, sous forme de texte (valeur `"Text"`) ou sous la forme d’un type de barre de progression (valeur `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="8a1e5-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="8a1e5-120">Dans l’attribut `DisplayPosition`, vous configurez l’emplacement où les informations s’affichent.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="8a1e5-121">Voici un exemple complet, notamment le contrôle de `ScriptManager` AJAX ASP.NET, le contrôle `PasswordStrength` et, bien sûr, une zone de texte dans laquelle l’utilisateur peut entrer un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="8a1e5-122">À des fins de démonstration, le champ de formulaire est un champ de texte normal et non un champ de mot de passe, qui vous permet de voir pendant le développement ce que vous tapez.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="8a1e5-123">Exécutez la page et tapez absent : une fois que vous avez entré des minuscules, des lettres majuscules, des chiffres et des symboles, le mot de passe est considéré comme non réutilisable.</span><span class="sxs-lookup"><span data-stu-id="8a1e5-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>

<span data-ttu-id="8a1e5-124">[![maintenant, le mot de passe est (très) correct](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8a1e5-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="8a1e5-125">Désormais, le mot de passe est (très) correct ([cliquez pour afficher l’image en taille réelle](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8a1e5-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8a1e5-126">Précédent</span><span class="sxs-lookup"><span data-stu-id="8a1e5-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
