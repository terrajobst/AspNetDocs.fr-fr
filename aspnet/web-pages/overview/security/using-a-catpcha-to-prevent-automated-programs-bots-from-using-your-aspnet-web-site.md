---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Utilisation d’un CAPTCHA pour empêcher les robots d’utiliser votre site ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (robots) d’effectuer des tâches dans un pages Web ASP.NET (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547046"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Utilisation d’un CAPTCHA pour empêcher les robots d’utiliser votre site ASP.NET Web Razor)

par [Microsoft](https://github.com/microsoft)

> Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés d’effectuer des tâches dans un site Web pages Web ASP.NET (Razor).
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment ajouter un test CAPTCHA à votre site.
> 
> Voici les fonctionnalités ASP.NET présentées dans l’article :
> 
> - Le programme d’assistance `ReCaptcha`.
> 
> > [!NOTE]
> > Les informations contenues dans cet article s’appliquent à pages Web ASP.NET 1,0 et pages Web 2.

## <a name="about-captchas"></a>À propos de CAPTCHAs

Chaque fois que vous autorisez les utilisateurs à s’inscrire sur votre site, ou même simplement à entrer un nom et une URL (comme pour un commentaire de blog), vous pouvez obtenir un flux de noms factices. Ils sont souvent laissés par des programmes automatisés (robots) qui essaient de laisser des URL dans chaque site Web qu’ils peuvent trouver. (Une motivation courante consiste à poster les URL des produits à vendre.)

Vous pouvez vous assurer qu’un utilisateur est bien une personne physique et non un programme informatique en utilisant un *captcha* pour valider les utilisateurs lorsqu’ils inscrivent ou entrent leur nom et leur site. CAPTCHA signifie que le test Turing public entièrement automatisé permet de distinguer les ordinateurs et les êtres humains. Un CAPTCHA est un test de *stimulation-réponse* dans lequel l’utilisateur est invité à effectuer une tâche facile à effectuer, mais difficile à exécuter par un programme automatisé. Le type le plus courant de CAPTCHA est celui dans lequel vous voyez des lettres distordues et vous êtes invité à les taper. (La distorsion est supposée être difficile pour les robots de déchiffrer les lettres.)

## <a name="adding-a-recaptcha-test"></a>Ajout d’un test ReCaptcha

Dans les pages ASP.NET, vous pouvez utiliser le programme d’assistance `ReCaptcha` pour restituer un test CAPTCHA basé sur le service ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). Le `ReCaptcha` Helper affiche une image de deux mots déformés que les utilisateurs doivent entrer correctement avant la validation de la page. La réponse de l’utilisateur est validée par le service ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Inscrivez votre site Web à ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Une fois l’inscription terminée, vous obtenez une clé publique et une clé privée.
2. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
3. Si vous ne disposez pas déjà d’un fichier *\_AppStart. cshtml* , dans le dossier racine d’un site Web, créez un fichier nommé *\_AppStart. cshtml*.
4. Ajoutez les paramètres d’assistance de `Recaptcha` suivants dans le fichier *\_AppStart. cshtml* : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Définissez les propriétés `PublicKey` et `PrivateKey` à l’aide de vos propres clés publiques et privées.
6. Enregistrez le fichier *\_AppStart. cshtml* et fermez-le.
7. Dans le dossier racine d’un site Web, créez une nouvelle page nommée *recaptcha. cshtml*.
8. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Exécutez la page *recaptcha. cshtml* dans un navigateur. Si la valeur `PrivateKey` est valide, la page affiche le contrôle ReCaptcha et un bouton. Si vous n’avez pas défini les clés globalement dans *\_AppStart. html*, la page affiche une erreur. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Entrez les mots du test. Si vous transmettez le test ReCaptcha, vous voyez un message à cet effet. Dans le cas contraire, un message d’erreur s’affiche et le contrôle ReCaptcha est réaffiché.

> [!NOTE]
> Si votre ordinateur se trouve sur un domaine qui utilise un serveur proxy, vous devrez peut-être configurer l’élément `defaultproxy` du fichier *Web. config* . L’exemple suivant montre un fichier *Web. config* avec l’élément `defaultproxy` configuré pour permettre le fonctionnement du service recaptcha.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Personnalisation du comportement à l’ensemble du site pour les sites pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Site ReCaptcha](https://www.google.com/recaptcha)
