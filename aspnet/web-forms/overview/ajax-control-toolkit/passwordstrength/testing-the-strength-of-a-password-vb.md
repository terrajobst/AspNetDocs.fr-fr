---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Test de la force de mot de passe (VB) | Microsoft Docs
author: wenz
description: Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différés ont tendance à choisir des mots de passe simples qui sont faciles à arrêter. Le contrôle PasswordStrength dans ASP. N....
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: fb185c4147d516ab28d632b3e874b6f1d46f6576
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408418"
---
# <a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="d8b21-104">Test de la force d’un mot de passe (VB)</span><span class="sxs-lookup"><span data-stu-id="d8b21-104">Testing the Strength of a Password (VB)</span></span>

<span data-ttu-id="d8b21-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d8b21-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d8b21-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d8b21-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="d8b21-107">Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différés ont tendance à choisir des mots de passe simples qui sont faciles à arrêter.</span><span class="sxs-lookup"><span data-stu-id="d8b21-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="d8b21-108">Le contrôle PasswordStrength dans ASP.NET AJAX Control Toolkit peut vérifier la qualité est d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8b21-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="d8b21-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="d8b21-109">Overview</span></span>

<span data-ttu-id="d8b21-110">Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différés ont tendance à choisir des mots de passe simples qui sont faciles à arrêter.</span><span class="sxs-lookup"><span data-stu-id="d8b21-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="d8b21-111">Le `PasswordStrength` contrôle dans ASP.NET AJAX Control Toolkit peut vérifier la qualité est d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8b21-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="d8b21-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="d8b21-112">Steps</span></span>

<span data-ttu-id="d8b21-113">Le `PasswordStrength` contrôle étend une zone de texte et vérifie si le mot de passe qu’il contient est suffisant.</span><span class="sxs-lookup"><span data-stu-id="d8b21-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="d8b21-114">Il offre une multitude d’options via des attributs ; Voici quelques-uns d'entre eux :</span><span class="sxs-lookup"><span data-stu-id="d8b21-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- `MinimumNumericCharacters` <span data-ttu-id="d8b21-115">nombre minimal de caractères numériques requis dans le mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8b21-115">minimum number of numeric characters required in the password</span></span>
- `MinimumSymbolCharacters` <span data-ttu-id="d8b21-116">nombre minimal de caractères de symbole (pas des lettres et chiffres) requis dans le mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8b21-116">minimum number of symbol characters (not letters and digits) required in the password</span></span>
- `PreferredPasswordLength` <span data-ttu-id="d8b21-117">longueur minimale du mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8b21-117">minimum length of the password</span></span>
- `RequiresUpperAndLowerCaseCharacters` <span data-ttu-id="d8b21-118">Indique si le mot de passe doit utiliser des caractères majuscules et minuscules</span><span class="sxs-lookup"><span data-stu-id="d8b21-118">whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="d8b21-119">Le `StrengthIndicatorType` fournit les informations comment présenter la force du mot de passe, sous forme de texte (valeur `"Text"`) ou comme une sorte de barre de progression (valeur `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="d8b21-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="d8b21-120">Dans le `DisplayPosition` attribut, vous configurez dans lequel les informations s’affichent.</span><span class="sxs-lookup"><span data-stu-id="d8b21-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="d8b21-121">Voici un exemple complet, y compris ASP.NET AJAX `ScriptManager` contrôle, le `PasswordStrength` contrôle et, bien sûr, une zone de texte où l’utilisateur peut entrer un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8b21-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="d8b21-122">Fins de démonstration, le champ de formulaire de ce dernier est un champ de texte normal et non un champ de mot de passe afin que vous puissiez voir pendant le développement ce que vous tapez.</span><span class="sxs-lookup"><span data-stu-id="d8b21-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="d8b21-123">Exécutez la page et tapez suite : Uniquement une fois que vous avez entré des lettres minuscules, lettres majuscules, chiffres et symboles, le mot de passe est jugé comme unbreakable.</span><span class="sxs-lookup"><span data-stu-id="d8b21-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


[![N<span data-ttu-id="d8b21-124">Comment puis-je le mot de passe est bonne (assez)]</span><span class="sxs-lookup"><span data-stu-id="d8b21-124">ow the password is (quite) good]</span></span>(testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

<span data-ttu-id="d8b21-125">Maintenant le mot de passe (assez) convient ([cliquez pour afficher l’image en taille réelle](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d8b21-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d8b21-126">Précédent</span><span class="sxs-lookup"><span data-stu-id="d8b21-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
