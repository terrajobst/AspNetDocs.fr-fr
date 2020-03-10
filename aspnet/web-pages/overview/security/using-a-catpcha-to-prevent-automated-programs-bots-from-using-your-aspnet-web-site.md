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
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="58e27-103">Utilisation d’un CAPTCHA pour empêcher les robots d’utiliser votre site ASP.NET Web Razor)</span><span class="sxs-lookup"><span data-stu-id="58e27-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="58e27-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="58e27-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="58e27-105">Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés d’effectuer des tâches dans un site Web pages Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="58e27-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="58e27-106">**Ce que vous allez apprendre :**</span><span class="sxs-lookup"><span data-stu-id="58e27-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="58e27-107">Comment ajouter un test CAPTCHA à votre site.</span><span class="sxs-lookup"><span data-stu-id="58e27-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="58e27-108">Voici les fonctionnalités ASP.NET présentées dans l’article :</span><span class="sxs-lookup"><span data-stu-id="58e27-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="58e27-109">Le programme d’assistance `ReCaptcha`.</span><span class="sxs-lookup"><span data-stu-id="58e27-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="58e27-110">Les informations contenues dans cet article s’appliquent à pages Web ASP.NET 1,0 et pages Web 2.</span><span class="sxs-lookup"><span data-stu-id="58e27-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="58e27-111">À propos de CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="58e27-111">About CAPTCHAs</span></span>

<span data-ttu-id="58e27-112">Chaque fois que vous autorisez les utilisateurs à s’inscrire sur votre site, ou même simplement à entrer un nom et une URL (comme pour un commentaire de blog), vous pouvez obtenir un flux de noms factices.</span><span class="sxs-lookup"><span data-stu-id="58e27-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="58e27-113">Ils sont souvent laissés par des programmes automatisés (robots) qui essaient de laisser des URL dans chaque site Web qu’ils peuvent trouver.</span><span class="sxs-lookup"><span data-stu-id="58e27-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="58e27-114">(Une motivation courante consiste à poster les URL des produits à vendre.)</span><span class="sxs-lookup"><span data-stu-id="58e27-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="58e27-115">Vous pouvez vous assurer qu’un utilisateur est bien une personne physique et non un programme informatique en utilisant un *captcha* pour valider les utilisateurs lorsqu’ils inscrivent ou entrent leur nom et leur site.</span><span class="sxs-lookup"><span data-stu-id="58e27-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="58e27-116">CAPTCHA signifie que le test Turing public entièrement automatisé permet de distinguer les ordinateurs et les êtres humains.</span><span class="sxs-lookup"><span data-stu-id="58e27-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="58e27-117">Un CAPTCHA est un test de *stimulation-réponse* dans lequel l’utilisateur est invité à effectuer une tâche facile à effectuer, mais difficile à exécuter par un programme automatisé.</span><span class="sxs-lookup"><span data-stu-id="58e27-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="58e27-118">Le type le plus courant de CAPTCHA est celui dans lequel vous voyez des lettres distordues et vous êtes invité à les taper.</span><span class="sxs-lookup"><span data-stu-id="58e27-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="58e27-119">(La distorsion est supposée être difficile pour les robots de déchiffrer les lettres.)</span><span class="sxs-lookup"><span data-stu-id="58e27-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="58e27-120">Ajout d’un test ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="58e27-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="58e27-121">Dans les pages ASP.NET, vous pouvez utiliser le programme d’assistance `ReCaptcha` pour restituer un test CAPTCHA basé sur le service ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="58e27-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="58e27-122">Le `ReCaptcha` Helper affiche une image de deux mots déformés que les utilisateurs doivent entrer correctement avant la validation de la page.</span><span class="sxs-lookup"><span data-stu-id="58e27-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="58e27-123">La réponse de l’utilisateur est validée par le service ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="58e27-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="58e27-124">Inscrivez votre site Web à ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="58e27-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="58e27-125">Une fois l’inscription terminée, vous obtenez une clé publique et une clé privée.</span><span class="sxs-lookup"><span data-stu-id="58e27-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="58e27-126">Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.</span><span class="sxs-lookup"><span data-stu-id="58e27-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="58e27-127">Si vous ne disposez pas déjà d’un fichier *\_AppStart. cshtml* , dans le dossier racine d’un site Web, créez un fichier nommé *\_AppStart. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="58e27-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="58e27-128">Ajoutez les paramètres d’assistance de `Recaptcha` suivants dans le fichier *\_AppStart. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="58e27-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="58e27-129">Définissez les propriétés `PublicKey` et `PrivateKey` à l’aide de vos propres clés publiques et privées.</span><span class="sxs-lookup"><span data-stu-id="58e27-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="58e27-130">Enregistrez le fichier *\_AppStart. cshtml* et fermez-le.</span><span class="sxs-lookup"><span data-stu-id="58e27-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="58e27-131">Dans le dossier racine d’un site Web, créez une nouvelle page nommée *recaptcha. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="58e27-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="58e27-132">Remplacez le contenu existant par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="58e27-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="58e27-133">Exécutez la page *recaptcha. cshtml* dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="58e27-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="58e27-134">Si la valeur `PrivateKey` est valide, la page affiche le contrôle ReCaptcha et un bouton.</span><span class="sxs-lookup"><span data-stu-id="58e27-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="58e27-135">Si vous n’avez pas défini les clés globalement dans *\_AppStart. html*, la page affiche une erreur.</span><span class="sxs-lookup"><span data-stu-id="58e27-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="58e27-136">Entrez les mots du test.</span><span class="sxs-lookup"><span data-stu-id="58e27-136">Enter the words for the test.</span></span> <span data-ttu-id="58e27-137">Si vous transmettez le test ReCaptcha, vous voyez un message à cet effet.</span><span class="sxs-lookup"><span data-stu-id="58e27-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="58e27-138">Dans le cas contraire, un message d’erreur s’affiche et le contrôle ReCaptcha est réaffiché.</span><span class="sxs-lookup"><span data-stu-id="58e27-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="58e27-139">Si votre ordinateur se trouve sur un domaine qui utilise un serveur proxy, vous devrez peut-être configurer l’élément `defaultproxy` du fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="58e27-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="58e27-140">L’exemple suivant montre un fichier *Web. config* avec l’élément `defaultproxy` configuré pour permettre le fonctionnement du service recaptcha.</span><span class="sxs-lookup"><span data-stu-id="58e27-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="58e27-141">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="58e27-141">Additional Resources</span></span>

- [<span data-ttu-id="58e27-142">Personnalisation du comportement à l’ensemble du site pour les sites pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="58e27-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="58e27-143">Site ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="58e27-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
